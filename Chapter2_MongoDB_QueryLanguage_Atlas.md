# Capítulo 2: El lenguaje de consulta MongoDB + Atlas

### 32 Artículos

Operaciones de creación, lectura, actualización y eliminación (CRUD); cursores, proyecciones, fundamentos básicos de Atlas.

## Lecciones

1. Tema: Introducción a CRUD
2. Examen
3. Tema: Instalación de mongo Shell (Windows)
4. Tema: Instalación de mongo Shell (OSX / Linux)
5. Tema: Conectando a nuestro grupo de clase Atlas desde el mongo Shell
6. Examen
7. Tema: Crear un clúster de sandbox de Atlas
8. Laboratorio 2.0: crear un clúster de sandbox de Atlas
9. Tema: Conectando a tu Sandbox Cluster desde el mongo Shell
10. Tema: Carga de datos en su grupo de Sandbox
11. Tema: Conexión a su clúster de sandbox desde Compass
12. Laboratorio 2.1: ¿Cuántas comedias?
13. Tema: Crear documentos: insertOne ()
14. Tema: Creación de documentos: insertMany ()
15. Laboratorio 2.2: ¿Cuántos insertados?
16. Tema: Lectura de documentos: campos escalares
17. Examen
18. Laboratorio 2.3: Consultas en campos escalares
19. Tema: Lectura de documentos: campos de matriz
20. Examen
21. Práctica de laboratorio 2.4: Consultas en campos de array, Parte 1
22. Práctica de laboratorio 2.5: Consultas en campos de array, Parte 2
23. Tema: Cursores
24. Tema: Proyecciones
25. Tema: Actualización de documentos: updateOne ()
26. Conferencia: Operadores de actualización
27. Examen
28. Tema: Actualización de documentos: updateMany ()
29. Tema: Upserts
30. Tema: Actualización de documentos: replaceOne ()
31. Laboratorio 2.6: Operadores de actualización
32. Tema: Eliminar documentos

## 1. Tema: Introducción a CRUD

Ahora es el momento de sumergirse y aprender el lenguaje de consulta MongoDB.

Independientemente de la aplicación que esté creando, esto será importante a medida que trabaje para que los datos entren y salgan de MongoDB.

Ya hemos tenido una muestra del lenguaje de consulta en nuestro trabajo con Compass hasta ahora.

En este capítulo, haremos una inmersión más profunda para que esté completamente preparado para trabajar con datos MongoDB.

Cubriremos las operaciones CRUD.

<img src="/images/c2/crud.png">

Si no está familiarizado con ese acrónimo, simplemente significa crear, leer, actualizar y eliminar.

Son estos cuatro tipos de operaciones que discutiremos en este capítulo.

Además de MongoDB Compass, usaremos el shell mongo.

Al igual que Compass, el shell mongo es un cliente MongoDB.

Sin embargo, el shell mongo está basado en texto, en lugar de ser visual.

También proporciona una serie de funciones administrativas además de admitir el lenguaje de consulta MongoDB.

Usaremos el shell a lo largo de este capítulo porque proporciona soporte completo para el lenguaje de consulta.

Vamos a sumergirnos.

## 2. Examen

**Problem:**

Which of the following operations will we focus on in our coverage of the MongoDB query language?

Check all answers that apply:

* review

* delete :+1:

* update :+1:

* decipher

* copy

* read :+1:

* unlink

* redact

* create :+1:

## 3. Tema: Instalación de mongo Shell (Windows)

## Notas de lectura

En este curso, utilizaremos Mongo Shell, ya que es totalmente compatible con todas las operaciones CRUD de MongoDB.

Descargue **MongoDB Enterprise Server** del [Centro de descargas de MongoDB](https://www.mongodb.com/download-center/enterprise).

## Transcripción

Compass aún no es totalmente compatible con todo el lenguaje de consulta MongoDB.

Entonces, para hacer algunas de las lecciones de este curso, necesitaremos usar el shell mongo.

El shell mongo es un cliente basado en texto que es totalmente compatible con todas las operaciones CRUD de MongoDB.

[Documentación Oficial: mongo Shell Quick Reference](https://docs.mongodb.com/manual/reference/mongo-shell/index.html)

<img src="/images/c2/mongo-shell.png">

En esta lección, lo guiaré a través de la instalación de Mongo Shell en una máquina Windows.

Aquí tenemos un entorno de Windows 10.

También le mostraré algunas de las diferencias sutiles que encontrará al instalar en Windows 7.

No hay muchos y debería estar bien en Windows 10 o en versiones anteriores de Windows, siempre que sean versiones de Windows de 64 bits.

Ahora para instalar el shell mongo, en realidad vamos a instalar MongoDB.

Vamos a instalar el servidor empresarial.

El shell viene empaquetado con el servidor como parte de la descarga desde el Centro de descargas de MongoDB.

Así que vaya al Centro de descargas.

Y luego, en lugar de Community Server, haga clic en la pestaña Enterprise Server y seleccionaré Windows.

Ahora esto es muy importante.

Incluso si ya tiene MongoDB instalado, siga estos pasos.

Las versiones anteriores de MongoDB en el shell no proporcionaban soporte para SSL, y vamos a necesitar soporte SSL para poder conectarnos a nuestro clúster Atlas.

Seleccione Windows 64 y luego Descargar.

<img src="/images/c2/windows.png">

Ahora, si no ha accedido previamente al Centro de descargas, se le pedirá que complete un formulario de descarga.

Simplemente ingrese sus datos allí y luego pase a descargar el MSI de Windows.

Ahora se nos presentan dos opciones aquí, archivo o MSI.

Por favor seleccione MSI.

Y de nuevo, es Enterprise Server, Windows 64 y luego MSI.

Y solo esperaremos a que se descargue.

Y una vez que lo haga, podemos instalarlo.

Y una vez que se descargue el MSI, podemos hacer clic en él y luego usar el asistente de instalación para instalar MongoDB.

Seleccione Completo y luego Instalar.

Permita que la aplicación realice cambios en nuestro dispositivo, y luego terminamos.

Ahora **es importante que comprenda dónde se instaló MongoDB en su computadora**.

Entonces, si hace clic en el Explorador de Windows y luego se desplaza hacia abajo hasta Disco local, en su máquina, la ubicación de descarga debería ser similar a la siguiente.

C:, Archivos de programa, MongoDB, Servidor, 3.4.

Y luego, si hacemos clic en el directorio 3.4, veremos que hay información de licencia, pero lo más importante es este directorio **bin**.

**El directorio bin contiene todos los ejecutables que se distribuyen con el servidor MongoDB**.

El que nos interesa es el **mongo**.

Ahora tenga en cuenta que es muy similar en nombre a **mongod**, que es el ejecutable para el servidor MongoDB.

Para este curso, utilizaremos el shell mongo, que se ejecuta desde el ejecutable mongo.

OK, antes de que podamos ejecutar mongo, necesitaremos abrir un shell de comandos.

Ahora, independientemente de si está trabajando en una máquina con Windows 10 o una versión anterior de Windows, tiene un cuadro de texto que puede escribir.

Y si escribe cmd, notará que se nos sugiere el símbolo del sistema.

Si presionamos Enter, lo que aparece es una interfaz de línea de comando desde la cual ejecutaremos el shell mongo.

Nuevamente, el shell mongo es un entorno basado en texto.

Puedo hacer algo muy similar en Windows 7 haciendo clic en el botón de inicio y luego en el cuadro de búsqueda escribiendo cmd y presionando Enter.

Y de nuevo, aparece un shell de comandos que aparece.

Volviendo a Windows 10, todo lo que hago aquí también lo puede hacer en versiones anteriores de Windows, como Windows 7.

El shell de comandos es prácticamente el mismo en la mayoría de las versiones del sistema operativo Windows.

Al menos es lo mismo para nuestros propósitos en la forma en que lo usaremos.

Entonces, si echas un vistazo a lo que se ha impreso en el shell, verás que dice `C:\Users\miNombre>`.

Esta es simplemente una carpeta en el sistema de archivos para mi máquina Windows, y puedo encontrar esa carpeta si busco en C :, Usuarios, shannon.

Entonces C :, Usuarios, shannon es exactamente esta carpeta.

`C:\Users\miNombre>`

Ahora, si quiero ejecutar el shell mongo en muchas versiones de Windows, necesito hacer un paso adicional, y es que necesito configurar mi PATH.

Si nunca has hecho esto antes, no te preocupes por eso.

Es un proceso bastante sencillo.

Pero está un poco abajo en la maleza.

Así que daremos un paso a la vez.

Para hacer esto, puedo escribir el **system environment**, y solo completa, **variables**.

Y mucho antes de eso, Windows habrá sugerido esta operación, **Editar las variables de entorno del sistema**.

Si hago clic en eso, lo que veré es una ventana emergente para **Propiedades del sistema**, y **la pestaña Avanzado** ya está seleccionada.

Si **hago clic en Variables de entorno**, verá que tengo variables de entorno de usuario para myNombre y variables de entorno del sistema.

El que nos interesa es el Path del entorno del sistema.

Y si seleccionamos **Path** y hacemos clic en Editar, veremos una lista de carpetas.

Estas carpetas contienen archivos ejecutables o programas que se pueden ejecutar en esta máquina con Windows.

Ahora en Windows 10, probablemente no sea estrictamente necesario que actualicemos las variables de entorno.

Pero en muchas versiones de Windows, es necesario.

Así que te voy a mostrar cómo hacerlo aquí.

Ahora, como dijimos anteriormente, MongoDB está instalado en C:, Archivos de programa, MongoDB, Servidor, 3.4, y todos los ejecutables están en el directorio bin.

Entonces, de nuevo, eso es C :, Archivos de programa, MongoDB, Servidor, 3.4, bin.

La forma en que se representa esa estructura de carpetas en nuestra ruta es separando cada componente de la ruta con una barra diagonal inversa.

Entonces, para nuestra ruta, se ve así.

`C:\Archivos de programa\MongoDB\servidor 3.4\bin`.

Finalmente, lo que me gustaría hacer antes de que terminemos de editar esta variable de entorno es mover esto a la parte superior.

Para entender por qué quería hacer eso, hablemos un poco sobre cómo se usa la ruta.

Cuando estoy en el shell y escribo un comando, como mongo, nuevamente, recuerde, mongo es el nombre del ejecutable del shell mongo, Windows buscará programas con ese nombre en cada una de las carpetas enumeradas aquí.

Y lo hace en orden.

Por lo tanto, primero verificará esta ruta para ver si hay ejecutables ubicados allí.

En este caso, encontrará uno llamado mongo y lo ejecutará.

La razón por la que quería colocar el directorio de instalación de MongoDB primero es en caso de que tenga instalada una versión anterior de MongoDB porque si tiene instalada una versión anterior de MongoDB, es probable que esté en su ruta y podría terminar ejecutando esa versión accidentalmente. de la concha de mongo en su lugar.

Nuevamente, las versiones anteriores de la consola mongo no incluían soporte para SSL, y lo necesitamos aquí.

Por lo tanto, realmente necesitamos ejecutar la última versión del shell mongo, que puede descargar del Centro de descargas de MongoDB como parte del paquete MongoDB Enterprise.

Entonces podemos hacer clic en Aceptar aquí.

Y ahora notará que nuestra ruta se ha actualizado para incluir esa ruta individual que acabamos de agregar, y se agrega al frente.

Y tenga en cuenta que está separado del siguiente elemento en la ruta por un punto y coma aquí.

Este es uno de esos puntos de diferencia de Windows 7.

Y con eso quiero decir que al editar una ruta en Windows 10 tienes esta buena lista de elementos en tu ruta.

En Windows 7, es un poco más arcano, a falta de una palabra mejor, porque para cambiar la variable de entorno, en realidad necesitamos editar una cadena que incluya todos los componentes de una ruta en una línea.

Así que aquí puedo hacer exactamente lo mismo, sistema, entorno, y antes de que termine de escribir, Windows me sugiere lo correcto.

Hago clic en él

Nuevamente, es Propiedades del sistema, pestaña Avanzado seleccionada, hago clic en Variables de entorno.

Y luego, si me desplazo hacia abajo aquí, puedo abrir mi Ruta y Editar.

Y mira aquí, esto es lo que quiero decir.

Solo tenemos una línea de texto.

Estos son todos los mismos elementos de la ruta que vimos en Windows 10.

Pero en lugar de estar en líneas separadas, todas están en una sola línea.

Para actualizar una ruta usando esta interfaz de usuario anterior, puedo copiar en mi ruta de bin C: Archivos de programa MongoDB Server 3.4, y luego escribir un punto y coma para que se separe del siguiente elemento en mi ruta.

Con eso, puedo hacer clic en Aceptar, Aceptar, Aceptar.

Y luego actualicé mi ruta en Windows 7.

Volviendo a Windows 10, quiero hacer lo mismo.

Haga clic en Aceptar.

Y luego, lo que quiero hacer es reiniciar el símbolo del sistema porque en muchas versiones de Windows, solo después de reiniciar el símbolo del sistema se reflejarán los cambios que hice en la variable de entorno de la ruta.

Así que voy a cerrar eso.

Y lo comenzaré de la misma manera, escribiendo el comando y luego presionando Enter.

Y ahora podré ejecutar el shell mongo.

Entonces, para verificar que lo hemos instalado correctamente y que hemos configurado nuestra ruta correctamente, lo que voy a hacer es escribir `mongo --nodb`.

Ahora `--nodb` simplemente significa que solo estamos iniciando el shell mongo sin intentar conectarnos a ninguna base de datos MongoDB.

Estamos haciendo esto simplemente como un medio para verificar que hemos instalado el shell de mongo correctamente y que hemos configurado nuestra ruta correctamente.

Y si ve un mensaje, como este, `MongoDB shell versión 3.4.6` o posterior, dependiendo de cuándo está realmente haciendo esta instalación.

A medida que lancemos la próxima versión de MongoDB, su número de versión puede cambiar.

Pero debería ver un mensaje muy similar a este.

Ha instalado correctamente el shell mongo y lo ha iniciado correctamente.

Para salir de la consola de mongo, simplemente puede escribir, salir.

Puede ejecutar la función `quit()`.

Y eso es todo para instalar el shell mongo y configurarlo para realizar varias operaciones CRUD diferentes en el shell mongo.

En aras de la integridad, haré lo mismo en Windows 7.

Así que cierro el símbolo del sistema, lo vuelvo a ejecutar y ejecuto `mongo --nodb`.

Y aquí podemos ver que tenemos un mensaje de inicio ligeramente diferente.

Esto le indica la documentación y los Grupos de Google donde puede hacer preguntas.

Para esta clase, asegúrese de dirigir sus preguntas al foro de discusión de MongoDB.

Si publica en Grupos de Google, de todos modos lo direccionaremos al foro de discusión.

Antes de cerrar, quiero decir una última cosa.

Si por alguna razón tiene problemas para instalar MongoDB, visite la documentación de MongoDB para instalar [MongoDB Enterprise en Window](https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-windows/).

Esta es una simple consulta de Google, y hay una extensa documentación sobre cómo instalar MongoDB en Windows.

Por supuesto, también puede pedir ayuda en el foro de discusión.

Pero antes de publicar, asegúrese de hacer una búsqueda en el foro para ver si su pregunta ya ha sido formulada y respondida.

## 4. Tema: Instalación de mongo Shell (OSX / Linux)

## 5. Tema: Conectando a nuestro grupo de clase Atlas desde el mongo Shell

## 6. Examen

## 7. Tema: Crear un clúster de sandbox de Atlas

## 8. Laboratorio 2.0: crear un clúster de sandbox de Atlas

## 9. Tema: Conectando a tu Sandbox Cluster desde el mongo Shell

## 10. Tema: Carga de datos en su grupo de Sandbox

## 11. Tema: Conexión a su clúster de sandbox desde Compass

## 12. Laboratorio 2.1: ¿Cuántas comedias?

## 13. Tema: Crear documentos: insertOne ()

## 14. Tema: Creación de documentos: insertMany ()

## 15. Laboratorio 2.2: ¿Cuántos insertados?

## 16. Tema: Lectura de documentos: campos escalares

## 17. Examen

## 18. Laboratorio 2.3: Consultas en campos escalares

## 19. Tema: Lectura de documentos: campos de matriz

## 20. Examen

## 21. Práctica de laboratorio 2.4: Consultas en campos de array, Parte 1

## 22. Práctica de laboratorio 2.5: Consultas en campos de array, Parte 2

## 23. Tema: Cursores

## 24. Tema: Proyecciones

## 25. Tema: Actualización de documentos: updateOne ()

## 26. Conferencia: Operadores de actualización

## 27. Examen

## 28. Tema: Actualización de documentos: updateMany ()

## 29. Tema: Upserts

## 30. Tema: Actualización de documentos: replaceOne ()

## 31. Laboratorio 2.6: Operadores de actualización

## 32. Tema: Eliminar documentos
