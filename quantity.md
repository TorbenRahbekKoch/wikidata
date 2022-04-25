# Wikidata Full JSON dump documentation

## Quantity

|datatype|type|Properties
|---|---|----|
|quantity|quantity|<https://www.wikidata.org/wiki/Special:ListProperties/quantity>

Layout:

````json
    ...
    "datavalue" : {
      "value" : {
        "amount": "+24234.34", // Signed amount
        "unit" : "1", // 1 = unit less, otherwise a link to a wiki
        "upperBound": string, // optional, together with lowerBound
        "lowerBound": string  // optional, together with upperBound
      },
      "type" : "quantity"
   },
   "datatype":"quantity"
   ...
````

**Note:** Even though `upperBound` and `lowerBound` are marked optional it is usually the case that both are optional *together*. That is, both are present or none of them are present. This is also stated in the [Wikidata Documentation](https://doc.wikimedia.org/Wikibase/master/php/md_docs_topics_json.html).

**About values:** The fields ``amount``, ``upperBound`` and ``lowerBound`` have string as type, but inside the string is
actually a signed (floating point) number. Even when positive
a ``+`` sign is present.

**About unit:** If the value is `1` it means that the amount is
*unit-less*. Otherwise it is a full link to a wiki-item, e.g. <http://www.wikidata.org/entity/Q4917> which then identifies the unit.
