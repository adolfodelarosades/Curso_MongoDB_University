# Capítulo 1: Aggregation Básica: `$match` y `$project`

### Lecciones

1. Tema: `$match`: Filtrado de documentos
2. Examen
3. Tema: Laboratorio - `$match`
4. Tareas
5. Tema: Dar forma a documentos con `$project`
6. Examen
7. Laboratorio: Cambio de la forma del documento con `$project`
8. Laboratorio - Campos Computados
9. Tema: Laboratorio opcional: Expresiones con `$project`
10. Examen

## 1. Tema: `$match`: Filtrado de Documentos

### Notas de lectura

[Página de documentación de `$match`](https://docs.mongodb.com/manual/reference/operator/aggregation/match/).

### Transcripción

Ahora que hemos discutido el concepto de qué son los pipelines, y le hemos dado una descripción general de la aggregation, la estructura y la sintaxis, es hora de que aprendamos sobre una de las etapas más importantes, `$match`.

El estado `$match` es vital para una pipeline exitoso.

Debería venir lo antes posible.

Y es libre de utilizar tantos cambios de `$match` como sea necesario en su pipeline.

Aquí hay una sintaxis básica para `$match`.

```sh
db.solarSystem.aggregate([{
  "$match": {  }
}])
```

Como es un operador de agregación, anteponemos un signo de dólar al nombre.

Nuevamente, `$match` se puede usar varias veces.

Y prácticamente cualquier otra etapa se puede usar después, con algunas excepciones que cubriremos más adelante en el curso.

Para comprender `$match` y el contexto de una aggregation pipeline, lo invito a pensar en `$match` como un filtro, en lugar de un find.

<img src="images/m121/c1/1-1-filter.png">

Configuramos los filtros en nuestra etapa `$match`.

Y a medida que fluyen los documentos, solo aquellos que cumplen con nuestros criterios se pasan más lejos en el pipeline.

Aquí, nuestra etapa `$match` solo dejará pasar círculos y estrellas.

`$match` utiliza la sintaxis de consulta de operación de lectura MongoDB estándar.

<img src="images/m121/c1/1-1-sintaxis.png">

Podemos realizar coincidencias basadas en la comparación, la lógica, los arrays y mucho más.

**Las únicas limitaciones son que no podemos usar el operador `$where`**.

**Y si queremos usar un operador `$test`, la etapa `$match` debe ser la primera etapa en una tubería**.

Si `$match` es la primera etapa, puede aprovechar los índices, lo que aumenta la velocidad de nuestras consultas.

Nuevamente, `$match` debería llegar temprano en nuestros pipelines.

Como recordatorio y como referencia, puede encontrar un enlace a esta página en las Notas de la Lección.

Lo alentamos a marcar esta página como referencia futura.

Aquí hay un ejemplo de `$match` en uso.

```sh
// $match all celestial bodies, not equal to Star
db.solarSystem.aggregate([{
  "$match": { "type": { "$ne": "Star" } }
}]).pretty()
```

Si le pregunto la siguiente agregación, que filtra la colección `solarSystem`, permitiendo solo documentos con tipos que no son iguales.

Entro a mongo:

```sh
mini-de-adolfo:~ adolfodelarosa$ mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/aggregations?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl -u m121 -p aggregations --norc
....

MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Ejecutamos el comando y puedo ver que obtengo los resultados que esperaba.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{
...   "$match": { "type": { "$ne": "Star" } }
... }]).pretty()
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d58"),
	"name" : "Uranus",
	"type" : "Gas giant",
	"orderFromSun" : 7,
	"radius" : {
		"value" : 25559,
		"units" : "km"
	},
	"mass" : {
		"value" : 8.6813e+25,
		"units" : "kg"
	},
	"sma" : {
		"value" : 2872460000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 8.87,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d52"),
	"name" : "Mercury",
	"type" : "Terrestrial planet",
	"orderFromSun" : 1,
	"radius" : {
		"value" : 4879,
		"units" : "km"
	},
	"mass" : {
		"value" : 3.3e+23,
		"units" : "kg"
	},
	"sma" : {
		"value" : 57910000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 3.24,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d54"),
	"name" : "Earth",
	"type" : "Terrestrial planet",
	"orderFromSun" : 3,
	"radius" : {
		"value" : 6378.137,
		"units" : "km"
	},
	"mass" : {
		"value" : 5.9723e+24,
		"units" : "kg"
	},
	"sma" : {
		"value" : 149600000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 9.8,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d56"),
	"name" : "Jupiter",
	"type" : "Gas giant",
	"orderFromSun" : 5,
	"radius" : {
		"value" : 71492,
		"units" : "km"
	},
	"mass" : {
		"value" : 1.89819e+27,
		"units" : "kg"
	},
	"sma" : {
		"value" : 778570000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 24.79,
		"units" : "m/s^2"
	},
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
	"radius" : {
		"value" : 6051.8,
		"units" : "km"
	},
	"mass" : {
		"value" : 4.8675e+24,
		"units" : "kg"
	},
	"sma" : {
		"value" : 108210000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 8.87,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d55"),
	"name" : "Mars",
	"type" : "Terrestrial planet",
	"orderFromSun" : 4,
	"radius" : {
		"value" : 3396.2,
		"units" : "km"
	},
	"mass" : {
		"value" : 6.4171e+23,
		"units" : "kg"
	},
	"sma" : {
		"value" : 227920000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 3.71,
		"units" : "m/s^2"
	},
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
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d57"),
	"name" : "Saturn",
	"type" : "Gas giant",
	"orderFromSun" : 6,
	"radius" : {
		"value" : 60268,
		"units" : "km"
	},
	"mass" : {
		"value" : 5.6834e+26,
		"units" : "kg"
	},
	"sma" : {
		"value" : 1433530000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 10.44,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d59"),
	"name" : "Neptune",
	"type" : "Gas giant",
	"orderFromSun" : 8,
	"radius" : {
		"value" : 24765,
		"units" : "km"
	},
	"mass" : {
		"value" : 1.02413e+26,
		"units" : "kg"
	},
	"sma" : {
		"value" : 4495060000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 11.15,
		"units" : "m/s^2"
	},
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
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```


Para mostrar que `$match` usa la sintaxis de consulta MongoDB, usemos find para ver si obtenemos resultados idénticos.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.find({ "type": { "$ne": "Star" } }).pretty();
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d58"),
	"name" : "Uranus",
	"type" : "Gas giant",
	"orderFromSun" : 7,
	"radius" : {
		"value" : 25559,
		"units" : "km"
	},
	"mass" : {
		"value" : 8.6813e+25,
		"units" : "kg"
	},
	"sma" : {
		"value" : 2872460000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 8.87,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d52"),
	"name" : "Mercury",
	"type" : "Terrestrial planet",
	"orderFromSun" : 1,
	"radius" : {
		"value" : 4879,
		"units" : "km"
	},
	"mass" : {
		"value" : 3.3e+23,
		"units" : "kg"
	},
	"sma" : {
		"value" : 57910000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 3.24,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d54"),
	"name" : "Earth",
	"type" : "Terrestrial planet",
	"orderFromSun" : 3,
	"radius" : {
		"value" : 6378.137,
		"units" : "km"
	},
	"mass" : {
		"value" : 5.9723e+24,
		"units" : "kg"
	},
	"sma" : {
		"value" : 149600000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 9.8,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d56"),
	"name" : "Jupiter",
	"type" : "Gas giant",
	"orderFromSun" : 5,
	"radius" : {
		"value" : 71492,
		"units" : "km"
	},
	"mass" : {
		"value" : 1.89819e+27,
		"units" : "kg"
	},
	"sma" : {
		"value" : 778570000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 24.79,
		"units" : "m/s^2"
	},
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
	"radius" : {
		"value" : 6051.8,
		"units" : "km"
	},
	"mass" : {
		"value" : 4.8675e+24,
		"units" : "kg"
	},
	"sma" : {
		"value" : 108210000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 8.87,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d55"),
	"name" : "Mars",
	"type" : "Terrestrial planet",
	"orderFromSun" : 4,
	"radius" : {
		"value" : 3396.2,
		"units" : "km"
	},
	"mass" : {
		"value" : 6.4171e+23,
		"units" : "kg"
	},
	"sma" : {
		"value" : 227920000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 3.71,
		"units" : "m/s^2"
	},
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
{
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d57"),
	"name" : "Saturn",
	"type" : "Gas giant",
	"orderFromSun" : 6,
	"radius" : {
		"value" : 60268,
		"units" : "km"
	},
	"mass" : {
		"value" : 5.6834e+26,
		"units" : "kg"
	},
	"sma" : {
		"value" : 1433530000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 10.44,
		"units" : "m/s^2"
	},
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
	"_id" : ObjectId("59a06674c8df9f3cd2ee7d59"),
	"name" : "Neptune",
	"type" : "Gas giant",
	"orderFromSun" : 8,
	"radius" : {
		"value" : 24765,
		"units" : "km"
	},
	"mass" : {
		"value" : 1.02413e+26,
		"units" : "kg"
	},
	"sma" : {
		"value" : 4495060000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 11.15,
		"units" : "m/s^2"
	},
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
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Los mismos resultados.

Observemos esto de otra manera.

Primero, contemos el número de documentos con tipos que no son iguales a la estrella.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.count({type: {"$ne": "Star"}});
8
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Debería ser ocho, ahora veamos cuántos documentos pasan por nuestra etapa de `$match`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{
...   "$match": { "type": { "$ne": "Star"} }
... }, {
...   "$count": "planets"
... }]);
{ "planets" : 8 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Voy a usar `count`, del que aprenderás más adelante.

Aquí `{ "planets" : 8 }`, podemos ver que ocho documentos pasan por nuestra agregación.

Lo siento Plutón.

Por último, `$match` no tiene ningún mecanismo de proyección.

<img src="images/m121/c1/1-1-projection.png">

Con `find`, podemos hacer algo como esto si queremos proyectar el campo no descrito.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.find({"name": "Earth"}, {"_id": 0}).pretty()
{
	"name" : "Earth",
	"type" : "Terrestrial planet",
	"orderFromSun" : 3,
	"radius" : {
		"value" : 6378.137,
		"units" : "km"
	},
	"mass" : {
		"value" : 5.9723e+24,
		"units" : "kg"
	},
	"sma" : {
		"value" : 149600000,
		"units" : "km"
	},
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
	"gravity" : {
		"value" : 9.8,
		"units" : "m/s^2"
	},
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
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```
Aunque esto puede parecer una limitación, pronto aprenderemos sobre una etapa poderosa que nos permite hacer esto y mucho, mucho más.

Y eso es todo por `$match`.

Nuevamente, le recomendamos que piense en `$match` como más un filtro que un find.

Una vez que los documentos están en una aggregation pipeline, y los estamos formando con nuevos campos y nuevos datos, usaremos `$match` en gran medida para seguir filtrando los documentos.

Algunas cosas clave para recordar.

<img src="images/m121/c1/1-1-resumen.png">

* Una etapa `$match` puede contener un operador de consulta `$text`, pero debe ser la primera etapa en la tubería.
* `$match` debe venir temprano en una aggregation pipeline, 
* No puede usar `$where` con `$match`, 
* `$match` usa la misma sintaxis de consulta que find.

## 2. Examen `$match`: Filtering documents

**Problem:**

Which of the following is/are true of the $match stage?

Check all answers that apply:

* It should come very early in an aggregation pipeline. :+1:

* It uses the familiar MongoDB query language. :+1:

* `$match` can use both query operators and aggregation expressions.

* `$match` can only filter documents on one field.

### See detailed answer

The correct answers are:

* It uses the familiar MongoDB query language.

`$match` uses the MongoDB query language query operators to express queries.

* It should come very early in an aggregation pipeline.

The earlier in the pipeline, the more efficient our pipelines will become. Not only because we will expression filters that reduce the number of documents to process, but also the fact that we might be using indexes withing the pipeline execution.

The remaining options are not correct.

## 3. Tema: Laboratorio - `$match`

Lab - $match

#### Download Course Materials

`m121/chapter1.zip`

Please connect to the class Atlas cluster through the mongo shell. The full command is:

```sh
mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/aggregations?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl -u m121 -p aggregations --norc
```

After connecting to the cluster, ensure you can see the movies collection by typing `show collections` and then run the command `db.movies.findOne()`. Take a moment to familiarize yourself with the schema.

Once you have familiarized yourself with the schema, continue to the next tab.

#### Ejecución de Comandos.

```sh
mini-de-adolfo:~ adolfodelarosa$ mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/aggregations?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl -u m121 -p aggregations --norc
2020-03-05T12:52:26.604+0100 W  CONTROL  [main] Option: ssl is deprecated. Please use tls instead.
MongoDB shell version v4.2.2
connecting to: mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/aggregations?authSource=admin&compressors=disabled&gssapiServiceName=mongodb&replicaSet=Cluster0-shard-0
2020-03-05T12:52:26.822+0100 I  NETWORK  [js] Starting new replica set monitor for Cluster0-shard-0/cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017
2020-03-05T12:52:26.822+0100 I  CONNPOOL [ReplicaSetMonitor-TaskExecutor] Connecting to cluster0-shard-00-01-jxeqq.mongodb.net:27017
2020-03-05T12:52:26.822+0100 I  CONNPOOL [ReplicaSetMonitor-TaskExecutor] Connecting to cluster0-shard-00-02-jxeqq.mongodb.net:27017
2020-03-05T12:52:26.822+0100 I  CONNPOOL [ReplicaSetMonitor-TaskExecutor] Connecting to cluster0-shard-00-00-jxeqq.mongodb.net:27017
2020-03-05T12:52:27.409+0100 I  NETWORK  [ReplicaSetMonitor-TaskExecutor] Confirmed replica set for Cluster0-shard-0 is Cluster0-shard-0/cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017
Implicit session: session { "id" : UUID("14051e4b-0ece-4584-9eb6-1b05c5ae0b56") }
MongoDB server version: 3.6.17
WARNING: shell and server versions do not match
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

MongoDB Enterprise Cluster0-shard-0:PRIMARY> show collections
air_airlines
air_alliances
air_routes
bronze_banking
child_reference
customers
employees
exoplanets
gold_banking
icecream_data
movies
nycFacilities
parent_reference
silver_banking
solarSystem
stocks
system.views
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.findOne()
{
	"_id" : ObjectId("573a1390f29313caabcd421c"),
	"title" : "A Turn of the Century Illusionist",
	"year" : 1899,
	"runtime" : 1,
	"cast" : [
		"Georges M�li�s"
	],
	"lastupdated" : "2015-08-29 00:21:21.547000000",
	"type" : "movie",
	"directors" : [
		"Georges M�li�s"
	],
	"imdb" : {
		"rating" : 6.6,
		"votes" : 580,
		"id" : 246
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Short"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.8,
			"numReviews" : 32
		},
		"lastUpdated" : ISODate("2015-08-20T18:46:44Z")
	}
}
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

## 4. Tareas

Lab - $match

**Problem:**

Help MongoDB pick a movie our next movie night! Based on employee polling, we've decided that potential movies must meet the following criteria.

* **`imdb.rating`** is at least 7
* **`genres`** does not contain "Crime" or "Horror"
* **`rated`** is either `"PG"` or `"G"`
* **`languages`** contains `"English"` and `"Japanese"`

Assign the aggregation to a variable named `pipeline`, like:

```sh
var pipeline = [ { $match: { ... } } ]
```

* As a hint, your aggregation should return 23 documents. You can verify this by typing 
   `db.movies.aggregate(pipeline).itcount()`   
* Load `validateLab1.js` into `mongo` shell

```sh
load('validateLab1.js')
```

* And run the `validateLab1` validation method

```sh
validateLab1(pipeline)
```

What is the answer?

Choose the best answer:

* 15

* 7

* 12

* 30


#### Mis comandos

```sh
db.movies.find({    
  $and:[     {"imdb.rating": {$gte: 7}},     
  {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},     
  {$or: [ {rated: "PG"}, {rated: "G"}]},     
  {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]  
},  
{_id:1, genres: 1, rated: 1, languages: 1, imdb: 1}  ).count()
23
```

```sh
db.movies.aggregate([{
  "$match": {    
    $and:[     {"imdb.rating": {$gte: 7}},     
    {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},     
    {$or: [ {rated: "PG"}, {rated: "G"}]},     
    {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]  
  }
}])
```

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> var pipeline = [{  
  "$match": {    
    $and:[     {"imdb.rating": {$gte: 7}},     
    {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},     
    {$or: [ {rated: "PG"}, {rated: "G"}]},     
    {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]  
 } }]
```

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({   $and:[     {"imdb.rating": {$gte: 7}},        {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},        {$or: [ {rated: "PG"}, {rated: "G"}]},        {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]   },   {_id:1, genres: 1, rated: 1, languages: 1, imdb: 1}  ).count()
23
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.aggregate([{   "$match": {       $and:[     {"imdb.rating": {$gte: 7}},        {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},        {$or: [ {rated: "PG"}, {rated: "G"}]},        {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]   } }])
....

MongoDB Enterprise Cluster0-shard-0:PRIMARY> var pipeline = [{  
...   "$match": {    
...     $and:[     {"imdb.rating": {$gte: 7}},     
...     {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},     
...     {$or: [ {rated: "PG"}, {rated: "G"}]},     
...     {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]  
...  } }]

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.aggregate(pipeline).itcount()
23

MongoDB Enterprise Cluster0-shard-0:PRIMARY> load('validateLab1.js')
true

MongoDB Enterprise Cluster0-shard-0:PRIMARY> validateLab1(pipeline)
Answer is 15
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

### See detailed answer

You can use nearly all of the familiar query operators in `$match`. We filter documents, retaining only those where the `imdb.rating` is 7 or more, `genres` does not include "Crime" or "Horror", the value for `rated` was "PG" or "G", and `languages` includes both "English" and "Japanese". .. code-block:

```sh
var pipeline = [
  {
    $match: {
      "imdb.rating": { $gte: 7 },
      genres: { $nin: [ "Crime", "Horror" ] } ,
      rated: { $in: ["PG", "G" ] },
      languages: { $all: [ "English", "Japanese" ] }
    }
  }
]
```

## 5. Tema: Dar Forma a Documentos con `$project`

### Notas de lectura

[$project](https://docs.mongodb.com/manual/reference/operator/aggregation/project/) página de documentación.

### Transcripción

<img src="images/m121/c1/5-titulo.png">

La siguiente etapa que aprenderemos es `$project`.

`$project`, como `$match`, es una etapa vital para comprender a fondo y tener éxito con el aggregation framework.

No piense en `$project` como la funcionalidad de proyección disponible con el find query operator.

Si bien es cierto, `$project` es mucho, mucho más.

No solo podemos eliminar y retener selectivamente campos, sino que podemos reasignar valores de campo existentes y derivar campos completamente nuevos.

Un método o función común disponible en muchos lenguajes de programación es `$map`.

Es una función de orden superior que aplica alguna transformación a una colección.

Si `$match` es como un método de filtro, `$project` es como `$map`.

Aquí está la sintaxis básica para `$project`.

```sh
db.solarSystem.aggregate([{ $project: {...} }])
```

Hemos agregado un signo de dólar para indicar que se trata de un operador de agregación, luego se abre con una llave y se cierra con una llave.

Entre estas dos llaves es donde usamos expresiones de agregación y realizamos la lógica de campo.

Más sobre eso pronto.

Aquí especificaríamos valores para eliminar y presentar, al igual que la funcionalidad de proyección disponible con el find query operator.

Esto especifica que deseamos eliminar el `_id` y presentar el campo `name`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{ "$project": { "_id": 0, "name": 1 } }]);
{ "name" : "Earth" }
{ "name" : "Neptune" }
{ "name" : "Uranus" }
{ "name" : "Saturn" }
{ "name" : "Jupiter" }
{ "name" : "Venus" }
{ "name" : "Mercury" }
{ "name" : "Sun" }
{ "name" : "Mars" }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Tenga en cuenta que, dado que hemos especificado un valor para presentar, debemos especificar cada valor que deseamos presentar.

Mantengamos también el campo `gravity` para que podamos ver alguna diferencia en los datos reales.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{ "$project": { "name": 1, "gravity": 1 } }]);
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d54"), "name" : "Earth", "gravity" : { "value" : 9.8, "units" : "m/s^2" } }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d59"), "name" : "Neptune", "gravity" : { "value" : 11.15, "units" : "m/s^2" } }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d58"), "name" : "Uranus", "gravity" : { "value" : 8.87, "units" : "m/s^2" } }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d57"), "name" : "Saturn", "gravity" : { "value" : 10.44, "units" : "m/s^2" } }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d56"), "name" : "Jupiter", "gravity" : { "value" : 24.79, "units" : "m/s^2" } }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d53"), "name" : "Venus", "gravity" : { "value" : 8.87, "units" : "m/s^2" } }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d52"), "name" : "Mercury", "gravity" : { "value" : 3.24, "units" : "m/s^2" } }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d51"), "name" : "Sun", "gravity" : { "value" : 274, "units" : "m/s^2" } }
{ "_id" : ObjectId("59a06674c8df9f3cd2ee7d55"), "name" : "Mars", "gravity" : { "value" : 3.71, "units" : "m/s^2" } }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y por supuesto, una excepción.

Aquí podemos decir que estamos obteniendo los campos `name` y `gravity`, pero también estamos obteniendo el campo `_id`.

El campo `_id` es el único campo que debemos eliminar explícitamente.

Todos los demás se eliminarán cuando especifiquemos al menos un campo para presentar.

Además, parece que quien reunió estos datos utilizó el sistema internacional de unidades `units`, así que también obtengamos su valor.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{ "$project": { "_id": 0, "name": 1, gravity.value: 1 } }]);
2020-03-06T12:34:09.679+0100 E  QUERY    [js] uncaught exception: SyntaxError: missing : after property id :
@(shell):1:70
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Un error.

Una cosa a tener en cuenta, una vez que comencemos a sumergirnos en la selección de documentos en subcampos, debemos rodear nuestros argumentos con comillas.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{ "$project": { "_id": 0, "name": 1, "gravity.value": 1 } }]);
{ "name" : "Earth", "gravity" : { "value" : 9.8 } }
{ "name" : "Neptune", "gravity" : { "value" : 11.15 } }
{ "name" : "Uranus", "gravity" : { "value" : 8.87 } }
{ "name" : "Saturn", "gravity" : { "value" : 10.44 } }
{ "name" : "Jupiter", "gravity" : { "value" : 24.79 } }
{ "name" : "Venus", "gravity" : { "value" : 8.87 } }
{ "name" : "Mercury", "gravity" : { "value" : 3.24 } }
{ "name" : "Sun", "gravity" : { "value" : 274 } }
{ "name" : "Mars", "gravity" : { "value" : 3.71 } }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Allí, los datos que queríamos.

`$project` ya está demostrando ser bastante útil, pero hasta ahora, parece ser idéntico a la proyección disponible con el find query operator.

Comencemos a explorar qué hace que `$project` sea tan poderoso.

En lugar de devolver un subdocumento con solo el campo de valor, asignemos directamente el valor al campo `gravity`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{"$project": { "_id": 0, "name": 1, "gravity": "$gravity.value" }}]);
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

Aquí podemos ver que de hecho estamos reasignando el campo `gravity` para que ahora contenga la información que estaba disponible en `gravity.value`.

**Estamos anteponiendo  `gravity.value` con un signo de dólar**.

Esta es una de las muchas expresiones de agregación, estamos dirigiendo el aggregation framework para visualizar y recuperar la información en el documento en `gravity.value`, o un field path expression.

Como se discutió en la estructura de agregación y la lección de sintaxis, esta es una de las formas en que hacemos referencia a los documentos para obtener información.

También podemos crear un nuevo campo llamado `surfaceGravity`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{"$project": { "_id": 0, "name": 1, "surfaceGravity": "$gravity.value" }}]);
{ "name" : "Earth", "surfaceGravity" : 9.8 }
{ "name" : "Neptune", "surfaceGravity" : 11.15 }
{ "name" : "Uranus", "surfaceGravity" : 8.87 }
{ "name" : "Saturn", "surfaceGravity" : 10.44 }
{ "name" : "Jupiter", "surfaceGravity" : 24.79 }
{ "name" : "Venus", "surfaceGravity" : 8.87 }
{ "name" : "Mercury", "surfaceGravity" : 3.24 }
{ "name" : "Sun", "surfaceGravity" : 274 }
{ "name" : "Mars", "surfaceGravity" : 3.71 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Esto no es solo renombrar el campo de `gravity`.

Está creando un campo completamente nuevo.

Muy poderoso.

Y utilizaremos esta funcionalidad mucho durante el curso.

Divirtámonos un poco y usemos el aggregation framework para calcular un valor.

Me gustaría ver cuál sería mi peso en cada planeta del sistema solar.

Voy a tener que usar una expresión para lograr esto.

```sh
{ $multiply: [ gravityRatio, weightOneEarth ] }
```

Cubriremos las expresiones con mucho mayor detalle en breve, pero voy a desglosar esto ya que es la primera vez que lo vemos, y la sintaxis puede sorprender a la gente con la guardia baja.

Yo peso unos 86 kilogramos.

Al observar nuestros resultados anteriores, 

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{"$project": { "_id": 0, "name": 1, "gravity": "$gravity.value" }}]);
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

parece que si divido la gravedad de un cuerpo por la gravedad de la Tierra y luego multiplico ese valor por mi peso, puedo averiguar cuánto pesaría en cada cuerpo principal.

Voy a tener que usar una expresión para lograr esto.

La primera expresión que voy a usar es la expresión aritmética `$multiply`.

```sh
{ $multiply: [ gravityRatio, weightOneEarth ] }
```

`$multiply` toma un array de valores y los multiplica.

Entonces sé que necesito multiplicar mi peso por la proporción de la gravedad de un planeta específico dividida por la gravedad de la Tierra.

Eso se verá más o menos así.

```sh
{ $multiply: [ gravityRatio, 86 ] }
```

Sé que mi peso es de aproximadamente 86 kilogramos, por lo que puedo codificar eso por ahora.

Para calcular la relación de gravedad, necesitaré usar la expresión aritmética `$divide`.

```sh
{ $divide: [ "$gravity.value", gravityOfEarth ] }
```

`$divide` toma un array de dos valores y divide el primero por el segundo.

Dentro de `$divide`, necesitaré hacer referencia a la información en el subcampo de valor dentro de la gravedad.

Veamos cómo se verá esto.

```sh
{ $divide: [ "$gravity.value", 9.8 ] }
```

Aquí `"$gravity.value"` estamos usando un field path expression para referirnos a la información dentro del documento, específicamente la información encontrada en el campo `value` dentro del campo `gravity`.

Sé que la gravedad de la Tierra es de alrededor de 9.8 metros por segundo por segundo, así que lo codificaré.

Poniendo todo junto, tenemos lo siguiente.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.solarSystem.aggregate([{"$project": { "_id": 0, "name": 1, "myWeight": { "$multiply": [ { "$divide": [ "$gravity.value", 9.8 ] }, 86 ] } }}]);
{ "name" : "Earth", "myWeight" : 86 }
{ "name" : "Neptune", "myWeight" : 97.8469387755102 }
{ "name" : "Uranus", "myWeight" : 77.83877551020407 }
{ "name" : "Saturn", "myWeight" : 91.61632653061224 }
{ "name" : "Jupiter", "myWeight" : 217.54489795918363 }
{ "name" : "Venus", "myWeight" : 77.83877551020407 }
{ "name" : "Mercury", "myWeight" : 28.432653061224492 }
{ "name" : "Sun", "myWeight" : 2404.4897959183672 }
{ "name" : "Mars", "myWeight" : 32.55714285714286 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Todo esto se asigna a un nuevo campo que creamos llamado `myWeight`.

Increíble.

Podemos ver que pesaría unos 32.5 kilogramos en Marte y 2.404 kilogramos en el Sol.

Estamos comenzando a ver el poder del proyecto $.

`$project` es una etapa poderosa del aggregation framework.

No solo podemos eliminar y retener campos, también podemos derivar nuevos campos y reasignar campos existentes.

`$project` se puede usar tantas veces como se desee dentro de una aggregation pipeline, y se debe usar agresivamente para recortar datos de documentos que no son necesarios para mantener el rendimiento de nuestras pipelines.

Algunas cosas clave para recordar.

Una vez que especificamos un campo para retener, debemos especificar todos los campos que queremos retener.

El campo `_id` es la única excepción a esto.

Más allá de simplemente eliminar y retener campos, `$project` agreguemos nuevos campos.

`$project` se puede usar tantas veces como sea necesario con una aggregation pipeline.

Y finalmente, `$project` se puede usar para reasignar valores a nombres de campo existentes y derivar campos completamente nuevos.

## 6. Examen Shaping documents with $project

**Problem:**

Which of the following statements are true of the $project stage?

Check all answers that apply:


* Once we specify a field to retain or perform some computation in a `$project` stage, we must specify all fields we wish to retain. The only exception to this is the `_id` field. :+1:

* Beyond simply removing and retaining fields, `$project` lets us add new fields. :+1:

* `$project` can only be used once within an Aggregation pipeline.

* `$project` cannot be used to assign new values to existing fields.

### See detailed answer

The correct answers are the following:

* Once we specify a field to retain or perform some computation in a `$project` stage, we must specify all fields we wish to retain. The only exception to this is the `_id` field.

`$project` implicitly removes all other fields once we have retained, reshaped, or computed a new field. The exception to this is the `_id` field, which we must explicitly remove.

* Beyond simply removing and retaining fields, `$project` lets us add new fields.

We can add new fields and reassign the values of existing ones, shaping the documents into different datastructures and computing values using expressions.

The remaining options are incorrect.


## 7. Laboratorio: Cambio de la forma del documento con `$project`

Lab - Changing Document Shape with `$project`

**Problem:**

Our first movie night was a success. Unfortunately, our ISP called to let us know we're close to our bandwidth quota, but we need another movie recommendation!

Using the same `$match` stage from the previous lab, add a `$project` stage to only display the the title and film rating (`title` and `rated` fields).

* Assign the results to a variable called `pipeline`.

```sh
var pipeline = [{ $match: {. . .} }, { $project: { . . . } }]
```

* Load `validateLab2.js` which was included in the same handout as `validateLab1.js` and execute `validateLab2(pipeline)`?

```sh
load('./validateLab2.js')
```

* And run the `validateLab2` validation method

```sh
validateLab2(pipeline)
```

What is the answer?

Choose the best answer:

* 4

* 30

* 7

* 15


## 8. Laboratorio - Campos Computados

Lab - Computing Fields

**Problem:**

Our movies dataset has a lot of different documents, some with more convoluted titles than others. If we'd like to analyze our collection to find movie titles that are composed of only one word, we **could** fetch all the movies in the dataset and do some processing in a client application, but the Aggregation Framework allows us to do this on the server!

Using the Aggregation Framework, find a count of the number of movies that have a title composed of one word. To clarify, "Cinderella" and "3-25" should count, where as "Cast Away" would not.

Make sure you look into the [$split String expression](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/#string-expressions) and the [$size Array expression](https://docs.mongodb.com/manual/meta/aggregation-quick-reference/#array-expressions)

To get the count, you can append `itcount()` to the end of your pipeline

```sh
db.movies.aggregate([...]).itcount()
```

Choose the best answer:

* 144

* 8068

* 9447

* 12373

## 9. Tema: Laboratorio opcional: Expresiones con `$project`

Optional Lab - Expressions with $project

This lab will have you work with data within arrays, a common operation.

Specifically, one of the arrays you'll work with is `writers`, from the `movies` collection.

There are times when we want to make sure that the field is an array, and that it is not empty. We can do this within `$match`

```sh
{ $match: { writers: { $elemMatch: { $exists: true } } }
```

However, the entries within `writers` presents another problem. A good amount of entries in `writers` look something like the following, where the writer is attributed with their specific contribution

```sh
"writers" : [ "Vincenzo Cerami (story)", "Roberto Benigni (story)" ]
```

But the writer also appears in the `cast` array as "Roberto Benigni"!

Give it a look with the following query

```sh
db.movies.findOne({title: "Life Is Beautiful"}, { _id: 0, cast: 1, writers: 1})
```

This presents a problem, since comparing `"Roberto Benigni"` to `"Roberto Benigni (story)"` will definitely result in a difference.

Thankfully there is a powerful expression to help us, `$map`. `$map` lets us iterate over an array, element by element, performing some transformation on each element. The result of that transformation will be returned in the same place as the original element.

Within `$map`, the argument to `input` can be any expression as long as it resolves to an array. The argument to `as` is the name of the variable we want to use to refer to each element of the array when performing whatever logic we want. The field `as` is optional, and if omitted each element must be referred to as `$$this::` The argument to `in` is the expression that is applied to each element of the `input` array, referenced with the variable name specified in `as`, and prepending two dollar signs:

```sh
writers: {
  $map: {
    input: "$writers",
    as: "writer",
    in: "$$writer"
```

in is where the work is performed. Here, we use the `$arrayElemAt` expression, which takes two arguments, the array and the index of the element we want. We use the `$split` expression, splitting the values on `" ("`.

If the string did not contain the pattern specified, the only modification is it is wrapped in an array, so `$arrayElemAt` will always work

```sh
writers: {
  $map: {
    input: "$writers",
    as: "writer",
    in: {
      $arrayElemAt: [
        {
          $split: [ "$$writer", " (" ]
        },
        0
      ]
    }
  }
}
```

## 10. Examen

Optional Lab - Expressions with $project

**Problem:**

Let's find how many movies in our **movies** collection are a "labor of love", where the same person appears in `cast`, `directors`, and `writers`

Note that you may have a dataset that has duplicate entries for some films. Don't worry if you count them few times, meaning you should not try to find those duplicates.

To get a count after you have defined your pipeline, there are two simple methods.

```sh
// add the $count stage to the end of your pipeline
// you will learn about this stage shortly!
db.movies.aggregate([
  {$stage1},
  {$stage2},
  ...$stageN,
  { $count: "labors of love" }
])

// or use itcount()
db.movies.aggregate([
  {$stage1},
  {$stage2},
  {...$stageN}
]).itcount()
```

How many movies are "labors of love"?

Choose the best answer:

* 1597

* 1263

* 1259

* 1595
