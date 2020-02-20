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

### Transcripción

En esta lección, discutiremos los Operadores de Consulta de Comparación ([comparison query operators](https://docs.mongodb.com/manual/reference/operator/query/#comparison)).

Estos son operadores que nos permiten hacer coincidir en función de un valor de campo relativo a algún otro valor.

En nuestra colección de detalles de películas, la mayoría de los documentos tienen un campo llamado run time (tiempo de ejecución).

Esto es un buen punto de partida para considerar algunos operadores de comparación.

Aquí tengo un shell mongo conectado a mi clúster de sandbox Atlas.

Vamos a filtrar todas las películas que tengan un tiempo de ejecución superior a 90.

Ahora el tiempo de ejecución se especifica en minutos, por lo que todas las películas duran más de una hora y media.

Si lo deseamos, podemos proyectar solo el título y el tiempo de ejecución para darnos un resumen más rápido de este conjunto de resultados.

Puede ver que cada documento en nuestro conjunto de resultados tiene un tiempo de ejecución mayor de 90 minutos.

Ahora revisemos la sintaxis por un minuto.

¿Por qué está estructurado de esta manera?

La idea aquí es mantener la coherencia con los filtros de igualdad.

Y para que quede claro por filtro de igualdad, simplemente me refiero a un filtro para un valor exacto.

Para los filtros de igualdad, definimos un valor específico que una clave debe tener en los documentos coincidentes.

En el caso de esta consulta, en lugar de especificar un valor al que necesitamos que sea equivalente el tiempo de ejecución, estamos expresando el tipo de relación que queremos que tengan todos los documentos del conjunto de resultados con respecto al campo de tiempo de ejecución.

Esta sintaxis también hace que sea muy conveniente expresar rangos.

Por ejemplo, puedo estipular que me gustaría ver películas de más de 90 minutos y menos de 120 minutos.

Para hacer eso, podemos usar esta consulta.

Y en aras de la exhaustividad, mencionaré que $ lt es el operador menor que.

Ahora lo que realmente quiero es que todas las películas que coincidan con mi filtro sean mayores o iguales a 90 minutos y menores que igual a 120 minutos.

Entonces, puedo modificar mi filtro ligeramente para usar el operador mayor o igual que $ gte y el operador $ lte.

Vamos a correr esto.

Aquí podemos ver que hemos perdido algunos resultados de la última vez porque ahora estamos especificando que queremos películas en un rango de solo dos horas.

Entonces, con esta consulta, tuvimos bastantes películas que en realidad duraron más de dos horas.

Y con este, solo los que duran entre 90 minutos y dos horas.

Por supuesto, con los operadores de comparación no se limitaron a trabajar con un solo campo.

Podemos trabajar fácilmente con tantos campos como sea necesario utilizando combinaciones de operadores de comparación, otros operadores y coincidencias de igualdad.

Supongamos que estamos interesados ​​en películas que están altamente calificadas y que también tienen tiempos de ejecución largos.

Mezclemos las cosas un poco más usando un campo de documento incrustado, el medidor de tomate.

Ahora para el medidor de tomate, el máximo es 100.

Entonces, lo que haré es combinar un selector para el medidor de tomate buscando valores que sean mayores o iguales a 95 con tiempo de ejecución buscando valores que sean mayores o iguales a 180.

Entonces, en este caso, películas que duran tres horas o más.

Aquí podemos ver que tenemos dos resultados que cumplen con estos criterios.

El padrino: la parte II, por supuesto, es muy larga y, en mi opinión, la mejor de las tres películas de padrino.

Así que echemos un vistazo a qué otros operadores de comparación hay.

$ eq, tiene exactamente la misma semántica que las coincidencias de igualdad con las que ya está familiarizado.

Hemos visto mayor que, mayor que o igual a, menor que y menor que o igual a.

Ahora echemos un vistazo a no igual a, o $ ne y $ in.

Primero, veremos $ ne.

Ahora, en muchas aplicaciones, particularmente si estamos haciendo algo como la limpieza de datos, podríamos estar interesados ​​en particionar nuestros datos porque sabemos que tenemos algunos campos que no son los esperados.

Muchas películas de esta colección tienen un valor de cuatro sin clasificar.

Entonces, tal vez solo queremos ver todos los documentos que tienen una calificación real, como PG, PG-13, R, etc.

Podemos usar $ ne para hacer esto, y la semántica de este filtro es que haremos coincidir todos los documentos que para la clave clasificada tendrán un valor que sea diferente o no sea igual a no calificado.

Ahora hay una cosa que debo mencionar sobre $ ne.

Además de hacer coincidir todos los documentos que contienen un campo calificado cuyo valor es algo diferente de no calificado, $ ne también devolverá documentos que no tengan un campo calificado en absoluto.

MongoDB admite un modelo de datos flexible.

Existen muchos casos de uso para que los documentos de la misma colección tengan campos que otros documentos no tienen.

En los modelos de datos MongoDB, en lugar de almacenar un valor nulo para el campo, a menudo simplemente no almacenaremos nada en el campo.

En otra lección, veremos cómo distinguir documentos que no tienen un campo determinado.

OK, entonces el último operador de comparación que quiero ver es el $ in.

$ in, nos permite especificar uno o más valores.

Cualquiera de los cuales debe hacer que se devuelva un documento.

Este filtro seleccionaremos todos los documentos donde el valor de calificación sea G o PG.

Los documentos con cualquiera de esos dos valores para la clave clasificada coincidirán con este filtro.

Y aquí podemos ver que, de hecho, todos los resultados tienen un valor de PG o G para la clave clasificada.

Si quisiéramos extender esto, simplemente agregamos otro elemento al valor de la matriz de nuestro operador $ in.

Tenga en cuenta que el valor de $ in debe ser una matriz.

En este caso, se devolverán los documentos que tengan un valor de calificación que sea cualquiera de estos tres valores.

Si inventamos esto un poco y lo cambiamos solo a películas con clasificación R y PG-13, verá que ahora nuestros resultados coinciden con esos criterios.

También hay un operador que nos permite hacer lo contrario de lo que hace $ in.

Lo dejaré como ejercicio para que experimente con ese operador.

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
