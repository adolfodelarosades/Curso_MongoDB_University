# Capítulo 5: Miscellaneous Aggregation

Lecciones

* Tema: La etapa `$redact`
* Tema: La etapa `$out`
* Examen
* Tema: Vistas
* Examen

## 1. Tema: La etapa `$redact`

### Notas de lectura

Página de documentación de [`$redact`](https://docs.mongodb.com/manual/reference/operator/aggregation/redact/).

### Transcripción

Aprendamos sobre una de las etapas del marco de agregación que puede ayudarnos a proteger la información del acceso no autorizado, la etapa de redacción.

La etapa de redacción tiene la siguiente forma.

La expresión puede ser cualquier expresión o combinación de expresiones, pero finalmente debe resolverse a uno de los tres valores, descender, podar y mantener.

Bien, al principio estos parecen bastante crípticos.

Así que examinemos qué hace cada uno de ellos.

Primero, veamos podar y mantener.

Podar y mantener son inversos entre sí.

Prune excluirá todos los campos en el nivel de documento actual sin inspección adicional, mientras que keep retendrá todos los campos en el nivel de documento actual sin inspección adicional.

Entonces, ¿qué entendemos por inspección adicional?

Veamos este documento de ejemplo de la colección de empleados.

Cada cuadrado de color representa un nivel de documento.

Al especificar mantener o podar en cualquier nivel de documento dado, se realizará la acción asociada y se aplicará automáticamente a todos los niveles del documento.

Veamos este documento de ejemplo de la colección de empleados.

Cada cuadrado de color representa un nivel de documento.

Al especificar mantener o podar en cualquier nivel de documento dado, se realizará la acción asociada y se aplicará automáticamente a todos los niveles de documentos por debajo del nivel especificado.

Bien, veamos descender.

Descender conserva el campo en el nivel de documento actual que se está evaluando, excepto los subdocumentos y las matrices de documentos.

En su lugar, atravesará hacia abajo, evaluando cada nivel.

Visualicemos cómo operaría descend sobre este documento, dada esta expresión condicional, determinando si el valor de acceso del usuario está en la matriz ACL.

Comenzamos con todo el documento y comparamos si la administración está en ACL.

Como es, desciende al subdocumento en compensación de empleados, aquí.

Ahora evaluamos si la administración está en ACL, que es.

Entonces descendemos más.

En este nivel, después de la evaluación, se devuelve la poda, porque la ACL en este nivel no incluye la administración.

Este nivel y cualquier nivel posterior, si hubiera alguno, no se devolverían.

Para el usuario, es como si este campo no existiera en absoluto.

Miremos esto en acción.

Configuramos nuestra variable de acceso de usuario y luego la canalización, asegurando que solo tengamos los niveles de documentos de acceso que deberíamos.

Excelente.

Podemos ver que de hecho solo estamos recuperando los niveles de documentos donde la administración estaba en la matriz de ACL.

La etapa de redacción puede ser útil para implementar listas de control de acceso, aunque no es la única forma de limitar el acceso a la información, como lo veremos más adelante en el curso.

Cualquier usuario que tenga acceso a una colección para realizar este tipo de agregación también puede realizar otras operaciones de lectura.

Por lo tanto, la etapa de redacción no es suficiente para las restricciones de recopilación y nivel de campo.

Por último, si se compara con un campo en el documento, el campo debe estar presente en cada nivel de uso descendente, o la expresión debe explicar y decidir qué hacer si falta el campo.

Si no tomamos ninguna de estas precauciones, es probable que la redacción tenga errores.

Resumamos algunos puntos clave.

Mantener y podar se aplica automáticamente a todos los niveles por debajo del nivel evaluado.

Descender conserva el nivel actual y evalúa el siguiente nivel hacia abajo.

Y redact no es para restringir el acceso a una colección.

Recuerde, si un usuario tiene acceso para realizar una agregación en una colección, tiene acceso para leer esa colección.

## 2. Tema: La etapa `$out`

### Transcripción

Aprendamos sobre una etapa útil para persistir los resultados de una agregación, la etapa $ out.

La etapa $ out tiene la siguiente forma.

Especificamos el nombre de la colección de salida que queremos.

La etapa de salida debe ser la última etapa en la tubería.

Como tal, no se puede usar dentro de una faceta.

MongoDB creará la colección con el nombre especificado si no existe ninguno.

De lo contrario, sobrescribirá una colección existente si se especifica un nombre de colección existente.

Ahora hay algunas cosas que debes saber.

Solo creará la nueva colección dentro de la misma base de datos.

Si se reemplaza una colección existente, los índices que existían en la colección original seguirán vigentes.

Si la canalización falla, no creará ni sobrescribirá una colección.

Esto también significa que la salida de out debe cumplir las restricciones de índice, como los índices únicos, puede incluir el campo _id.

Entonces, esta agregación aquí donde hacemos coincidir cada documento, realizamos alguna operación de agrupación, nos desenrollamos para crear muchos documentos y luego intentamos que una salida a una nueva colección fallara porque daría como resultado muchos documentos con el mismo valor _id.

Y eso cubre la etapa de $ out.

Esta etapa es muy útil para realizar una agregación contra los datos existentes para realizar una migración, sembrar una colección con datos útiles o distribuir instantáneas de datos para su análisis.

Aquí hay algunas cosas para recordar sobre la etapa $ out.

Creará una nueva colección o sobrescribirá una colección existente si se especifica.

Honra los índices en colecciones existentes.

No creará ni sobrescribirá datos si hay errores en la tubería.

Y crea colecciones en la misma base de datos que la colección de origen.

## 3. Examen The $out Stage

**Problem:**

Which of the following statements is true regarding the `$out` stage?

Choose the best answer:

* `$out` removes all indexes when it overwrites a collection.

* Using `$out` within many sub-piplines of a $facet stage is a quick way to generate many differently shaped collections.

* If a pipeline with `$out` errors, you must delete the collection specified to the `$out` stage.

* `$out` will overwrite an existing collection if specified.

## 4. Tema: Vistas

### Transcripción

Analicemos ahora una característica poderosa de MongoDB-- Vistas.

MongoDB habilita vistas no materializadas, lo que significa que se calculan cada vez que se realiza una operación de tasa en esa vista.

Hay una manera de utilizar una canalización de agregación como colección.

Desde la perspectiva del usuario, las vistas se perciben como colecciones, con algunas diferencias clave que veremos más adelante en la lección.

Entonces, ¿para qué pueden ser útiles las Vistas?

Supongamos que somos una gran institución financiera con clientes de diferentes niveles.

Recientemente lanzamos una gran promoción y estamos realizando una campaña telefónica.

Hemos contratado una agencia de personal temporal con varias oficinas regionales.

Asignaremos un nivel diferente a cada oficina regional.

Esta es una muestra de un registro de nuestra colección de clientes.

Como podemos ver, hay información sensible y potencialmente sesgada a la que no queremos permitir el acceso.

Las vistas nos permiten crear cortes verticales y horizontales de nuestra colección.

¿Qué queremos decir con un corte horizontal y vertical?

El corte vertical se realiza mediante el uso de una etapa de proyecto y otras etapas similares que cambian la forma del documento que se devuelve.

Aquí hemos cortado verticalmente nuestro documento para retener solo el campo accountType.

Los sectores verticales cambiarán la forma que se devuelve, pero no la cantidad de documentos que se devuelven.

El corte horizontal se realiza mediante el uso de etapas de partido.

Seleccionamos solo un subconjunto de documentos según algunos criterios.

Aquí, dividimos horizontalmente nuestra colección con el valor del tipo de cuenta.

De hecho, los documentos que están atenuados no serían operados en absoluto en la siguiente etapa del proyecto.

Podríamos dividir aún más estos datos horizontalmente, seleccionando solo las cuentas que tenían un saldo mínimo especificado, y están dentro de un rango de edad deseado, y, se entiende la idea.

Incluso puede ser necesario usar una etapa de modelado intermedia para calcular un valor en el que deseamos filtrar los documentos.

Los cortes horizontales afectarán la cantidad de documentos devueltos, no su forma.

Veamos otro ejemplo de esto, con documentos que tienen el siguiente esquema.

Me gustaría dividir verticalmente los documentos para eliminar información confidencial, así como poner a disposición la información de nombre y género, pero presentarla en un formato más formal para los empleados del centro de llamadas.

También me gustaría dividir horizontalmente nuestra colección, filtrando documentos que no tienen un tipo de cuenta de bronce.

Aquí hay un ejemplo de creación de una vista que realiza cortes horizontales y verticales.

Para que los datos estén disponibles para el centro de llamadas, vamos a asignar miembros de nivel bronce.

Especificamos el nombre de la vista, la colección de origen y luego la canalización que se almacenará para calcular esta vista.

Dentro de la tubería, realizamos nuestro corte horizontal inicial con una etapa de coincidencia, seleccionando solo miembros de nivel de bronce.

Luego, dentro de la etapa del proyecto, realizamos nuestro corte vertical, reteniendo los campos que queramos y reasignando el campo de nombre con un nombre con un formato más formal.

Puede ver esta vista en acción usted mismo.

Ejecutemos el comando para obtener información de recopilación para la base de datos actual.

Aquí, vemos información sobre cada colección.

Ya he creado tres vistas: banca de bronce, banca de plata y banca de oro.

Podemos ver que se muestran como colecciones, excepto que su tipo es view.

Y luego, en las opciones, podemos ver la vista en la que se encuentran y la tubería que los financia.

No podrá crear vistas en el clúster de atlas de clase.

Si desea ver estas vistas en acción y cuán restrictivas pueden ser, junto con un control de acceso adecuado basado en roles, las credenciales de inicio de sesión se encuentran en el folleto de esta lección.

Si desea obtener más información sobre el control de acceso basado en roles, consulte nuestro curso de seguridad, que está vinculado debajo de este video.

Las vistas se pueden crear de dos maneras diferentes.

Tenemos el método de ayuda de shell, db.createView, que ya vimos, y el método createCollection aquí.

Una vista consiste en el nombre, una colección de origen, una canalización de agregación y, si es necesario, una clasificación específica.

En esencia, se llamaría una vista y se ejecutará la canalización de agregación que se utiliza para definir la vista.

La nueva metainformación para incluir la canalización que calcula la vista se almacena en la colección system.views.

Miremos esta información.

Nuevamente, podemos ver la misma información que vimos antes con el comando get collection info, pero ahora solo para nuestras vistas.

Con suerte, esto ilustra que la única información almacenada sobre una vista es el nombre, la colección de origen, la canalización que la define y, opcionalmente, la recopilación.

Todas las operaciones de lectura de colección están disponibles como vistas.

Y sí, también podemos realizar agregaciones en las vistas.

Las vistas tienen algunas restricciones: no hay operaciones de escritura.

Las vistas son de solo lectura y se calculan cuando emitimos una operación de tasa contra ellas.

Son un reflejo de la agregación definida en la colección de origen.

Sin operaciones de índice: s******Dado que las vistas utilizan la colección de origen para obtener sus datos, las operaciones de índice deben realizarse en esa colección de origen.

Las vistas utilizarán los índices de las colecciones de origen durante su creación.

Sin cambio de nombre: los nombres de las vistas son inmutables, por lo que no se pueden renombrar.

Dicho esto, siempre podemos soltar una vista y crearla nuevamente, con una nueva tubería, sin afectar la E / S del servidor.

Sin texto en dólares: el operador de consulta de texto solo se puede usar en la primera etapa de una canalización de agregación.

Y una vista ejecutará primero la canalización definida.

Este operador de consulta no se puede usar en una vista.

Sin geoNear o la etapa geoNear.

Al igual que con la prueba, se requiere que junior sea la primera etapa de nuestra cartera.

Restricciones de clasificación: las vistas tienen restricciones de clasificación, como las vistas que no heredan la clasificación predeterminada de la colección de origen como se especifica.

Existen otras inquietudes específicas sobre colaciones que puede leer siguiendo el enlace debajo de este video.

Por último, no se permiten operaciones de búsqueda con los siguientes operadores de proyección.

Se permite eliminar y retener campos, pero fallará intentar utilizar cualquiera de estos operadores.

Las definiciones de vistas son públicas.

Cualquier rol que pueda enumerar colecciones en una base de datos puede ver una definición de vista como vimos anteriormente.

Evite referirse a información confidencial dentro de la tubería de definición.

Todo bien.

Eso resume Vistas.

Aquí hay algunas cosas para recordar.

Las vistas no contienen datos por sí mismas.

Se crean a pedido y reflejan los datos en la colección de origen.

Las vistas son de solo lectura.

Escribir operaciones en Vistas producirá un error.

Las vistas tienen algunas restricciones.

Deben cumplir con las reglas del marco de agregación y no pueden contener operadores de proyección de búsqueda.

El corte horizontal se realiza con la etapa coincidente, lo que reduce la cantidad de documentos que se devuelven.

El corte vertical se realiza con un proyecto u otra etapa de configuración, modificando documentos individuales.

## 5. Examen Views

**Problem:**

Which of the following statements are true regarding MongoDB Views?

Choose the best answer:

* View performance can be increased by creating the appropriate indexes on the source collection.

* Inserting data into a view is slow because MongoDB must perform the pipeline in reverse.

* A view cannot be created that contains both horizontal and vertical slices.

* Views should be used cautiously because the documents they contain can grow incredibly large.
