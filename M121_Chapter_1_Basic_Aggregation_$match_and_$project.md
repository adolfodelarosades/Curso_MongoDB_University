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

* 15 :+1:

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

Estamos comenzando a ver el poder del `$project`.

`$project` es una etapa poderosa del aggregation framework.

No solo podemos eliminar y retener campos, también podemos derivar nuevos campos y reasignar campos existentes.

`$project` se puede usar tantas veces como se desee dentro de una aggregation pipeline, y se debe usar agresivamente para recortar datos de documentos que no son necesarios para mantener el rendimiento de nuestras pipelines.

Algunas cosas clave para recordar.

<img src="images/m121/c1/5-resumen.png">

* Una vez que especificamos un campo a presentar, debemos especificar todos los campos que queremos presentar.

* El campo `_id` es la única excepción a esto.

* Más allá de simplemente eliminar y presentar campos, `$project` agreguemos nuevos campos.

* `$project` se puede usar tantas veces como sea necesario con una aggregation pipeline.

* Y finalmente, `$project` se puede usar para reasignar valores a nombres de campo existentes y derivar campos completamente nuevos.

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

* 15 :+1:

#### Mis comandos

```sh
db.movies.aggregate([{
  "$match": {    
    $and:[     {"imdb.rating": {$gte: 7}},     
    {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},     
    {$or: [ {rated: "PG"}, {rated: "G"}]},     
    {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]  
  }
},
{ 
  "$project": { _id: 0, title: 1, rated: 1 }
}
])

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.aggregate([{
...   "$match": {    
...     $and:[     {"imdb.rating": {$gte: 7}},     
...     {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},     
...     {$or: [ {rated: "PG"}, {rated: "G"}]},     
...     {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]  
...   }
... },
... { 
...   "$project": { _id: 0, title: 1, rated: 1 }
... }
... ])
{ "title" : "Those Magnificent Men in Their Flying Machines or How I Flew from London to Paris in 25 hours 11 minutes", "rated" : "G" }
{ "title" : "Red Sun", "rated" : "PG" }
{ "title" : "Babies", "rated" : "PG" }
{ "title" : "The Karate Kid", "rated" : "PG" }
{ "title" : "Dragon Ball Z: Tree of Might", "rated" : "PG" }
{ "title" : "Cars", "rated" : "G" }
{ "title" : "Jack and the Beanstalk", "rated" : "G" }
{ "title" : "The Transformers: The Movie", "rated" : "PG" }
{ "title" : "Defending Your Life", "rated" : "PG" }
{ "title" : "The Cat Returns", "rated" : "G" }
{ "title" : "Hell in the Pacific", "rated" : "G" }
{ "title" : "The Goodbye Girl", "rated" : "PG" }
{ "title" : "Tora! Tora! Tora!", "rated" : "G" }
{ "title" : "Local Hero", "rated" : "PG" }
{ "title" : "Summer Wars", "rated" : "PG" }
{ "title" : "The Secret World of Arrietty", "rated" : "G" }
{ "title" : "Empire of the Sun", "rated" : "PG" }
{ "title" : "Dreams", "rated" : "PG" }
{ "title" : "Millennium Actress", "rated" : "PG" }
{ "title" : "Whisper of the Heart", "rated" : "G" }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

MongoDB Enterprise Cluster0-shard-0:PRIMARY> var pipeline = [{
...   "$match": {    
...     $and:[     {"imdb.rating": {$gte: 7}},     
...     {$and: [ {genres: {$ne: "Crime" }},   {genres:{$ne: "Horror"}}]},     
...     {$or: [ {rated: "PG"}, {rated: "G"}]},     
...     {$and: [ {languages: "English"}, {languages: "Japanese"}]}         ]  
...   }
... },
... { 
...   "$project": { _id: 0, title: 1, rated: 1 }
... }]

MongoDB Enterprise Cluster0-shard-0:PRIMARY> load('./validateLab2.js')
true

MongoDB Enterprise Cluster0-shard-0:PRIMARY> validateLab2(pipeline)
Answer is 15
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

### See detailed answer

Remember that when using `$project` to be selective on which fields you pass further, the only field you must specify to remove is `_id`. When you specify a field to retain `(title: 1)`, `$project` assumes that all other fields you haven't specified to retain should be removed.

```sh
var pipeline = [
  {
    $match: {
      "imdb.rating": { $gte: 7 },
      genres: { $nin: [ "Crime", "Horror" ] } ,
      rated: { $in: ["PG", "G" ] },
      languages: { $all: [ "English", "Japanese" ] }
    }
  },
  {
    $project: { _id: 0, title: 1, "rated": 1 }
  }
]
```

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

* 8068 :+1:

* 9447

* 12373

### Código Ejecutado

```sh
db.movies.aggregate([ 
	   { "$project": {"_id": 0, "title": 1, "words": { $size: { $split: [ "$title", " " ] } }     } },   
	   {  "$match": { "words": {$lt : 2 } } }   
	]).itcount()
	
	

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.aggregate([ { "$project": {"_id": 0, "title": 1, "words": { $size: { $split: [ "$title", " " ] }  }        } },   {  "$match": { "words": {$lt : 2 } } }   ]).itcount()
8068
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

### See detailed answer

```sh
db.movies.aggregate([
  {
    $match: {
      title: {
        $type: "string"
      }
    }
  },
  {
    $project: {
      title: { $split: ["$title", " "] },
      _id: 0
    }
  },
  {
    $match: {
      title: { $size: 1 }
    }
  }
]).itcount()
```

We begin with a `$match` stage, ensuring that we only allow movies where the `title` is a string

```sh
db.movies.aggregate([
  {
    $match: {
      title: {
        $type: "string"
      }
    }
  },
```

Next is our `$project` stage, splitting the title on spaces. This creates an array of strings

```sh
{
  $project: {
    title: { $split: ["$title", " "] },
    _id: 0
  }
},
```

We use another `$match` stage to filter down to documents that only have one element in the newly computed `title` field, and use `itcount()` to get a count

```sh
  {
    $match: {
      title: { $size: 1 }
    }
  }
]).itcount()
```

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

### Ejecutando los Comandos

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.aggregate([ 
... { $match: { writers: { $elemMatch: { $exists: true } } } }
...  ]).pretty()
{
	"_id" : ObjectId("573a1390f29313caabcd4cf1"),
	"title" : "Ingeborg Holm",
	"year" : 1913,
	"runtime" : 96,
	"released" : ISODate("1913-10-27T00:00:00Z"),
	"cast" : [
		"Hilda Borgstr�m",
		"Aron Lindgren",
		"Erik Lindholm",
		"Georg Gr�nroos"
	],
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTI5MjYzMTY3Ml5BMl5BanBnXkFtZTcwMzY1NDE2Mw@@._V1_SX300.jpg",
	"plot" : "Ingeborg Holm's husband opens up a grocery store and life is on the sunny side for them and their three children. But her husband becomes sick and dies. Ingeborg tries to keep the store, ...",
	"fullplot" : "Ingeborg Holm's husband opens up a grocery store and life is on the sunny side for them and their three children. But her husband becomes sick and dies. Ingeborg tries to keep the store, but because of the lazy, wasteful staff she eventually has to close it. With no money left, she has to move to the poor-house and she is separated from her children. Her children are taken care of by foster-parents, but Ingeborg simply has to get out of the poor-house to see them again...",
	"lastupdated" : "2015-08-25 00:11:47.743000000",
	"type" : "movie",
	"directors" : [
		"Victor Sj�str�m"
	],
	"writers" : [
		"Nils Krok (play)",
		"Victor Sj�str�m"
	],
	"imdb" : {
		"rating" : 7,
		"votes" : 493,
		"id" : 3014
	},
	"countries" : [
		"Sweden"
	],
	"genres" : [
		"Drama"
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4386"),
	"title" : "The Story of the Kelly Gang",
	"year" : 1906,
	"runtime" : 70,
	"released" : ISODate("1906-12-26T00:00:00Z"),
	"cast" : [
		"Elizabeth Tait",
		"John Tait",
		"Norman Campbell",
		"Bella Cola"
	],
	"plot" : "True story of notorious Australian outlaw Ned Kelly (1855-80).",
	"fullplot" : "True story of notorious Australian outlaw Ned Kelly (1855-80).",
	"lastupdated" : "2015-08-29 00:29:52.170000000",
	"type" : "movie",
	"directors" : [
		"Charles Tait"
	],
	"writers" : [
		"Charles Tait"
	],
	"imdb" : {
		"rating" : 6.3,
		"votes" : 285,
		"id" : 574
	},
	"countries" : [
		"Australia"
	],
	"genres" : [
		"Biography",
		"Crime",
		"Drama"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.5,
			"numReviews" : 2
		},
		"lastUpdated" : ISODate("2015-08-14T18:51:10Z")
	},
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Pypar",
			"email" : "josef_altin@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd4386"),
			"text" : "Cumque ut officiis commodi odio veritatis expedita. Assumenda magni sequi quo occaecati minima.\nTemporibus vero quia ipsam molestias illum. Non nobis deserunt atque voluptatem ex consectetur.",
			"date" : ISODate("2015-08-16T14:53:56Z")
		},
		{
			"name" : "Cheryl Smith",
			"email" : "cheryl_smith@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4386"),
			"text" : "Sequi non corrupti quam. Incidunt similique nesciunt nihil impedit dolore nobis totam numquam. Quasi quisquam non laboriosam dolorem. Asperiores ipsum id nobis enim consectetur.",
			"date" : ISODate("2015-07-30T23:27:29Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd45ec"),
	"title" : "Frankenstein",
	"year" : 1910,
	"runtime" : 16,
	"released" : ISODate("1910-03-18T00:00:00Z"),
	"cast" : [
		"Mary Fuller",
		"Charles Ogle",
		"Augustus Phillips"
	],
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTg2NjMyMjgxOF5BMl5BanBnXkFtZTgwOTMyNzM2MzE@._V1_SX300.jpg",
	"plot" : "Frankenstein, a young medical student, trying to create the perfect human being, instead creates a misshapen monster. Made ill by what he has done, Frankenstein is comforted by his fianc�e ...",
	"fullplot" : "Frankenstein, a young medical student, trying to create the perfect human being, instead creates a misshapen monster. Made ill by what he has done, Frankenstein is comforted by his fianc�e but on his wedding night he is visited by the monster. A fight ensues but the monster, seeing himself in a mirror, is horrified and runs away. He later returns, entering the new bride's room, and finds her alone.",
	"lastupdated" : "2015-08-28 00:56:44.097000000",
	"type" : "movie",
	"languages" : [
		"English"
	],
	"directors" : [
		"J. Searle Dawley"
	],
	"writers" : [
		"Mary Shelley (novel)",
		"J. Searle Dawley"
	],
	"imdb" : {
		"rating" : 6.5,
		"votes" : 2149,
		"id" : 1223
	},
	"countries" : [
		"USA"
	],
	"rated" : "UNRATED",
	"genres" : [
		"Short",
		"Fantasy",
		"Horror"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.4,
			"numReviews" : 541,
			"meter" : 57
		},
		"lastUpdated" : ISODate("2015-08-26T18:01:25Z")
	},
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Eddie Hunter",
			"email" : "eddie_hunter@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd45ec"),
			"text" : "Et quo alias repudiandae quis expedita placeat est aut. Facere molestias a quis sed consequuntur omnis. Unde dignissimos quod hic ex ipsa voluptatem qui.",
			"date" : ISODate("2010-01-19T00:42:34Z")
		},
		{
			"name" : "Bowen Marsh",
			"email" : "michael_condron@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd45ec"),
			"text" : "Delectus ut accusantium nihil facilis. Consequatur possimus incidunt aut blanditiis distinctio blanditiis est. Ratione dicta laboriosam animi magni adipisci a pariatur.",
			"date" : ISODate("1986-01-16T13:13:52Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd458c"),
	"title" : "The Woman Always Pays",
	"year" : 1910,
	"runtime" : 38,
	"released" : ISODate("1912-04-18T00:00:00Z"),
	"cast" : [
		"Asta Nielsen",
		"Robert Dinesen",
		"Poul Reumert",
		"Hans Neergaard"
	],
	"plot" : "At a tramcar in Copenhagen the piano teacher Magda Vang meets the young man Knud Svane, who falls in love with her. She is invited to spend the summer with him and his parents at the ...",
	"fullplot" : "At a tramcar in Copenhagen the piano teacher Magda Vang meets the young man Knud Svane, who falls in love with her. She is invited to spend the summer with him and his parents at the vicarage in Gjerslev. Outside the vicarage a circus troupe passes by, and Magda is saluted by the performer Rudolph Stern. In the night Rudolph climbs a ladder to Magda's bedroom. She tries to flee his advances, but after a hot kiss she surrenders, and runs away with him. Magda is hired as a dancer with Rudolph at the Empire Variet�. When Rudolph fondles a ballet dancer Magda gets furious, and starts a fight in front of the audience. Magda and Rudolph are fired. To earn some money Rudolph forces Magda to play the piano in a band at a garden restaurant. Knud turns up and recognizes her. Incognito he asks her for a private meeting. Magda thinks she is asked to sell her body and refuses, but Rudolph forces her to go. When Rudloph after a while interrupts and finds Magda with Knud, he gets furious and starts to beat her. During the turmoil she grabs a knife and stabs Rudolph in his chest. In her despair she clings to his dead body, and has to be taken away by force.",
	"lastupdated" : "2015-08-29 00:44:49.343000000",
	"type" : "movie",
	"languages" : [
		"Danish"
	],
	"directors" : [
		"Urban Gad"
	],
	"writers" : [
		"Urban Gad"
	],
	"imdb" : {
		"rating" : 6.6,
		"votes" : 429,
		"id" : 1105
	},
	"countries" : [
		"Denmark"
	],
	"genres" : [
		"Short",
		"Drama"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.3,
			"numReviews" : 55,
			"meter" : 50
		},
		"lastUpdated" : ISODate("2015-07-14T18:49:06Z")
	},
	"num_mflix_comments" : 3,
	"comments" : [
		{
			"name" : "Shae",
			"email" : "sibel_kekilli@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd458c"),
			"text" : "Voluptatibus in voluptatem itaque consequuntur explicabo. Maxime deserunt similique totam ab. Corporis nostrum et voluptate. Aliquam enim distinctio excepturi accusantium.",
			"date" : ISODate("2000-10-19T00:54:54Z")
		},
		{
			"name" : "Kyle Cooper",
			"email" : "kyle_cooper@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd458c"),
			"text" : "Ea dicta laboriosam sit alias. Quos molestiae explicabo minus quibusdam officiis.",
			"date" : ISODate("1991-04-25T14:57:24Z")
		},
		{
			"name" : "Petyr Baelish",
			"email" : "aidan_gillen@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd458c"),
			"text" : "Eveniet rem eligendi eos minima. Aliquam eos laboriosam laudantium ipsum minus consequatur iusto.",
			"date" : ISODate("1974-11-20T06:29:01Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4961"),
	"title" : "Cleopatra",
	"year" : 1912,
	"runtime" : 88,
	"released" : ISODate("1912-11-13T00:00:00Z"),
	"cast" : [
		"Helen Gardner",
		"Pearl Sindelar",
		"Miss Fielding",
		"Miss Robson"
	],
	"plot" : "The fabled queen of Egypt's affair with Roman general Marc Antony is ulimately disastrous for both of them.",
	"fullplot" : "When she discovers that a slave named Pharon professes his love for her, Cleopatra makes a bargain with him: she will give him ten days of \"love,\" at the end of which he is to commit suicide. He agrees, although the queen's handmaiden Iras, in love with the slave, isn't happy with the arrangement. Later when Cleopatra is seducing Marc Antony, her relationship with Pharon is used against her, but with little effect. She allies herself with Antony against Octavius, participates in a brief war, then meets her end rather than be subjected to Roman rule.",
	"lastupdated" : "2015-08-26 00:15:57.057000000",
	"type" : "movie",
	"languages" : [
		"English"
	],
	"directors" : [
		"Charles L. Gaskill"
	],
	"writers" : [
		"Victorien Sardou (adapted from the play by)"
	],
	"imdb" : {
		"rating" : 5.1,
		"votes" : 291,
		"id" : 2101
	},
	"countries" : [
		"USA"
	],
	"rated" : "UNRATED",
	"genres" : [
		"Drama",
		"History"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.7,
			"numReviews" : 13
		},
		"lastUpdated" : ISODate("2015-08-23T18:50:13Z")
	}
}
{
	"_id" : ObjectId("573a1390f29313caabcd418c"),
	"title" : "The House of the Devil",
	"year" : 1896,
	"runtime" : 3,
	"cast" : [
		"Jeanne d'Alcy",
		"Georges M�li�s"
	],
	"plot" : "A bat flies into an ancient castle and transforms itself into Mephistopheles himself. Producing a cauldron, Mephistopheles conjures up a young girl and various supernatural creatures, one ...",
	"fullplot" : "A bat flies into an ancient castle and transforms itself into Mephistopheles himself. Producing a cauldron, Mephistopheles conjures up a young girl and various supernatural creatures, one of which brandishes a crucifix in an effort to force the devil-vampire to vanish.",
	"lastupdated" : "2015-08-26 00:06:16.697000000",
	"type" : "movie",
	"directors" : [
		"Georges M�li�s"
	],
	"writers" : [
		"Georges M�li�s"
	],
	"imdb" : {
		"rating" : 6.8,
		"votes" : 1135,
		"id" : 91
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Short",
		"Horror"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 5,
			"numReviews" : 23
		},
		"lastUpdated" : ISODate("2015-06-02T19:48:08Z")
	},
	"num_mflix_comments" : 1,
	"comments" : [
		{
			"name" : "Oscar Sanchez",
			"email" : "oscar_sanchez@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd418c"),
			"text" : "Non repellat atque in ipsa accusantium. Assumenda modi magni quis.\nRecusandae recusandae dicta repellat ad reprehenderit mollitia quam. Itaque voluptate asperiores quia alias.",
			"date" : ISODate("2017-02-11T07:00:52Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd41b1"),
	"title" : "The Bewitched Inn",
	"year" : 1897,
	"runtime" : 2,
	"cast" : [
		"Georges M�li�s"
	],
	"plot" : "A weary traveler stops at an inn along the way to get a good night's sleep, but his rest is interrupted by odd happenings when he gets to his room--beds vanishing and re-appearing, candles ...",
	"fullplot" : "A weary traveler stops at an inn along the way to get a good night's sleep, but his rest is interrupted by odd happenings when he gets to his room--beds vanishing and re-appearing, candles exploding, pants flying through the air and his shoes walking away by themselves.",
	"lastupdated" : "2015-08-29 00:17:59.703000000",
	"type" : "movie",
	"directors" : [
		"Georges M�li�s"
	],
	"writers" : [
		"Georges M�li�s"
	],
	"imdb" : {
		"rating" : 6.5,
		"votes" : 329,
		"id" : 138
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Short",
		"Comedy"
	],
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Steven Mcdonald",
			"email" : "steven_mcdonald@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd41b1"),
			"text" : "Corrupti perspiciatis aspernatur unde assumenda. Repellat velit animi at. Distinctio amet enim voluptas.",
			"date" : ISODate("1994-09-20T01:47:13Z")
		},
		{
			"name" : "Meera Reed",
			"email" : "ellie_kendrick@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd41b1"),
			"text" : "Ratione deleniti delectus quae deleniti adipisci. Sit nam error temporibus velit. Eum aperiam ratione consectetur illum rem eveniet illo.",
			"date" : ISODate("1981-06-24T07:48:10Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4318"),
	"title" : "Rescued by Rover",
	"year" : 1905,
	"runtime" : 7,
	"released" : ISODate("1905-08-19T00:00:00Z"),
	"cast" : [
		"Blair",
		"May Clark",
		"Barbara Hepworth",
		"Cecil M. Hepworth"
	],
	"plot" : "A dog leads its master to his kidnapped baby.",
	"fullplot" : "Rescued by Rover is a film about a dog. Not only is it a film about a dog it is also a film about the kidnaping of a young baby by an old woman. With these two classical Hollywood ingredients and some stunning dialogue, (It's silent!), Rescued by Rover really hits a nerve. The repeated shots provide some interest in what, surprisingly, is an entertaining example of early cinema, canine heroics and a man wearing mascara.",
	"lastupdated" : "2015-08-29 00:28:01.313000000",
	"type" : "movie",
	"directors" : [
		"Lewin Fitzhamon",
		"Cecil M. Hepworth"
	],
	"writers" : [
		"Mrs. Hepworth (story)"
	],
	"imdb" : {
		"rating" : 6.7,
		"votes" : 666,
		"id" : 498
	},
	"countries" : [
		"UK"
	],
	"genres" : [
		"Short",
		"Drama",
		"Family"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.9,
			"numReviews" : 74,
			"meter" : 94
		},
		"lastUpdated" : ISODate("2015-07-03T19:29:07Z")
	},
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Douglas Williams",
			"email" : "douglas_williams@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4318"),
			"text" : "Veritatis dolorum debitis tempore eos possimus animi. Repellat accusantium molestias quibusdam nemo ipsam. Nemo quo consequuntur reiciendis nam perspiciatis. Laborum eveniet harum ex debitis rem.",
			"date" : ISODate("2010-01-05T11:19:01Z")
		},
		{
			"name" : "Tonya Johnson",
			"email" : "tonya_johnson@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4318"),
			"text" : "Eveniet labore velit praesentium nemo doloribus tempore sint explicabo. Molestiae dicta fuga aperiam. Eligendi magni id modi veniam molestiae. Pariatur sit doloremque pariatur id dolorem odit.",
			"date" : ISODate("1984-05-18T16:01:44Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4217"),
	"title" : "Cinderella",
	"year" : 1899,
	"runtime" : 6,
	"released" : ISODate("1899-12-25T00:00:00Z"),
	"cast" : [
		"Barral",
		"Bleuette Bernon",
		"Carmely",
		"Jeanne d'Alcy"
	],
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTgwMDY1MzM1NV5BMl5BanBnXkFtZTgwMjM1MzUwMzE@._V1_SX300.jpg",
	"plot" : "A fairy godmother magically turns Cinderella's rags to a beautiful dress, and a pumpkin into a coach. Cinderella goes to the ball, where she meets the Prince - but will she remember to leave before the magic runs out?",
	"fullplot" : "A fairy godmother magically turns Cinderella's rags to a beautiful dress, and a pumpkin into a coach. Cinderella goes to the ball, where she meets the Prince - but will she remember to leave before the magic runs out?",
	"lastupdated" : "2015-08-29 00:20:56.217000000",
	"type" : "movie",
	"directors" : [
		"Georges M�li�s"
	],
	"writers" : [
		"Charles Perrault (story)"
	],
	"imdb" : {
		"rating" : 6.6,
		"votes" : 586,
		"id" : 230
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Drama",
		"Short"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.7,
			"numReviews" : 79
		},
		"lastUpdated" : ISODate("2015-09-14T18:42:47Z")
	},
	"num_mflix_comments" : 1,
	"comments" : [
		{
			"name" : "Osha",
			"email" : "natalia_tena@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd4217"),
			"text" : "Odit fugit ipsa repellat laudantium labore quae sunt. Asperiores quasi sint animi cum illum. Pariatur eum cupiditate tempora voluptate magnam velit.",
			"date" : ISODate("1999-09-26T14:56:12Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4218"),
	"title" : "The Sign of the Cross",
	"year" : 1899,
	"runtime" : 3,
	"released" : ISODate("1900-06-30T00:00:00Z"),
	"cast" : [
		"Georges M�li�s"
	],
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMjM1NTQyNDQxOV5BMl5BanBnXkFtZTgwMTQ1MzUwMzE@._V1_SX300.jpg",
	"lastupdated" : "2015-08-29 00:21:15.827000000",
	"type" : "movie",
	"directors" : [
		"Georges M�li�s"
	],
	"writers" : [
		"Georges M�li�s"
	],
	"imdb" : {
		"rating" : 6.3,
		"votes" : 388,
		"id" : 242
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Short",
		"Fantasy"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.4,
			"numReviews" : 33
		},
		"lastUpdated" : ISODate("2015-08-15T18:50:24Z")
	},
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Amy Wolfe",
			"email" : "amy_wolfe@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4218"),
			"text" : "Commodi vitae dolorum quaerat provident explicabo. Tempora dolor blanditiis dolorum. Dignissimos in in quasi voluptatibus ex nisi quos ipsam. Facere facilis eos doloribus debitis beatae cumque.",
			"date" : ISODate("2007-04-21T23:04:03Z")
		},
		{
			"name" : "Daario Naharis",
			"email" : "michiel_huisman@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd4218"),
			"text" : "Quasi voluptatem ea qui ipsam repellendus qui numquam nesciunt. Neque molestiae tempore temporibus distinctio ipsam natus ipsum. Qui corrupti unde at.",
			"date" : ISODate("1994-10-22T12:11:01Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd46e7"),
	"title" : "The Wonderful Wizard of Oz",
	"year" : 1910,
	"runtime" : 13,
	"released" : ISODate("1910-03-24T00:00:00Z"),
	"cast" : [
		"Bebe Daniels",
		"Hobart Bosworth",
		"Eugenie Besserer",
		"Robert Z. Leonard"
	],
	"plot" : "An early version of the classic, based more on the 1902 stage musical than on the original novel.",
	"fullplot" : "Chased off by the antics of Hank the Mule, Dorothy ends up in her cornfield, where she realizes her family's Scarecrow is alive. She helps him down and he takes a tumble on the turnstyle. A cyclone soon arrives and leaves Dorothy, Scarecrow, Toto and Hank spinning around on a haystack, with Imogene the Cow flying soon after. Soon after their arrival, the Wizard of Oz issues a public decree that he is a humbug, to make sure no one ever finds out. Glinda pops up out of the background and transforms Toto into a man in a bulldog suit to serve as a better protector for Dorothy. Then they encounter the Tin Woodman, the Cowardly Lion, and Eureka. Nevertheless, she is captured by Momba, the Wicked Witch of the West (suggesting Baum thought the other witches were Mombe, Mombo, and Mombu, in keeping with the council in _Queen Zixi of Ix_) and her flying lizards and soldiers. Dorothy defeats Momba, and they arrive at the Emerald City just in time for the Wizard's going away party.",
	"lastupdated" : "2015-08-29 00:54:49.313000000",
	"type" : "movie",
	"languages" : [
		"English"
	],
	"directors" : [
		"Otis Turner"
	],
	"writers" : [
		"L. Frank Baum (novel)",
		"Otis Turner (scenario)"
	],
	"imdb" : {
		"rating" : 5.7,
		"votes" : 1030,
		"id" : 1463
	},
	"countries" : [
		"USA"
	],
	"rated" : "NOT RATED",
	"genres" : [
		"Adventure",
		"Fantasy",
		"Short"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 2.9,
			"numReviews" : 124,
			"meter" : 24
		},
		"lastUpdated" : ISODate("2015-03-30T19:17:26Z")
	},
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Angela Smith",
			"email" : "angela_smith@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd46e7"),
			"text" : "Nihil quasi nostrum sapiente sit corrupti alias adipisci maiores. Quos libero quaerat temporibus tempora minus quaerat vitae aut. Earum animi aliquid eos dolor.",
			"date" : ISODate("2014-12-20T05:49:56Z")
		},
		{
			"name" : "Amy Wolfe",
			"email" : "amy_wolfe@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd46e7"),
			"text" : "Rem ex est ad sunt veniam blanditiis. Ad asperiores numquam nihil dicta necessitatibus. Vitae minus beatae magnam impedit voluptatibus quam molestiae.",
			"date" : ISODate("1985-07-02T03:36:20Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4962"),
	"title" : "The Conquest of the Pole",
	"year" : 1912,
	"runtime" : 33,
	"cast" : [
		"Georges M�li�s",
		"Fernande Albany"
	],
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMjE1MDI5MTMxMF5BMl5BanBnXkFtZTgwOTg4NzYwMjE@._V1_SX300.jpg",
	"plot" : "Scientists from all over the world are meeting to discuss the best way to reach the North Pole. Professor Maboul demonstrates for them the innovative equipment that he has designed for the ...",
	"fullplot" : "Scientists from all over the world are meeting to discuss the best way to reach the North Pole. Professor Maboul demonstrates for them the innovative equipment that he has designed for the purpose. When everything is ready, Maboul and several other scientists depart for the pole. Their trip will prove to be even more eventful than expected.",
	"lastupdated" : "2015-08-26 00:16:14.773000000",
	"type" : "movie",
	"languages" : [
		"French"
	],
	"directors" : [
		"Georges M�li�s"
	],
	"writers" : [
		"Georges M�li�s (screenplay)",
		"Georges M�li�s (story)",
		"Jules Verne (novel)"
	],
	"imdb" : {
		"rating" : 6.9,
		"votes" : 458,
		"id" : 2113
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Short",
		"Adventure",
		"Sci-Fi"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.7,
			"numReviews" : 13
		},
		"lastUpdated" : ISODate("2015-08-23T18:50:13Z")
	},
	"num_mflix_comments" : 1,
	"comments" : [
		{
			"name" : "Dana Cross",
			"email" : "dana_cross@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4962"),
			"text" : "Nemo tempore quod facilis nulla numquam ullam. Eligendi fuga perferendis ratione. Labore consectetur magnam alias.",
			"date" : ISODate("1978-01-27T12:24:04Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd41aa"),
	"title" : "Une partie de cartes",
	"year" : 1896,
	"runtime" : 1,
	"cast" : [
		"Gaston M�li�s",
		"Georges M�li�s",
		"Georgette M�li�s"
	],
	"plot" : "Three friends are playing cards in a beer garden. One of them orders drinks. The waitress comes back with a bottle of wine and three glasses on a tray. The man serves his friends. They ...",
	"fullplot" : "Three friends are playing cards in a beer garden. One of them orders drinks. The waitress comes back with a bottle of wine and three glasses on a tray. The man serves his friends. They clink glasses and drink. Then the man asks for a newspaper. He reads a funny story in it and the three friends burst out laughing while the waitress merely smiles.",
	"lastupdated" : "2015-07-27 00:27:37.370000000",
	"type" : "movie",
	"directors" : [
		"Georges M�li�s"
	],
	"writers" : [
		"Georges M�li�s"
	],
	"imdb" : {
		"rating" : 5.1,
		"votes" : 462,
		"id" : 132
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Short",
		"Biography"
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd41f0"),
	"title" : "Ella Lola, a la Trilby",
	"year" : 1898,
	"cast" : [
		"Ella Lola"
	],
	"plot" : "Dancer Ella Lola dances a routine based on the famous character of \"Trilby\".",
	"fullplot" : "Dancer Ella Lola dances a routine based on the famous character of \"Trilby\".",
	"lastupdated" : "2015-08-29 00:19:44.140000000",
	"type" : "movie",
	"directors" : [
		"James H. White"
	],
	"writers" : [
		"George L. Du Maurier (novel)"
	],
	"imdb" : {
		"rating" : 4.7,
		"votes" : 83,
		"id" : 192
	},
	"countries" : [
		"USA"
	],
	"genres" : [
		"Short"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 4.1,
			"numReviews" : 46
		},
		"lastUpdated" : ISODate("2015-09-14T17:32:55Z")
	},
	"num_mflix_comments" : 1,
	"comments" : [
		{
			"name" : "Tyrion Lannister",
			"email" : "peter_dinklage@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd41f0"),
			"text" : "Ipsa dignissimos quibusdam id doloremque quis corrupti placeat. Fugit velit natus nesciunt iure impedit at aut. Doloribus unde dolores deleniti aspernatur eos.",
			"date" : ISODate("1996-06-11T07:01:30Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd434b"),
	"title" : "Dream of a Rarebit Fiend",
	"year" : 1906,
	"runtime" : 7,
	"released" : ISODate("1906-02-01T00:00:00Z"),
	"cast" : [
		"Jack Brawn"
	],
	"plot" : "The fiend faces the spectacular mind-bending consequences of his free-wheeling rarebit binge.",
	"fullplot" : "Adapted from Winsor McCay's films and comics of the period, this film follows the established theme: the \"Rarebit Fiend\" gorges himself on rarebit and thus suffers spectacular hallucinatory dreams.",
	"lastupdated" : "2015-08-29 00:29:11.733000000",
	"type" : "movie",
	"languages" : [
		"English"
	],
	"directors" : [
		"Wallace McCutcheon",
		"Edwin S. Porter"
	],
	"writers" : [
		"Winsor McCay (comic strip)"
	],
	"imdb" : {
		"rating" : 6.7,
		"votes" : 1082,
		"id" : 546
	},
	"countries" : [
		"USA"
	],
	"genres" : [
		"Short",
		"Fantasy"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.6,
			"numReviews" : 62,
			"meter" : 65
		},
		"lastUpdated" : ISODate("2015-07-02T18:49:19Z")
	},
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Joffrey Baratheon",
			"email" : "jack_gleeson@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd434b"),
			"text" : "Ab possimus exercitationem alias et accusantium impedit facere. Ipsa assumenda quaerat quisquam exercitationem excepturi non dignissimos. Recusandae accusamus veritatis fugiat quos reiciendis.",
			"date" : ISODate("2014-08-22T00:48:51Z")
		},
		{
			"name" : "Robert Baratheon",
			"email" : "mark_addy@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd434b"),
			"text" : "Sit nostrum quasi tempora totam magni placeat. Deserunt accusamus vel veritatis quisquam quam. Repellendus eveniet porro nihil mollitia porro error. A delectus nisi qui perferendis suscipit nobis.",
			"date" : ISODate("1984-02-29T10:51:31Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd42ca"),
	"title" : "Alice in Wonderland",
	"year" : 1903,
	"runtime" : 8,
	"released" : ISODate("1903-10-17T00:00:00Z"),
	"cast" : [
		"May Clark",
		"Cecil M. Hepworth",
		"Blair",
		"Geoffrey Faithfull"
	],
	"plot" : "Alice dozes in a garden, awakened by a dithering white rabbit in waistcoat with pocket watch. She follows him down a hole and finds herself in a hall of many doors. A key opens a small door...",
	"fullplot" : "Alice dozes in a garden, awakened by a dithering white rabbit in waistcoat with pocket watch. She follows him down a hole and finds herself in a hall of many doors. A key opens a small door: eventually, she's through into a garden where a dog awaits. Later, in the rabbit's home, her size is again a problem. She tries to help a nanny with a howling baby, then a Cheshire cat directs her to a tea party where the Mad Hatter and March Hare dunk a dormouse. Expelled from the party, Alice happens on a royal processional: all the cards in the deck precede the Queen of Hearts, who welcomes then turns on Alice and calls on the royal executioner. Alice must run for her life.",
	"lastupdated" : "2015-08-22 00:28:40.907000000",
	"type" : "movie",
	"directors" : [
		"Cecil M. Hepworth",
		"Percy Stow"
	],
	"writers" : [
		"Lewis Carroll (novel)",
		"Cecil M. Hepworth"
	],
	"imdb" : {
		"rating" : 6.3,
		"votes" : 1572,
		"id" : 420
	},
	"countries" : [
		"UK"
	],
	"genres" : [
		"Fantasy",
		"Short"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.2,
			"numReviews" : 373,
			"meter" : 48
		},
		"lastUpdated" : ISODate("2015-07-01T18:48:05Z")
	},
	"num_mflix_comments" : 1,
	"comments" : [
		{
			"name" : "Barristan Selmy",
			"email" : "ian_mcelhinney@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd42ca"),
			"text" : "Natus illum vel aspernatur est minima voluptates modi perferendis. Minima eum vitae magni maiores animi distinctio. Odio eaque atque minus ut quod ad itaque.",
			"date" : ISODate("2003-12-06T13:06:50Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4c3c"),
	"title" : "Fantomas",
	"year" : 1913,
	"runtime" : 54,
	"cast" : [
		"Ren� Navarre",
		"Edmund Breon",
		"Georges Melchior",
		"Ren�e Carl"
	],
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTA3MTk5MjI1ODBeQTJeQWpwZ15BbWU3MDc1MzAzMzQ@._V1_SX300.jpg",
	"plot" : "Fant�mas makes it as the emperor of Crime. First is the robbery at the Royal Palace Hotel. Then he abducts Lord Beltham. As Fant�mas' fame increases actor Valgrand creates the r�le of ...",
	"fullplot" : "Fant�mas makes it as the emperor of Crime. First is the robbery at the Royal Palace Hotel. Then he abducts Lord Beltham. As Fant�mas' fame increases actor Valgrand creates the r�le of public enemy No.1 on stage. Eventually Inspector Juve, with a little help from Fandor, arrests Fant�mas and he is soon sentenced to die on the guillotine. But...",
	"lastupdated" : "2015-08-21 00:57:27.687000000",
	"type" : "movie",
	"languages" : [
		"French"
	],
	"directors" : [
		"Louis Feuillade"
	],
	"writers" : [
		"Marcel Allain (novel)",
		"Louis Feuillade",
		"Pierre Souvestre (novel)"
	],
	"imdb" : {
		"rating" : 6.8,
		"votes" : 1337,
		"id" : 2844
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Crime",
		"Drama"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.8,
			"numReviews" : 336,
			"meter" : 90
		},
		"lastUpdated" : ISODate("2015-09-01T19:12:25Z")
	},
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Stannis Baratheon",
			"email" : "stephen_dillane@gameofthron.es",
			"movie_id" : ObjectId("573a1390f29313caabcd4c3c"),
			"text" : "Enim eum eum cum dolorum asperiores. Ea totam ducimus eaque laboriosam. Doloremque illo exercitationem reprehenderit provident quidem.",
			"date" : ISODate("2014-10-08T14:33:47Z")
		},
		{
			"name" : "Alexander Robinson",
			"email" : "alexander_robinson@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4c3c"),
			"text" : "Est nostrum debitis veniam. Ad odio magni odit iure. Eligendi harum reprehenderit sunt voluptas.",
			"date" : ISODate("1976-12-17T22:38:42Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4319"),
	"title" : "The Voyage Across the Impossible",
	"year" : 1904,
	"runtime" : 24,
	"released" : ISODate("1904-10-01T00:00:00Z"),
	"cast" : [
		"Georges M�li�s"
	],
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTYxNjExMzk5Nl5BMl5BanBnXkFtZTgwMzE5MjAwMzE@._V1_SX300.jpg",
	"plot" : "Using every known means of transportation, several savants from the Geographic Society undertake a journey through the Alps to the Sun which finishes under the sea.",
	"fullplot" : "Using every known means of transportation, several savants from the Geographic Society undertake a journey through the Alps to the Sun which finishes under the sea.",
	"lastupdated" : "2015-08-26 00:06:49.650000000",
	"type" : "movie",
	"languages" : [
		"French"
	],
	"directors" : [
		"Georges M�li�s"
	],
	"writers" : [
		"Georges M�li�s",
		"Jules Verne (play)",
		"Adolphe d'Ennery (play)"
	],
	"imdb" : {
		"rating" : 7.7,
		"votes" : 2022,
		"id" : 499
	},
	"countries" : [
		"France"
	],
	"genres" : [
		"Short",
		"Adventure",
		"Fantasy"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 4.1,
			"numReviews" : 311,
			"meter" : 93
		},
		"lastUpdated" : ISODate("2015-08-20T18:41:25Z")
	}
}
{
	"_id" : ObjectId("573a1390f29313caabcd4964"),
	"title" : "The Burglar's Dilemma",
	"year" : 1912,
	"runtime" : 15,
	"released" : ISODate("1912-12-16T00:00:00Z"),
	"cast" : [
		"Lionel Barrymore",
		"Henry B. Walthall",
		"Robert Harron",
		"Harry Carey"
	],
	"plot" : "In this latter day Cain and Abel story, a jealous brother strikes down his sibling just as a young burglar is about to enter the house. The jealous brother summons police, who then charge ...",
	"fullplot" : "In this latter day Cain and Abel story, a jealous brother strikes down his sibling just as a young burglar is about to enter the house. The jealous brother summons police, who then charge the young intruder with murder. How can the burglar prove his innocence?",
	"lastupdated" : "2015-08-26 00:15:24.197000000",
	"type" : "movie",
	"languages" : [
		"English"
	],
	"directors" : [
		"D.W. Griffith"
	],
	"writers" : [
		"Lionel Barrymore"
	],
	"imdb" : {
		"rating" : 6.1,
		"votes" : 210,
		"id" : 2082
	},
	"countries" : [
		"USA"
	],
	"genres" : [
		"Short",
		"Drama"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 0,
			"numReviews" : 0
		},
		"lastUpdated" : ISODate("2015-01-05T16:39:41Z")
	},
	"num_mflix_comments" : 1,
	"comments" : [
		{
			"name" : "Mark Brown",
			"email" : "mark_brown@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4964"),
			"text" : "Animi amet molestiae recusandae rerum voluptatum quibusdam cumque quidem. Ducimus omnis reiciendis doloribus asperiores quis. Asperiores dicta iusto tempore nihil.",
			"date" : ISODate("1983-03-15T10:26:48Z")
		}
	]
}
{
	"_id" : ObjectId("573a1390f29313caabcd4ad2"),
	"title" : "The Life and Death of King Richard III",
	"year" : 1912,
	"runtime" : 55,
	"released" : ISODate("1912-10-15T00:00:00Z"),
	"cast" : [
		"Robert Gemp",
		"Frederick Warde",
		"Albert Gardner",
		"James Keane"
	],
	"poster" : "http://ia.media-imdb.com/images/M/MV5BMTQ1ODY2NTU4OF5BMl5BanBnXkFtZTgwNTc3NjU4MDE@._V1_SX300.jpg",
	"plot" : "Richard of Gloucester uses manipulation and murder to gain the English throne.",
	"fullplot" : "Shakespeare's tragedy of the hump-backed Duke of Gloucester, who rises to the throne of England by chicanery, treachery, and brilliance, only to find that his own methods have prepared the groundwork for his downfall.",
	"lastupdated" : "2015-08-05 00:52:19.570000000",
	"type" : "movie",
	"languages" : [
		"English"
	],
	"directors" : [
		"Andr� Calmettes",
		"James Keane"
	],
	"writers" : [
		"James Keane",
		"William Shakespeare (play)"
	],
	"imdb" : {
		"rating" : 5.6,
		"votes" : 173,
		"id" : 2461
	},
	"countries" : [
		"France",
		"USA"
	],
	"genres" : [
		"Drama"
	],
	"tomatoes" : {
		"viewer" : {
			"rating" : 3.5,
			"numReviews" : 130,
			"meter" : 67
		},
		"dvd" : ISODate("2001-06-26T00:00:00Z"),
		"lastUpdated" : ISODate("2015-05-15T18:04:47Z")
	},
	"num_mflix_comments" : 2,
	"comments" : [
		{
			"name" : "Renee Yu",
			"email" : "renee_yu@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4ad2"),
			"text" : "Quo fuga aperiam eaque fugit facere. Molestias unde qui dolor sunt vitae consectetur nisi. Expedita molestiae repellendus doloribus magni.",
			"date" : ISODate("2016-10-16T01:35:17Z")
		},
		{
			"name" : "Melissa Baxter",
			"email" : "melissa_baxter@fakegmail.com",
			"movie_id" : ObjectId("573a1390f29313caabcd4ad2"),
			"text" : "Tempora labore unde vel repellat. Esse repellendus atque quis fugiat inventore. Qui tempora animi impedit hic nam quos natus. Sed fuga mollitia maiores ab iste ab atque.",
			"date" : ISODate("1997-12-01T17:17:31Z")
		}
	]
}
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.findOne({title: "Life Is Beautiful"}, { _id: 0, cast: 1, writers: 1})
{
	"cast" : [
		"Roberto Benigni",
		"Nicoletta Braschi",
		"Giustino Durano",
		"Giorgio Cantarini"
	],
	"writers" : [
		"Vincenzo Cerami (story)",
		"Roberto Benigni (story)"
	]
}
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
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

* 1597 :+1:

* 1263

* 1259

* 1595


### See detailed answer

One solution is below.

```sh
db.movies.aggregate([
  {
    $match: {
      cast: { $elemMatch: { $exists: true } },
      directors: { $elemMatch: { $exists: true } },
      writers: { $elemMatch: { $exists: true } }
    }
  },
  {
    $project: {
      _id: 0,
      cast: 1,
      directors: 1,
      writers: {
        $map: {
          input: "$writers",
          as: "writer",
          in: {
            $arrayElemAt: [
              {
                $split: ["$$writer", " ("]
              },
              0
            ]
          }
        }
      }
    }
  },
  {
    $project: {
      labor_of_love: {
        $gt: [
          { $size: { $setIntersection: ["$cast", "$directors", "$writers"] } },
          0
        ]
      }
    }
  },
  {
    $match: { labor_of_love: true }
  },
  {
    $count: "labors of love"
  }
])
```

With our first `$match` stage, we filter out documents that are not an array or have an empty array for the fields we are interested in.

```sh
{
  $match: {
    cast: { $elemMatch: { $exists: true } },
    directors: { $elemMatch: { $exists: true } },
    writers: { $elemMatch: { $exists: true } }
  }
},
```

Next is a `$project` stage, removing the `_id` field and retaining both the `directors` and `cast` fields. We replace the existing `writers` field with a new computed value, cleaning up the strings within `writers`

```sh
  {
    $project: {
      _id: 0,
      cast: 1,
      directors: 1,
      writers: {
          $map: {
            input: "$writers",
            as: "writer",
            in: {
              $arrayElemAt: [
                {
                  $split: ["$$writer", " ("]
                },
                0
              ]
            }
          }
        }
      }
    }
  }
},
```

We use another `$project` stage to computer a new field called `labor_of_love` that ensures the intersection of `cast, writers`, and our newly cleaned `directors` is greater than 0. This definitely means that at least one element in each array is identical! `$gt` will return true or false.

```sh
{
  $project: {
    labor_of_love: {
      $gt: [
        { $size: { $setIntersection: ["$cast", "$directors", "$writers"] } },
        0
      ]
    }
  }
},
```

Lastly, we follow with a `$match` stage, only allowing documents through where `labor_of_love` is `true`. In our example we use a `$match` stage, but `itcount()` works too.

```sh
{
  $match: { labor_of_love: true }
},
{
  $count: "labors of love"
}

// or

  {
    $match: { labor_of_love: true }
  }
]).itcount()
```

This produces 1597, as expected.
