# Wikidata Full JSON dump documentation

## Wikibase entityid

**TODO: Create one file for each of the data types:**

*datatype: * One of:

- wikibase-item
- wikibase-property
- wikibase-form
- wikibase-lexeme
- wikibase-sense

*type: wikibase-entityid* 

Examples: 

```` json

    "datavalue": {
        "value": {
            "entity-type": "item", 
            "numeric-id": number,  // Only for some types, see table below
            "id": string" // e.g. Q484
        },
        "type": "wikibase-entityid"
    },
    "datatype":"wikibase-item",
````

````json

    "datavalue":{
        "value":{
            "entity-type":"lexeme",
            "numeric-id":484,
            "id":"L484"
        },
        "type":"wikibase-entityid"
    },
    "datatype":"wikibase-lexeme",
````

The `entity-type` can have several values and the properties vary 
with the type as follows:

| entity-type | numeric-id  | id | id-prefix |
|---|---|---|---| 
| item | X | X | 
| property | X | X |
| form  |  | X |
| lexeme | X | X | L
| sense |  | X |
