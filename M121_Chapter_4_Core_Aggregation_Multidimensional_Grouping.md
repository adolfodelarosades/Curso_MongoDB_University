
# Capítulo 4 Core Aggregation Multidimensional Grouping

Lecciones

* Tema: Facetas: Introducción
* Tema: Facetas: consulta de una sola faceta
* Examen
* Tema: La Etapa `$bucket`
* Tema: Facetas: Buckets manuales
* Examen
* Tema: La Etapa `$bucketAuto`
* Tema: Facetas: Auto Buckets
* Examen
* Tema: Facetas: Múltiples Facetas
* Examen
* Laboratorio - `$facets`
* Tema: La etapa `$sortByCount`

## 1. Tema: Facetas: Introducción

### Transcripción

## 2. Tema: Facetas: consulta de una sola faceta

### Notas de lectura

[$sortByCount stage](https://docs.mongodb.com/manual/reference/operator/aggregation/sortByCount/?jmp=university)

### Transcripción

## 3. Examen Facets: Single Facet Query

**Problem:**

Which of the following aggregation pipelines are single facet queries?

Check all answers that apply:

1)
```sh
[
  {"$match": { "$text": {"$search": "network"}}},
  {"$sortByCount": "$offices.city"}
]
```

2)
```sh
[
  {"$unwind": "$offices"},
  {"$project": { "_id": "$name", "hq": "$offices.city"}},
  {"$sortByCount": "$hq"},
  {"$sort": {"_id":-1}},
  {"$limit": 100}
]
```

3)
```sh
[
  {"$match": { "$text": {"$search": "network"}}},
  {"$unwind": "$offices"},
  {"$sort": {"_id":-1}}
]
```

## 4. Tema: La Etapa `$bucket`

### Notas de lectura

[$bucket](https://docs.mongodb.com/manual/reference/operator/aggregation/bucket/)

### Transcripción

## 5. Tema: Facetas: Buckets manuales

### Notas de lectura

[$bucket stage]()

At minute 4:45 we show the error message for trying to group a value that falls outside the boundaries.

```sh
$switch could not find matching branch for an input, and no default was specified
```

This message might be a bit confusing since we mention *switch* and *branches*. This is related with the internal `$bucket` stage implementation that uses a `$group` and `$switch` stages. We can see that in detail when running the **explain()** command.

```sh
db.coll.explain().aggregate([{ $bucket: {groupBy: "$x", boundaries: [0, 50, 100]}}])
{
  "stages" : [
    {
      "$cursor" : {
        "query" : {

        },
        "fields" : {
          "x" : 1,
          "_id" : 0
        },
        "queryPlanner" : {
          ...
        }
      }
    },
    {
      "$group" : {
        "_id" : {
          "$switch" : {
            "branches" : [
              {
                "case" : {
                  "$and" : [
                    {
                      "$gte" : [
                        "$x",
                        {
                          "$const" : 0
                        }
                      ]
                    },
                    {
                      "$lt" : [
                        "$x",
                        {
                          "$const" : 50
                        }
                      ]
                    }
                  ]
                },
                "then" : {
                  "$const" : 0
                }
              },
              {
                "case" : {
                  "$and" : [
                    {
                      "$gte" : [
                        "$x",
                        {
                          "$const" : 50
                        }
                      ]
                    },
                    {
                      "$lt" : [
                        "$x",
                        {
                          "$const" : 100
                        }
                      ]
                    }
                  ]
                },
                "then" : {
                  "$const" : 50
                }
              }
            ]
          }
        },
        "count" : {
          "$sum" : {
            "$const" : 1
          }
        }
      }
    },
    {
      "$sort" : {
        "sortKey" : {
          "_id" : 1
        }
      }
    }
  ],
  "ok" : 1
}
```

### Transcripción


## 6. Examen Facets: Manual Buckets

**Problem:**

Assuming that `field1` is composed of double values, ranging between 0 and Infinity, and `field2` is of type *string*, which of the following stages are correct?

Choose the best answer:

* `{'$bucket': { 'groupBy': '$field1', 'boundaries': [ "a", 3, 5.5 ]}}`

* `{'$bucket': { 'groupBy': '$field1', 'boundaries': [ 0.4, Infinity ]}}`

* `{'$bucket': { 'groupBy': '$field2', 'boundaries': [ "a", "asdas", "z" ], 'default': 'Others'}}`

## 7. Tema: La Etapa `$bucketAuto`

### Notas de lectura

[$bucketAuto](https://docs.mongodb.com/manual/reference/operator/aggregation/bucketAuto/)

### Transcripción

## 8. Tema: Facetas: Auto Buckets

### Notas de lectura

[$bucketAuto stage](https://docs.mongodb.com/manual/reference/operator/aggregation/bucketAuto/)

### Transcripción

## 9. Examen Facets: Auto Buckets

**Problem:**

Auto Bucketing will ...

Check all answers that apply:

* given a number of buckets, try to distribute documents evenly accross buckets.

* adhere bucket boundaries to a numerical series set by the **granularity** option.

* randomly distributed documents accross arbitrarily defined bucket boundaries.

* count only documents that contain the **groupBy** field defined in the documents.

## 10. Tema: Facetas: Múltiples Facetas

### Notas de lectura

[$facet stage](https://docs.mongodb.com/manual/reference/operator/aggregation/facet/?jmp=university)

### Transcripción

## 11. Examen Facets: Multiple Facets

**Problem:**

Which of the following statement(s) apply to the `$facet` stage?

Check all answers that apply:

* The `$facet` stage allows several sub-pipelines to be executed to produce multiple facets.

* The `$facet` stage allows the application to generate several different facets with one single database request.

* The output of the individual `$facet` sub-pipelines can be shared using the expression `$$FACET.$`.

* We can only use facets stages (`$sortByCount`, `$bucket` and `$bucketAuto`) as sub-pipelines of `$facet` stage.

## 12. Laboratorio - `$facets`

Lab - $facets

**Problem:**

How many movies are in both the top ten highest rated movies according to the `imdb.rating` and the `metacritic` fields? We should get these results with exactly one access to the database.

**Hint:** What is the *intersection*?

Choose the best answer:

* 1

* 5

* 2

* 3

## 13. Tema: La etapa `$sortByCount`

### Notas de lectura

[$sortByCount](https://docs.mongodb.com/manual/reference/operator/aggregation/sortByCount/)

### Transcripción
