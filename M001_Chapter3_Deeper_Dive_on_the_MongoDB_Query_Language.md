# Capítulo 3: Inmersión más Profunda en el Lenguaje de Consulta MongoDB

### 16 Artículos

Operadores de consulta: Operadores de Elementos, Operadores Lógicos, Operadores de Array y el Operador Regex.

## Lecciones

1. Tema: Introducción a los Operadores de Consultas (Query)
2. Examen
3. Tema: Operadores de Comparación
4. Examen
5. Tema: Operadores de Elementos
6. Examen
7. Tema: Operadores Lógicos
8. Examen
9. Tema: Operador de Array: `$all`
10. Examen
11. Tema: Operador de Array: `$size`
12. Examen
13. Tema: Operador de Array: `$elemMatch`
14. Examen
15. Tema: Operador `$regex`
16. Problema de desafío: Valor Único en un Array de Integers

## 1. Tema: Introducción a los Operadores de Consultas (Query)

En este capítulo, vamos a hablar sobre algunos conceptos más avanzados del lenguaje de consulta MongoDB.

El lenguaje de consulta MongoDB proporciona un amplio soporte para filtrar documentos de todas las formas y tamaños, incluso aquellos con esquemas complejos. Veremos la variedad de operadores de consulta disponibles y cómo usarlos.

<img src="/images/c3/crud.png">

Tenga en cuenta que debido a que los cuatro tipos de operaciones CRUD (crear, leer, actualizar y eliminar) requieren un filtro, nuestros operadores de consulta pueden usarse en sus filtros para cualquiera de los métodos de lenguaje de consulta que hemos discutido.

Puede encontrar una lista completa, con enlaces a material de referencia para todos los operadores de consulta, en la documentación de MongoDB [Query and Projection Operators](https://docs.mongodb.com/manual/reference/operator/query/index.html).

Vamos a hacer una descripción general de cada tipo de operador con algunos ejemplos muy concretos.

La mejor manera de aprender es ensuciarse las manos.

Como parte de este capítulo, haremos que realice una serie de pruebas y laboratorios para que tenga bastante experiencia con estos operadores.

Empecemos.

## 2. Examen Introduction to Query Operators

**Problem:**

For which CRUD operations can one use the query operators we will discuss in this chapter?


Check all answers that apply:

* Create :+1:

* Read :+1:

* Update :+1:

* Delete :+1:

## 3. Tema: Operadores de Comparación

### Notas de lectura

A lo largo de este capítulo haremos referencia a la documentación de [Query Operators](https://docs.mongodb.com/manual/reference/operator/query/). Manténgalo abierto mientras trabaja en este capítulo.

#### Operadores de Comparación

Para la comparación de diferentes valores de tipo BSON, consulte el orden de comparación BSON especificado.

Nombre | Descripción
-------|------------
`$eq`| Coincide con valores que son iguales a un valor especificado.
`$gt`|  Coincide con valores que son mayores que un valor especificado.
`$gte` | Coincide con valores mayores o iguales que un valor especificado.
`$in`| 	Coincide con cualquiera de los valores especificados en un array.
`$lt`| 	Coincide con valores inferiores a un valor especificado.
`$lte` | 	Coincide con valores menores o iguales a un valor especificado.
`$ne`| 	Coincide con todos los valores que no son iguales a un valor especificado.
`$nin` | No coincide con ninguno de los valores especificados en un array.

#### Comandos de conexión:

```sh
mini-de-adolfo:~ adolfodelarosa$ mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/100YWeatherSmall?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics
```

```sh
mongo "mongodb+srv://cluster0-3bh0e.mongodb.net/test"  --username m001-student --password m001-mongodb-basics
```

### Transcripción

En esta lección, discutiremos los Operadores de Consulta de Comparación ([comparison query operators](https://docs.mongodb.com/manual/reference/operator/query/#comparison)).

Estos son operadores que nos permiten hacer coincidir en función de un valor de campo relativo a algún otro valor.

En nuestra colección de detalles de películas, la mayoría de los documentos tienen un campo llamado `runtime` (tiempo de ejecución).

Esto es un buen punto de partida para considerar algunos operadores de comparación.

Aquí tengo un shell mongo conectado a su clúster de sandbox Atlas.

```sh
mongo "mongodb+srv://cluster0-3bh0e.mongodb.net/test"  --username m001-student --password m001-mongodb-basics
```

Vamos a filtrar todas las películas que tengan un tiempo de ejecución superior a 90.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use video
switched to db video

MongoDB Enterprise Cluster0-shard-0:PRIMARY> show collections
movieDetails
moviesScratch
reviews

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.count()
2295

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({runtime: {$gt: 90} }, {_id: 0, title: 1, runtime: 1})
{ "_id" : ObjectId("5e3fc385d519ebad6471fd9d"), "title" : "Once Upon a Time in the West", "year" : 1968, "rated" : "PG-13", "runtime" : 175, "countries" : [ "Italy", "USA", "Spain" ], "genres" : [ "Western" ], "director" : "Sergio Leone", "writers" : [ "Sergio Donati", "Sergio Leone", "Dario Argento", "Bernardo Bertolucci", "Sergio Leone" ], "actors" : [ "Claudia Cardinale", "Henry Fonda", "Jason Robards", "Charles Bronson" ], "plot" : "Epic story of a mysterious stranger with a harmonica who joins forces with a notorious desperado to protect a beautiful widow from a ruthless assassin working for the railroad.", "poster" : "http://ia.media-imdb.com/images/M/MV5BMTEyODQzNDkzNjVeQTJeQWpwZ15BbWU4MDgyODk1NDEx._V1_SX300.jpg", "imdb" : { "id" : "tt0064116", "rating" : 8.6, "votes" : 201283 }, "tomato" : { "meter" : 98, "image" : "certified", "rating" : 9, "reviews" : 54, "fresh" : 53, "consensus" : "A landmark Sergio Leone spaghetti western masterpiece featuring a classic Morricone score.", "userMeter" : 95, "userRating" : 4.3, "userReviews" : 64006 }, "metacritic" : 80, "awards" : { "wins" : 4, "nominations" : 5, "text" : "4 wins & 5 nominations." }, "type" : "movie" }
...
```

Ahora el tiempo de ejecución se especifica en minutos, por lo que todas las películas duran más de una hora y media.

Si lo deseamos, podemos proyectar solo el título y el tiempo de ejecución para darnos un resumen más rápido de este conjunto de resultados.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({runtime: {$gt: 90} }, {_id: 0, title: 1, runtime: 1})
{ "title" : "Once Upon a Time in the West", "runtime" : 175 }
{ "title" : "A Million Ways to Die in the West", "runtime" : 116 }
{ "title" : "Wild Wild West", "runtime" : 106 }
{ "title" : "West Side Story", "runtime" : 152 }
{ "title" : "Red Rock West", "runtime" : 98 }
{ "title" : "How the West Was Won", "runtime" : 164 }
{ "title" : "Journey to the West", "runtime" : 110 }
{ "title" : "West of Memphis", "runtime" : 147 }
{ "title" : "Star Wars: Episode IV - A New Hope", "runtime" : 121 }
{ "title" : "Star Wars: Episode V - The Empire Strikes Back", "runtime" : 124 }
{ "title" : "Star Wars: Episode VI - Return of the Jedi", "runtime" : 131 }
{ "title" : "Star Wars: Episode I - The Phantom Menace", "runtime" : 136 }
{ "title" : "Star Wars: Episode III - Revenge of the Sith", "runtime" : 140 }
{ "title" : "Star Trek", "runtime" : 127 }
{ "title" : "Star Wars: Episode II - Attack of the Clones", "runtime" : 142 }
{ "title" : "Star Trek Into Darkness", "runtime" : 132 }
{ "title" : "Star Trek: First Contact", "runtime" : 111 }
{ "title" : "Star Trek II: The Wrath of Khan", "runtime" : 113 }
{ "title" : "Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb", "runtime" : 95 }
{ "title" : "Love Actually", "runtime" : 135 }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Puede ver que cada documento en nuestro conjunto de resultados tiene un tiempo de ejecución mayor de 90 minutos.

Ahora revisemos la sintaxis por un minuto.

¿Por qué está estructurado de esta manera? `db.movieDetails.find({runtime: {$gt: 90}}, {_id: 0, title: 1, runtime: 1})`

La idea aquí es mantener la coherencia con los filtros de igualdad.

Y para que quede claro por filtro de igualdad, simplemente me refiero a un filtro para un valor exacto.

Para los filtros de igualdad, definimos un valor específico que una clave debe tener en los documentos coincidentes.

En el caso de esta consulta, en lugar de especificar un valor al que necesitamos que sea equivalente el tiempo de ejecución, estamos expresando el tipo de relación que queremos que tengan todos los documentos del conjunto de resultados con respecto al campo de tiempo de ejecución.

Esta sintaxis también hace que sea muy conveniente expresar rangos.

Por ejemplo, puedo estipular que me gustaría ver películas de más de 90 minutos y menos de 120 minutos.

Para hacer eso, podemos usar esta consulta. `db.movieDetails.find({runtime: {$gt: 90, $lt: 120}}, {_id: 0, title: 1, runtime: 1})`

Y en aras de la exhaustividad, mencionaré que `$lt` es el operador menor que.

Ahora lo que realmente quiero es que todas las películas que coincidan con mi filtro sean mayores o iguales a 90 minutos y menores que igual a 120 minutos.

Entonces, puedo modificar mi filtro ligeramente para usar el operador mayor o igual que es `$gte` y el operador `$lte`.

Vamos a correr esto.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({runtime: {$gte: 90, $lte: 120}}, {_id: 0, title: 1, runtime: 1})
{ "title" : "A Million Ways to Die in the West", "runtime" : 116 }
{ "title" : "Wild Wild West", "runtime" : 106 }
{ "title" : "Red Rock West", "runtime" : 98 }
{ "title" : "Journey to the West", "runtime" : 110 }
{ "title" : "Star Trek: First Contact", "runtime" : 111 }
{ "title" : "Star Trek II: The Wrath of Khan", "runtime" : 113 }
{ "title" : "Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb", "runtime" : 95 }
{ "title" : "I Love You, Man", "runtime" : 105 }
{ "title" : "Love & Other Drugs", "runtime" : 112 }
{ "title" : "Punch-Drunk Love", "runtime" : 95 }
{ "title" : "From Paris with Love", "runtime" : 92 }
{ "title" : "From Russia with Love", "runtime" : 115 }
{ "title" : "I Love You Phillip Morris", "runtime" : 98 }
{ "title" : "Zathura: A Space Adventure", "runtime" : 101 }
{ "title" : "Turks in Space", "runtime" : 110 }
{ "title" : "2001: A Space Travesty", "runtime" : 99 }
{ "title" : "The Adventures of Tintin", "runtime" : 107 }
{ "title" : "The Adventures of Robin Hood", "runtime" : 102 }
{ "title" : "The Adventures of Priscilla, Queen of the Desert", "runtime" : 104 }
{ "title" : "Adventures in Babysitting", "runtime" : 102 }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Aquí podemos ver que hemos perdido algunos resultados de la última vez porque ahora estamos especificando que queremos películas en un rango de solo dos horas.

Entonces, con esta consulta, tuvimos bastantes películas que en realidad duraron más de dos horas. `db.movieDetails.find({runtime: {$gt: 90}}, {_id: 0, title: 1, runtime: 1})`

Y con este  `db.movieDetails.find({runtime: {$gt: 90, $lt: 120}}, {_id: 0, title: 1, runtime: 1})`
, solo los que duran entre 90 minutos y dos horas.

Por supuesto, con los operadores de comparación no se limitaron a trabajar con un solo campo.

Podemos trabajar fácilmente con tantos campos como sea necesario utilizando combinaciones de operadores de comparación, otros operadores y coincidencias de igualdad.

Supongamos que estamos interesados en películas que están altamente calificadas y que también tienen tiempos de ejecución largos.

Mezclemos las cosas un poco más usando un campo de documento incrustado, el medidor de tomate (tomato meter).

Ahora para el medidor de tomate, el máximo es 100.

Entonces, lo que haré es combinar un selector para el medidor de tomate buscando valores que sean mayores o iguales a 95 con tiempo de ejecución buscando valores que sean mayores o iguales a 180.

Entonces, en este caso, películas que duran tres horas o más.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({runtime: {$gte: 180}, "tomato.meter": {$gte: 95}}, 
{_id: 0, title: 1, runtime: 1})
{ "title" : "Lagaan: Once Upon a Time in India", "runtime" : 224 }
{ "title" : "The Godfather: Part II", "runtime" : 202 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Aquí podemos ver que tenemos dos resultados que cumplen con estos criterios.

El padrino: la parte II, por supuesto, es muy larga y, en mi opinión, la mejor de las tres películas de padrino.

Así que echemos un vistazo a qué otros operadores de comparación hay [comparison query operators](https://docs.mongodb.com/manual/reference/operator/query/#comparison).

`$eq`, tiene exactamente la misma semántica que las coincidencias de igualdad con las que ya está familiarizado.

Hemos visto mayor que `$gt`, mayor que o igual a `$gte`, menor que `$lt` y menor que o igual a `$lte`.

Ahora echemos un vistazo a no igual a, o `$ne` y `$in`.

Primero, veremos `$ne`.

Ahora, en muchas aplicaciones, particularmente si estamos haciendo algo como la limpieza de datos, podríamos estar interesados en particionar nuestros datos porque sabemos que tenemos algunos campos que no son los esperados.

Muchas películas de esta colección tienen uno de cuatro valores para el campo `rated` ("PG", "PG-13", "G", "R", "APPROVED") o un valor "UNRATED".

Entonces, tal vez solo queremos ver todos los documentos que tienen una calificación real, como PG, PG-13, R, etc.

Podemos usar `$ne` para hacer esto, y la semántica de este filtro es que haremos coincidir todos los documentos que para la clave `rated` (clasificada) tendrán un valor que sea diferente o no sea igual a "UNRATED" (no calificado).

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({reted: {$ne: "UNRATED"}}, {_id: 0, title: 1, rated: 1})
{ "title" : "Once Upon a Time in the West", "rated" : "PG-13" }
{ "title" : "A Million Ways to Die in the West", "rated" : "R" }
{ "title" : "Wild Wild West", "rated" : "PG-13" }
{ "title" : "West Side Story", "rated" : "UNRATED" }
{ "title" : "Slow West", "rated" : "R" }
{ "title" : "An American Tail: Fievel Goes West", "rated" : "G" }
{ "title" : "Red Rock West", "rated" : "R" }
{ "title" : "How the West Was Won", "rated" : "APPROVED" }
{ "title" : "Journey to the West", "rated" : "PG-13" }
{ "title" : "West of Memphis", "rated" : "R" }
{ "title" : "Star Wars: Episode IV - A New Hope", "rated" : "PG" }
{ "title" : "Star Wars: Episode V - The Empire Strikes Back", "rated" : "PG" }
{ "title" : "Star Wars: Episode VI - Return of the Jedi", "rated" : "PG" }
{ "title" : "Star Wars: Episode I - The Phantom Menace", "rated" : "PG" }
{ "title" : "Star Wars: Episode III - Revenge of the Sith", "rated" : "PG-13" }
{ "title" : "Star Trek", "rated" : "PG-13" }
{ "title" : "Star Wars: Episode II - Attack of the Clones", "rated" : "PG" }
{ "title" : "Star Trek Into Darkness", "rated" : "PG-13" }
{ "title" : "Star Trek: First Contact", "rated" : "PG-13" }
{ "title" : "Star Trek II: The Wrath of Khan", "rated" : "PG" }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Ahora hay una cosa que debo mencionar sobre `$ne`.

Además de hacer coincidir todos los documentos que contienen un campo `rated` cuyo valor es diferente de "UNRATED", `$ne` **también devolverá documentos que no tengan un campo `rated` en absoluto**. Vea `{ "title" : "Turks in Space" }` a continuación: 

```sh
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> it
{ "title" : "Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb", "rated" : "PG" }
{ "title" : "Love Actually", "rated" : "R" }
{ "title" : "Shakespeare in Love", "rated" : "R" }
{ "title" : "I Love You, Man", "rated" : "R" }
{ "title" : "P.S. I Love You", "rated" : "PG-13" }
{ "title" : "Love & Other Drugs", "rated" : "R" }
{ "title" : "Punch-Drunk Love", "rated" : "R" }
{ "title" : "From Paris with Love", "rated" : "R" }
{ "title" : "From Russia with Love", "rated" : "APPROVED" }
{ "title" : "I Love You Phillip Morris", "rated" : "R" }
{ "title" : "2001: A Space Odyssey", "rated" : "G" }
{ "title" : "Office Space", "rated" : "R" }
{ "title" : "Zathura: A Space Adventure", "rated" : "PG" }
{ "title" : "Space Cowboys", "rated" : "PG-13" }
{ "title" : "Lost in Space", "rated" : "PG-13" }
{ "title" : "Plan 9 from Outer Space", "rated" : "UNRATED" }
{ "title" : "Muppets from Space", "rated" : "G" }
{ "title" : "Turks in Space" }
{ "title" : "2001: A Space Travesty", "rated" : "R" }
{ "title" : "Space Chimps", "rated" : "G" }
Type "it" for more

```

MongoDB admite un modelo de datos flexible.

Existen muchos casos de uso para que los documentos de la misma colección tengan campos que otros documentos no tienen.

**En los modelos de datos MongoDB, en lugar de almacenar un valor nulo para el campo, a menudo simplemente no almacenaremos nada en el campo**.

En otra lección, veremos cómo distinguir documentos que no tienen un campo determinado.

OK, entonces el último operador de comparación que quiero ver es el `$in`.

`$in`, nos permite especificar uno o más valores.

Cualquiera de los cuales debe hacer que se devuelva un documento.

En este filtro seleccionaremos todos los documentos donde el valor de calificación sea "G" o "PG".

Los documentos con cualquiera de esos dos valores para la clave clasificada coincidirán con este filtro.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({rated: {$in: ["G", "PG"] }}, {_id: 0, title: 1, rated: 1})
{ "title" : "An American Tail: Fievel Goes West", "rated" : "G" }
{ "title" : "Star Wars: Episode IV - A New Hope", "rated" : "PG" }
{ "title" : "Star Wars: Episode V - The Empire Strikes Back", "rated" : "PG" }
{ "title" : "Star Wars: Episode VI - Return of the Jedi", "rated" : "PG" }
{ "title" : "Star Wars: Episode I - The Phantom Menace", "rated" : "PG" }
{ "title" : "Star Wars: Episode II - Attack of the Clones", "rated" : "PG" }
{ "title" : "Star Trek II: The Wrath of Khan", "rated" : "PG" }
{ "title" : "Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb", "rated" : "PG" }
{ "title" : "2001: A Space Odyssey", "rated" : "G" }
{ "title" : "Zathura: A Space Adventure", "rated" : "PG" }
{ "title" : "Muppets from Space", "rated" : "G" }
{ "title" : "Space Chimps", "rated" : "G" }
{ "title" : "The Adventures of Tintin", "rated" : "PG" }
{ "title" : "The Adventures of Baron Munchausen", "rated" : "PG" }
{ "title" : "The Adventures of Robin Hood", "rated" : "PG" }
{ "title" : "The Many Adventures of Winnie the Pooh", "rated" : "G" }
{ "title" : "The Adventures of Sharkboy and Lavagirl 3-D", "rated" : "PG" }
{ "title" : "The Adventures of Buckaroo Banzai Across the 8th Dimension", "rated" : "PG" }
{ "title" : "The Adventures of Rocky & Bullwinkle", "rated" : "PG" }
{ "title" : "The Extraordinary Adventures of Adèle Blanc-Sec", "rated" : "PG" }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y aquí podemos ver que, de hecho, todos los resultados tienen un valor de PG o G para la clave clasificada.

Si quisiéramos extender esto, simplemente agregamos otro elemento al valor del array de nuestro operador `$in`.

Tenga en cuenta que el valor de `$in` debe ser un array.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({rated: {$in: ["G", "PG", "PG-13"] }}, {_id: 0, title: 1, rated: 1})
{ "title" : "Once Upon a Time in the West", "rated" : "PG-13" }
{ "title" : "Wild Wild West", "rated" : "PG-13" }
{ "title" : "An American Tail: Fievel Goes West", "rated" : "G" }
{ "title" : "Journey to the West", "rated" : "PG-13" }
{ "title" : "Star Wars: Episode IV - A New Hope", "rated" : "PG" }
{ "title" : "Star Wars: Episode V - The Empire Strikes Back", "rated" : "PG" }
{ "title" : "Star Wars: Episode VI - Return of the Jedi", "rated" : "PG" }
{ "title" : "Star Wars: Episode I - The Phantom Menace", "rated" : "PG" }
{ "title" : "Star Wars: Episode III - Revenge of the Sith", "rated" : "PG-13" }
{ "title" : "Star Trek", "rated" : "PG-13" }
{ "title" : "Star Wars: Episode II - Attack of the Clones", "rated" : "PG" }
{ "title" : "Star Trek Into Darkness", "rated" : "PG-13" }
{ "title" : "Star Trek: First Contact", "rated" : "PG-13" }
{ "title" : "Star Trek II: The Wrath of Khan", "rated" : "PG" }
{ "title" : "Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb", "rated" : "PG" }
{ "title" : "P.S. I Love You", "rated" : "PG-13" }
{ "title" : "2001: A Space Odyssey", "rated" : "G" }
{ "title" : "Zathura: A Space Adventure", "rated" : "PG" }
{ "title" : "Space Cowboys", "rated" : "PG-13" }
{ "title" : "Lost in Space", "rated" : "PG-13" }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

En este caso, se devolverán los documentos que tengan un valor de calificación que sea cualquiera de estos tres valores `"G", "PG", "PG-13"`.

Si inventamos esto un poco y lo cambiamos solo a películas con clasificación "R" y "PG-13", verá que ahora nuestros resultados coinciden con esos criterios.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({rated: {$in: ["R", "PG-13"] }}, {_id: 0, title: 1, rated: 1})
{ "title" : "Once Upon a Time in the West", "rated" : "PG-13" }
{ "title" : "A Million Ways to Die in the West", "rated" : "R" }
{ "title" : "Wild Wild West", "rated" : "PG-13" }
{ "title" : "Slow West", "rated" : "R" }
{ "title" : "Red Rock West", "rated" : "R" }
{ "title" : "Journey to the West", "rated" : "PG-13" }
{ "title" : "West of Memphis", "rated" : "R" }
{ "title" : "Star Wars: Episode III - Revenge of the Sith", "rated" : "PG-13" }
{ "title" : "Star Trek", "rated" : "PG-13" }
{ "title" : "Star Trek Into Darkness", "rated" : "PG-13" }
{ "title" : "Star Trek: First Contact", "rated" : "PG-13" }
{ "title" : "Love Actually", "rated" : "R" }
{ "title" : "Shakespeare in Love", "rated" : "R" }
{ "title" : "I Love You, Man", "rated" : "R" }
{ "title" : "P.S. I Love You", "rated" : "PG-13" }
{ "title" : "Love & Other Drugs", "rated" : "R" }
{ "title" : "Punch-Drunk Love", "rated" : "R" }
{ "title" : "From Paris with Love", "rated" : "R" }
{ "title" : "I Love You Phillip Morris", "rated" : "R" }
{ "title" : "Office Space", "rated" : "R" }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

También hay un operador que nos permite hacer lo contrario de lo que hace `$in`.

Lo dejaré como ejercicio para que experimente con ese operador (`$nin`).

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({rated: {$nin: ["R", "PG-13"] }}, {_id: 0, title: 1, rated: 1})
{ "title" : "West Side Story", "rated" : "UNRATED" }
{ "title" : "An American Tail: Fievel Goes West", "rated" : "G" }
{ "title" : "How the West Was Won", "rated" : "APPROVED" }
{ "title" : "Star Wars: Episode IV - A New Hope", "rated" : "PG" }
{ "title" : "Star Wars: Episode V - The Empire Strikes Back", "rated" : "PG" }
{ "title" : "Star Wars: Episode VI - Return of the Jedi", "rated" : "PG" }
{ "title" : "Star Wars: Episode I - The Phantom Menace", "rated" : "PG" }
{ "title" : "Star Wars: Episode II - Attack of the Clones", "rated" : "PG" }
{ "title" : "Star Trek II: The Wrath of Khan", "rated" : "PG" }
{ "title" : "Dr. Strangelove or: How I Learned to Stop Worrying and Love the Bomb", "rated" : "PG" }
{ "title" : "From Russia with Love", "rated" : "APPROVED" }
{ "title" : "2001: A Space Odyssey", "rated" : "G" }
{ "title" : "Zathura: A Space Adventure", "rated" : "PG" }
{ "title" : "Plan 9 from Outer Space", "rated" : "UNRATED" }
{ "title" : "Muppets from Space", "rated" : "G" }
{ "title" : "Turks in Space" }
{ "title" : "Space Chimps", "rated" : "G" }
{ "title" : "The Adventures of Tintin", "rated" : "PG" }
{ "title" : "The Adventures of Baron Munchausen", "rated" : "PG" }
{ "title" : "The Adventures of Robin Hood", "rated" : "PG" }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y con eso, hemos cubierto los operadores de comparación para hacer consultas dentro de MongoDB.

Estos expanden significativamente los tipos de consultas que podemos hacer contra una colección MongoDB.

## 4. Examen Comparison Operators

**Problem:**

Using the `$in` operator, filter the `video.movieDetails` collection to determine how many movies list "Ethan Coen" or "Joel Coen" among their writers. Your filter should match all movies that list one of the Coen brothers as writers regardless of how many other writers are also listed. Select the number of movies matching this filter from the choices below.

Choose the best answer:

* 0

* 3 :+1:

* 7

* 12

* 16

Solution:

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.find({writers: {$in: ["Ethan Coen", "Joel Coen"] }}, {_id: 0, title: 1, writers: 1})
{ "title" : "The Big Lebowski", "writers" : [ "Ethan Coen", "Joel Coen" ] }
{ "title" : "No Country for Old Men", "writers" : [ "Joel Coen", "Ethan Coen", "Cormac McCarthy" ] }
{ "title" : "Paris, je t'aime", "writers" : [ "Tristan Carné", "Emmanuel Benbihy", "Emmanuel Benbihy", "Bruno Podalydès", "Paul Mayeda Berges", "Gurinder Chadha", "Gus Van Sant", "Joel Coen", "Ethan Coen", "Walter Salles", "Daniela Thomas", "Christopher Doyle", "Rain Li", "Gabrielle Keng", "Isabel Coixet", "Nobuhiro Suwa", "Sylvain Chomet", "Alfonso Cuarón", "Olivier Assayas", "Oliver Schmitz", "Richard LaGravenese", "Vincenzo Natali", "Wes Craven", "Tom Tykwer", "Gena Rowlands", "Alexander Payne", "Nadine Eïd", "Frédéric Auburtin" ] }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

```

## 5. Tema: Operadores de Elementos

### Notas de lectura

Si desea revisar cómo filtrar otros tipos, consulte la [documentación de `$type`](https://docs.mongodb.com/manual/reference/operator/query/type/).

### Transcripción

En otra lección mencione que, debido a su modelo de datos flexible, MongoDB admite operadores que nos permiten detectar la presencia o ausencia de un campo determinado.

Además, como sabe, aunque no suele ser una buena idea, es posible que el mismo campo en la colección tenga un tipo de valor diferente de un documento a otro.

El lenguaje de consulta MongoDB proporciona operadores que nos permiten manejar ambas situaciones.

Se llaman `$exist` y `$type`.

Así que echemos un vistazo a un ejemplo de `$exist` primero.

Para esto, veremos nuevamente el clúster Atlas clase M001 y la colección `video.movies`.

```sh
mini-de-adolfo:~ adolfodelarosa$ mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/100YWeatherSmall?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics

MongoDB Enterprise Cluster0-shard-0:PRIMARY> show dbs
100YWeatherSmall  0.128GB
admin             0.000GB
aggregations      0.067GB
citibike          0.367GB
city              0.002GB
config            0.000GB
coursera-agg      0.083GB
local             0.940GB
mflix             0.449GB
results           0.000GB
ships             0.001GB
video             0.513GB

MongoDB Enterprise Cluster0-shard-0:PRIMARY> use video
switched to db video

MongoDB Enterprise Cluster0-shard-0:PRIMARY> show collections
movies

MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

```
Dentro de la colección de películas, tenemos bastantes películas antiguas.

Muchas de estas películas son anteriores al sistema de clasificación de películas MPAA.

Lo que podríamos querer hacer con una aplicación es probar la existencia de una calificación(rating).

Aquí en Compass, me he conectado al grupo de clase Atlas.

Filtremos por documentos que contengan el campo de calificación MPAA `{mpaaRating: {$exists: true}}`.

Entonces, la sintaxis de `$exist` es que especificamos la clave(key) en la que estamos interesados en probar su existencia.

Y como el valor de esa clave en nuestro filtro, proporcionamos un documento que tiene `$exist` como key y un valor de verdadero o falso.

Si el valor que especificamos aquí es verdadero, entonces haremos coincidir los documentos que contienen esta clave.

<img src="/images/c3/5-exists-true.png">

Y si es falso, haremos coincidir los documentos que no contienen la clave.

Tenga en cuenta que los documentos que contienen la clave de calificación MPAA son sustancialmente menores que el número total de documentos en esta colección.

Hagamos lo contrario ahora y establezca `$exist` en false.

<img src="/images/c3/5-exists-false.png">

Ahora podemos ver que las películas recuperadas en respuesta a esta consulta no contienen ningún campo de calificación MPAA (`mpaaRating`).

Si nos desplazamos a través de la vista de esquema, vemos que aquí, donde debería aparecer la calificación MPAA, no es así.

Esto se debe a que con este filtro aplicado, Compass solo está mirando documentos que no tienen este campo.

Debo señalar aquí algo con lo que podría toparse con respecto a los campos faltantes.

No es un valor que se usa comúnmente en bases de datos relacionales para filas que no tienen datos para una columna en particular.

Y para este ejemplo, vamos a cambiar rápidamente a nuestro clúster de sandbox Atlas y echar un vistazo a la colección de detalles de la película.

```sh
mini-de-adolfo:~ adolfodelarosa$ mongo "mongodb+srv://cluster0-3bh0e.mongodb.net/test"  --username m001-student --password m001-mongodb-basics
MongoDB shell version v4.2.2

MongoDB Enterprise Cluster0-shard-0:PRIMARY> show dbs
admin               0.000GB
local               1.635GB
sample_airbnb       0.053GB
sample_geospatial   0.001GB
sample_mflix        0.042GB
sample_supplies     0.001GB
sample_training     0.068GB
sample_weatherdata  0.004GB
video               0.002GB

MongoDB Enterprise Cluster0-shard-0:PRIMARY> use video
switched to db video

MongoDB Enterprise Cluster0-shard-0:PRIMARY> show collections
movieDetails
moviesScratch
reviews
```

Algunos usuarios de MongoDB prefieren incluir una clave y simplemente establecer su valor en nulo para documentos que no tienen valor para ese campo.

Para admitir consultas de valores nulos, si filtra algo como esto, `{"tomato.consensus": null})`, este filtro coincidirá con ambos documentos que tienen explícitamente el valor establecido en nulo, como vemos en este documento, 

```sh
...
"imdb" : {
		"id" : "tt0073629",
		"rating" : 7.4,
		"votes" : 97249
	},
	"tomato" : {
		"meter" : 80,
		"image" : "fresh",
		"rating" : 6.8,
		"reviews" : 41,
		"fresh" : 33,
		"consensus" : null,
		"userMeter" : 85,
		"userRating" : 3.6,
		"userReviews" : 364776
	},
	"metacritic" : 58,
	"awards" : {
		"wins" : 2,
		"nominations" : 4,
		"text" : "2 wins & 4 nominations."
	},
...
```

y aquellos que no contienen la clave `tomate.consensus`, como vemos en este documento, 

```sh
"imdb" : {
		"id" : "tt0067328",
		"rating" : 8.1,
		"votes" : 29830
	},
	"awards" : {
		"wins" : 16,
		"nominations" : 22,
		"text" : "Won 2 Oscars. Another 16 wins & 22 nominations."
	},
	"type" : "movie"
```

que no solo no tiene `tomate.consensus` sino que no tiene el campo período de `tomato` (tomato field period).

**Para las colecciones que contienen documentos con valores nulos para algunos campos, inevitablemente se encontrará con esto.**

Así que mantente atento.

Ahora echemos un vistazo al operador `$type`.

Seguiremos trabajando con nuestra colección `video.movies` para nuestro ejemplo.

Echa un vistazo al campo de calificación del espectador (`viewerRating`).

<img src="/images/c3/5-viewer-rating.png">

Lo que vemos aquí en la vista de esquema para `video.movies` es que el tipo de valor para la calificación del espectador realmente depende del documento que veamos.

Algunos tienen un tipo de valor para doble, algunos tienen un tipo de valor de Int32, otros tienen un tipo de valor de indefinido (`undefined`).

Podemos filtrar los documentos que tienen un tipo de valor particular para un campo utilizando el operador `$type`.

Veamos un ejemplo.

Y para esto, me gustaría volver al Shell de Mongo.

Aquí tengo un shell conectado al clúster Atlas clase M001.


```sh
mini-de-adolfo:~ adolfodelarosa$ mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/100YWeatherSmall?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics

MongoDB Enterprise Cluster0-shard-0:PRIMARY> show dbs
100YWeatherSmall  0.128GB
admin             0.000GB
aggregations      0.067GB
citibike          0.367GB
city              0.002GB
config            0.000GB
coursera-agg      0.083GB
local             0.940GB
mflix             0.449GB
results           0.000GB
ships             0.001GB
video             0.513GB
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use video
switched to db video
MongoDB Enterprise Cluster0-shard-0:PRIMARY> show collections
movies

```

Con este comando, nuestro filtro solo coincidirá con los documentos de la colección video.movies que tengan un valor para viewerRating que sea un entero de 32 bits.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({viewerRating: {$type: "int"}}).pretty()
{
	"_id" : ObjectId("58c59c6d99d4ee0af9e13851"),
	"title" : "El derrumbamiento del Ejèrcito Rojo",
	"year" : 1939,
	"imdbId" : "tt0030050",
	"genre" : "Documentary, War",
	"viewerRating" : 1,
	"viewerVotes" : 6,
	"runtime" : 72,
	"director" : "Antonio Calvache",
	"cast" : [
		"Miguel Cabanellas",
		"Conde de Rodezno",
		"Von Faupel",
		"Raimundo Fern�ndez Cuesta"
	]
}
{
	"_id" : ObjectId("58c59c7299d4ee0af9e1e322"),
	"title" : "Godfather's Fury",
	"year" : 1978,
	"imdbId" : "tt0077619",
	"genre" : "Adventure, Crime",
	"viewerRating" : 1,
....
```

Y podemos ver que en cada uno de estos documentos, la clasificación del espectador es, de hecho, un número entero.

Puedo cambiar esto y buscar solo `doubles` en su lugar.

Y aquí podemos ver que estos son valores de coma flotante.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({viewerRating: {$type: "double"}}).pretty()
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1d3ea"),
	"title" : "Ich denk', mich tritt ein Pferd",
	"year" : 1975,
	"imdbId" : "tt0073143",
	"genre" : "",
	"viewerRating" : 1.1,
	"viewerVotes" : 28,
	"runtime" : 91,
	"director" : "Theo Maria Werner",
	"cast" : [
		"Uschi Glas",
		"Michael Cramer",
		"Susi Engel",
		"Barbara Nielsen"
	],
	"language" : "German"
}
{
	"_id" : ObjectId("58c59c7699d4ee0af9e2625b"),
	"title" : "Die Tunnelgangster von Berlin",
	"year" : 1996,
	"imdbId" : "tt0114744",
	"genre" : "Crime, Thriller",
	"viewerRating" : 1.1,
...
```

Si desea revisar cómo filtrar otros tipos, consulte la documentación.

Incluye una cobertura integral para todos los tipos de valor por los que puede filtrar, junto con detalles adicionales sobre el operador [`$type`](https://docs.mongodb.com/manual/reference/operator/query/type/).

`$exists` y `$type` nos permiten hacer meta preguntas sobre los documentos de una colección y, por lo tanto, nos brindan algunas herramientas importantes para trabajar con el soporte de MongoDB para modelos de datos flexibles.

## 6. Examen Element Operators

Connect to our class Atlas cluster from the mongo shell or Compass and answer the following question. How many documents in the `100YWeatherSmall.data` collection do **NOT** contain the key `atmosphericPressureChange`.

Choose the best answer:

* 1

* 2679

* 10345

* 33989

* 40668 :+1:


Solution:

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.data.find({atmosphericPressureChange : {$exists: false} }).count()
40668
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

## 7. Tema: Operadores Lógicos

### Transcripción

En esta lección, vamos a considerar operadores lógicos.

En particular, nos vamos a centrar en `$or` y `$and`.

Ahora también quiero hacerle saber que hay un operador `$not` y un operador `$nor`.

Pero en esta lección, vamos a ver solo `$or` y `$and`.

Echemos un vistazo a un ejemplo usando el operador `$or`.

Lo que vamos a hacer aquí es buscar documentos basados en su calificación(rating) utilizando tanto el medidor de tomate (tomato meter), que es producido por las revisiones del público en general que se preocupa por comentar, como el metacrítico (metacritic), que es un puntaje generado en base a las revisiones de críticos de cine.

Ahora, la suposición incluida en esta consulta es que el público en general es un calificador más fácil que los críticos de cine.

Estamos buscando documentos en nuestra colección de detalles de películas que tengan una `tomato.meter` mayor que 95 o una calificación metacrítica mayor de 88.

Veamos la sintaxis aquí.

`$or` toma como valor un array en la que especificamos selectores.

En este caso, estamos usando más de 95 para el `tomato.meter` y más de 88 para el `metacritic`.

Ahora ejecutemos esta consulta y echemos un vistazo a los resultados.

Podemos ver que "Toy Story 3" en realidad coincide con ambos criterios.

Tiene una calificación de `tomato.meter` mayor de 95 y una puntuación metacrítica mayor de 92.

Y luego aquí hay uno en el que el público en general y los críticos estamos bastante separados en la calificación de esta película, este es el documento para el "Día de la Marmota".

Entonces, de nuevo,`$or` toma un array como argumento.

Los elementos del array son selectores.

Cualquiera de los cuales puede ser cierto para que coincida con un documento que será devuelto por esta consulta.

Ahora veamos el operador `$and`.

Algo que quiero señalar sobre `$and` es que es necesario solo en ciertas situaciones.

Por ejemplo, podríamos usar `$and` aquí y restringir los resultados a aquellas películas donde tanto el puntaje metacrítico como el puntaje del `tomato.meter` fueron altos.

Veamos un ejemplo de eso.

Así que ahora solo se nos devuelven películas en las que tanto el `tomato.meter` como los puntajes metacríticos son altos.

Pero lo que hay que tener en cuenta es que el operador`$and` en esta consulta en particular es superfluo.

La razón es que la consulta que acabamos de hacer aquí es equivalente a esta.

Los selectores en un filtro ya están implícitamente unidos.

Y podemos ver que si aplicamos esta consulta sin el uso de `$and` que obtenemos exactamente los mismos resultados de búsqueda.

Entonces, ¿por qué hay un `$and` un operador?

La razón es porque a veces necesitamos especificar el mismo campo más de una vez en un filtro.

Si intentara hacer esto, usando solo un filtro simple, no obtendría los resultados deseados porque las claves en un documento JSON deben ser únicas.

Por ejemplo, si nuestra consulta fuera esta, el último uso de la clave metacrítica sería el utilizado.

El operador `$and` me permite especificar múltiples restricciones en el mismo campo en situaciones como esta donde necesito hacerlo.

En este caso, necesitamos todos los documentos para los cuales metacrítico no es igual a nulo, pero existe.

Recuerde que nulo coincidirá con las claves que realmente tienen el valor nulo y las que no contienen la clave en absoluto.

Este tipo de consulta podría ser útil para una aplicación en la que sabemos que tenemos un poco de datos sucios donde posiblemente hay campos que tienen un valor metacrítico que es igual a nulo.

Pero lo que realmente queremos es que todos nuestros valores metacríticos tengan un valor numérico de algún tipo.

Y si ejecuto esto, entonces obtengo documentos donde existe el campo `metacritic` y donde tengo un valor distinto de nulo para ese campo.

Y siempre podemos cambiar esto para ver solo documentos donde `metacritic` es nulo y el campo realmente existe.

Ejecutemos esta consulta en su lugar.

Ahora, en realidad no hay ningún documento de este tipo en esta colección.

Y lo que quiero decir con eso es que, de hecho, no son documentos en los que el campo `metacritic` tenga un valor nulo.

Así que creemos uno.

Para esto, voy a ir a Compass.

Y a pesar de que el documento que estamos buscando ya está aquí, me gustaría limitar el conjunto de resultados que estamos viendo solo a este documento.

Aplicando ese filtro, vemos un documento devuelto y que de hecho es "Once Upon a Time in the West".

Ahora, para crear un documento que tenga un valor nulo para `metacritic`, lo que voy a hacer aquí es usar el botón copiar documento que veo a la derecha de mi documento aquí en la interfaz de Compass.

Eso muestra un modal con una copia de ese documento y me da la oportunidad de insertarlo.

Pero antes de insertarlo, lo que quiero hacer es ir al campo `metacritic`, cambiar su valor a nulo y asegurarme de que también cambie su tipo a nulo.

Entonces lo que haré es insertarlo.

Pero primero, tenga en cuenta que no hay un `_id` para esta copia del documento.

Cuando lo inserte, se creará otro valor de ID de puntaje para mí.

Y ahora tengo dos documentos que tienen este título.

El primero es el documento original.

Y el segundo es mi copia con `metacritic` establecido en nulo.

Ahora, si ejecuto esa misma consulta en un shell, podemos ver que coincidimos con el documento que acabamos de insertar.

Antes de cerrar esta lección, volveré a Compass y eliminaré ese documento que inserté para limpiarlo después de mí mismo.

Nuevamente, recuerde que `$and` se usa en situaciones en las que necesitamos especificar múltiples criterios en el mismo campo.

## 8. Examen

## 9. Tema: Operador de Array: `$all`

### Transcripción

## 10. Examen

## 11. Tema: Operador de Array: `$size`

### Transcripción

## 12. Examen

## 13. Tema: Operador de Array: `$elemMatch`

### Transcripción

## 14. Examen

## 15. Tema: Operador `$regex`

### Transcripción

## 16. Problema de desafío: Valor Único en un Array de Integers
