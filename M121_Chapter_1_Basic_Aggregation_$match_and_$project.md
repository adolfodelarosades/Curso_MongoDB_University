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

Configuramos los filtros en nuestra etapa `$match`.

Y a medida que fluyen los documentos, solo aquellos que cumplen con nuestros criterios se pasan más lejos en el pipeline.

Aquí, nuestra etapa `$match` solo dejará pasar círculos y estrellas.

`$match` utiliza la sintaxis de consulta de operación de lectura MongoDB estándar.

Podemos realizar coincidencias basadas en la comparación, la lógica, los arrays y mucho más.

Las únicas limitaciones son que no podemos usar el operador `$where`.

**Y si queremos usar un operador `$test`, la etapa `$match` debe ser la primera etapa en una tubería**.

Si `$match` es la primera etapa, puede aprovechar los índices, lo que aumenta la velocidad de nuestras consultas.

Nuevamente, `$match` debería llegar temprano en nuestras tuberías.

Como recordatorio y como referencia, puede encontrar un enlace a esta página justo debajo del video.

Lo alentamos a marcar esta página como referencia futura.

Aquí hay un ejemplo de `$match` en uso.

Si le pregunto la siguiente agregación, que filtra la colección del sistema solar, permitiendo solo documentos con tipos que no son iguales, puedo ver que obtengo los resultados que esperaba.

Para mostrar que `$match` usa la sintaxis de consulta MongoDB, usemos find para ver si obtenemos resultados idénticos.

Los mismos resultados.

Observemos esto de otra manera.

Primero, contemos el número de documentos con tipos que no son iguales a la estrella.

Debería ser ocho, ahora veamos cuántos documentos pasan por nuestra etapa de `$match`.

Voy a usar la estación de servicio, este ejemplo llamado `count`, del que aprenderás más adelante.

Aquí, podemos ver que ocho documentos pasan por nuestra agregación.

Lo siento Plutón.

Por último, `$match` no tiene ningún mecanismo de proyección.

Con `find`, podemos hacer algo como esto si queremos proyectar el campo no descrito.

Aunque esto puede parecer una limitación, pronto aprenderemos sobre una etapa poderosa que nos permite hacer esto y mucho, mucho más.

Y eso es todo por `$match`.

Nuevamente, le recomendamos que piense en `$match` como más un filtro que un find.

Una vez que los documentos están en una aggregation pipeline, y los estamos formando con nuevos campos y nuevos datos, usaremos `$match` en gran medida para seguir filtrando los documentos.

Algunas cosas clave para recordar.

Una etapa `$match` puede contener un operador de consulta `$text`, pero debe ser la primera etapa en la tubería.

`$match` viene temprano en una aggregation pipeline, no puede usar `$where` con `$match`, y `$match` usa la misma sintaxis de consulta que find.

## 2. Examen `$match`: Filtering documents

**Problem:**

Which of the following is/are true of the $match stage?

Check all answers that apply:

* It should come very early in an aggregation pipeline.

* It uses the familiar MongoDB query language.

* `$match` can use both query operators and aggregation expressions.

* `$match` can only filter documents on one field.


## 3. Tema: Laboratorio - `$match`

### Transcripción

## 4. Tareas

## 5. Tema: Dar forma a documentos con `$project`

### Transcripción

## 6. Examen

## 7. Laboratorio: Cambio de la forma del documento con `$project`

## 8. Laboratorio - Campos Computados

## 9. Tema: Laboratorio opcional: Expresiones con `$project`

### Transcripción

## 10. Examen
