# Wikidata Full JSON dump documentation

## Monolingual text

|datatype|type|Wikidata|Properties
|---|---|----|---|
|monolingualtext|monolingualtext|<https://www.wikidata.org/wiki/Q21044568>|<https://www.wikidata.org/wiki/Special:ListProperties/monolingualtext>

Layout:

````json
    "datavalue" : {
      "value" : {
        "text": "Some text",
        "language" : "it"
      },
      "type" : "monolingualtext"
   },
   "datatype":"monolingualtext"
   ...
````

The monolingual text datatype is very useful as a value for e.g. the [title property P1476](https://www.wikidata.org/wiki/Property:P1476). 

The value of `language` is the [Wikimedia language code (P424)](https://www.wikidata.org/wiki/Property:P424).

It is worth noting that even though it is called **mono***lingualtext* there can easily be several entries in e.g. the P1476 claim. This would be the
case if e.g. the item has multiple titles, which it can have even for one language, see e.g. [Q12018](https://www.wikidata.org/wiki/Q12018), which
have more than one title for the italian language.
