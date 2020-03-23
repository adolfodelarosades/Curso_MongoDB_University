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

## Final: Question 4

**Problem:**

`$facet` is an aggregation stage that allows for sub-pipelines to be executed.

```sh
var pipeline = [
  {
    $match: { a: { $type: "int" } }
  },
  {
    $project: {
      _id: 0,
      a_times_b: { $multiply: ["$a", "$b"] }
    }
  },
  {
    $facet: {
      facet_1: [{ $sortByCount: "a_times_b" }],
      facet_2: [{ $project: { abs_facet1: { $abs: "$facet_1._id" } } }],
      facet_3: [
        {
          $facet: {
            facet_3_1: [{ $bucketAuto: { groupBy: "$_id", buckets: 2 } }]
          }
        }
      ]
    }
  }
]
```

In the above pipeline, which uses `$facet`, there are some incorrect stages or/and expressions being used.

Which of the following statements point out errors in the pipeline?

Check all answers that apply:

* can not nest a `$facet` stage as a sub-pipeline. :+1:

* `facet_2` uses the output of a parallel sub-pipeline, `facet_1`, to compute an expression :+1:

* `$sortByCount` cannot be used within `$facet` stage.

* a `$type` expression does not take a string as its value; only the BSON numeric values can be specified to identify the types.

* a `$multiply` expression takes a document as input, not an array.

### See detailed answer

The following options are not true:

* a `$multiply` expression takes a document as input, not an array.

This is not true, a `$multiply` expression does take as input an array of expressions.

* a `$type` expression does not take a string as its value; only the BSON numeric values can be specified to identify the types.

We can use either the numeric BSON representation, as well as a string alias to evaluate a field type.

* `$sortByCount` cannot be used within `$facet` stage.

`$facet` does accept `$sortByCount` as a sub-pipeline stage.

The correct answers, that reflect problems with the pipeline, are the following:

* can not nest a `$facet` stage as a sub-pipeline.

This is correct. `$facet` does not accept all sub-pipelines that include other `$facet` stages

* `facet_2` uses the output of a parallel sub-pipeline, `facet_1`, to compute an expression

Each sub-pipeline are completely independent of one another. The output of one sub-pipeline cannot be used as the input for different sub-pipelines.

## Final: Question 5

**Problem:**

Consider a company producing solar panels and looking for the next markets they want to target in the USA. We have a collection with all the major **cities** (more than 100,000 inhabitants) from all over the World with recorded number of sunny days for some of the last years.

A sample document looks like the following:

```sh
db.cities.findOne()
{
"_id": 10,
"city": "San Diego",
"region": "CA",
"country": "USA",
"sunnydays": [220, 232, 205, 211, 242, 270]
}
```

The collection also has these indexes:

```sh
db.cities.getIndexes()
[
{
  "v": 2,
  "key": {
    "_id": 1
  },
  "name": "_id_",
  "ns": "test.cities"
},
{
  "v": 2,
  "key": {
    "city": 1
  },
  "name": "city_1",
  "ns": "test.cities"
},
{
  "v": 2,
  "key": {
    "country": 1
  },
  "name": "country_1",
  "ns": "test.cities"
}
]
```

We would like to find the cities in the USA where the minimum number of sunny days is 200 and the average number of sunny days is at least 220. Lastly, we'd like to have the results sorted by the city's name. The matching documents may or may not have a different shape than the initial one.

We have the following query:

```sh
var pipeline = [
    {"$addFields": { "min": {"$min": "$sunnydays"}}},
    {"$addFields": { "mean": {"$avg": "$sunnydays" }}},
    {"$sort": {"city": 1}},
    {"$match": { "country": "USA", "min": {"$gte": 200}, "mean": {"$gte": 220}}}
]
db.cities.aggregate(pipeline)
```

However, this pipeline execution can be optimized!

Which of the following choices is still going to produce the expected results and likely improve the most the execution of this aggregation pipeline?

Choose the best answer:

1) :+1:
```sh
var pipeline = [
    {"$match": { "country": "USA"}},
    {"$addFields": { "mean": {"$avg": "$sunnydays"}}},
    {"$match": { "mean": {"$gte": 220}, "sunnydays": {"$not": {"$lt": 200 }}}},
    {"$sort": {"city": 1}}
]
```
2)
```sh
var pipeline = [
    {"$sort": {"city": 1}},
    {"$match": { "country": "USA"}},
    {"$addFields": { "min": {"$min": "$sunnydays"}}},
    {"$match": { "min": {"$gte": 200}, "mean": {"$gte": 220}}},
    {"$addFields": { "mean": {"$avg": "$sunnydays" }}}
]
```

3)
```sh
var pipeline = [
    {"$sort": {"city": 1}},
    {"$addFields": { "min": {"$min": "$sunnydays"}}},
    {"$addFields": { "mean": {"$avg": "$sunnydays" }}},
    {"$match": { "country": "USA", "min": {"$gte": 200}, "mean": {"$gte": 220}}}
]
```

4)
```sh
var pipeline = [
    {"$sort": {"city": 1}},
    {"$addFields": { "min": {"$min": "$sunnydays"}}},
    {"$match": { "country": "USA", "min": {"$gte": 200}}}
]
```

5)
```sh
var pipeline = [
    {"$match": { "country": "USA"}},
    {"$sort": {"city": 1}},
    {"$addFields": { "min": {"$min": "$sunnydays"}}},
    {"$match": { "min": {"$gte": 200}, "mean": {"$gte": 220}}},
    {"$addFields": { "mean": {"$avg": "$sunnydays" }}}
]
```

### See detailed answer

The correct answer is the following:

```sh
var pipeline = [
    {"$match": { "country": "USA"}},
    {"$addFields": { "mean": {"$avg": "$sunnydays"}}},
    {"$match": { "mean": {"$gte": 220}, "sunnydays": {"$not": {"$lt": 200 }}}},
    {"$sort": {"city": 1}}
]
```

In this case, we try to remove as much data as possible upfront, all cities not matching the right country, using the available index.

We then calculate the mean number of sunny days.

The `$match` stage then filters out documents where the mean isn't greater than or equal to 220, and there are no entries in the **sunnydays** vector less than 200.

We are left with a sort in memory, however the number should be small enough to not take much resources. There are 285 cities with 100,000 habitants in the USA, and some are likely not to match the number of sunny days criteria.

Another answer provides the desired results, but will not improve the performance as much:

```sh
var pipeline = [
  {"$sort": {"city": 1}},
  {"$addFields": { "min": {"$min": "$sunnydays"}}},
  {"$addFields": { "mean": {"$avg": "$sunnydays" }}},
  {"$match": { "country": "USA", "min": {"$gte": 200}, "mean": {"$gte": 220}}}
]
```

The above approach uses the index to sort, however it performs an unnecessary calculation to get the minimum value within **sunnydays**. Because the `$match` stage did not come prior to these `$addFields` stages, all source documents will pass through them, a wasteful computation.

The pipeline:

```sh
var pipeline = [
    {"$sort": {"city": 1}},
    {"$addFields": { "min": {"$min": "$sunnydays"}}},
    {"$match": { "country": "USA", "min": {"$gte": 200}}}
]
```

does not satisfy the query requirements.

The last 2 queries are doing a `$match` on `mean` before it is calculated, making them also invalid.

