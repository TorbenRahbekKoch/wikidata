# Wikidata Full JSON dump documentation

## Monolingual text

*datatype:monolingualtext*
*type:monolingualtext*

The monolingual text datatype is very useful as a value for e.g. the [title property P1476](https://www.wikidata.org/wiki/Property:P1476). 

Layout of the monolingual text datavalue is as follows:

````
    ...
    "datavalue" : {
      "value" : {
        "text": "Some text",
        "language" : "it"
      },
      "type" : "monolingualtext"
   }
   ...
````

The value of *language* is the [Wikimedia language code (P424)](https://www.wikidata.org/wiki/Property:P424).

It is worth noting that even though it is called *monolingualtext* there can easily be several entries in the P1476 claim. This would be the
case if e.g. the item has multiple titles, which it can have even for one language, see e.g. [Q12018](https://www.wikidata.org/wiki/Q12018), which
have more than one title for the italian language.
