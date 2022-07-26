# Wikidata Full JSON dump documentation

This documentation is not supposed to be the authoritative guide but being based
partly on [Wikibase/DataModel/JSON](https://doc.wikimedia.org/Wikibase/master/php/md_docs_topics_json.html), which is partly incorrect and incomplete,
and on countless hours sifting through the data will lend it some weight.

## Overview

The full dump file is a JSON file - located on the [wikimedia website](https://dumps.wikimedia.org/wikidatawiki/entities/) - in various compression formats starting
with the name *latest-all*.

The format is a mess, lengthy and hard to read, probably stuff has been added on
over the years, but that's how it it is right now - please see [Improved Format](Wikidata%20Json%20Improved%20Format.md) for some sugggestions on how to improve the format.

The file is a JSON array of all the items and properties
which are conveniently placed with one item/property on one line (with UNIX line endings) making it considerably easier to read the file one line/item/property at a time, which also is - due to the excessive amount of data - very necessary.

Around 1th of July 2022 the downloaded, compressed json-file (latest-all.json.bz2) is roughly 71GB and the decompressed file (latest-all.json) is
14068MB or close to 1.4TB. That the file is so
compressible does say something about the inherent
redundancy in the file.

Each line is a JSON object making up either an item or a property as seen by the
``type`` property. From there on the formats differ, but only slightly. The basic
format of the file is like this:

```` json
[
{ ... } // item or property },
{ ... } // item or property },
...
]
````

Since the document is valid JSON there is a comma at the end of each line. This may be a challenge for some JSON readers.

## Properties

A property is used to *state* something about an *Item*.

```` json
{
    "type":         // string: values: "property"
    "datatype":     // string: values: see below
    "id":           // string: values: integer starting with P - e.g. "P31"
    "labels":       // object
    "descriptions": // object
    "aliases":      // object
    "claims":       // object
    "pageid":       // number: - internal something
    "ns":           // number: - something else internal
    "title":        // string: - something internal    
    "lastrevid":    // number: An internal revision id from Wikidata
    "lastmodified": // datetime - last modified date of the item
}
````

## Items

An item is an actual object, thing or concept, e.g. like **Back to The Future**, which
is a movie or **London**, which is a city.

The general structure for items looks like this. The order of properties is important:

```` json
{
    "type":    // string: values: "item"
    "id":      // string: values: integer starting with Q   - e.g. "Q91540"
    "labels":  // object
    "descriptions": // object
    "aliases":      // object
    "claims":       // object
    "sitelinks":    // object
    "pageid":       // number: - internal something
    "ns":           // number: - something else internal
    "title":        // string: - something internal    
    "lastrevid":    // number: An internal revision id from Wikidata
    "lastmodified": // datetime - last modified date of the item
}
````

**NOTE:** According to [Wikibase/DataModel/JSON](https://doc.wikimedia.org/Wikibase/master/php/md_docs_topics_json.html)
there is a ``modified`` property for an item (and property) which is now finally available from around 1. July 2022 (after years of waiting). Thanks! Unfortunately along
with it came the properties ``pageid``, ``ns``, ``title`` which all are utterly useless
but of course takes up valuable space, bandwidth and reading time.

## About languages

Language identifiers are used all over the place and can be looked up on
[Help:Wikimedia language codes/lists/all](https://www.wikidata.org/wiki/Help:Wikimedia_language_codes/lists/all),
but more importantly each of them is in itself a Wikidata item with the property P424
[Wikimedia language code (P424)](https://www.wikidata.org/wiki/Property:P424).

For some reason, though, links to languages in labels, aliases, descriptions etc. are never done with the Qxxxx item
identifier but always with Wikimedia language code.

## ``labels``

The ``labels`` section is an object with a property for each language for which a label exists:

```` json
"labels": {
    "en-gb": {
        "language": "en-gb",
        "value": "Northern Ireland"
    },...
````

As it can be seen the ``language`` property is redundant.

A label is what the "thing" is mainly called in different languages.

Since JSON only allows one instance of each property only one label can exist for each language. This also means that having the value inside an object
is really not necessary.

## ``descriptions``

The ``descriptions`` section is an object with a property for each language for which a description exists:

```` json
"descriptions": {
    "en-gb": {
        "language": "en-gb",
        "value": "region in north-west Europe, part of the United Kingdom"
    },...
````

As it can be seen the ``language`` property is redundant.

A description is a more
thorough explanation than a label. There is not necessarily both a label and a
description in the same language.

Since JSON only allows one instance of each property only one description can exist for each language.

## ``aliases``

The ``aliases`` section is an object with a property for each language for which aliases exist. In contrast to 
``labels`` and ``descriptions`` there can be more than one ``alias`` for each language, for which an array
is needed:

```` json
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

As it can be seen the ``language`` property is very redundant.

An alias can be seen as alternative, secondary labels.

## Claims

Claims are used to tell *what* an item actually is. The ``claims`` section is an object with a
property for each claim. Each claim is an array of anonymous objects which together make up actual, individual claims - or *statements*.

Claims exist for both properties and items.

Claims are rather complex and difficult to describe, but here goes, starting with the general
layout of the claims object.

Please note that the [official documentation](https://doc.wikimedia.org/Wikibase/master/php/md_docs_topics_json.html) lists the properties in **incorrect order** which might
surprise you when trying to read the JSON manually.

The correct order is as follows:

```` json
"claims": {
    "P31": [ // The type of claim - here "instance of" - note the array start [. 
        {    // The statement - note the anonymous object
            "mainsnak": {
                "snaktype": string,  // values: "value", "somevalue", "novalue"
                "property": string, // always same as enclosing property value, e.g. "P31"
                "datavalue":    object, // type and layout depending on the datatype of the property
                "datatype": string,  // See discussion of data types and values below
            },
            "type": string, // Always "statement", see below
            "qualifiers": array of object // optional - see below
            "qualifiers-order": array of string // optional
            "id": string // unique (and long...) id for the claim
            "rank": string // "normal", "preferred", "deprecated",
            "references": array of object // optional
        },
        {
        }
    ], // Array end
    "P349": [...] // Another claim
}

````

A qualifier *qualifies* a statement, it could be e.g. qualifying that the statement
is only correct for a given time period.

Qualifiers look somewhat like claims:

````json
    "qualifiers": {
        "P7141":[ // The type of the qualifier - here "measure number" (musical notation)
        {
            "snaktype":"value", // Not sure, but I think only "value" is applicable here
            "property":"P7141", // Always same as enclosing property
            "hash":"6e8532d38c30dde9077dcc09ea752af75420e3a1", 
            "datavalue":{
                "value":"92\u2013107","type":"string"
            },
            "datatype":"string"
        }]
    },

    "qualifiers-order":[
        "P1741"
    ]

````

References:

````json
    "references":[{
        "hash":"fa278ebfc458360e5aed63d5058cca83c46134f1",
        "snaks":{
            "P143":[{
                "snaktype":"value",
                "property":"P143",
                "datavalue":{
                    "value":{
                        "entity-type":"item",
                        "numeric-id":328,
                        "id":"Q328"
                    },
                    "type":"wikibase-entityid"
                },
                "datatype":"wikibase-item"
            }]
        }

````

Sitelinks:

````json
    "sitelinks":{
        "commonswiki":{
            "site":"commonswiki",
            "title":"Club Deportivo Mirand\u00e9s",
            "badges":[]
        },
        "enwiki":{
            "site":"enwiki",
            "title":"CD Mirand\u00e9s",
            "badges":[]
        }
    }    
````

Each individual claim (the anonymous object)
is a *statement* about the item - or rather about the property in the respect that it
*states* something about the property. E.g. When the property is *"P31" instance of* the statement states that
the item is an *instance of* whatever the
*statement*'s datavalue contains, e.g. a City or a Movie.

The property describes what type of a claim
we are talking about. Each claim furthermore has a ``datavalue`` which qualifies the claim.

Note that each claim is actually a array of individual statements for that property. This is
useful e.g. for songs on an album where the property ``P658 (tracklist)`` is a
array of claims, one for each track. Note that the order is *probably* not important. Probably
a qualifier (in this case ``P1545 (series ordinal)``) is used for stating the order.

### snaktype

**"novalue"**:

When a claim deliberably does not have a value, e.g. (to use Wikidata's example) Angela Merkel has _no_ children.

**Note!** Even though there is no _"datavalue"_ property the _"datatype"_ property is still present.

**"somevalue"**:

Used when a claim probably *should* have a value, but we don't know what it is.

Even though there is no _"datavalue"_ property the _"datatype"_ property is still present.

**"value"**:

In this case the claim does have an actual value, available in the _"datavalue"_ property.

### datavalue

The _datavalue_ property is always an object with the following layout:

```` json
"datavalue": {
    "value": object or string
    "type": string
}
````

The *type* property is one of the following values:

- globecoordinate
- monolingualtext
- quantity
- string
- time
- wikibase-entityid

The *type* is redundant, see discussion below:

## datatype

The datatype does, naturally, tell something about the format of the data represented - unfortunately it is not very useful when sequentially parsing
datavalue, since the *datatype* property comes *after* the *datavalue* property...

Each datatype is also represented in Wikidata as an item with the relation
[instance of (P31)](https://www.wikidata.org/wiki/Property:P31) ->
[Wikibase datatype (Q19798645)](https://www.wikidata.org/wiki/Q19798645).

All datatypes are listed on [wikidata](https://www.wikidata.org/wiki/Special:ListDatatypes) along with other useful information.

There is a consistent relation between the *type* of the *datavalue* and the
*datatype* (in the *mainsnak*) as seen by this table:

| *datatype* (of snak) | *type* of *datavalue* | Friendly name
|---------------- |---------|---------|
| [commonsMedia](commonsMedia.md) | string | Commons media file
| [external-id](external-id.md) | string | External identifier
| [geo-shape](geo-shape.md) | string | Geographic shape
| [globe-coordinate](globe-coordinate.md) | globecoordinate | Geographic coordinates |
| [math](math.md) | string | Mathematical expression |
| [monolingualtext](monolingualtext.md) | monolingualtext | Monolingual text |
|  [musical-notation](musical-notation.md) | string | Musical notation |
| [quantity](quantity.md) | quantity | Quantity ||
| [string](string.md)| string | String |
| [tabular-data](tabular-data.md) | string | Tabular data |
| [time](time.md) | time | Point in time |
| [url](url.md) | string | URL |
| [wikibase-form](wikibase-form.md) | wikibase-entityid | Form |
| [wikibase-item](wikibase-item.md) |  wikibase-entityid | Item |
| [wikibase-lexeme](wikibase-lexeme.md)|wikibase-entityid | Lexeme
| [wikibase-property](wikibase-property.md) |  wikibase-entityid | Property |
|[wikibase-sense](wikibase-sense.md)|wikibase-entityid|Sense

**Note about `wikibase-entityid`:** According to [Wikidata Documentation](https://doc.wikimedia.org/Wikibase/master/php/md_docs_topics_json.html) not all entries have `numeric-id`. The meaning of this is not clear
and does sound rather strange to me, but what I think is meant
by this is that there are different `wikibase-entityid` types and
not all of those have a `numeric-id`.

**Note:** The consistent relation between *type* and *datatype* basically makes the *type* property redundant.
