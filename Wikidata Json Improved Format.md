# Wikidata Full JSON dump improved format suggestion

## Overview

The [current format](Wikidata Json Format.md) has probably been patched and added to for several
years. There is a lot of redundancy and reading it sequentially
is quite hard.

This format suggestion tries both to remedy both the redundancy and the problems with the file being hard to read sequentially.

The basic premise of the file: One line per item is the same.


## Entry types - Item or Property

The first property in each an every entry is the 
`type` property which states whether the entry
represents an `item` or `property`.

This is redunant since the `id` property always
starts with `Q` for items and `P` for properties.

So I suggest that the `type` property is dropped.

## Languages

Some easy space saving wins come from the labels, descriptions and aliases.

They make up a substantial amount of a lot of items and making these smaller of course makes it faster to create the dump, but will also make it a lot faster to read and process the dump, including using less memory.

A few, hand-optimized examples suggests a space saving on 30% only by optimizing these three sections.

### ``labels``


The ``labels`` section is an object with a property for each language for which a label exists:

**Original format:**

````json
"labels": {
    "en-gb": {
        "language": "en-gb",
        "value": "Northern Ireland"
    },...
````

**New format:**

````json
"labels": [
    "en-gb": "Northern Ireland",
    ...
]
````

**or:**

````json
"labels": [
    "Q7979": "Northern Ireland",
    ...
]
````

Since there is only one value per language it makes no sense
to have an object for each.

The original, compressed version:

````json
"en-gb":{"language":"en-gb","value":"Northern Ireland"}
````

takes up 55 characters, whereas the new, compressed version:

````json
"en-gb":"Northern Ireland"
````

takes up only 26 characters!

### ``descriptions``

The same can be done with descriptions:

````json
"descriptions": {
    "en-gb": {
        "language": "en-gb",
        "value": "region in north-west Europe, part of the United Kingdom"
    },...
````

````json
"descriptions": [
    "en-gb": "region in north-west Europe, part of the United Kingdom",
    ...
]
````

As with *labels* we could also use *Q7979* here.

The compressed versions:

````json
"en-gb":{"language":"en-gb","value":"region in north-west Europe, part of the United Kingdom"}
````

````json
"en-gb":"region in north-west Europe, part of the United Kingdom"
````

Goes from a whopping 94 characters to a mere 65.

### ``aliases``

Aliases are a bit different, since there can be more than one alias per language.

Original version:

````json
"aliases": {
  "sco": [
    {
      "language": "sco",
      "value": "Norlin Airlan"
    },
    {
      "language": "sco",
      "value": "Norlin Airlann"
    }
  ],...
}
````

Still, quite a lot can be saved:

````json
"sco": [
      "Norlin Airlan",
      "Norlin Airlann",
      ...
    }
  ]  
````

The corresponding compressed versions:

````json
"sco":[{"language":"sco","value":"Norlin Airlan"},{"language":"sco","value":"Norlin Airlann"}]
````

````json
"sco":["Norlin Airlan","Norlin Airlann"]
````

Goes from 94 to a meagre 40 characters per language. A substantial saving!

## Claims

Claims can of course be simplified like crazy - here's
a first draft idea:

````json
"claims": [
  {    
    "property": "P31",
    "id": string, //unique and looong id for the claim
    "snak": {
      "type": number, // 0="value", 1="somevalue", 2="novalue"
      "type": string, // "type" before "value"
      "value": object, // type and layout depending on datatype
    },
    "qualifiers": object[] // list of qualifier snaks, optional
    "qorder": string[], // optional
    "rank": number, // 0="normal", 1="preferred", 2="deprecated"
    "refs": object[],// optional
  }
]
````
