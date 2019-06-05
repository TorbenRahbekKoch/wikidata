# Wikidata Full JSON dump documentation

This documentation is not supposed to be the authoritative guide but being based 
partly on [Wikibase/DataModel/JSON](https://www.mediawiki.org/wiki/Wikibase/DataModel/JSON) 
(which is partly incorrect)
and on countless hours sifting through the data will lend it some weight.

# Overview


The full dump file is a JSON file - located on the [wikimedia website](https://dumps.wikimedia.org/wikidatawiki/entities/) - in various compression formats starting
with the name *latest-all*.

The file is a JSON array of all the items and properties
which are conveniently placed with one element on a line making it considerably 
easier to read the file one line/item/property at a time, which also is - due to the
amount of data - very necessary.

Each line is a JSON object making up either an item or a property as seen by the
``type`` property. From there on the formats differ, but only slightly. The basic 
format of the file is like this:

````
[
{ ... } // item or property },
{ ... } // item or property },
...
]
````

Since the document is valid JSON there is a comma at the end of each line. This 
may be a challenge for some JSON readers.

## Properties


````
{
    "type":         // string: values: "property"
    "datatype":     // string: values: see below
    "id":           // string: values: integer starting with P - e.g. "P31"
    "labels":       // object
    "descriptions": // object
    "aliases":      // object
    "claims":       // object
    "sitelinks":    // object
}
````


## Items

An item is an actual object, thing or concept, e.g. like **Back to The Future**, which
is a movie or **London**, which is a city.

The general structure for items looks like this. The order of properties is important:

````
{
    "type":    // string: values: "item"
    "id":      // string: values: integer starting with either Q   - e.g. "Q91540"
    "labels":  // object
    "descriptions": // object
    "aliases":      // object
    "claims":       // object
    "sitelinks":    // object
}
````

According to [Wikibase/DataModel/JSON](https://www.mediawiki.org/wiki/Wikibase/DataModel/JSON)
there should also be ``lastrevid`` and ``modified`` properties for an item (and they would be 
immensely useful), but I have not seen them in any file.

## About languages

Language identifiers are used all over the place and can be looked up on 
[Help:Wikimedia language codes/lists/all](https://www.wikidata.org/wiki/Help:Wikimedia_language_codes/lists/all),
but more importantly each of them is in itself a Wikidata item with the property P424 
[Wikimedia language code (P424)](https://www.wikidata.org/wiki/Property:P424).

## ``labels``

The ``labels`` section is an object with a property for each language for which a label exists:

````
"labels": {
    "en-gb": {
        "language": "en-gb",
        "value": "Northern Ireland"
    },...
````

As it can be seen the ``language`` property is redundant. 

A label is what the "thing" is mainly called in different languages.

Since JSON only allows one instance of each property (?) only one label can exist for each language.

## ``descriptions``

The ``descriptions`` section is an object with a property for each language for which a description exists:

````
"descriptions": {
    "en-gb": {
        "language": "en-gb",
        "value": "region in north-west Europe, part of the United Kingdom"
    },...
````

As it can be seen the ``language`` property is redundant. 

A description is a more
thorough explanation than the label. There is not necessarily both a label and a 
description in the same language.

Since JSON only allows one instance of each property (?) only one description can exist for each language.

## ``aliases``

The ``aliases`` section is an object with a property for each language for which aliases exist. In contrast to 
``labels`` and ``descriptions`` there can be more than one ``alias`` for each language, for which an array
is needed:

````
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

As it can be seen the ``language`` property is redundant.

An alias can be seen as alternative, secondary labels.

## claims

Claims are used to tell _what_ an item actually is. The ``claims`` section is an object with a
property for each claim. Each claim is an array of anonymous objects which make up actual 
individual claims. 

Claims exist for both properties and items.

Claims are rather complex and difficult to describe, but here goes, starting with the general
layout of the claims object:

````
"claims": {
    "P31": [ // The type of claim - here "instance of" - note the array start. 
        {
            "mainsnak": {
                "snaktype": string,  // values: "value", "somevalue", "novalue"
                "property": str ing, // always same as enclosing property value, e.g. "P31"
                "datavalue": object, // layout depending on the datatype of the property
                "datatype": string,  // See discussion of data types
            },
            "type": string, // Always "statement", see below
            "qualifiers": ?? // optional
            "qualifiers-order": ?? // optional
            "id": string // unique id for the claim
            "rank": string // "normal", "preferred", "deprecated",
            "references": ?? // optional
        },
        {
        }
    ],
    "P349": [...]
}
````

Each individual claim is a _statement_ about the item. The property describes what type of a claim
we are talking about. Each claim furthermore has a ``datavalue`` which qualifies the claim.

Note that each claim is actually a list of individual claims for that property. This is 
useful e.g. for songs on an album where the property ``P658 (tracklist)`` is a
list of claims, one for each track. Note that the order is *probably* not important. Usually
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

"datavalue": {
    "value": object or string
    "type": string
}

The _type_ property is one of the following values:

- globecoordinate
- monolingualtext
- quantity
- string
- time
- wikibase-entityid

The _type_ is redundant, see discussion below:

## datatype

The datatype does, naturally, tell something about the format of the data represented. 
Each datatype is also represented in Wikidata as an item with the relation 
[instance of (P31)](https://www.wikidata.org/wiki/Property:P31) ->
[Wikibase datatype (Q19798645)](https://www.wikidata.org/wiki/Q19798645).

All datatypes are listed on [wikidata](https://www.wikidata.org/wiki/Special:ListDatatypes) along with 
other useful information. 

There are several values applicable for the ``datatype`` property:

| datatype | Friendly name | Wiki page |
|---|
| commonsMedia | Commons media | [CommonsMedia (Q29934260)](https://www.wikidata.org/wiki/Q29934260)|
| external-id | External identifier | [external identifier (Q21754218)](https://www.wikidata.org/wiki/Q21754218) |
| wikibase-form | Form | [form (Q54285143)](https://www.wikidata.org/wiki/Q54285143)
| geo-shape | Geographic shape |  [GeoShape (Q42742911)](https://www.wikidata.org/wiki/Q42742911) |
| globe-coordinate | Globe Coordinate ||
| lexeme | Lexeme |
| math | Mathematical expression | [Math (Q42742777)](https://www.wikidata.org/wiki/Q42742777) |
| monolingualtext | Monolingual text |
| musical-notation | String |
| quantity | Quantity ||
| sense | Sense |
| string | String |
| tabular-data | Tabular data |
| time | Time |
| url | URL |
| wikibase-item | Item | 
| wikibase-property | Property |

There is a consistent relation between the _type_ of the _datavalue_ and the
_datatype_ (in the _mainsnak_) as seen by this table:

| datatype | value type |
|---|---|
|commonsMedia|string
|external-id|string
|geo-shape|string
|globe-coordinate|globecoordinate
|math|string|
|monolingualtext|monolingualtext
|musical-notation|string|
|quantity|quantity
|string|string
|tabular-data|string
|time|time
|url|string
|wikibase-form|wikibase-entityid
|wikibase-item|wikibase-entityid
|wikibase-property|wikibase-entityid
|wikibase-lexeme|wikibase-entityid
|wikibase-sense|wikibase-entityid

This basically makes the _type_ property redundant. **NOTE:** Even though the value type is stated as e.g. *wikibase-entityid* the
actual contents on *value* is determined by the *datatype* and can therefore vary in both members and types. See (upcoming) description of
the content for individual value types below.

[Monolingualtext](monolingualtext.md)