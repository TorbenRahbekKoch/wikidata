# Wikidata Full JSON dump documentation

## Item

|datatype|type|Wikidata|Properties
|---|---|----|----|
|wikibase-item|wikibase-entityid|<https://www.wikidata.org/wiki/Q29934218>|<https://www.wikidata.org/wiki/Special:ListProperties/wikibase-item>

Layout:

```` json

    "datavalue": {
        "value": {
            "entity-type": "item", // Always "item" => redundant
            "numeric-id": number,
            "id": string" // e.g. Q484
        },
        "type": "wikibase-entityid"
    },
    "datatype":"wikibase-item",
````

The content corresponding to  ``id`` can be looked up on the wikidata website by
substituting the id (*Q484*) in the url: <https://www.wikidata.org/wiki/Q484>
