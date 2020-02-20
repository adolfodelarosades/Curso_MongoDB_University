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

## 4. Examen

## 5. Tema: Operadores de Elementos

## 6. Examen

## 7. Tema: Operadores Lógicos

## 8. Examen

## 9. Tema: Operador de Array: `$all`

## 10. Examen

## 11. Tema: Operador de Array: `$size`

## 12. Examen

## 13. Tema: Operador de Array: `$elemMatch`

## 14. Examen

## 15. Tema: Operador `$regex`

## 16. Problema de desafío: Valor Único en un Array de Integers
