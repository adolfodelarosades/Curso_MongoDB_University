# Capítulo 2: Aggregation Básica: Utility Stages
 
Lecciones

1. Tema: `$addFields` y cómo este es similar a `$project`
2. Tema: `geoNear`Stage
3. Tema: Cursor-like stages: Parte 1
4. Tema: Cursor-like stages: Parte 2
5. Tema: `$sample` Stage
6. Laboratorio: Uso de Cursor-like Stages
7. Laboratorio: Uniendo todo junto


## 1. Tema: `$addFields` y cómo este es similar a `$project`

### Notas de lectura

Página de documentación de [`$addFields`](https://docs.mongodb.com/manual/reference/operator/aggregation/addFields/).

### Transcripción

Analicemos ahora otra etapa transformadora, `$addFields`.

`$addFields` es extremadamente similar a `$project`, con una diferencia clave.

Como su nombre lo dice, `$addFields` agrega campos a un documento.

Mientras que con `$project`, podemos eliminar y retener campos selectivamente, `$addFields` solo le permite modificar los documentos de pipeline entrantes con nuevos campos calculados o modificar campos existentes.

A menudo, queremos derivar un nuevo campo o cambiar los campos existentes, y el requisito en `$project` de que una vez que realicemos una transformación o presentemos un campo, debemos especificar todos los campos que deseamos presentar, esto puede volverse tediosos.

Veamos un ejemplo.

Entramos a Mongo:

```sh
mini-de-adolfo:~ adolfodelarosa$ mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/aggregations?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl -u m121 -p aggregations --norc
....
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Primero, veremos `$project`.

Simplemente extraigamos los datos del campo `gravity.value` y reasignemoslo al nivel superior, al campo `gravity`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{"$project": { "gravity": "$gravity.value" } }]);
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d54"), "gravity" : 9.8 }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d59"), "gravity" : 11.15 }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d58"), "gravity" : 8.87 }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d57"), "gravity" : 10.44 }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d56"), "gravity" : 24.79 }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d53"), "gravity" : 8.87 }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d52"), "gravity" : 3.24 }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d51"), "gravity" : 274 }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d55"), "gravity" : 3.71 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Como se esperaba, podemos recuperar los resultados con el campo `_id` y el campo `gravity` que acabamos de calcular.

Ahora eliminemos el campo `_id` y agreguemos el campo `name` para una referencia más fácil.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{"$project": { "_id": 0, "name": 1, "gravity": "$gravity.value" } }])
{ "name" : "Earth", "gravity" : 9.8 }
{ "name" : "Neptune", "gravity" : 11.15 }
{ "name" : "Uranus", "gravity" : 8.87 }
{ "name" : "Saturn", "gravity" : 10.44 }
{ "name" : "Jupiter", "gravity" : 24.79 }
{ "name" : "Venus", "gravity" : 8.87 }
{ "name" : "Mercury", "gravity" : 3.24 }
{ "name" : "Sun", "gravity" : 274 }
{ "name" : "Mars", "gravity" : 3.71 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Muy bien, esto es bastante bueno.

Pero, ¿qué sucede si también queremos mantener los campos `temperature`, `density`, `mass`, `radius` y SMA?

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([
... {"$project":{
...     "_id": 0,
...     "name": 1,
...     "gravity": "$gravity.value",
...     "meanTemperature": 1,
...     "density": 1,
...     "mass": "$mass.value",
...     "radius": "$radius.value",
...     "sma": "$sma.value" }
... }]);
{ "name" : "Earth", "meanTemperature" : 15, "gravity" : 9.8, "mass" : 5.9723e+24, "radius" : 6378.137, "sma" : 149600000 }
{ "name" : "Neptune", "meanTemperature" : -210, "gravity" : 11.15, "mass" : 1.02413e+26, "radius" : 24765, "sma" : 4495060000 }
{ "name" : "Uranus", "meanTemperature" : -200, "gravity" : 8.87, "mass" : 8.6813e+25, "radius" : 25559, "sma" : 2872460000 }
{ "name" : "Saturn", "meanTemperature" : -170, "gravity" : 10.44, "mass" : 5.6834e+26, "radius" : 60268, "sma" : 1433530000 }
{ "name" : "Jupiter", "meanTemperature" : -150, "gravity" : 24.79, "mass" : 1.89819e+27, "radius" : 71492, "sma" : 778570000 }
{ "name" : "Venus", "meanTemperature" : 465, "gravity" : 8.87, "mass" : 4.8675e+24, "radius" : 6051.8, "sma" : 108210000 }
{ "name" : "Mercury", "meanTemperature" : 125, "gravity" : 3.24, "mass" : 3.3e+23, "radius" : 4879, "sma" : 57910000 }
{ "name" : "Sun", "meanTemperature" : 5600, "gravity" : 274, "mass" : 1.9885e+30, "radius" : 695700, "sma" : 0 }
{ "name" : "Mars", "meanTemperature" : -53, "gravity" : 3.71, "mass" : 6.4171e+23, "radius" : 3396.2, "sma" : 227920000 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Como podemos ver, para mantener la información que queremos, tuvimos que ser explícitos, especificando qué campos presentamos junto con la realización de nuestras transformaciones (ej `"gravity": "$gravity.value", "mass": "$mass.value"` etc).

Como se dijo, esto puede volverse tedioso.

Para eso sirve `$addFields`.

Si sustituimos `$addFields` por `$project` y ejecutamos la siguiente pipeline, podemos ver que efectivamente realizamos las transformaciones deseadas.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([
... {"$addFields":{
...     "gravity": "$gravity.value",
...     "mass": "$mass.value",
...     "radius": "$radius.value",
...     "sma": "$sma.value"}
... }]).pretty()
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d54"),
	"name" : "Earth",
	"type" : "Terrestrial planet",
	"orderFromSun" : 3,
	"radius" : 6378.137,
	"mass" : 5.9723e+24,
	"sma" : 149600000,
	"orbitalPeriod" : {
		"value" : 1,
		"units" : "years"
	},
	"eccentricity" : 0.0167,
	"meanOrbitalVelocity" : {
		"value" : 29.78,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 1,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 23.45,
		"units" : "degrees"
	},
	"meanTemperature" : 15,
	"gravity" : 9.8,
	"escapeVelocity" : {
		"value" : 11.18,
		"units" : "km/sec"
	},
	"meanDensity" : 5.52,
	"atmosphericComposition" : "N2+O2",
	"numberOfMoons" : 1,
	"hasRings" : false,
	"hasMagneticField" : true
}
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d59"),
	"name" : "Neptune",
	"type" : "Gas giant",
	"orderFromSun" : 8,
	"radius" : 24765,
	"mass" : 1.02413e+26,
	"sma" : 4495060000,
	"orbitalPeriod" : {
		"value" : 164.79,
		"units" : "years"
	},
	"eccentricity" : 0.0113,
	"meanOrbitalVelocity" : {
		"value" : 5.43,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 0.72,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 28.8,
		"units" : "degrees"
	},
	"meanTemperature" : -210,
	"gravity" : 11.15,
	"escapeVelocity" : {
		"value" : 23.5,
		"units" : "km/sec"
	},
	"meanDensity" : 1.638,
	"atmosphericComposition" : "H2+He",
	"numberOfMoons" : 14,
	"hasRings" : true,
	"hasMagneticField" : true
}
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d58"),
	"name" : "Uranus",
	"type" : "Gas giant",
	"orderFromSun" : 7,
	"radius" : 25559,
	"mass" : 8.6813e+25,
	"sma" : 2872460000,
	"orbitalPeriod" : {
		"value" : 84.01,
		"units" : "years"
	},
	"eccentricity" : 0.0457,
	"meanOrbitalVelocity" : {
		"value" : 6.8,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 0.72,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 97.77,
		"units" : "degrees"
	},
	"meanTemperature" : -200,
	"gravity" : 8.87,
	"escapeVelocity" : {
		"value" : 21.3,
		"units" : "km/sec"
	},
	"meanDensity" : 1.271,
	"atmosphericComposition" : "H2+He",
	"numberOfMoons" : 27,
	"hasRings" : true,
	"hasMagneticField" : true
}
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d57"),
	"name" : "Saturn",
	"type" : "Gas giant",
	"orderFromSun" : 6,
	"radius" : 60268,
	"mass" : 5.6834e+26,
	"sma" : 1433530000,
	"orbitalPeriod" : {
		"value" : 29.457,
		"units" : "years"
	},
	"eccentricity" : 0.0566,
	"meanOrbitalVelocity" : {
		"value" : 9.68,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 0.445,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 26.73,
		"units" : "degrees"
	},
	"meanTemperature" : -170,
	"gravity" : 10.44,
	"escapeVelocity" : {
		"value" : 35.5,
		"units" : "km/sec"
	},
	"meanDensity" : 0.687,
	"atmosphericComposition" : "H2+He",
	"numberOfMoons" : 62,
	"hasRings" : true,
	"hasMagneticField" : true
}
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d56"),
	"name" : "Jupiter",
	"type" : "Gas giant",
	"orderFromSun" : 5,
	"radius" : 71492,
	"mass" : 1.89819e+27,
	"sma" : 778570000,
	"orbitalPeriod" : {
		"value" : 11.86,
		"units" : "years"
	},
	"eccentricity" : 0.0489,
	"meanOrbitalVelocity" : {
		"value" : 13.06,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 0.41,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 3.08,
		"units" : "degrees"
	},
	"meanTemperature" : -150,
	"gravity" : 24.79,
	"escapeVelocity" : {
		"value" : 59.5,
		"units" : "km/sec"
	},
	"meanDensity" : 1.33,
	"atmosphericComposition" : "H2+He",
	"numberOfMoons" : 67,
	"hasRings" : true,
	"hasMagneticField" : true
}
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d53"),
	"name" : "Venus",
	"type" : "Terrestrial planet",
	"orderFromSun" : 2,
	"radius" : 6051.8,
	"mass" : 4.8675e+24,
	"sma" : 108210000,
	"orbitalPeriod" : {
		"value" : 0.615,
		"units" : "years"
	},
	"eccentricity" : 0.0067,
	"meanOrbitalVelocity" : {
		"value" : 35.02,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 243.69,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 177.36,
		"units" : "degrees"
	},
	"meanTemperature" : 465,
	"gravity" : 8.87,
	"escapeVelocity" : {
		"value" : 10.36,
		"units" : "km/sec"
	},
	"meanDensity" : 5.25,
	"atmosphericComposition" : "CO2",
	"numberOfMoons" : 0,
	"hasRings" : false,
	"hasMagneticField" : false
}
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d52"),
	"name" : "Mercury",
	"type" : "Terrestrial planet",
	"orderFromSun" : 1,
	"radius" : 4879,
	"mass" : 3.3e+23,
	"sma" : 57910000,
	"orbitalPeriod" : {
		"value" : 0.24,
		"units" : "years"
	},
	"eccentricity" : 0.2056,
	"meanOrbitalVelocity" : {
		"value" : 47.36,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 58.65,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 0,
		"units" : "degrees"
	},
	"meanTemperature" : 125,
	"gravity" : 3.24,
	"escapeVelocity" : {
		"value" : 4.25,
		"units" : "km/sec"
	},
	"meanDensity" : 5.43,
	"atmosphericComposition" : "",
	"numberOfMoons" : 0,
	"hasRings" : false,
	"hasMagneticField" : true
}
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d51"),
	"name" : "Sun",
	"type" : "Star",
	"orderFromSun" : 0,
	"radius" : 695700,
	"mass" : 1.9885e+30,
	"sma" : 0,
	"orbitalPeriod" : {
		"value" : 0,
		"units" : "years"
	},
	"eccentricity" : 0,
	"meanOrbitalVelocity" : {
		"value" : 0,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 25.449,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 7.25,
		"units" : "degrees"
	},
	"meanTemperature" : 5600,
	"gravity" : 274,
	"escapeVelocity" : {
		"value" : 617.7,
		"units" : "km/sec"
	},
	"meanDensity" : 1.4,
	"atmosphericComposition" : "H2+He",
	"numberOfMoons" : 0,
	"hasRings" : false,
	"hasMagneticField" : true
}
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d55"),
	"name" : "Mars",
	"type" : "Terrestrial planet",
	"orderFromSun" : 4,
	"radius" : 3396.2,
	"mass" : 6.4171e+23,
	"sma" : 227920000,
	"orbitalPeriod" : {
		"value" : 1.881,
		"units" : "years"
	},
	"eccentricity" : 0.0935,
	"meanOrbitalVelocity" : {
		"value" : 24.07,
		"units" : "km/sec"
	},
	"rotationPeriod" : {
		"value" : 1.029,
		"units" : "days"
	},
	"inclinationOfAxis" : {
		"value" : 25.19,
		"units" : "degrees"
	},
	"meanTemperature" : -53,
	"gravity" : 3.71,
	"escapeVelocity" : {
		"value" : 5.03,
		"units" : "km/sec"
	},
	"meanDensity" : 3.93,
	"atmosphericComposition" : "CO2",
	"numberOfMoons" : 2,
	"hasRings" : false,
	"hasMagneticField" : false
}
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Sin embargo, no eliminamos ningún campo del documento original.

En cambio, agregamos nuevos campos de transformación al documento.

OK.

Un último ejemplo.

Al combinar `$project` con `$addFields`, eliminamos la molestia de la necesidad explícita de eliminar o retener campos.

```sh
db.solarSystem.aggregate([
{"$project": {
    "_id": 0,
    "name": 1,
    "gravity": 1,
    "mass": 1,
    "radius": 1,
    "sma": 1}
},
{"$addFields": {
    "gravity": "$gravity.value",
    "mass": "$mass.value",
    "radius": "$radius.value",
    "sma": "$sma.value"
}}]);
```

En este ejemplo, con `$project`, estamos seleccionando los campos que deseamos retener, y en `$addFields`, estamos realizando nuestra transformación en esos campos preseleccionados.

No es necesario ir uno por uno y eliminar o presentar campos mientras realizamos nuestras transformaciones.

Esta es una opción de estilo y puede evitar tener que especificar repetidamente qué campos retener en pipelines más grandes al realizar muchos cálculos diferentes.

Vamos a verlo en acción.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([
... {"$project": {
...     "_id": 0,
...     "name": 1,
...     "gravity": 1,
...     "mass": 1,
...     "radius": 1,
...     "sma": 1}
... },
... {"$addFields": {
...     "gravity": "$gravity.value",
...     "mass": "$mass.value",
...     "radius": "$radius.value",
...     "sma": "$sma.value"
... }}]);
{ "name" : "Earth", "radius" : 6378.137, "mass" : 5.9723e+24, "sma" : 149600000, "gravity" : 9.8 }
{ "name" : "Neptune", "radius" : 24765, "mass" : 1.02413e+26, "sma" : 4495060000, "gravity" : 11.15 }
{ "name" : "Uranus", "radius" : 25559, "mass" : 8.6813e+25, "sma" : 2872460000, "gravity" : 8.87 }
{ "name" : "Saturn", "radius" : 60268, "mass" : 5.6834e+26, "sma" : 1433530000, "gravity" : 10.44 }
{ "name" : "Jupiter", "radius" : 71492, "mass" : 1.89819e+27, "sma" : 778570000, "gravity" : 24.79 }
{ "name" : "Venus", "radius" : 6051.8, "mass" : 4.8675e+24, "sma" : 108210000, "gravity" : 8.87 }
{ "name" : "Mercury", "radius" : 4879, "mass" : 3.3e+23, "sma" : 57910000, "gravity" : 3.24 }
{ "name" : "Sun", "radius" : 695700, "mass" : 1.9885e+30, "sma" : 0, "gravity" : 274 }
{ "name" : "Mars", "radius" : 3396.2, "mass" : 6.4171e+23, "sma" : 227920000, "gravity" : 3.71 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Como podemos ver, conservaremos los campos especificados y realizaremos la transformación especificada.

## 2. Tema: `geoNear`Stage

### Transcripción

## 3. Tema: Cursor-like stages: Parte 1

### Transcripción

## 4. Tema: Cursor-like stages: Parte 2

### Transcripción

## 5. Tema: `$sample` Stage

### Transcripción

## 6. Laboratorio: Uso de Cursor-like Stages

## 7. Laboratorio: Uniendo todo junto


