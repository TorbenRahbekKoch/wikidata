# Wikidata Full JSON dump documentation

## Point in Time

|datatype|type|Properties
|---|---|----|
|time|time|<https://www.wikidata.org/wiki/Special:ListProperties/time>

Layout:

````json
    "datavalue":{
        "value":{
            "time":"+2015-07-00T00:00:00Z",
            "timezone":0,
            "before":0,
            "after":0,
            "precision":10,
            "calendarmodel":"http:\/\/www.wikidata.org\/entity\/Q1985727"
        },
        "type":"time"
    },
    "datatype":"time"
````

Please see the "time" section in [the official docs](https://doc.wikimedia.org/Wikibase/master/php/md_docs_topics_json.html) for more details.

One important thing to highlight is that: "**Hour, minute, and second are currently unused and should always be 00.**". This is also the case for `timezone`.

| Precision | Meaning | Examples of `time` |
|--|--|--|
| 0 | 1 Gigayear | +1000000000000000-00-00T00:00:00Z</br>+2000-00-00T00:00:00Z |
| 1 | 100 Megayears | -300000000-00-00T00:00:00Z
| 2 | 10 Megayears | -2050000000-00-00T00:00:00Z
| 3 | 1 Megayear | -145000000-00-00T00:00:00Z
| 4 | 100 Kiloyears | -326400000-00-00T00:00:00Z
| 5 | 10 Kiloyears | -107590000-00-00T00:00:00Z
| 6 | Millenium | -2000-00-00T00:00:00Z </br>+0400-00-00T00:00:00Z |
| 7 | Century | +0500-01-01T00:00:00Z |
| 8 | 10 years | +1970-00-00T00:00:00Z |
| 9 | 1 years | +1993-00-00T00:00:00Z |
| 10 | Months | +2003-04-00T00:00:00Z |
| 11 | Days | +1979-03-20T00:00:00Z |
| 12 | Hours | Unused
| 13 | Hours | Unused
| 14 | Hours | Unused

Notice that the relevant parts of `time` is `00`
depending on `precision`, which may be a bit surprising when parsing the value as a date.

The `before` and `after` can be used to give a date range
but this is not widely used.
