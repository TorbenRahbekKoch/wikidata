# Wikidata Full JSON dump documentation

## Lexeme

|datatype|type|Wikidata|Properties
|---|---|----|----|
|wikibase-lexeme|wikibase-entityid|<https://www.wikidata.org/wiki/Q51885771>|<https://www.wikidata.org/wiki/Special:ListProperties/wikibase-lexeme>

Layout:

```` json

    "datavalue": {
        "value": {
            "entity-type": "lexeme", // Always "lexeme" => redundant
            "numeric-id": number,
            "id": string" // e.g. L19793
        },
        "type": "wikibase-entityid"
    },
    "datatype":"wikibase-item",
````

The content corresponding to  ``id`` can be looked up on the wikidata website by
substituting the id (*L19793*) in the url: <https://www.wikidata.org/wiki/Lexeme:L19793>
