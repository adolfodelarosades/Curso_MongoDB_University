# Final Exam

## Final: Question 1

**Problem:**

Consider the following aggregation pipelines:

* Pipeline 1

```sh
db.coll.aggregate([
  {"$match": {"field_a": {"$gt": 1983}}},
  {"$project": { "field_a": "$field_a.1", "field_b": 1, "field_c": 1  }},
  {"$replaceRoot":{"newRoot": {"_id": "$field_c", "field_b": "$field_b"}}},
  {"$out": "coll2"},
  {"$match": {"_id.field_f": {"$gt": 1}}},
  {"$replaceRoot":{"newRoot": {"_id": "$field_b", "field_c": "$_id"}}}
])
```

* Pipeline 2

```sh
db.coll.aggregate([
  {"$match": {"field_a": {"$gt": 111}}},
  {"$geoNear": {
    "near": { "type": "Point", "coordinates": [ -73.99279 , 40.719296 ] },
    "distanceField": "distance"}},
  {"$project": { "distance": "$distance", "name": 1, "_id": 0  }}
])
```

* Pipeline 3

```sh
db.coll.aggregate([
  {
    "$facet": {
      "averageCount": [
        {"$unwind": "$array_field"},
        {"$group": {"_id": "$array_field", "count": {"$sum": 1}}}
      ],
      "categorized": [{"$sortByCount": "$arrayField"}]
    },
  },
  {
    "$facet": {
      "new_shape": [{"$project": {"range": "$categorized._id"}}],
      "stats": [{"$match": {"range": 1}}, {"$indexStats": {}}]
    }
  }
])
```

Which of the following statements are correct?

Check all answers that apply:


* **Pipeline 3** executes correctly

* **Pipeline 2** fails because we cannot project `distance` field

* **Pipeline 1** fails since `$out` is required to be the last stage of the pipeline :+1:

* **Pipeline 2** is incorrect because `$geoNear` needs to be the first stage of our pipeline :+1:

* **Pipeline 1** is incorrect because you can only have one `$replaceRoot` stage in your pipeline

* **Pipeline 3** fails because `$indexStats` must be the first stage in a pipeline and may not be used within a `$facet` :+1:

* **Pipeline 3** fails since you can only have one $facet stage per pipeline

### See detailed answer

The correct statements are the following:

* **Pipeline 3** fails because `$indexStats` must be the first stage in a pipeline and may not be used within a `$facet`

`$indexStats` must be the first stage in an aggregation pipeline and cannot be used within a `$facet` stage.

* **Pipeline 1** fails since $out is required to be the last stage of the pipeline

`$out` is required to be the last stage of the pipeline.

* **Pipeline 2** is incorrect because $geoNear needs to be the first stage of our pipeline

`$geoNear` is required to be the first stage of a pipeline.

All other statements are incorrect.

## Final: Question 2

**Problem:**

Consider the following collection:

```sh
db.collection.find()
{
  "a": [1, 34, 13]
}
```

The following pipelines are executed on top of this collection, using a mixed set of different expression accross the different stages:

* Pipeline 1

```sh
db.collection.aggregate([
  {"$match": { "a" : {"$sum": 1}  }},
  {"$project": { "_id" : {"$addToSet": "$a"}  }},
  {"$group": { "_id" : "", "max_a": {"$max": "$_id"}  }}
])
```

* Pipeline 2

```sh
db.collection.aggregate([
    {"$project": { "a_divided" : {"$divide": ["$a", 1]}  }}
])
```

* Pipeline 3

```sh
db.collection.aggregate([
    {"$project": {"a": {"$max": "$a"}}},
    {"$group": {"_id": "$$ROOT._id", "all_as": {"$sum": "$a"}}}
])
```

Given these pipelines, which of the following statements are correct?

Check all answers that apply:

* **Pipeline 3** is correct and will execute with no error :+1:

* **Pipeline 1** will fail because `$max` can not operator on `_id` field

* **Pipeline 2** fails because the `$divide` operator only supports numeric types :+1:

* **Pipeline 2** is incorrect since `$divide` cannot operate over field expressions

* **Pipeline 1** is incorrect because you cannot use an accumulator expression in a `$match` stage. :+1:

### See detailed answer

The correct answers are the following:

 * **Pipeline 1** is incorrect because you cannot use an accumulator expression on `$match` stage.
 
We cannot use accumulator expressions within `$match`. Only query expressions are allowed within `$match`

* **Pipeline 3** is correct and will execute with no error

This is correct. Although we may argue that `$ROOT` variable is totally unnecessary, since `_id` field will be projected by default from the first `$project` stage of this pipeline, there are no observable errors with the use of this expression variable

* **Pipeline 2** fails because `$divide` operator only supports numeric types

This is true, `$divide` operator will only supports expressions that represent numeric value types.

All the other statements are not true.

## Final: Question 3

**Problem:**

Consider the following collection documents:

```sh
db.people.find()
{ "_id" : 0, "name" : "Bernice Pope", "age" : 69, "date" : ISODate("2017-10-04T18:35:44.011Z") }
{ "_id" : 1, "name" : "Eric Malone", "age" : 57, "date" : ISODate("2017-10-04T18:35:44.014Z") }
{ "_id" : 2, "name" : "Blanche Miller", "age" : 35, "date" : ISODate("2017-10-04T18:35:44.015Z") }
{ "_id" : 3, "name" : "Sue Perez", "age" : 64, "date" : ISODate("2017-10-04T18:35:44.016Z") }
{ "_id" : 4, "name" : "Ryan White", "age" : 39, "date" : ISODate("2017-10-04T18:35:44.019Z") }
{ "_id" : 5, "name" : "Grace Payne", "age" : 56, "date" : ISODate("2017-10-04T18:35:44.020Z") }
{ "_id" : 6, "name" : "Jessie Yates", "age" : 53, "date" : ISODate("2017-10-04T18:35:44.020Z") }
{ "_id" : 7, "name" : "Herbert Mason", "age" : 37, "date" : ISODate("2017-10-04T18:35:44.020Z") }
{ "_id" : 8, "name" : "Jesse Jordan", "age" : 47, "date" : ISODate("2017-10-04T18:35:44.020Z") }
{ "_id" : 9, "name" : "Hulda Fuller", "age" : 25, "date" : ISODate("2017-10-04T18:35:44.020Z") }
```

And the aggregation pipeline execution result:

```sh
db.people.aggregate(pipeline)
{ "_id" : 8, "names" : [ "Sue Perez" ], "word" : "P" }
{ "_id" : 9, "names" : [ "Ryan White" ], "word" : "W" }
{ "_id" : 10, "names" : [ "Eric Malone", "Grace Payne" ], "word" : "MP" }
{ "_id" : 11, "names" : [ "Bernice Pope", "Jessie Yates", "Jesse Jordan", "Hulda Fuller" ], "word" : "PYJF" }
{ "_id" : 12, "names" : [ "Herbert Mason" ], "word" : "M" }
{ "_id" : 13, "names" : [ "Blanche Miller" ], "word" : "M" }
```

Which of the following pipelines generates the output result?

Choose the best answer:

1)
```sh
var pipeline = [{
    "$sort": { "date": 1 }
  },
  {
    "$group": {
      "_id": { "$size": { "$split": ["$name", " "]} },
      "names": {"$push": "$name"}
    }
  },
  {
    "$project": {
      "word": {
        "$zip": {
          "inputs": ["$names"],
          "useLongestLength": false,
        }
      },
      "names": 1
    }
  }]
```

2) :+1:
```sh
var pipeline = [{
    "$project": {
      "surname_capital": { "$substr": [{"$arrayElemAt": [ {"$split": [ "$name", " " ] }, 1]}, 0, 1 ] },
      "name_size": {  "$add" : [{"$strLenCP": "$name"}, -1]},
      "name": 1
    }
  },
  {
    "$group": {
      "_id": "$name_size",
      "word": { "$push": "$surname_capital" },
      "names": {"$push": "$name"}
    }
  },
  {
    "$project": {
      "word": {
        "$reduce": {
          "input": "$word",
          "initialValue": "",
          "in": { "$concat": ["$$value", "$$this"] }
        }
      },
      "names": 1
    }
  },
  {
    "$sort": { "_id": 1}
  }
]
```

3)
```sh
var pipeline = [{
    "$project": {
      "surname": { "$arrayElemAt": [ {"$split": [ "$name", " " ] }, 1]},
      "name_size": {  "$add" : [{"$strLenCP": "$name"}, -1]},
      "name":1
    }
  },
  {
    "$group": {
      "_id": "$name_size",
      "word": { "$addToSet": {"$substr": [{"$toUpper":"$name"}, 3, 2]} },
      "names": {"$push": "$surname"}
    }
  },
  {
    "$sort": {"_id": -1}
  }
]
```

### See detailed answer

The correct pipeline is the following:

```sh
var pipeline = [{
    "$project": {
      "surname_capital": { "$substr": [{"$arrayElemAt": [ {"$split": [ "$name", " " ] }, 1]}, 0, 1 ] },
      "name_size": {  "$add" : [{"$strLenCP": "$name"}, -1]},
      "name": 1
    }
  },
  {
    "$group": {
      "_id": "$name_size",
      "word": { "$push": "$surname_capital" },
      "names": {"$push": "$name"}
    }
  },
  {
    "$project": {
      "word": {
        "$reduce": {
          "input": "$word",
          "initialValue": "",
          "in": { "$concat": ["$$value", "$$this"] }
        }
      },
      "names": 1
    }
  },
  {
    "$sort": { "_id": 1}
  }
]
```

For this lab we picked the first letter of each person surname, surname_capital, by splitting the name into an array

```sh
{"$split": [ "$name", " " ] }
```

And by gathering the first letter of the surname using `$substr` and `$arrayElemAt`:

```sh
{ "$substr": [{"$arrayElemAt": [ {"$split": [ "$name", " " ] }, 1]}, 0, 1 ] }
```

We've also captured the number of all alphanumeric characters of the `name` field, except " ":

```sh
"name_size": {  "$add" : [{"$strLenCP": "$name"}, -1]}
```

After grouping all first capital letters into `word` array, and all `name` into `names` values by the `name_size`:

```sh
{
  "$group": {
    "_id": "$name_size",
    "word": { "$push": "$surname_capital" },
    "names": {"$push": "$name"}
  }
},
```

We then `$reduced` the resulting `word` array into a single string:

```sh
{
  "$project": {
    "word": {
      "$reduce": {
        "input": "$word",
        "initialValue": "",
        "in": { "$concat": ["$$value", "$$this"] }
      }
    },
    "names": 1
  }
}

And finally sort the result:
```

```sh
{
  "$sort": { "_id": 1}
}
```


