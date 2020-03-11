# Chapter 3: Core Aggregation - Combining Information
`$group`, `$unwind`, `$lookup`, `$graphLookup`, `$facet`

### Lecciones

* Tema: La etapa `$group`
* Tema: Etapas del Acumulador con `$project`
* Laboratorio - `$group` y Acumuladores
* Tema: La etapa `$unwind`
* Laboratorio - `$unwind`
* Tema: La etapa $lookup
* Examen
* Laboratorio: Uso de `$lookup`
* Tema: `$graphLookup` Introducción
* Examen
* Tema: `$graphLookup`: Búsqueda simple
* Examen
* Tema: `$graphLookup`: Esquema inverso de búsqueda simple
* Tema: `$graphLookup`: `maxDepth` y `depthField`
* Examen
* Tema: `$graphLookup`: Cross Collection Lookup
* Tema: `$graphLookup`: Consideraciones generales
* Examen
* Laboratorio: `$graphLookup`


## 1. Tema: La etapa `$group`

### Notas de lectura

Página de documentación de [`$group`](https://docs.mongodb.com/manual/reference/operator/aggregation/group/).

### Transcripción

## 2. Tema: Etapas del Acumulador con `$project`

### Notas de lectura

Página de documentación de [`accumulator expressions`](https://docs.mongodb.com/manual/reference/operator/aggregation/#group-accumulator-operators).

### Transcripción

## 3. Laboratorio - `$group` y Acumuladores

Lab - $group and Accumulators

**Problem:**

In the last lab, we calculated a normalized rating that required us to know what the minimum and maximum values for `imdb.votes` were. These values were found using the `$group` stage!

For all films that won at least 1 Oscar, calculate the standard deviation, highest, lowest, and average `imdb.rating`. Use the **sample** standard deviation expression.

**HINT** - All movies in the collection that won an Oscar begin with a string resembling one of the following in their `awards` field

```sh
Won 13 Oscars
Won 1 Oscar
```

Select the correct answer from the choices below. Numbers are truncated to 4 decimal places.

Choose the best answer:


* `{ "highest_rating" : 9.8, "lowest_rating" : 6.5, "average_rating" : 7.5270, "deviation" : 0.5988 }`

* `{ "highest_rating" : 9.2, "lowest_rating" : 4.5, "average_rating" : 7.5270, "deviation" : 0.5984 }`

* `{ "highest_rating" : 9.2, "lowest_rating" : 4.5, "average_rating" : 7.5270, "deviation" : 0.5988 }`

* `{ "highest_rating" : 9.5, "lowest_rating" : 5.9, "average_rating" : 7.5290, "deviation" : 0.5988 }`


## 4. Tema: La etapa `$unwind`

### Notas de lectura

Página de documentación de [`$unwind`](https://docs.mongodb.com/manual/reference/operator/aggregation/unwind/).

### Transcripción

## 5. Laboratorio - `$unwind`

Lab - $unwind

**Problem:**

Let's use our increasing knowledge of the Aggregation Framework to explore our movies collection in more detail. We'd like to calculate how many movies every **cast** member has been in and get an average `imdb.rating` for each `cast` member.

What is the name, number of movies, and average rating (truncated to one decimal) for the cast member that has been in the most number of movies with **English** as an available language?

Provide the input in the following order and format

```sh
{ "_id": "First Last", "numFilms": 1, "average": 1.1 }
```

Enter answer here:


## 6. Tema: La etapa $lookup

### Notas de lectura

Página de documentación de [`$lookup`](https://docs.mongodb.com/manual/reference/operator/aggregation/lookup/).

### Transcripción

## 7. Examen The $lookup Stage

**Problem:**

Which of the following statements is true about the `$lookup` stage?

Check all answers that apply:

* Specifying an existing field name to **as** will overwrite the the existing field

* The collection specified in **from** cannot be sharded

* `$lookup` matches between **localField** and **foreignField** with an equality match

* You can specify a collection in another database to **from**

## 8. Laboratorio: Uso de `$lookup`

Lab - Using $lookup

**Problem:**

Which alliance from `air_alliances` flies the most **routes** with either a `Boeing 747 or an Airbus A380` (abbreviated 747 and 380 in `air_routes`)?

Choose the best answer:

* "OneWorld"

* "Star Alliance"

* "SkyTeam"

## 9. Tema: `$graphLookup` Introducción

### Transcripción

## 10. Examen $graphLookup Introduction

**Problem:**

Which of the following statements apply to `$graphLookup` operator? *check all that apply*

Check all answers that apply:

* `$graphLookup` is a new stage of the aggregation pipeline introduced in MongoDB 3.2

* `$lookup` and `$graphLookup` stages require the exact same fields in their specification.

* Provides MongoDB with graph or graph-like capabilities

* `$graphLookup` provides MongoDB a transitive closure implementation

* `$graphLookup` depends on `$lookup` operator. Cannot be used without `$lookup`

## 11. Tema: `$graphLookup`: Búsqueda Simple 

### Transcripción

## 12. Examen `$graphLookup`: Simple Lookup

**Problem:**

Which of the following statements is/are correct? *Check all that apply*.

Check all answers that apply:

* `connectToField` will be used on recursive find operations

* `as` determines a collection where `$graphLookup` will store the stage results

* `connectFromField` value will be use to match `connectToField` in a recursive match

* `startWith` indicates the index that should be use to execute the recursive match

## 13. Tema: `$graphLookup`: Esquema inverso de búsqueda simple

### Transcripción

## 14. Tema: `$graphLookup`: `maxDepth` y `depthField`

### Transcripción

## 15. Examen `$graphLookup`: `maxDepth` and `depthField``

**Problem:**

Which of the following statements are incorrect? *Check all that apply*

Check all answers that apply:

* `maxDepth` allows to specify the number of recursive lookups

* `depthField` determines a field, in the result document, which specifies the number of recursive lookup needed to reach that document

* `maxDepth` only takes `$long` values

* `depthField` determines a field, which contains the value number of documents matched by the recursive lookup

## 16. Tema: `$graphLookup`: Cross Collection Lookup

### Transcripción

## 17. Tema: `$graphLookup`: Consideraciones Generales

### Transcripción

## 18. Examen `$graphLookup`: General Considerations

**Problem:**

Consider the following statement:

```sh
``$graphLookup`` is required to be the last element on the pipeline.
```

Which of the following is true about the statement?

Choose the best answer:

* This is incorrect. `graphLookup` needs to be the first element of the pipeline, regardless of other stages needed to perform the desired query.

* This is incorrect. `$graphLookup` can be used in any position of the pipeline and acts in the same way as a regular `$lookup`.

* This is correct because `$graphLookup` pipes out the results of recursive search into a collection, similar to `$out` stage.

* This is correct because of the recursive nature of `$graphLookup` we want to save resources for last.

## 19. Laboratorio: `$graphLookup`

Lab: $graphLookup

**Problem:**

Now that you have been introduced to `$graphLookup`, let's use it to solve an interesting need. You are working for a travel agency and would like to find routes for a client! For this exercise, we'll be using the **air_airlines**, **air_alliances**, and **air_routes** collections in the **aggregations** database.

* The **air_airlines** collection will use the following schema:

```sh
{
    "_id" : ObjectId("56e9b497732b6122f8790280"),
    "airline" : 4,
    "name" : "2 Sqn No 1 Elementary Flying Training School",
    "alias" : "",
    "iata" : "WYT",
    "icao" : "",
    "active" : "N",
    "country" : "United Kingdom",
    "base" : "HGH"
}
```

* The **air_routes** collection will use this schema:

```sh
{
    "_id" : ObjectId("56e9b39b732b6122f877fa31"),
    "airline" : {
            "id" : 410,
            "name" : "Aerocondor",
            "alias" : "2B",
            "iata" : "ARD"
    },
    "src_airport" : "CEK",
    "dst_airport" : "KZN",
    "codeshare" : "",
    "stops" : 0,
    "airplane" : "CR2"
}
```

* Finally, the **air_alliances** collection will show the airlines that are in each alliance, with this schema:

```sh
{
    "_id" : ObjectId("581288b9f374076da2e36fe5"),
    "name" : "Star Alliance",
    "airlines" : [
            "Air Canada",
            "Adria Airways",
            "Avianca",
            "Scandinavian Airlines",
            "All Nippon Airways",
            "Brussels Airlines",
            "Shenzhen Airlines",
            "Air China",
            "Air New Zealand",
            "Asiana Airlines",
            "Brussels Airlines",
            "Copa Airlines",
            "Croatia Airlines",
            "EgyptAir",
            "TAP Portugal",
            "United Airlines",
            "Turkish Airlines",
            "Swiss International Air Lines",
            "Lufthansa",
            "EVA Air",
            "South African Airways",
            "Singapore Airlines"
    ]
}
```

Determine the approach that satisfies the following question in the most efficient manner:

Find the list of all possible distinct destinations, with at most one layover, departing from the base airports of airlines that make part of the "OneWorld" alliance. The airlines should be national carriers from Germany, Spain or Canada only. Include both the destination and which airline services that location. As a small hint, you should find **158** destinations.

Select the correct pipeline from the following set of options:

Choose the best answer:

* 1)

```sh
var airlines = [];
db.air_alliances.find({"name": "OneWorld"}).forEach(function(doc){
  airlines = doc.airlines
})
var oneWorldAirlines = db.air_airlines.find({"name": {"$in": airlines}})

oneWorldAirlines.forEach(function(airline){
  db.air_alliances.aggregate([
  {"$graphLookup": {
    "startWith": airline.base,
    "from": "air_routes",
    "connectFromField": "dst_airport",
    "connectToField": "src_airport",
    "as": "connections",
    "maxDepth": 1
  }}])
})
```

* 2)

```sh
db.air_airlines.aggregate(
  [
    {"$match": {"country": {"$in": ["Spain", "Germany", "Canada"]}}},
    {"$lookup": {
      "from": "air_alliances",
      "foreignField": "airlines",
      "localField": "name",
      "as": "alliance"
    }},
    {"$match": {"alliance.name": "OneWorld"}},
    {"$graphLookup": {
      "startWith": "$base",
      "from": "air_routes",
      "connectFromField": "dst_airport",
      "connectToField": "src_airport",
      "as": "connections",
      "maxDepth": 1
    }},
    {"$project":{ "connections.dst_airport": 1 }},
    {"$unwind": "$connections"},
    {"$group": { "_id": "$connections.dst_airport" }}
  ]
)
```

* 3)

```sh
db.air_alliances.aggregate([{
  $match: { name: "OneWorld" }
}, {
  $graphLookup: {
    startWith: "$airlines",
    from: "air_airlines",
    connectFromField: "name",
    connectToField: "name",
    as: "airlines",
    maxDepth: 0,
    restrictSearchWithMatch: {
      country: { $in: ["Germany", "Spain", "Canada"] }
    }
  }
}, {
  $graphLookup: {
    startWith: "$airlines.base",
    from: "air_routes",
    connectFromField: "dst_airport",
    connectToField: "src_airport",
    as: "connections",
    maxDepth: 1
  }
}, {
  $project: {
    validAirlines: "$airlines.name",
    "connections.dst_airport": 1,
    "connections.airline.name": 1
  }
},
{ $unwind: "$connections" },
{
  $project: {
    isValid: { $in: ["$connections.airline.name", "$validAirlines"] },
    "connections.dst_airport": 1
  }
},
{ $match: { isValid: true } },
{ $group: { _id: "$connections.dst_airport" } }
])
```

* 4)

```sh
db.air_routes.aggregate(
  [
    {"$lookup": {
      "from": "air_alliances",
      "foreignField": "airlines",
      "localField": "airline.name",
      "as": "alliance"
    }},
    {"$match": {"alliance.name": "OneWorld"}},
    {"$lookup": {
      "from": "air_airlines",
      "foreignField": "name",
      "localField": "airline.name",
      "as": "airline"
    }},
    {"$graphLookup": {
      "startWith": "$airline.base",
      "from": "air_routes",
      "connectFromField": "dst_airport",
      "connectToField": "src_airport",
      "as": "connections",
      "maxDepth": 1
    }},
    {"$project":{ "connections.dst_airport": 1 }},
    {"$unwind": "$connections"},
    {"$group": { "_id": "$connections.dst_airport" }}
  ]
)
```


