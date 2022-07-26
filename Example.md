# Example of P527 - Has Parts

Let's take a look at how the entry about [Duran Duran](https://www.wikidata.org/wiki/Q58381),
denotes band members using the property
[Has parts(P527)](https://www.wikidata.org/wiki/Property:P527).

This is how it looks on the web site:

![Duran Duran](/images/DuranDuran.png)

It is an interesting example
because of the multiple start and end times.

Let's look at the JSON for John Taylor (see below):

There's a wikibase-entity pointing to Q540283, which is John Taylor - instance of (P31) Human.

Then there's the qualifiers - and quite a few of them.
First an array of P580 - Start time, John Taylor seemingly had difficulty
deciding whether he wanted to stay in the band - so he joined the band
twice.

Luckily he left it only once: End time (P582).

He was versatile, though, and played two instruments (P1303)

```` json
{
    "mainsnak": {
     "snaktype": "value",
     "property": "P527",
     "datavalue": {
      "value": {
       "entity-type": "item",
       "numeric-id": 540283,
       "id": "Q540283"
      },
      "type": "wikibase-entityid"
     },
     "datatype": "wikibase-item"
    },
    "type": "statement",
    "qualifiers": {
     "P580": [{
       "snaktype": "value",
       "property": "P580",
       "hash": "884835ec6930b796613caa4c2056ca2801f07b5a",
       "datavalue": {
        "value": {
         "time": "+1978-00-00T00:00:00Z",
         "timezone": 0,
         "before": 0,
         "after": 0,
         "precision": 9,
         "calendarmodel": "http:\/\/www.wikidata.org\/entity\/Q1985727"
        },
        "type": "time"
       },
       "datatype": "time"
      }, {
       "snaktype": "value",
       "property": "P580",
       "hash": "8752b82cdffad4bcee274913d130a05ca27d08b0",
       "datavalue": {
        "value": {
         "time": "+2001-00-00T00:00:00Z",
         "timezone": 0,
         "before": 0,
         "after": 0,
         "precision": 9,
         "calendarmodel": "http:\/\/www.wikidata.org\/entity\/Q1985727"
        },
        "type": "time"
       },
       "datatype": "time"
      }
     ],
     "P582": [{
       "snaktype": "value",
       "property": "P582",
       "hash": "7fe90dfc5fe456b77c59aae38383949656ba20c5",
       "datavalue": {
        "value": {
         "time": "+1997-00-00T00:00:00Z",
         "timezone": 0,
         "before": 0,
         "after": 0,
         "precision": 9,
         "calendarmodel": "http:\/\/www.wikidata.org\/entity\/Q1985727"
        },
        "type": "time"
       },
       "datatype": "time"
      }
     ],
     "P1303": [{
       "snaktype": "value",
       "property": "P1303",
       "hash": "4baccfdc36c0c2d92f0ea72233de8323566053a1",
       "datavalue": {
        "value": {
         "entity-type": "item",
         "numeric-id": 46185,
         "id": "Q46185"
        },
        "type": "wikibase-entityid"
       },
       "datatype": "wikibase-item"
      }, {
       "snaktype": "value",
       "property": "P1303",
       "hash": "32ba5bbd0bb5778c1097444f80b17d3de3b3cdab",
       "datavalue": {
        "value": {
         "entity-type": "item",
         "numeric-id": 6607,
         "id": "Q6607"
        },
        "type": "wikibase-entityid"
       },
       "datatype": "wikibase-item"
      }
     ]
    },
    "qualifiers-order": ["P580", "P582", "P1303"],
    "id": "Q58381$1f06839e-4b98-0bb6-1db0-12f4e2bbcb23",
    "rank": "normal"
   }
````
