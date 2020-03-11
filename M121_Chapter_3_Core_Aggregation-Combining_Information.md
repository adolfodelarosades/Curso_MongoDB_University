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

## 9. Tema: `$graphLookup` Introducción

### Notas de lectura

Página de documentación de [``]().

### Transcripción

## 10. Examen

## 11. Tema: `$graphLookup`: Búsqueda Simple 

### Notas de lectura

Página de documentación de [``]().

### Transcripción

## 12. Examen

## 13. Tema: `$graphLookup`: Esquema inverso de búsqueda simple

### Notas de lectura

Página de documentación de [``]().

### Transcripción

## 14. Tema: `$graphLookup`: `maxDepth` y `depthField`

### Notas de lectura

Página de documentación de [``]().

### Transcripción

## 15. Examen

## 16. Tema: `$graphLookup`: Cross Collection Lookup

### Notas de lectura

Página de documentación de [``]().

### Transcripción

## 17. Tema: `$graphLookup`: Consideraciones Generales

### Notas de lectura

Página de documentación de [``]().

### Transcripción

## 18. Examen

## 19. Laboratorio: `$graphLookup`
