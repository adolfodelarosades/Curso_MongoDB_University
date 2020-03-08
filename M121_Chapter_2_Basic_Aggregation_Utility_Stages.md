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

Primero, veremos `$project`.

Simplemente extraigamos los datos del campo `gravity.value` y reasignemoslo al nivel superior, al campo `gravity`.

Como se esperaba, podemos recuperar los resultados con el campo `_id` y el campo `gravity` que acabamos de calcular.

Ahora eliminemos el campo `_id` y agreguemos el campo `name` para una referencia más fácil.

Muy bien, esto es bastante bueno.

Pero, ¿qué sucede si también queremos mantener los campos `temperature`, `density`, `mass`, `radius` y SMA?

Como podemos ver, para mantener la información que queremos, tuvimos que ser explícitos, especificando qué campos presentamos junto con la realización de nuestras transformaciones.

Como se dijo, esto puede volverse tedioso.

Para eso sirve `$addFields`.

Si sustituimos `$addFields` por `$project` y ejecutamos la siguiente pipeline, podemos ver que efectivamente realizamos las transformaciones deseadas.

Sin embargo, no eliminamos ningún campo del documento original.

En cambio, agregamos nuevos campos de transformación al documento.

OK.

Un último ejemplo.

Al combinar `$project` con `$addFields`, eliminamos la molestia de la necesidad explícita de eliminar o retener campos.

En este ejemplo, con `$project`, estamos seleccionando los campos que deseamos retener, y en `$addFields`, estamos realizando nuestra transformación en esos campos preseleccionados.

No es necesario ir uno por uno y eliminar o presentar campos mientras realizamos nuestras transformaciones.

Esta es una opción de estilo y puede evitar tener que especificar repetidamente qué campos retener en pipelines más grandes al realizar muchos cálculos diferentes.

Vamos a verlo en acción.

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


