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

### Notas de lectura

En este curso, utilizaremos Mongo Shell, ya que es totalmente compatible con todas las operaciones CRUD de MongoDB.

Descargue **MongoDB Enterprise Server** del [Centro de descargas de MongoDB](https://www.mongodb.com/download-center/enterprise).

### Transcripción

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

### Notas de lectura

En este curso, utilizaremos Mongo Shell, ya que es totalmente compatible con todas las operaciones CRUD de MongoDB.

Descargue **MongoDB Enterprise Server** del [Centro de descargas de MongoDB](https://www.mongodb.com/download-center/enterprise).

**Nota para Mac OS:**

A las 1:16 en el video, Shannon instala la versión 3.4.6 del shell mongo. Haga caso omiso de esto e instale la última versión del shell desde el centro de descargas.

Las ediciones empresariales de MongoDB Mongo Shell tienen SSL habilitado, desde la versión 3.6.5.

### Transcripción

Compass no es totalmente compatible con el lenguaje de consulta MongoDB.

Entonces, para explorar todas las operaciones CRUD en MongoDB, vamos a necesitar instalar el shell mongo.

El shell mongo es un cliente basado en texto que utilizaremos mediante cualquier interfaz de línea de comandos que tenga disponible en su sistema.

[Documentación Oficial: mongo Shell Quick Reference](https://docs.mongodb.com/manual/reference/mongo-shell/index.html)

En esta lección, veremos cómo instalar el shell mongo en un sistema Mac OS X.

Si está en Windows, vea el video para instalar el shell mongo en sistemas Windows.

Ahora, para instalar el shell Mongo, en realidad vamos a seguir adelante y solo instalaremos todo MongoDB Enterprise Server.

Enterprise Server se incluye con una serie de ejecutables que forman parte del ecosistema MongoDB, incluido el shell mongo.

Visite el Centro de descargas de MongoDB.

Puede llegar allí con una simple consulta de Google.

Haga clic en la pestaña Enterprise Server y luego seleccione su sistema operativo.

En este caso, queremos seleccionar OS X 64.

Ahora siga estas instrucciones de instalación, incluso si ya tiene instalada una versión de MongoDB.

Las versiones anteriores de la consola mongo no admitían SSL, y necesitamos compatibilidad con SSL para poder conectarnos a los clústeres de Atlas.

Una vez que haya seleccionado su sistema operativo, haga clic en Descargar.

<img src="/images/c2/mac.png">

Ahora, es posible que aparezca un formulario de contacto emergente.

Si ve un formulario de contacto, complételo.

Y luego descargue Enterprise Server.

Una vez que se completa la descarga, podemos abrir el buscador y navegar a Descargas.

Aquí está el archivo que acabamos de descargar.

Voy a **moverlo a mi directorio de inicio**.

Ahora si navego a mi directorio de inicio, podemos ver el archivo aquí mismo.

Voy a hacer doble clic en él y descomprimir el archivo comprimido.

Luego, si miro dentro de esta carpeta, puedo ver que hay, además de cierta información de licencia, un directorio **bin** que contiene varios archivos ejecutables.

El archivo ejecutable que nos interesa es este, **mongo**.

Este es el ejecutable para el **shell mongo**.

Por favor no confunda esto con **mongod**.

**Mongod es el ejecutable para el servidor MongoDB**.

Entonces, cuando decimos que tenemos una implementación de MongoDB ejecutándose, está ejecutando uno o más **mongods**.

Nuevamente, estamos interesados en el shell mongo o el ejecutable mongo.

Coloque la descarga que ha desempaquetado en su directorio de inicio, como lo hice aquí.

Eso hará que seguir estas instrucciones sea un poco más simple.

Ahora para ejecutar convenientemente el shell mongo, vamos a tener que actualizar nuestra variable de entorno de ruta.

La ruta le dice a OS X dónde buscar los programas que nos gustaría ejecutar.

Puede establecer su ruta en un archivo oculto llamado **.bash_profile**.

Si abro Sublime Text y selecciono Abrir, puedo ver archivos ocultos en mi Mac escribiendo **Command-Shift-dot**.

Luego, si navego a mi directorio de inicio, puedo ver que, de hecho, hay un **.bash_profile**.

Ahora en su sistema, es posible que no tenga un .bash_profile.

Si no ha creado uno anteriormente, probablemente no.

No se alarme si no ve uno.

Puede crear uno fácilmente, simplemente creando un nuevo archivo en su editor de texto favorito y luego guardándolo como .bash_profile.

**Asegúrese de guardarlo en su directorio de inicio**.

No lo guarde en el escritorio, no lo guarde en ningún otro lugar.

**Asegúrese de guardarlo en su directorio de inicio**.

Simplemente puede hacer clic en Guardar, y se creará un archivo .bash_profile para usted.

Ya tengo uno, así que simplemente voy a abrir el que tengo.

De nuevo es **Command-Shift-dot**.

Para ver archivos ocultos.

Y ahí está mi .bash_profile nuevamente, en mi directorio de inicio.

Si abro esto, podemos ver que hay un comando bash aquí.

Este comando establece el aviso para mi aplicación de terminal.

Ahora, para usar el shell mongo, lo ejecutaremos desde una interfaz de línea de comandos.

La aplicación de terminal se envía con Mac OS X.

Así que ese es el que voy a usar.

Y una cosa que señalaré aquí es que puedes ver que mi indicador es $ y luego un espacio.

Esto se debe a que mi perfil de bash especifica que la solicitud de terminal, o para cualquier shells de bash, debe ir seguida de un espacio.

Una inmersión en bash, una inmersión en el uso de interfaces de línea de comandos que no sean el shell Mongo está un poco fuera del alcance de este curso.

Entonces, si desea profundizar en esto, hay una cantidad de tutoriales sobre bash y la aplicación de terminal, o si está en Windows, el shell de comandos o PowerShell, cualquier cantidad de tutoriales disponibles en línea que le recomendaría echar un vistazo a

Entonces, para ejecutar el shell mongo de manera conveniente, queremos configurar nuestra variable de entorno de ruta.

Cuando tiene una aplicación de terminal abierta, en cualquier momento, puede ver las variables de entorno que se han establecido.

Por ejemplo, si quisiera ver qué es PS1, puedo hacerlo simplemente escribiendo echo $ PS1.

Debe usar el carácter del dólar para estipular que desea ver el valor de esa variable de entorno que está definida.

Puedo hacer lo mismo con la variable de entorno PATH.

Y aquí, puedo ver que está configurado en algunos directorios de sistema estándar donde se encuentran archivos o programas ejecutables.

Esto es para que pueda ejecutar una serie de comandos desde el terminal, incluidos varios programas del sistema que hacen que la interfaz de línea de comandos realmente funcione para mí.

Lo que me gustaría hacer es actualizar nuestra RUTA para incluir la ubicación donde acabamos de colocar MongoDB. Como recordatorio, está en mi directorio de inicio debajo de esta carpeta, y dentro del subdirectorio bin.

Ahora tengo una RUTA existente.

Eso es todo esto.

Y no quiero volar eso.

Por lo tanto, debo asegurarme de que al actualizar mi RUTA, mantengo todos esos directorios, así como parte de la RUTA.

La forma en que lo hacemos es a través de un comando como este.

Entonces, aquí digo, como el valor de la variable de entorno de ruta, use esta secuencia de directorios.

Entonces, esto simplemente significa mi directorio de inicio, y luego dentro de mi directorio de inicio, ese nombre largo de descarga para MongoDB.

Y solo por conveniencia, no intentes escribirlo, porque probablemente arruinarás algo.

Simplemente haga clic derecho y seleccione Copiar.

Y eso copiará el nombre de ese directorio para usted.

Y luego puedes pegarlo fácilmente.

Entonces mi directorio de inicio, el nombre de la descarga, / bin.

Y luego verás que tengo un colon aquí.

Ese punto dice que todo lo que sigue es otro directorio, en el cual buscar programas ejecutables.

Entonces, lo que esto está diciendo es que cada vez que un usuario solicite ejecutar un programa, búsquelo primero en el directorio de inicio de Shannon, mongodb-osx-x86_64-3.4.6 / bin.

Así que mira aquí primero.

Y luego, si no lo encuentra allí, continúe buscando en otros directorios.

Ahora, al especificar $ PATH aquí, no olvide el dólar, lo que estamos diciendo es que tome todos estos directorios, tenga en cuenta que están separados por dos puntos, y póngalos aquí.

La RUTA siempre se busca en orden, por lo que se buscará primero.

Y luego, cada uno de los directorios que se enumeran después de este directorio, después de estos dos puntos, se buscará en secuencia después.

Le pido que coloque el directorio que acabamos de descargar primero en su ruta, de modo que si tiene una instalación previa de MongoDB, cuando intente ejecutar el shell mongo, porque el primer lugar donde buscará el sistema es en ese directorio acabamos de descargar, esa es la versión del shell mongo que se ejecutará, no alguna versión que haya instalado anteriormente.

Entonces, si guardamos esto, nuestro perfil de bash se actualizará con una nueva RUTA.

Lo último que debo señalar al respecto es que su RUTA en el terminal no se actualizará hasta la próxima vez que inicie un terminal, o a menos que ejecute un comando especial.

Entonces, si vuelvo a hacer eco de $ PATH, veremos que es exactamente lo mismo que antes.

Sin embargo, si escribo source ~ / .bash_profile, lo que hace es leer mi perfil bash que acabo de actualizar.

Y luego, si hago eco de $ PATH, ahora vemos que ese nuevo directorio que agregamos está incluido en PATH.

Lo que esto hace por nosotros es garantizar que todos estos archivos ejecutables se puedan encontrar si escribimos su nombre aquí en el shell.

Entonces, si escribo mongo y voy a agregar la opción de línea de comando --nodb, ¿lo ves?

Lo que sucedió es exactamente contra lo que te advertí.

Y es que no he especificado correctamente la carpeta en la que existe nuestra versión descargada de MongoDB.

Entonces, volveré al buscador y seleccionaré Copiar.

Y luego volveré a Sublime, y actualizaré mi RUTA con el valor correcto.

Voy a guardarlo, y luego voy a buscar el perfil de bash nuevamente.

Y ahora, si reviso mi RUTA, podemos ver que se ha actualizado correctamente.

Y ahora, si ejecuto mongo, puedo ver que, de hecho, se inicia para mí.

Lo que hemos recorrido aquí es cómo descargar el paquete MongoDB Enterprise.

Hemos hablado sobre el hecho de que el shell mongo está incluido con el servidor mongo y una serie de otros ejecutables.

Hemos explicado cómo configurar nuestra variable de entorno PATH, para que podamos ejecutar el mongo desde cualquier lugar, y le advertimos sobre un par de cosas que pueden salir mal cuando intentamos configurar el PATH.

Entonces resulta que, dados los pasos que hemos seguido, en realidad tenemos la versión correcta y la versión incorrecta aquí en nuestra RUTA, porque simplemente estábamos concatenando lo que estaba aquí en el frente de lo que ya era el camino.

Pero no se preocupe, porque si simplemente inicio un nuevo shell, verá que solo está el correcto.

Entonces, en las nuevas invocaciones de la aplicación de terminal, recogeré exactamente lo que quiero de mi RUTA.

Y podré correr nodb.

Vemos algunos consejos más sobre cómo usar la aplicación de terminal, y profundizamos mucho sobre cómo usar el shell mongo en otras lecciones.

Una última cosa antes de cerrar esta lección es que si tiene algún problema para instalar MongoDB en su entorno OS X, visite la documentación de MongoDB para obtener instrucciones detalladas sobre cómo instalar [MongoDB Enterprise en OSX](https://docs.mongodb.com/manual/tutorial/install-mongodb-enterprise-on-os-x/).

Además de OS X, hay instrucciones detalladas para instalar [MongoDB Enterprise en Linux](https://docs.mongodb.com/manual/administration/install-enterprise-linux/), y todos los sabores de Linux y Windows.

Y nuevamente, la instalación de Windows se trata en otra lección.

## 5. Tema: Conectando a nuestro grupo de clase Atlas desde el mongo Shell

### Notas de lectura

A las 3:12, dijimos por error la *video collection* en lugar de *video database*.

Utilice el siguiente comando para conectarse al clúster de clase Atlas. Debe emitir este comando en el shell de cmd, la aplicación Terminal OSX u otra interfaz de línea de comandos que elija.

```sh
mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics
```

Mientras se conecta a su clúster MongoDB Atlas, o cualquier servidor MongoDB, desde el shell mongo, puede ver el siguiente mensaje de advertencia:

```sh
... WARNING: shell and server versions do not match
```

Este mensaje de advertencia se puede descartar de forma segura a los fines de este curso. El propósito de este mensaje es hacerle saber que la versión del servidor / clúster al que se está conectando tiene una versión diferente a la del shell de mongo de su cliente. Dadas las diferentes versiones entre el servidor y el cliente, puede haber algunos comandos / características de incompatibilidad que el shell del cliente o el servidor no admiten. Sin embargo, en este curso, no encontrará ningún problema de este tipo.

Si encuentra el siguiente error al conectarse a un clúster de atlas, salga del shell Mongo (si está en uno) escribiendo `quit()`. Necesita ejecutar el comando desde su interfaz de línea de comandos.

```sh
2019-01-11T13:35:53.633-0500 E QUERY [js] SyntaxError: missing ; before statement @(shell):1:6
```

### Transcripción

Vamos a conectarnos al Atlas Cluster para esta clase, la que todos hemos estado usando juntos.

Aquí le mostraré cómo hacerlo, pero consulte las **notas de la lectura** para conocer la cadena de conexión exacta y otros parámetros que debe pasar a Mongo para conectarse a nuestro grupo de Atlas.

Es posible que necesitemos cambiar esta información de vez en cuando a medida que hacemos actualizaciones.

Las notas de la lectura también se actualizarán para que tenga los detalles necesarios para conectarse a este clúster desde Mongo Shell.

```sh
mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,
cluster0-shard-00-02-jxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase 
admin --ssl --username m001-student --password m001-mongodb-basics
```
Pegaré esos detalles aquí, y luego hablemos un poco sobre este comando.

<img src="/images/c2/shell-atlas.png">

Quiero señalar un par de cosas aquí.

La primera es que, debido a que nos estamos conectando a un clúster, queríamos darle a Mongo Shell el nombre de todos los servidores en este clúster.

Esos se enumeran aquí hasta aquí.
```sh
mongo "mongodb://
cluster0-shard-00-00-jxeqq.mongodb.net:27017,
cluster0-shard-00-01-jxeqq.mongodb.net:27017,
cluster0-shard-00-02-jxeqq.mongodb.net:27017
```

MongoDB está diseñado para proporcionar acceso de alta disponibilidad a sus datos.

Lo hace permitiéndole mantener copias redundantes de sus datos en un clúster llamado **conjunto de réplicas (replica set)**.

Configuramos nuestro Atlas Cluster para que sea un conjunto de réplica de tres servidores para ayudar a garantizar que siempre tenga acceso a los datos.

Hay miles de estudiantes que toman esta clase.

En el caso de que el servidor primario se caiga debido a una falla de software o hardware, uno de los otros servidores intervendrá para continuar sirviendo datos a los clientes.

Si desea obtener más información sobre cómo funcionan los conjuntos de réplica, consulte la documentación de MongoDB.

Otra cosa que me gustaría señalar sobre este comando es que al final aquí, podemos ver la palabra `test` inmediatamente después de esta barra inclinada.

```sh
mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,
cluster0-shard-00-01-jxeqq.mongodb.net:27017,
cluster0-shard-00-02-jxeqq.mongodb.net:27017/test
?replicaSet=Cluster0-shard-0" 
--authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics
```

Eso indica que nos vamos a conectar a este clúster y lo vamos a conectar a una base de datos llamada `test`.

Ahora, si preferimos conectarnos a una base de datos diferente, podemos cambiar esto a una de las bases de datos que sabemos que está disponible en nuestro Grupo de Atlas.

Pongamos la base de datos **100YWeatherSmall**.

```sh
mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,
cluster0-shard-00-01-jxeqq.mongodb.net:27017,
cluster0-shard-00-02-jxeqq.mongodb.net:27017/100YWeatherSmall
?replicaSet=Cluster0-shard-0" 
--authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics
```
<img src="/images/c2/shell-atlas-100.png">

Y si recuerdas, esa base de datos tiene una colección llamada `data` donde se encuentran todas las lecturas del clima.

Finalmente, lo último que me gustaría señalar sobre este comando es el hecho de que estamos haciendo una conexión segura a este clúster.

Como lo hicimos al conectarnos con Compass, estamos usando una **conexión encriptada a través de SSL** y proporcionamos el mismo nombre de usuario y contraseña que hicimos para conectarnos con Compass.

Así que sigamos adelante y conectemos a nuestro Atlas Cluster con Mongo Shell.

<img src="/images/c2/shell-atlas-connect.png">

Veremos un mensaje que indica que estamos conectados al **primary** para este clúster.

El shell siempre se conectará al primario para un clúster como se especifica aquí, porque es el primario al que normalmente se dirigen la mayoría de las lecturas y al que deben dirigirse todas las escrituras para un clúster MongoDB.

Solo las primarias en un clúster pueden aceptar escrituras y para cualquier clúster, hay una y solo una primaria.

Ahora recuerde, nos conectamos a la base de datos `100YWeatherSmall`.

Si escribimos `show collections`, efectivamente, vemos la recopilación de datos, porque nos hemos conectado a esta base de datos en particular.

Si preferimos acceder a la colección de videos, podemos usar el comando `use video`.

Y luego, si mostramos colecciones aquí con `show collections`, veremos nuestra colección de `movies` (películas).

Ahora voy a mostrarle solo un comando más `db.movies.find().pretty()`, y eso es solo para que podamos ver los documentos que están en esta colección de películas.

¿De acuerdo?

<img src="/images/c2/shell-1.png">

Y aquí, esto debería resultarle familiar dado que ya hemos analizado muchos de estos datos en Compass.

<img src="/images/c2/shell-2.png">

## 6. Examen

**Problem:**

When connecting to an Atlas cluster using the shell, why do we provide the hostnames for all nodes when we launch mongo? Choose the best answer from the choices below.

Choose the best answer:

* So that if the primary node goes down, the shell can connect to other nodes in the cluster instead :+1:

* Because our authentication credentials are not stored on the primary, but on other nodes in our cluster.

* So that other nodes in the cluster can contact our client, if necessary

Th* ere really isn't a good reason

* To make it possible for all the nodes to communicate with each other

## 7. Tema: Crear un clúster de sandbox de Atlas

### Notas de lectura

Descargar materiales del curso

Para comenzar a crear su clúster Atlas Sandbox, visite [https://cloud.mongodb.com/links/registerForAtlas](https://cloud.mongodb.com/links/registerForAtlas) y complete el formulario de creación de cuenta que ve en esa página.

Las instrucciones detalladas para crear un clúster de sandbox se pueden encontrar en el laboratorio inmediatamente después de esta lección.

### Transcripción

Tres de las cuatro operaciones CRUD son **right operations** (operaciones correctas), create, update y delete.

Las tres implican cambiar los datos de una colección de una forma u otra.

**Solo tiene acceso de lectura a nuestro grupo Atlas de clase**.

Afortunadamente, con la opción de nivel gratuito Atlas tenemos un medio para crear un **clúster de Sandbox MongoDB** en minutos y sin costo alguno.

El nivel gratuito de Atlas es ideal para proyectos de prueba de concepto y en las primeras etapas de desarrollo de una aplicación de software.

También es ideal para pruebas y con fines educativos.

Siga y cree su propio clúster de niveles gratuitos de Atlas.

Lo usarás durante el resto de este capítulo.

También he incluido instrucciones para crear su clúster de sandbox en el laboratorio que sigue inmediatamente a esta lección.

OK, aquí necesitamos completar el formulario de registro.

[https://account.mongodb.com/account/register](https://account.mongodb.com/account/register)

<img src="/images/c2/register.png">

Una vez que haya completado el formulario de registro, continúe y haga clic en continuar.

Al completar el formulario, se le pedirá que especifique un nombre de grupo.

Grupos o cómo gestionamos el acceso a los clústeres de Atlas.

Simplemente voy a especificar **shannon-m001** como mi nombre de grupo, y crearé el grupo.

Una vez que haya creado un grupo, continúe y cree un clúster.

<img src="/images/c2/new-cluster.png">

Es posible que reciba una notificación de la integración de intercomunicadores que tenemos aquí cuando haga esto, no dude en ignorarla.

Además, no se asuste por lo que ve cuando aparece por primera vez con respecto a los precios.

El nivel gratuito es realmente gratuito.

Simplemente necesitamos seleccionar el tamaño de instancia **M0**, y notará que el precio ha cambiado a cero.

<img src="/images/c2/m0.png">

Luego, necesitaremos ingresar un nombre de usuario y contraseña.

Una vez que haya ingresado su nombre de usuario y contraseña, puede continuar e implementar su clúster.

Ahora de nuevo, el clúster es libre.

No se requiere tarjeta de crédito para nada de esto.

Asegúrese de recordar el nombre de usuario y la contraseña que ingresó.

Lo necesitarás más tarde.

Ahora, una vez que haya implementado su clúster, deberá esperar solo uno o dos minutos para que el clúster aparezca.

Una vez implementado, el clúster está listo para las conexiones.

<img src="/images/c2/cluster0.png">

Pero antes de conectarnos, necesitamos cambiar una configuración de seguridad para que sea más fácil acceder al clúster desde cualquier lugar.

Haga clic en la opción **Security / Network Access**.

<img src="/images/c2/security.png">

Haga clic en **+ADD IP ADDRESS** Agregar dirección IP y Permitir **ALLOW ACCESS FROM ANYWHERE** acceso desde cualquier lugar y **Confirm**.

Tomará solo un momento actualizar nuestra configuración de seguridad en el clúster.

<img src="/images/c2/active.png">

Mientras eso sucede, **permítanme enfatizar que hacer que un clúster sea accesible desde cualquier lugar no es una buena práctica**.

Debe ser lo más restrictivo posible con respecto a la **IP Whitelist** al configurar un clúster Atlas.

Sin embargo, para los propósitos de esta clase, esto está bien y nos ahorrará tiempo perdido en la depuración de problemas de red.

## 8. Laboratorio 2.0: crear un clúster de sandbox de Atlas

**Problema:**

En este laboratorio deberás completar dos tareas diferentes:

* Crea una cuenta Atlas
* Crear Atlas Sandbox Cluster

### Crear una nueva cuenta Atlas MongoDB:

Si no tiene una cuenta Atlas existente, continúe y [cree una cuenta Atlas](https://account.mongodb.com/account/register) completando los campos requeridos.

### Creación de un clúster de sandbox de Atlas:

1. Después de crear una nueva cuenta, se le pedirá que cree su primer clúster:

   <img src="/images/c2/cluster_create.png">

2. Ahora elegirá un proveedor cloud para su clúster.

   Seleccione AWS como el proveedor de la nube y para la Región, seleccione la ubicación más cercana a usted que tenga la etiqueta Nivel gratuito disponible: tenga en cuenta que la ubicación predeterminada es N.Virginia (us-east-1) Nivel gratuito disponible

   <img src="/images/c2/cluster_provider.png">
   
3. Seleccione Cluster Tier `M0 Sandbox`:

   <img src="/images/c2/cluster_tier.png">
   
4. Haga clic en `Cluster Name`, ingrese `"Sandbox"` en el cuadro de texto correspondiente y haga clic en `"Create Cluster"`:

   <img src="/images/c2/m001_cluster_name.png">
   
5. Después de crear el clúster, será redirigido al panel de la cuenta:

   <img src="/images/c2/account_dashboard.png">
   
   Vaya a la opción `Settings` y cambie el nombre del proyecto a "M001":

    <img src="/images/c2/m001_project_rename.png">
    
6. A continuación, configure los ajustes de seguridad de este clúster, habilitando la *IP Whitelist*:

   Actualice su IP Whitelist para que su aplicación pueda comunicarse con el clúster. Haga clic en la pestaña **"Network Access"** y luego haga clic en **"+ADD ADDRESS"**.

   <img src="/images/c2/m001_ip_whitelisting0.png">
   
   Aparecerá un nuevo mensaje en la pantalla pidiendo **"Add Whitelist Entry"**. Haga clic en **"Allow Access from Anywhere"** y luego haga clic en **"Confirm"**.

   <img src="/images/c2/m001_ip_whitelisting.png">
   
   *Tenga en cuenta que generalmente no recomendamos abrir un clúster Atlas para permitir el acceso desde cualquier lugar. Hacemos eso para esta clase para minimizar los problemas de red con los que se pueda encontrar y poder brindarle un mejor soporte.*

7. Luego, cree la aplicación que necesita el usuario de la base de datos MongoDB para este curso.

   Haga clic en la pestaña **"Database Access"** y luego haga clic en **"ADD NEW USER"**.

   <img src="/images/c2/m001_user0.png">

   Cree un usuario con las siguientes credenciales:

   * username: **`m001-student`**
   * password: **`m001-mongodb-basics`**
   
   Otorgue a este usuario el privilegio de **`Read and write to any database`**:

   <img src="/images/c2/m001_user.png">

   ¡Felicitaciones por crear su primera aplicación Atlas! Ahora tiene una base de datos MongoDB a la que puede acceder desde cualquier parte del mundo. A continuación, lo rellenaremos con datos para analizar y manipular en los próximos laboratorios.

   A creado el Atlas Sandbox Cluster.

## 9. Tema: Conectando a tu Sandbox Cluster desde el mongo Shell

### Notas de lectura

Si se conecta al *nodo secundario* de su conjunto de réplicas de Sandbox, ejecute el comando `rs.slaveOk()` en shell,  para enumerar bases de datos, colecciones y ejecutar consultas de solo lectura. Para realizar operaciones de escritura como insertar, eliminar y actualizar, necesitamos conectarnos solo al *nodo primario*.

Mientras se conecta a su clúster MongoDB Atlas Sandbox desde el shell mongo, puede ver el siguiente mensaje de advertencia:

```sh
Error while trying to show server startup warnings: user is not allowed to do action [getlog] on [admin]
```

Este mensaje de advertencia se puede descartar de forma segura a los fines de este curso.

### Transcripción

Ahora que hemos creado un clúster de sandbox, conectemos el shell Mongo a este clúster, en caso de que no esté 100% claro, a partir de ahora vamos a utilizar dos clústeres Atlas diferentes, uno es el clúster Atlas de clase que utilizamos en el Capítulo uno, donde nos conectamos desde Compass y exploramos los conjuntos de datos, a medida que exploramos el lenguaje de consulta, usaremos ese mismo clúster de vez en cuando, pero también usaremos el clúster sandbox que usted acabomos de crear en Atlas.

En el clúster sandbox tiene permisos completos que le permitirá emitir permisos a los datasets que están en este clúster o, más precisamente, a los datasets que cargaremos en este clúster.

Así que en este punto, visite su clúster de sandbox en Atlas, conectarse al clúster de sandbox es una operación bastante sencilla dado lo que Atlas nos brinda desde la vista Atlas.

<img src="/images/c2/cluster-init.png">

Simplemente necesitamos hacer clic en el botón de conexión

<img src="/images/c2/cluster-connect.png">

y seleccionar **connecting with the Mongo shell** 

<img src="/images/c2/cluster-connect-steps.png">

1. Download and install the mongo shell

* First, download the Mongo Shell

   ```sh
   curl -OL https://downloads.mongodb.org/osx/mongodb-shell-macos-x86_64-4.2.3.tgz
   ```

* Second, extract the Mongo Shell

   ```sh
   tar -xvf mongodb-shell-macos-x86_64-4.2.3.tgz
   ```

* Third, go to the created directory

   ```sh
   cd mongodb-macos-x86_64-4.2.3/bin
   ```
2. Run your connection string in your command line

   ```sh
   mongo "mongodb+srv://cluster0-3bh0e.mongodb.net/test"  --username m001-student --password m001-mongodb-basics
   ```
   
<img src="/images/c2/cluster-connect-shell.png">

## 10. Tema: Carga de datos en su grupo de Sandbox

### Transcripción

Ahora veamos cómo cargar datos en su clúster de sandbox de Atlas.

Así que aquí tenemos un shell conectado al clúster de sandbox.

Veamos que hay ahí.

Podemos ver qué bases de datos hay actualmente en este clúster escribiendo show dbs.

<img src="/images/c2/cluster-connect-shell.png">

Podemos ver que hay una base de datos `admin` y una base de datos `local` (en mi caso presenta varias más).

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> show dbs
admin               0.000GB
local               1.635GB
sample_airbnb       0.053GB
sample_geospatial   0.001GB
sample_mflix        0.042GB
sample_supplies     0.001GB
sample_training     0.068GB
sample_weatherdata  0.004GB
```

Lo que nos gustaría hacer aquí es agregar una base de datos de video que contendrá una versión más detallada del conjunto de datos de la película que hemos visto en el grupo de clase Atlas.

Ahora nuevamente, aquí, estoy **conectado a mi clúster de sandbox Atlas, no al clúster de clase Atlas**.

Para seguirlo, debe conectarse a su propio clúster de sandbox de Atlas.

Entonces, para hacer las cosas un poco más fáciles, lo que me gustaría hacer es alentarlo a configurar una pequeña carpeta en su computadora local donde pueda colocar el handout(folleto) que usaremos para esta lección.

Cuando descargue y descomprima el handout para esta lección, debe tener un directorio titulado LoadMovieDetailsDataset.

Y debería estar en su directorio de descargas.

Lo mismo será cierto si está ejecutando en una máquina Windows, como podemos ver aquí.

Lo que me gustaría animarlo a hacer es navegar a su directorio de inicio y crear una nueva carpeta llamada M001.

Y luego copie esa carpeta, LoadMovieDetailsDataset, en su carpeta M001.

Cuando termine, debería ver esa carpeta dentro de la carpeta M001 en su directorio de inicio.

Hacer lo mismo en Windows se parece a esto.

En su disco duro local, busque usuarios y luego su directorio de inicio.

En este caso, es Shannon, para mí.

Y lo que voy a hacer aquí es crear una nueva carpeta llamada M001, y luego copiar y pegar esa carpeta en mi nueva carpeta M001.

<img src="/images/c2/carpeta-M001.png">

Con esta configuración, será mucho más fácil para nosotros cargar nuestros datos en nuestro clúster Atlast.

A partir de ahora en esta lección, simplemente voy a usar la aplicación de terminal aquí en mi Mac.

OK, entonces lo que voy a hacer ahora es cerrar el shell, porque quiero navegar a la ubicación donde acabo de guardar el conjunto de datos de detalles de la película.

Y debo señalar que el directorio que copié contiene un archivo js que terminará ejecutándose en el shell Mongo.

Lo mismo, por supuesto, es cierto en Windows.

Ahora, para navegar a este directorio, cuando inicie mi aplicación de terminal, estaré en mi directorio de inicio.

Si escribo pwd, esto imprimirá el directorio en el que estoy ubicado actualmente.

Y como se esperaba, es mi directorio de inicio en la Mac.

Si hago una lista de directorios usando el comando ls, puedo ver ese directorio M001 que creé aquí.

Y puedo cd, o cambiar el directorio, en M001 escribiendo `cd M001`.

Nuevamente, haciendo una lista de directorio, puedo ver la carpeta de descargas que puse allí (lo descargue directamente).

Y de nuevo, puedo cd en esa carpeta.

Y luego, con una lista final del directorio, puedo ver el archivo js aquí.

<img src="/images/c2/carpeta-M001-contenido.png">

Bien, haciendo algo similar en Windows, cerrando mi shell.

Y puedo ver desde el indicador que realmente estoy en mi directorio de inicio aquí.

Ahora, la forma en que hago una lista de directorios en Windows es con DIR, que es una abreviatura de directorio.

Veo M001 aquí.

Al igual que con la aplicación de terminal en Mac y otros shells de bash, puedo cambiar el directorio y hacer otra lista de directorios.

Y allí, veo el mismo archivo js.

Ahora un par de cosas para señalar.

Una es que he estado usando la finalización de pestañas para no tener que escribir todo el directorio.

**Si escribe algunos caracteres, presione Tab**, si solo hay un directorio o archivo con ese nombre, por ejemplo, loa, si escribo tab, porque ese es el único directorio con ese nombre aquí, **el símbolo del sistema completará el nombre** para mí para no tener que escribirlo.

Es solo una conveniencia.

Lo mismo funciona exactamente en la aplicación de terminal en Mac y en la mayoría de los otros shells.

La otra cosa que quiero señalar aquí es que **al usar las teclas de flecha, podemos volver al historial de comandos** que hemos ejecutado, **incluso cuando lanzamos el shell Mongo** en el símbolo del sistema.

Y de nuevo, exactamente lo mismo funciona en Mac.

Ahora lo bueno es que si ejecuto el shell Mongo aquí desde la carpeta en la que he guardado mi archivo js, entonces cuando ejecuto el shell Mongo, el shell Mongo tiene como directorio de trabajo el directorio que contiene ese archivo js.

Ahora, el shell Mongo es un intérprete de JavaScript totalmente funcional.

Y si eche un vistazo a ese archivo js, puedo ver que tengo una serie de comandos JavaScript aquí, incluido este último, que carga un montón de datos `less loadMovieDetailsDataset.js`.

<img src="/images/c2/comand-less.png">

La forma en que vamos a obtener datos en su clúster de sandbox es ejecutando este archivo JavaScript.

Pero lo haremos desde el shell de Mongo.

Y puedo ejecutar este archivo usando el comando de carga que me proporciona el shell.

Entonces, simplemente necesito escribir `load("loadMovieDetailsDataset.js")`.

<img src="/images/c2/load-script.png">

Llamo a la función de carga.

Y entre paréntesis, le paso el nombre de nuestro archivo js entre comillas.

Ahora recuerda, actualmente, solo tenemos administrador y local aquí.

Si ejecuto este comando `show dbs`, ahora podemos ver que se ha agregado una base de datos de video.

Y si quiero acceder a los datos en esta base de datos, entonces puedo usar la base de datos `use video` y ver qué colecciones hay usando el comando `show collections`.

<img src="/images/c2/database-movies.png">

Puede seguir exactamente estos mismos pasos en Windows para cargar el conjunto de datos, porque el shell funciona más o menos igual en diferentes plataformas.

Y si quiero echar un vistazo a los datos que están aquí, puedo hacerlo ejecutando este comando `db.movieDetails.find().pretty()`.

<img src="/images/c2/find-collection.png">

Y hablaremos mucho más sobre el comando find mientras nos sumergimos en el lenguaje de consulta MongoDB.

## 11. Tema: Conexión a su clúster de sandbox desde Compass

### Transcripción

Ahora conectemos Compass a su clúster de sandbox Atlas.

Para hacer esto, querrá mirar la vista de clúster para su clúster de sandbox.

Y haga clic en el nombre del clúster aquí.

<img src="/images/c2/11-cluster-init.png">

Es posible que haya nombrado a su clúster de otra manera, pero aquí es donde encuentra el nombre de su clúster.

Haz clic a través de él.

<img src="/images/c2/11-cluster-intances.png">

Y entonces, lo que estamos buscando aquí es lo principal.

Compass actualmente solo nos permite especificar un solo nombre de host en el campo Formulario.

Nos ayuda un poco si tenemos una URI de conexión.

Para nuestros propósitos, será más simple identificar simplemente el primario y luego hacer clic para hacerlo.

Este es el nombre de host que queremos.

<img src="/images/c2/11-cluster-primary.png">

Su nombre de host será diferente porque tiene su propio clúster de sandbox.

Pero para el mío, este es el nombre de host `cluster0-shard-00-01-3bh0e.mongodb.net:`.

Puede encontrar el suyo exactamente en el mismo lugar en la interfaz de usuario de Compass.

Cópialo.

Vuelva a Compass y péguelo en el cuadro Nombre de host.

El puerto ya está provisto para nosotros de manera predeterminada.

Entonces, en este punto, lo que tendremos que hacer es simplemente especificar el nombre de usuario y la contraseña.

Ahora, si siguió las instrucciones para configurar su clúster de sandbox, para crear un usuario administrativo con el nombre de usuario y la contraseña solicitados, aquí debe ingresar, para nombre de usuario, m001-students y para la contraseña m001-mongodb-basics.

El resto de los valores predeterminados deberían estar bien.

<img src="/images/c2/11-compass-my-cluster.png">

Luego podemos hacer clic en Conectar.

Y aquí vemos que, como vimos con show DBs, tenemos todas las bases de datos.

<img src="/images/c2/11-compass-databases.png">

El de interés es la base de datos de video.

Y en la base de datos de video, debería ver una colección de detalles de películas con 2.295 documentos.

<img src="/images/c2/11-compass-video.png">

Podemos hacer clic en `movieDetails` y ver los documentos de la misma manera que podríamos y podemos hacerlo con el clúster de clase Atlas.

<img src="/images/c2/11-compass-video-documents.png">

Así es como conecta Compass a su clúster de sandbox Atlas.

## 12. Laboratorio 2.1: ¿Cuántas comedias?

**Problem:**

1. If you have not already loaded the `video.movieDetails` collection, please review the lesson "Loading Data into Your Sandbox Cluster" for a tutorial. Then, use the script, `loadMovieDetailsDataset.js`, provided in the handouts for the lesson, "Connecting to an Atlas Cluster from the Mongo Shell" to load the `movieDetails` collection.
2. Use Compass to connect to your sandbox cluster.
3. In Compass, view the `video.movieDetails` collection and apply the filter `{genres: "Comedy"}`.

How many documents in `video.movieDetails` match the filter `{genres: "Comedy"}`?

Choose the best answer:

* 457

* 558

* 603

* 749 :+1:

* 816


## 13. Tema: Crear documentos: insertOne() :green_book:

### Transcripción

Ahora comencemos a explorar algunos conceptos básicos de operaciones CRUD.

En MongoDB, generalmente hablamos de operaciones de creación como la inserción de documentos, o simplemente inserciones.

Veamos cómo hacer esto con Compass.

Así que aquí estoy conectado al clúster Atlas gratuito que creé.

<img src="/images/c2/11-compass-video-documents.png">

Nuevamente, este es un sandbox que es perfecto para desarrollar una prueba de concepto o el tipo de demostración y prueba que estoy haciendo en este curso.

Por favor, siga en su propio grupo.

Vamos a crear algunos datos en esta lección.

Primero, me gustaría crear algunos documentos de película simples.

Estos serán en su mayoría descartables.

Así que los pondré en una colección llamada `moviesScratch`.

Usando la vista de la base de datos de Compass, puedo crear una colección.

Si hago clic en la base de datos de video, puedo ver las colecciones que contiene esta base de datos.

<img src="/images/c2/11-compass-video.png">

Tenga en cuenta que la colección `moviesDetails` que ya creamos está aquí.

Si quiero crear una segunda colección haciendo clic en el botón `CREATE COLLECTION`.

<img src="/images/c2/13-create-collection.png">

Nombro mi colección y la creo.

<img src="/images/c2/13-databases.png">

Si hago clic en esta colección, veo que actualmente no tiene documentos.

<img src="/images/c2/13-movies-scratch.png">

Pero puedo crear uno haciendo clic en la opción `Insert Document`.

Creemos un documento con `title : "Rocky" `, `year: 1976` e incluso proporcionaremos el ID de la **[Internet Movie Database (IMDb, en español: Base de datos de películas en Internet)](https://www.imdb.com/)** esto es `imdb : "tt0075148"`.

La interfaz de usuario de Compass me permite pasar las teclas y los valores, por lo que ingresar estos datos es bastante rápido.

Ahora para el año, necesito cambiar el tipo de valor a entero, más específicamente Int32.

<img src="/images/c2/13-movie-rocky.png">

Ahora podemos hacer clic en `INSERT`, y vemos que este documento ahora está en la colección.

<img src="/images/c2/13-document-rocky.png">

La colección tiene un recuento total de documentos de un documento.

Agreguemos otra película  `title: "Creed`, `year: 2015` y `imdb: "tt3076658"`.

<img src="/images/c2/13-movie-creed.png">

Ahora, puedo volver y editar un documento si es necesario.

En este caso, olvidé cambiar el tipo del año a Int32.

Entonces, si hago clic en Editar, 

<img src="/images/c2/13-document-creed.png">

vuelvo a donde estaba y puedo cambiar el tipo de valor por año.

<img src="/images/c2/13-document-creed-update.png">

Hago clic en `UPDATE` y mi documento se actualiza.

<img src="/images/c2/13-document-creed-good.png">

Podemos realizar exactamente la misma operación en el shell Mongo.

Para hacer eso, necesitaremos aprender nuestro primer método en el lenguaje de consulta MongoDB.

Ese método es `insertOne`.

`insertOne` es un método de la clase `collection` en MongoDB.

Para usarlo, primero debemos especificar qué base de datos usar.

Y usaremos la base de datos `video`.

Ahora, lo que sucedió aquí es que cuando intenté usar la base de datos `video` por primera vez, recibí un error de red.

Lo que sucedió aquí es que, como dejé el shell inactivo, había terminado la conexión.

Entonces, cuando intenté emitir un comando que requiere una conexión de red, recibí un error.

Pero el shell también se vuelve a conectar automáticamente al clúster.

Así que aquí, donde intenté usar el video por segunda vez, tuve éxito.

<img src="/images/c2/13-shell-disconect.png">

Si muestro colecciones, podemos ver que tengo tanto mi colección de `movieDetails` como `moviesScratch`.

Ahora usemos `insertOne` para crear otro documento de película en la colección `moviesScratch`.

Ya estoy usando la base de datos de video, entonces `db` se refiere a la base de datos `video`.

Si escribo `db.moviesScratch`, entonces estoy especificando el espacio de nombre completo para la colección.

Y como dije, `insertOne` es un método en la clase de colección, por lo que puedo llamarlo así `db.moviesScratch.insertOne`.

Aquí usaremos `insertOne` para crear otro documento más en la colección `moviesScratch`.

Entonces `db.moviesScratch.insertOne` y le pasamos el documento que deseamos insertar.

```sh
db.moviesScratch.insertOne({title: "Star Trek II: La ira de Khan", year: 1982, imdb: "tt0084726"})
```

La respuesta de llamar a este método es en sí misma un documento.

<img src="/images/c2/13-shell-star-trek.png">

`"acknowledged" : true` significa que tuvimos éxito al hacer el insert.

Y tenga en cuenta que también se nos proporciona el identificador único para este documento creado por MongoDB `"insertedId" : ObjectId("5e3ffecdd519ebad64720694")`.

Ahora, `insertOne` **crea un documento en la colección que especificamos**.

**Este método también creará una colección si aún no existían documentos**.

Ahora, hagamos un poco de experimentación.

Después de insertar el documento, podemos ver en la vista de documentos de Compass que este valor de ID es exactamente igual a que se nos mostró en el shell.

<img src="/images/c2/13-compass-star-trek.png">

Todos los documentos en MongoDB deben contener un campo `_id`.

Este es el identificador único para un documento dado dentro de una colección.

Por lo tanto, todos los valores de `_id` dentro de una sola colección son únicos.

Si no proporcionamos un `_id`, MongoDB o el cliente que estamos usando crearán uno para nosotros, como vimos que sucedió aquí.

Si se crea una `_id` para nosotros, será del tipo `ObjectID`.

Debido a la forma en que se crean, se garantiza que las ID de objeto serán únicas para una colección determinada y aumentarán monotónicamente.

También podemos insertar un documento y proporcionar el valor de ID nosotros mismos.

Hagamos un ejemplo de eso.

Volviendo al shell, en lugar de dejar que MongoDB cree el valor del `_id` por nosotros, le proporcionaremos uno.

Aquí, simplemente voy a usar el valor `imdb` para este documento.

Y voy a dejar todo lo demás igual, incluso este campo, incluso el campo `imdb` por razones que creo que se aclararán un poco más tarde.

<img src="/images/c2/13-shell-star-trek-2.png">

Si presiono Enter, vemos que, nuevamente, recibimos un reconocimiento de que la inserción fue exitosa.

Y vemos que el `_id` es el que proporcionamos en lugar del que se creá automáticamente.

Volviendo a nuestra interfaz de Compass, si actualizamos la colección, podemos ver que tenemos dos documentos que tienen exactamente los mismos datos pero con diferentes valores `_id`.

<img src="/images/c2/13-compass-start-trek-2.png">

En la práctica, en realidad no tendría dos documentos diferentes que contengan los mismos datos, sino simplemente valores diferentes subrayados.

Y en la práctica, querrás asegurarte de que tus valores de `_id` sean de la misma forma en una colección determinada.

Por lo tanto, no querrá tener cadenas como valor de `_id` para algunos de sus documentos e `ObjectId` como valor de `_id` para otros.

Ahora en esta lección, hemos visto cómo insertar documentos en MongoDB un documento a la vez y exploramos cómo hacerlo tanto en Compass como en el shell Mongo.

En otra lección, veremos cómo hacer inserciones masivas en MongoDB usando `insertMany`.

## 14. Tema: Creación de documentos: insertMany()

### Notas de lectura

[insertMany](https://docs.mongodb.com/manual/reference/method/db.collection.insertMany/index.html) Página de documentación del comando .

### Transcripción

En muchos casos, tenemos varios documentos que nos gustaría insertar a la vez en algún tipo de operación por lotes.

El lenguaje de consulta MongoDB tiene un método para esto llamado Insert Many.

Echemos un vistazo a un ejemplo de este comando.

Lo tengo aquí en un editor de texto.

```js
db.moviesScratch.insertMany(
    [
        {
  	    "_id" : "tt0084726",
  	    "title" : "Star Trek II: The Wrath of Khan",
  	    "year" : 1982,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0796366",
  	    "title" : "Star Trek",
  	    "year" : 2009,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0084726",
  	    "title" : "Star Trek II: The Wrath of Khan",
  	    "year" : 1982,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt1408101",
  	    "title" : "Star Trek Into Darkness",
  	    "year" : 2013,
  	    "type" : "movie"
          },
          {
  	    "_id" : "tt0117731",
  	    "title" : "Star Trek: First Contact",
  	    "year" : 1996,
  	    "type" : "movie"
        }
    ]
);
```

Voy a insertar algunos documentos en `videos.moviesScratch`.

Esto es solo para que pueda hacer un poco de noodling por aquí demostrando este comando, y luego deshacerme de esta colección sin corromper los datos que realmente quiero mantener, con respecto a las películas (movies).

Consulte los Handouts de esta lección para descargar los ejemplos aquí para que pueda probarlos usted mismo.

Ahora, en lugar de pasar un solo documento como primer argumento, como hicimos para Insert One, con Insert Many, pasamos una array.

En este ejemplo, el array comienza aquí y termina aquí.

Y puede ver que cada elemento es un documento que nos gustaría insertar en esta colección.

Ahora hay opciones adicionales para Insert Many,.

Y los veremos un poco más adelante.

Por ahora, ejecutemos este comando. (**Antes borramos todos los registros que tenga o borramos la colección `moviesScratch` **)

Solo voy a copiarlo y pegarlo en nuestro shell.

En este shell, estoy conectado a mi Sandbox Atlas cluster.

```sh
192:MongoDB adolfodelarosa$ mongo "mongodb+srv://cluster0-3bh0e.mongodb.net/test"  --username m001-student --password m001-mongodb-basics

MongoDB Enterprise Cluster0-shard-0:PRIMARY> use video
switched to db video

MongoDB Enterprise Cluster0-shard-0:PRIMARY> show collections
movieDetails

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.moviesScratch.insertMany(
...     [
...         {
...       "_id" : "tt0084726",
...       "title" : "Star Trek II: The Wrath of Khan",
...       "year" : 1982,
...       "type" : "movie"
...           },
...           {
...       "_id" : "tt0796366",
...       "title" : "Star Trek",
...       "year" : 2009,
...       "type" : "movie"
...           },
...           {
...       "_id" : "tt0084726",
...       "title" : "Star Trek II: The Wrath of Khan",
...       "year" : 1982,
...       "type" : "movie"
...           },
...           {
...       "_id" : "tt1408101",
...       "title" : "Star Trek Into Darkness",
...       "year" : 2013,
...       "type" : "movie"
...           },
...           {
...       "_id" : "tt0117731",
...       "title" : "Star Trek: First Contact",
...       "year" : 1996,
...       "type" : "movie"
...         }
...     ]
... );
2020-02-14T19:26:11.817+0100 E  QUERY    [js] uncaught exception: BulkWriteError({
	"writeErrors" : [
		{
			"index" : 2,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: test.moviesScratch index: _id_ dup key: { _id: \"tt0084726\" }",
			"op" : {
				"_id" : "tt0084726",
				"title" : "Star Trek II: The Wrath of Khan",
				"year" : 1982,
				"type" : "movie"
			}
		}
	],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
}) :
BulkWriteError({
	"writeErrors" : [
		{
			"index" : 2,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: test.moviesScratch index: _id_ dup key: { _id: \"tt0084726\" }",
			"op" : {
				"_id" : "tt0084726",
				"title" : "Star Trek II: The Wrath of Khan",
				"year" : 1982,
				"type" : "movie"
			}
		}
	],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
BulkWriteError@src/mongo/shell/bulk_api.js:367:48
BulkWriteResult/this.toError@src/mongo/shell/bulk_api.js:332:24
Bulk/this.execute@src/mongo/shell/bulk_api.js:1186:23
DBCollection.prototype.insertMany@src/mongo/shell/crud_api.js:326:5
@(shell):1:1
```

Y podemos ver que lo que sucede aquí se trata de lo que esperaría, dada nuestra comprensión de cómo funciona la inserción de documentos en MongoDB.

Echemos un vistazo más de cerca a esta operación.

Entonces nuestro comando es Insert Many.

Y tenemos 1, 2, 3, 4, 5 documentos que estamos intentando insertar.

Tenga en cuenta, sin embargo, que los documentos 1 y 3 son idénticos.

Con una inserción masiva, es posible que se produzca un error o una excepción para uno de los documentos que está insertando, especialmente si estamos haciendo algo como una operación de limpieza de datos donde hay mucho ruido en los datos o errores de algún tipo en nuestro conjunto de datos.

Para manejar estos problemas con gracia, hay un par de opciones diferentes para Insert Many que debe conocer.

Puede elegir entre inserciones ordenadas (ordered) o inserciones no ordenadas (unordered).

El valor predeterminado es inserciones ordenadas.

Esto proporciona un buen ejemplo, ya que estamos usando valores IMDB como nuestro `_id` para cada documento en esta llamada Insert Many.

De nuevo, estos dos documentos son idénticos.

Pero lo más importante, son sus valores de `_id` los que son idénticos.

Y sabemos que en una colección individual, los valores de `_id` deben ser únicos.

Entonces, si nos desplazamos hacia abajo y vemos qué sucedió con este comando, podemos ver que tenemos un error de clave duplicada (dup key).

Y el error está en la película Wrath of Khan.

También podemos ver, si miramos un poco más abajo en la salida, que el número de documentos insertados fue dos `"nInserted" : 2,`, a pesar de que tenemos cinco documentos en esta llamada Insert Many.

Y si vamos a Compass y nos refrescamos, vemos que hay una película de Star Trek y una película de Wrath of Khan en esta colección.

<img src="/images/c2/14-compass-2movies.png">

Entonces, de hecho, solo dos documentos se insertaron de los cinco que intentamos insertar.

Ahora volvamos a nuestro shell.

Como mencioné, Insert Many hace una inserción ordenada, lo que significa que tan pronto como encuentre un error, dejará de insertar documentos.

Debido a que encontró un error aquí (3er documento), solo estos dos documentos se insertaron en esta colección con esta llamada.

Ahora, en muchas aplicaciones, es posible que simplemente queramos que nuestra aplicación continúe si encuentra un error, porque estamos bien con los documentos que erraron al no insertarse, o tenemos un proceso separado para tratarlos de otra manera.

Para admitir este caso de uso, podemos especificar un segundo argumento para nuestro método Insert Many.

```sh
db.moviesScratch.insertMany(
    [
        {
	    "_id" : "tt0084726",
	    "title" : "Star Trek II: The Wrath of Khan",
	    "year" : 1982,
	    "type" : "movie"
        },
        {
	    "_id" : "tt0796366",
	    "title" : "Star Trek",
	    "year" : 2009,
	    "type" : "movie"
        },
        {
	    "_id" : "tt0084726",
	    "title" : "Star Trek II: The Wrath of Khan",
	    "year" : 1982,
	    "type" : "movie"
        },
        {
	    "_id" : "tt1408101",
	    "title" : "Star Trek Into Darkness",
	    "year" : 2013,
	    "type" : "movie"
        },
        {
	    "_id" : "tt0117731",
	    "title" : "Star Trek: First Contact",
	    "year" : 1996,
	    "type" : "movie"
        }
    ],
    {
        "ordered": false 
    }
);
```
Entonces, volviendo a nuestro editor de texto, el array es el primer argumento para Insert Many.

Como segundo argumento, puedo proporcionar un documento que especifique un valor de `false` para la clave ordenada `"ordered": false`.

Este **documento de Opciones** nos permite cambiar el comportamiento predeterminado.

Y en este caso, en lugar de hacer una inserción ordenada, voy a hacer una inserción no ordenada.

Avancemos y ejecutemos esto ahora.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.moviesScratch.insertMany(
...     [
...         {
...     "_id" : "tt0084726",
...     "title" : "Star Trek II: The Wrath of Khan",
...     "year" : 1982,
...     "type" : "movie"
...         },
...         {
...     "_id" : "tt0796366",
...     "title" : "Star Trek",
...     "year" : 2009,
...     "type" : "movie"
...         },
...         {
...     "_id" : "tt0084726",
...     "title" : "Star Trek II: The Wrath of Khan",
...     "year" : 1982,
...     "type" : "movie"
...         },
...         {
...     "_id" : "tt1408101",
...     "title" : "Star Trek Into Darkness",
...     "year" : 2013,
...     "type" : "movie"
...         },
...         {
...     "_id" : "tt0117731",
...     "title" : "Star Trek: First Contact",
...     "year" : 1996,
...     "type" : "movie"
...         }
...     ],
...     {
...         "ordered": false 
...     }
... );
2020-02-14T20:06:41.273+0100 E  QUERY    [js] uncaught exception: BulkWriteError({
	"writeErrors" : [
		{
			"index" : 0,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: video.moviesScratch index: _id_ dup key: { _id: \"tt0084726\" }",
			"op" : {
				"_id" : "tt0084726",
				"title" : "Star Trek II: The Wrath of Khan",
				"year" : 1982,
				"type" : "movie"
			}
		},
		{
			"index" : 1,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: video.moviesScratch index: _id_ dup key: { _id: \"tt0796366\" }",
			"op" : {
				"_id" : "tt0796366",
				"title" : "Star Trek",
				"year" : 2009,
				"type" : "movie"
			}
		},
		{
			"index" : 2,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: video.moviesScratch index: _id_ dup key: { _id: \"tt0084726\" }",
			"op" : {
				"_id" : "tt0084726",
				"title" : "Star Trek II: The Wrath of Khan",
				"year" : 1982,
				"type" : "movie"
			}
		}
	],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
}) :
BulkWriteError({
	"writeErrors" : [
		{
			"index" : 0,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: video.moviesScratch index: _id_ dup key: { _id: \"tt0084726\" }",
			"op" : {
				"_id" : "tt0084726",
				"title" : "Star Trek II: The Wrath of Khan",
				"year" : 1982,
				"type" : "movie"
			}
		},
		{
			"index" : 1,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: video.moviesScratch index: _id_ dup key: { _id: \"tt0796366\" }",
			"op" : {
				"_id" : "tt0796366",
				"title" : "Star Trek",
				"year" : 2009,
				"type" : "movie"
			}
		},
		{
			"index" : 2,
			"code" : 11000,
			"errmsg" : "E11000 duplicate key error collection: video.moviesScratch index: _id_ dup key: { _id: \"tt0084726\" }",
			"op" : {
				"_id" : "tt0084726",
				"title" : "Star Trek II: The Wrath of Khan",
				"year" : 1982,
				"type" : "movie"
			}
		}
	],
	"writeConcernErrors" : [ ],
	"nInserted" : 2,
	"nUpserted" : 0,
	"nMatched" : 0,
	"nModified" : 0,
	"nRemoved" : 0,
	"upserted" : [ ]
})
BulkWriteError@src/mongo/shell/bulk_api.js:367:48
BulkWriteResult/this.toError@src/mongo/shell/bulk_api.js:332:24
Bulk/this.execute@src/mongo/shell/bulk_api.js:1186:23
DBCollection.prototype.insertMany@src/mongo/shell/crud_api.js:326:5
@(shell):1:1
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Ahora aquí, en realidad vemos tres errores de escritura.

Hay tres errores de claves duplicadas (dup key): uno para Wrath of Khan, uno para Star Trek y otro para Wrath of Khan.

Echemos un vistazo a nuestra consulta nuevamente.

Entonces, estas tres inserciones causaron un error de clave duplicada (duplicate key error).

¿Porqué es eso?

Bueno, si recuerdas, los dos primeros ya estaban en la colección por nuestro intento anterior de usar Insert Many.

Entonces, un intento de insertar los dos primeros nuevamente obtuvo errores de clave duplicados debido a esa inserción anterior.

El tercero, por supuesto, tiene exactamente el mismo `_id` que el primero, por lo que falla por la misma razón que el primer documento.

Lo interesante aquí es que, en lugar de simplemente eliminar el error y dejar de ejecutar inserciones, Insert Many continuó e insertó los dos documentos 4 y 5 (`"nInserted" : 2,)`: Star Trek Into Darkness y Star Trek First Contact.

Si vamos a Compass y actualizamos nuestra vista de colección, podemos ver que ahora hay cuatro documentos en esta colección: Wrath of Khan, Star Trek, Star Trek into Darkness y Star Trek First Contact.

<img src="/images/c2/14-compass-4movies.png">

Entonces, al especificar `false` para ordenado, aunque haya, en este caso, tres errores de clave duplicados, Insert Many continua insertando documentos.

Y terminá insertando los dos últimos documentos en nuestra lista de documentos que pasamos a Insert Many.

En esta lección, vimos Insert Many y las versiones ordenadas y desordenadas de una operación Insert Many.

Insert One e Insert Many son dos formas principales en las que podemos crear documentos.

Hay un par de otras formas en que podemos crear documentos mediante el uso de operaciones de actualización.

Los comandos de actualización pueden provocar la inserción de documentos.

Llamamos a estas operaciones **Upserts**.

Discutimos Upserts en detalle en otra lección.

Con esto, hemos discutido, con cierto detalle, las formas principales en las que creamos documentos en MongoDB e insinuamos la tercera forma en que los datos se pueden insertar en una colección de MongoDB.

Y con eso, hemos abordado la parte Create (Crear) de nuestras operaciones CRUD.

## 15. Laboratorio 2.2: ¿Cuántos insertados?

Lab 2.2: How Many Inserted?

Problem:

If the collection `video.myMovies` is currently empty, how many documents would be inserted by the following call to `insertMany()`.

```sh
db.myMovies.insertMany(
  [
    {
      "_id" : "tt0084726",
      "title" : "Star Trek II: The Wrath of Khan",
      "year" : 1982,
      "type" : "movie"
    },
    {
      "_id" : "tt0796366",
      "title" : "Star Trek",
      "year" : 2009,
      "type" : "movie"
    },
    {
      "_id" : "tt0084726",
      "title" : "Star Trek II: The Wrath of Khan",
      "year" : 1982,
      "type" : "movie"
    },
    {
      "_id" : "tt1408101",
      "title" : "Star Trek Into Darkness",
      "year" : 2013,
      "type" : "movie"
    },
    {
      "_id" : "tt0117731",
      "title" : "Star Trek: First Contact",
      "year" : 1996,
      "type" : "movie"
    }
  ],
  {
    ordered: false
  }
);
```

Choose the best answer:

* 1

* 2

* 3

* 4 :+1:

* 5

## 16. Tema: Lectura de documentos: campos escalares

### Notas de lectura

Notas

* Para seguir esta conferencia, debe conectarse al **cluster de clases (class cluster)**.

* Como recordatorio, use el siguiente comando para conectarse al clúster de clase Atlas. Debe ejecutar este comando en el `cmd` shell, la aplicación de terminal OSX u otra interfaz de línea de comandos que elija.

```sh
mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics
```

* En el minuto 3:30, Shannon menciona que no es necesario usar comillas al compilar sus consultas en MongoDB Compass. ¡Esto ya no es correcto! Debe proporcionar comillas al usar la notación de puntos para expresar consultas.

* Es una buena práctica rodear las keys con comillas en cualquier situación.

### Transcripción

Recuerde conectarse en el shell y en Compas en el **class cluster**

<img src="/images/c2/16-shell-class-cluster.png">

<img src="/images/c2/16-compass-class-cluster.png">

Para comenzar nuestra discusión sobre las operaciones de lectura en el lenguaje de consulta MongoDB, analizaremos los filtros de igualdad.

Esta discusión sentará las bases para nuestros tipos más sofisticados de consultas que involucran el amplio conjunto de operadores proporcionados por el lenguaje de consulta MongoDB.

Ya has visto algunos ejemplos de filtros de igualdad en nuestro trabajo con filtros en Compass.

Veamos primero otro filtro en Compass, y luego profundizaremos en los métodos que componen el lenguaje de consulta MongoDB a medida que se implementa en el shell mongo.

Para esta lección, me gustaría ver los datos de la película.

Usemos Compass para filtrar nuestra colección `video.movies` para películas con clasificación `PG-13`.

La calificación que nos interesa se almacena en el campo `mpaaRating` para los documentos de esta colección.

En este filtro, estoy especificando que simplemente quiero ver los documentos que tienen `PG-13` como su valor para este campo.

<img src="/images/c2/16-compass-mpaaRating-PG-13.png">

Aplicando el filtro aquí en la pestaña Documentos, podemos ver que cada documento devuelto tiene una clasificación de hecho `PG-13`.

Tenga en cuenta que solo 5,295 de los casi un millón de documentos en esta colección coinciden con este filtro.

Podemos restringir aún más el conjunto de resultados agregando selectores adicionales a nuestro documento de consulta.

<img src="/images/c2/16-compass-mpaaRating-PG-13-year-2009.png">

La aplicación de este filtro reduce significativamente la cantidad de documentos que son conjuntos de resultados.

Entonces, lo que es **importante** notar aquí es que los selectores en los filtros MongoDB se agrupan de manera predeterminada, lo que significa que **solo se recuperan los documentos que coinciden con estos dos criterios**.

En otras lecciones consideramos **operadores de consulta que nos permiten expresar filtros que coinciden con cualquiera de dos o más condiciones**.

En lugar de todas las dos o más condiciones que tenemos aquí, pero eso es para otra lección.

Ahora echemos un vistazo a cómo realizar este mismo tipo de consulta en el shell mongo.

El método para realizar operaciones de lectura en el lenguaje de consulta MongoDB se llama **`find`**.

Para encontrar películas en nuestra colección de `videos.movies` con clasificación `PG-13`, primero debemos usar nuestra base de datos de video y luego llamar a buscar en la colección de la película (movies).

Y podemos mejorar eso un poco usando el comando `pretty`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use video
switched to db video
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ mpaaRating: "PG-13"}).pretty()
...
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7099d4ee0af9e1b14f"),
	"title" : "Hell's Belles",
	"year" : 1969,
	"imdbId" : "tt0063058",
	"mpaaRating" : "PG-13",
	"genre" : "Action, Drama, Thriller",
	"viewerRating" : 5.8,
	"viewerVotes" : 156,
	"runtime" : 95,
	"director" : "Maury Dexter",
	"cast" : [
		"Jeremy Slate",
		"Adam Roarke",
		"Jocelyn Lane",
		"Angelique Pettyjohn"
	],
	"plot" : "When hot-headed Dan out-drives the thoroughly vicious Tony in a motorcycle race and wins a brand new bike, he sets in motion a chain of events that includes one blazing gas station and a disastrous rock slide.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7099d4ee0af9e1aff6"),
	"title" : "Bandolero!",
	"year" : 1968,
	"imdbId" : "tt0062708",
	"mpaaRating" : "PG-13",
	"genre" : "Action, Crime, Drama",
	"viewerRating" : 6.5,
	"viewerVotes" : 2921,
	"runtime" : 106,
	"director" : "Andrew V. McLaglen",
	"cast" : [
		"James Stewart",
		"Dean Martin",
		"Raquel Welch",
		"George Kennedy"
	],
	"plot" : "Mace Bishop masquerades as a hangman in order to save his outlaw brother from the gallows, runs to Mexico chased by the sheriff's posse and fights against Mexican bandits.",
	"language" : "English, Spanish"
}
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y aquí, como vimos en Compass, podemos ver que todos nuestros resultados tienen una clasificación `mpaaRating` de `PG-13`.

Nuevamente, como hicimos en Compass, podemos agregar un selector adicional por año y restringir nuestros resultados a películas `PG-13` a partir del año 2009.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ mpaaRating: "PG-13", year: 2009 }).pretty()
...
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c9b99d4ee0af9e76ced"),
	"title" : "Avatar",
	"year" : 2009,
	"imdbId" : "tt0499549",
	"mpaaRating" : "PG-13",
	"genre" : "Action, Adventure, Fantasy",
	"viewerRating" : 7.9,
	"viewerVotes" : 811261,
	"runtime" : 162,
	"director" : "James Cameron",
	"cast" : [
		"Sam Worthington",
		"Zoe Saldana",
		"Sigourney Weaver",
		"Stephen Lang"
	],
	"plot" : "A paraplegic marine dispatched to the moon Pandora on a unique mission becomes torn between following his orders and protecting the world he feels is his home.",
	"language" : "English, Spanish"
}
{
	"_id" : ObjectId("58c59c9c99d4ee0af9e77382"),
	"title" : "I Hate Valentine's Day",
	"year" : 2009,
	"imdbId" : "tt0762105",
	"mpaaRating" : "PG-13",
	"genre" : "Comedy, Romance",
	"viewerRating" : 4.7,
	"viewerVotes" : 6215,
	"runtime" : 98,
	"director" : "Nia Vardalos",
	"cast" : [
		"Nia Vardalos",
		"John Corbett",
		"Stephen Guarino",
		"Amir Arison"
	],
	"plot" : "A florist, who abides by a strict five-date-limit with any man, finds herself wanting more with the new restaurateur in town.",
	"language" : "English"
}
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y nuevamente, vemos que de hecho todos estos documentos coinciden con ambos criterios.

Ahora que sabemos cómo crear filtros de calidad en los campos de nivel superior, centremos nuestra atención en cómo podemos igualar la igualdad con los documentos incrustados.

Para esto, veamos nuestro conjunto de datos meteorológicos de 100 años `100YWeatherSmall`.

Las lecturas del clima en este conjunto de datos capturan bastantes detalles sobre el viento (wind).

Tenga en cuenta que el campo `wind` tiene su valor de tipo document u Object, como se llama en la vista de documentos en Compass.

Si expandimos `wind`, el campo de viento describe las condiciones generales del viento.

<img src="/images/c2/16-compass-wind.png">

Un valor de `C` para el tipo de viento `type` indica que las condiciones del viento eran tranquilas cuando se tomó esta lectura.

En el lenguaje de consulta, nuestro filtro tendría este formulario.

Y después de aplicar ese filtro, echemos un vistazo a un par de documentos coincidentes.

<img src="/images/c2/16-compass-wind-type-C.png">

Podemos ver aquí que, de hecho, el tipo de viento es `C`.

Y del mismo modo para este documento y todos los demás en este conjunto de resultados.

Echando un vistazo más de cerca a nuestra sintaxis de consulta, estamos utilizando lo que llamamos notación de punto para identificar un campo de un documento anidado dentro del campo `wind` .

Ahora la notación de puntos (`"wind.type"`) funciona para documentos anidados en cualquier nivel.

Para esto, vamos un nivel más profundo y veamos el ángulo de dirección del viento (`"wind.direction.angle"`).

Entonces, en lugar del tipo de ventana, expresaremos nuestro filtro como `wind.direction.angle`.

Y como el ángulo es un campo de valor entero, hagamos 290 y luego apliquemos nuestro filtro.

<img src="/images/c2/16-compass-wind-direction-angle.png">

Echando un vistazo a los resultados, el viento, la dirección, el ángulo, podemos ver que, de hecho, los documentos en el conjunto de resultados coinciden con esta consulta utilizando la notación de puntos.

Una cosa que me gusta mucho del lenguaje de consulta MongoDB es que es intuitivo en muchos aspectos.

La notación de puntos es, en mi opinión, un ejemplo.

**Cuando utilice la notación de puntos en Compass o en el shell, debe encerrar la clave entre comillas**.

Primero, usamos la base de datos de 100 años, y luego usamos find para expresar nuestro filtro.

Si no usamos las comillas, obtendremos un error porque, como mencioné, se requieren comillas alrededor de la key que usan notación de puntos.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use 100YWeatherSmall
switched to db 100YWeatherSmall
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.data.find({"wind.direction.angle": 290}).pretty()
...
	"dewPoint" : {
		"value" : 999.9,
		"quality" : "9"
	},
	"pressure" : {
		"value" : 1015.1,
		"quality" : "1"
	},
	"wind" : {
		"direction" : {
			"angle" : 290,
			"quality" : "1"
		},
		"type" : "N",
		"speed" : {
			"rate" : 17,
			"quality" : "1"
		}
	},
	"visibility" : {
		"distance" : {
			"value" : 500,
			"quality" : "1"
		},
		"variability" : {
			"value" : "N",
			"quality" : "9"
		}
	},
	"skyCondition" : {
		"ceilingHeight" : {
			"value" : 30,
			"quality" : "1",
			"determination" : "C"
		},
		"cavok" : "N"
	},
	"sections" : [
		"AG1",
		"AY1",
		"GA1",
		"GF1",
		"MD1",
		"MW1",
		"UA1"
	],
...
```

Y aquí podemos ver que, de hecho, los documentos que coinciden con esta consulta tienen el `wind.direction.angle` de 290.

En resumen, para reducir la jerarquía de documentos anidados que alcanza en documentos en cada nivel adicional de anidamiento al unir los nombres de los campos mediante la notación de puntos, **no olvide las comillas**.

## 17. Examen Reading Documents: Scalar Fields

**Problem:**

Explore the movieDetails collection that you loaded into your Atlas sandbox cluster and then issue a query to answer the following question. How many movies in the movieDetails collection have exactly 2 award wins and 2 award nominations?

You will find the [`count()`](https://docs.mongodb.com/manual/reference/method/cursor.count/) method useful in answering this question using the mongo shell.

Choose the best answer:

* 3

* 7

* 12 :+1:

* 15

* 20


## 18. Laboratorio 2.3: Consultas en Campos Escalares

Lab 2.3: Queries on Scalar Fields

**Problem:**

Explore the movieDetails collection that you loaded into your Atlas sandbox cluster and then issue a query to answer the following question. How many movies in the movieDetails collection are rated PG and have exactly 10 award nominations?

You will find the [`count()`](https://docs.mongodb.com/manual/reference/method/cursor.count/) method useful in answering this question using the mongo shell.

Choose the best answer:

* 0

* 1

* 3 :+1:

* 6

* 11


## 19. Tema: Lectura de Documentos: Campos Array

### Notas de lectura

A las 4:01, utilizamos la consulta `{cast.0: "Jeff Bridges"}`, que funcionaba con la versión de Compass utilizada en ese momento.

Con versiones actuales, siempre que el operador se utiliza en una clave para indicar una clave secundaria como "imdb.rating" o cuando se utiliza. para utilizar una consulta posicional como "cast.0", la clave debe estar entre comillas como esta:

```sh
{ "cast.0": "Jeff Bridges" }
```

### Transcripción

Ahora hablemos de coincidencias exactas para campos array.

Nuevamente, estamos preparando el escenario aquí para una comprensión completa del lenguaje de consulta MongoDB, incluido el uso de una variedad de operadores que nos permiten hacer una serie de consultas muy sofisticadas.

Por lo tanto, al considerar las coincidencias de igualdad en los arrays, podemos considerar las coincidencias en todo el array, las coincidencias basadas en cualquier elemento del array y las basadas en un elemento específico, por ejemplo, haciendo coincidir solo los arrays cuyo primer elemento coincide con un criterio específico.

<img src="/images/c2/19-compass-the-big-lebowski.png">

También podemos ver coincidencias más complejas utilizando operadores.

Lo haremos en otras lecciones.

Aquí estamos mirando nuestra base de datos de video en Compass.

Echemos un vistazo a esta misma base de datos en el shell Mongo.

Bien, aquí, en el shell Mongo, estoy conectado a nuestro cluster Atlas de clase.

Usemos la base de datos de video y luego, en este primer ejemplo, filtraremos por una coincidencia exacta con un array.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use video
switched to db video
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({cast: ["Jeff Bridges", "Tim Robbins"]}).pretty()
{
	"_id" : ObjectId("58c59cd599d4ee0af9ef0c1e"),
	"title" : "Making of Arlington Road",
	"year" : 1999,
	"imdbId" : "tt3522952",
	"genre" : "Documentary",
	"runtime" : 30,
	"director" : "Richard Leyland",
	"cast" : [
		"Jeff Bridges",
		"Tim Robbins"
	],
	"plot" : "The making of the film 'Arlington Road' with interviews with the leading actors Jeff Bridges and Tim Robbins illustrated with clips from the film.",
	"language" : "English"
}
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Ahora, para este tipo de filtros, **los documentos coincidentes deben tener un valor que sea exactamente el que hemos ingresado como array** `cast: ["Jeff Bridges", "Tim Robbins"]`.

Entonces, para la consulta de reparto(cast), y luego para buscar a Jeff Bridges y Tim Robbins, **los documentos coincidentes tendrán un campo cast que tiene como valor un array con exactamente dos elementos: Jeff Bridges, seguido de Tim Robbins, en ese orden**.

Solo hay una película que coincide con estos criterios.

Es un documental llamado "Making of Arlington Road". **Tenga en cuenta que el campo de array coincide exactamente con nuestro filtro**.

También tenga en cuenta que, debido a la **semántica de los array matches**, la película "Arlington Road" en sí misma **no coincide, porque también incluye a Joan Cusack y Hope Davis, además de Jeff Bridges y Tim Robbins, en el elenco**.

Hagamos una consulta rápida para eso.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({title: "Arlington Road"}).pretty()
{
	"_id" : ObjectId("58c59c7899d4ee0af9e2b101"),
	"title" : "Arlington Road",
	"year" : 1999,
	"imdbId" : "tt0137363",
	"mpaaRating" : "R",
	"genre" : "Crime, Drama, Thriller",
	"viewerRating" : 7.2,
	"viewerVotes" : 66888,
	"runtime" : 117,
	"director" : "Mark Pellington",
	"cast" : [
		"Jeff Bridges",
		"Tim Robbins",
		"Joan Cusack",
		"Hope Davis"
	],
	"plot" : "A man begins to suspect his neighbors are not what they appear to be and their secrets could be deadly.",
	"language" : "English"
}
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y aquí podemos ver que Joan Cusack y Hope Davis también figuran en el elenco.

Como nuestra **consulta original especificaba que queremos que la conversión sea exactamente este array** `cast: ["Jeff Bridges", "Tim Robbins"]`, este documento en particular no coincide.

Ahora es más común que filtremos por un solo elemento de un campo de array.

Para hacer esto en MongoDB, simplemente podemos filtrar, por ejemplo, Jeff Bridges, de esta manera.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ cast: "Jeff Bridges" }).pretty()
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1baba"),
	"title" : "Halls of Anger",
	"year" : 1970,
	"imdbId" : "tt0065810",
	"mpaaRating" : "R",
	"genre" : "Drama",
	"viewerRating" : 5.9,
	"viewerVotes" : 245,
	"runtime" : 96,
	"director" : "Paul Bogart",
	"cast" : [
		"Calvin Lockhart",
		"Janet MacLachlan",
		"Jeff Bridges",
		"James A. Watson Jr."
	],
	"plot" : "An all-black inner city school has to become an integrated school. Few dozen white kids are transfered there, but the black students are aggressively opposed to this. The school then approaches a tough black teacher for help.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1bb01"),
	"title" : "In Search of America",
	"year" : 1971,
	"imdbId" : "tt0065885",
	"genre" : "Drama",
	"viewerRating" : 5.4,
	"viewerVotes" : 111,
	"runtime" : 90,
	"director" : "Paul Bogart",
	"cast" : [
		"Carl Betz",
		"Vera Miles",
		"Ruth McDevitt",
		"Jeff Bridges"
	],
	"plot" : "A college dropout convinces his family to re-examine its goals and gets them to leave it all for a cross-country odyssey in a 1928 Greyhound bus.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1bd99"),
	"title" : "The Yin and the Yang of Mr. Go",
	"year" : 1970,
	"imdbId" : "tt0066592",
	"mpaaRating" : "PG",
	"genre" : "Fantasy, Mystery, Thriller",
	"viewerRating" : 3.6,
	"viewerVotes" : 168,
	"runtime" : 89,
	"director" : "Burgess Meredith",
	"cast" : [
		"James Mason",
		"Jack MacGowran",
		"Irene Tsu",
		"Jeff Bridges"
	],
	"plot" : "Buddha has the power to change the nature of a person into their opposite. He uses this power only when the world is in danger. When a villain obtains plans that could be used for peace or war, Buddha turns him into a good guy. Now what?",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1c007"),
	"title" : "The Last Picture Show",
	"year" : 1971,
	"imdbId" : "tt0067328",
	"mpaaRating" : "R",
	"genre" : "Drama",
	"viewerRating" : 8.1,
	"viewerVotes" : 29214,
	"runtime" : 118,
	"director" : "Peter Bogdanovich",
	"cast" : [
		"Timothy Bottoms",
		"Jeff Bridges",
		"Cybill Shepherd",
		"Ben Johnson"
	],
	"plot" : "A group of 1950s high schoolers come of age in a bleak, isolated, atrophied West Texas town that is slowly dying, both economically and culturally.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1c2ee"),
	"title" : "Bad Company",
	"year" : 1972,
	"imdbId" : "tt0068245",
	"mpaaRating" : "PG",
	"genre" : "Drama, Western",
	"viewerRating" : 7.1,
	"viewerVotes" : 2423,
	"runtime" : 93,
	"director" : "Robert Benton",
	"cast" : [
		"Jeff Bridges",
		"Barry Brown",
		"Jim Davis",
		"David Huddleston"
	],
	"plot" : "A god-fearing Ohio boy dodging the Civil War draft arrives in Jefferson City where he joins up with a hardscrabble group of like runaways heading west.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1c423"),
	"title" : "Fat City",
	"year" : 1972,
	"imdbId" : "tt0068575",
	"mpaaRating" : "PG",
	"genre" : "Drama, Sport",
	"viewerRating" : 7.5,
	"viewerVotes" : 4798,
	"runtime" : 100,
	"director" : "John Huston",
	"cast" : [
		"Stacy Keach",
		"Jeff Bridges",
		"Susan Tyrrell",
		"Candy Clark"
	],
	"plot" : "Two men, working as professional boxers, come to blows when their careers each begin to take opposite momentum.",
	"language" : "English, Spanish"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1c9d5"),
	"title" : "The Iceman Cometh",
	"year" : 1973,
	"imdbId" : "tt0070212",
	"mpaaRating" : "PG",
	"genre" : "Drama",
	"viewerRating" : 7.5,
	"viewerVotes" : 967,
	"runtime" : 239,
	"director" : "John Frankenheimer",
	"cast" : [
		"Lee Marvin",
		"Fredric March",
		"Robert Ryan",
		"Jeff Bridges"
	],
	"plot" : "A salesman with a sudden passion for reform has an idea to sell to his barfly buddies: throw away your pipe dreams. The drunkards, living in a flophouse above a saloon, resent the idea.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1ca26"),
	"title" : "The Last American Hero",
	"year" : 1973,
	"imdbId" : "tt0070287",
	"mpaaRating" : "PG",
	"genre" : "Drama, Sport",
	"viewerRating" : 6.3,
	"viewerVotes" : 1016,
	"runtime" : 95,
	"director" : "Lamont Johnson",
	"cast" : [
		"Jeff Bridges",
		"Valerie Perrine",
		"Geraldine Fitzgerald",
		"Ned Beatty"
	],
	"plot" : "A young hellraiser quits his moonshine business to try to become the best NASCAR racer the south had ever seen.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1ca69"),
	"title" : "Lolly-Madonna XXX",
	"year" : 1973,
	"imdbId" : "tt0070332",
	"mpaaRating" : "PG",
	"genre" : "Drama",
	"viewerRating" : 6.3,
	"viewerVotes" : 265,
	"runtime" : 103,
	"director" : "Richard C. Sarafian",
	"cast" : [
		"Rod Steiger",
		"Robert Ryan",
		"Jeff Bridges",
		"Scott Wilson"
	],
	"plot" : "Two rustic families, headed by patriarchs Laban Feather and Pap Gutshall, are feuding. At first, it is comical, with just the sons of the two families playing tricks on each other. But soon...",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1d3c0"),
	"title" : "Hearts of the West",
	"year" : 1975,
	"imdbId" : "tt0073096",
	"mpaaRating" : "PG",
	"genre" : "Western, Comedy",
	"viewerRating" : 6.4,
	"viewerVotes" : 934,
	"runtime" : 102,
	"director" : "Howard Zieff",
	"cast" : [
		"Jeff Bridges",
		"Andy Griffith",
		"Donald Pleasence",
		"Blythe Danner"
	],
	"plot" : "Lewis Tater writes Wild West dime novels and dreams of actually becoming a cowboy. When he goes west to find his dream he finds himself in possession of the loot box of two crooks who tried...",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1d12d"),
	"title" : "Thunderbolt and Lightfoot",
	"year" : 1974,
	"imdbId" : "tt0072288",
	"mpaaRating" : "R",
	"genre" : "Comedy, Crime, Drama",
	"viewerRating" : 7.1,
	"viewerVotes" : 14675,
	"runtime" : 115,
	"director" : "Michael Cimino",
	"cast" : [
		"Clint Eastwood",
		"Jeff Bridges",
		"George Kennedy",
		"Geoffrey Lewis"
	],
	"plot" : "With the help of an irreverent young sidekick, a bank robber gets his old gang back together to organize a daring new heist.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1d5e7"),
	"title" : "Rancho Deluxe",
	"year" : 1975,
	"imdbId" : "tt0073605",
	"mpaaRating" : "R",
	"genre" : "Comedy, Romance, Western",
	"viewerRating" : 6.4,
	"viewerVotes" : 1164,
	"runtime" : 93,
	"director" : "Frank Perry",
	"cast" : [
		"Jeff Bridges",
		"Sam Waterston",
		"Elizabeth Ashley",
		"Clifton James"
	],
	"plot" : "Two drifters, of widely varying backgrounds, rustle cattle and try to avoid being caught in contemporary Montana.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1d969"),
	"title" : "King Kong",
	"year" : 1976,
	"imdbId" : "tt0074751",
	"mpaaRating" : "PG",
	"genre" : "Adventure, Fantasy, Horror",
	"viewerRating" : 5.8,
	"viewerVotes" : 20784,
	"runtime" : 134,
	"director" : "John Guillermin",
	"cast" : [
		"Jeff Bridges",
		"Charles Grodin",
		"Jessica Lange",
		"John Randolph"
	],
	"plot" : "A petroleum exploration expedition comes to an isolated island and encounters a colossal giant gorilla.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7199d4ee0af9e1db4e"),
	"title" : "Stay Hungry",
	"year" : 1976,
	"imdbId" : "tt0075268",
	"mpaaRating" : "R",
	"genre" : "Drama, Comedy",
	"viewerRating" : 5.7,
	"viewerVotes" : 2831,
	"runtime" : 102,
	"director" : "Bob Rafelson",
	"cast" : [
		"Jeff Bridges",
		"Sally Field",
		"Arnold Schwarzenegger",
		"R.G. Armstrong"
	],
	"plot" : "A syndicate wants to buy a whole district to rebuild it. They've bought every house except the small gym \"Olympic\", where Mr. Austria Joe Santo prepares for the Mr. Universum championships ...",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7299d4ee0af9e1e6da"),
	"title" : "The American Success Company",
	"year" : 1980,
	"imdbId" : "tt0078765",
	"mpaaRating" : "PG",
	"genre" : "Comedy, Drama",
	"viewerRating" : 5.7,
	"viewerVotes" : 205,
	"runtime" : 91,
	"director" : "William Richert",
	"cast" : [
		"Jeff Bridges",
		"Belinda Bauer",
		"Ned Beatty",
		"Steven Keats"
	],
	"plot" : "A husband is humiliated at home and at work. He decides he has had enough of it and hires a prostitute to help him get back at his boss, wife and friends and get a lot richer in the process.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7299d4ee0af9e1e5a6"),
	"title" : "Somebody Killed Her Husband",
	"year" : 1978,
	"imdbId" : "tt0078294",
	"mpaaRating" : "PG",
	"genre" : "Comedy, Crime, Mystery",
	"viewerRating" : 4.9,
	"viewerVotes" : 268,
	"runtime" : 97,
	"director" : "Lamont Johnson",
	"cast" : [
		"Farrah Fawcett",
		"Jeff Bridges",
		"John Wood",
		"Tammy Grimes"
	],
	"plot" : "A woman's husband is murdered and she and her lover must find the killer or stand accused of doing it themselves.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7299d4ee0af9e1ec10"),
	"title" : "Winter Kills",
	"year" : 1979,
	"imdbId" : "tt0080139",
	"mpaaRating" : "R",
	"genre" : "Drama, Thriller",
	"viewerRating" : 6.4,
	"viewerVotes" : 1324,
	"runtime" : 97,
	"director" : "William Richert",
	"cast" : [
		"Jeff Bridges",
		"John Huston",
		"Anthony Perkins",
		"Eli Wallach"
	],
	"plot" : "The Pickering Commission concluded that a lone gunman killed the US President in 1960, in Philadelphia, but 19 years later a dying man confesses to be one of the real hit-men who killed President Kegan, sparking an investigation.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7299d4ee0af9e1f303"),
	"title" : "Cutter's Way",
	"year" : 1981,
	"imdbId" : "tt0082220",
	"mpaaRating" : "R",
	"genre" : "Crime, Drama, Mystery",
	"viewerRating" : 6.9,
	"viewerVotes" : 3287,
	"runtime" : 109,
	"director" : "Ivan Passer",
	"cast" : [
		"Jeff Bridges",
		"John Heard",
		"Lisa Eichhorn",
		"Ann Dusenberry"
	],
	"plot" : "Richard spots a man dumping a body, and decides to expose the man he thinks is the culprit with his friend Alex Cutter.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7299d4ee0af9e1fa39"),
	"title" : "The Last Unicorn",
	"year" : 1982,
	"imdbId" : "tt0084237",
	"mpaaRating" : "G",
	"genre" : "Family, Animation, Fantasy",
	"viewerRating" : 7.5,
	"viewerVotes" : 17586,
	"runtime" : 92,
	"director" : "Jules Bass, Arthur Rankin Jr.",
	"cast" : [
		"Alan Arkin",
		"Jeff Bridges",
		"Mia Farrow",
		"Tammy Grimes"
	],
	"plot" : "A brave unicorn and a magician fight an evil king who is obsessed with attempting to capture the world's unicorns.",
	"language" : "English, German"
}
{
	"_id" : ObjectId("58c59c7299d4ee0af9e1fa2b"),
	"title" : "Kiss Me Goodbye",
	"year" : 1982,
	"imdbId" : "tt0084210",
	"mpaaRating" : "PG",
	"genre" : "Comedy, Fantasy, Romance",
	"viewerRating" : 6.1,
	"viewerVotes" : 1353,
	"runtime" : 101,
	"director" : "Robert Mulligan",
	"cast" : [
		"Sally Field",
		"James Caan",
		"Jeff Bridges",
		"Paul Dooley"
	],
	"plot" : "Not until three years after the death of her husband Jolly, Kay dares to move back into their former home, persuaded by her new fianc�e Rupert. But soon her worst expectations come true, ...",
	"language" : "English"
}
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Los documentos coincidentes contienen la cadena Jeff Bridges como un elemento del reparto del campo de array.

Podemos ver que Jeff Bridges está aquí.

Si nos desplazamos a través de los documentos del array, aparece en cada una de los arrays de reparto, a veces como el primer actor en la lista, y a veces en otras posiciones, como aquí.

**Lo bueno de esta sintaxis es que es exactamente lo mismo que los selectores para valores escalares**.

En algunas situaciones, queremos hacer coincidir un elemento específico de un array.

Los actores se enumeran con frecuencia en orden de precedencia, y aquellos cuyas contribuciones a una película son más altas aparecen antes en la lista.

Por ejemplo, la estrella de una película aparecerá antes de los actores secundarios, como vemos en el documento de "Iron Man" (`"year" : 2008`). En esta película, Robert Downey Jr. es la estrella, con Jeff Bridges desempeñando un papel secundario, por lo que Robert Downey Jr. aparece primero.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({title: "Iron Man"}).pretty()
{
	"_id" : ObjectId("58c59c6c99d4ee0af9e11902"),
	"title" : "Iron Man",
	"year" : 1931,
	"imdbId" : "tt0022002",
	"genre" : "Drama",
	"viewerRating" : 6.2,
	"viewerVotes" : 119,
	"runtime" : 73,
	"director" : "Tod Browning",
	"cast" : [
		"Lew Ayres",
		"Robert Armstrong",
		"Jean Harlow",
		"John Miljan"
	],
	"plot" : "Prizefighter Mason loses his opening fight so wife Rose leaves him for Hollywood. Without her around Mason trains and starts winning. Rose comes back and wants Mason to dump his manager Regan and replace him with her secret lover Lewis.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c6e99d4ee0af9e16c23"),
	"title" : "Iron Man",
	"year" : 1951,
	"imdbId" : "tt0043678",
	"genre" : "Drama, Sport",
	"viewerRating" : 6.3,
	"viewerVotes" : 93,
	"runtime" : 81,
	"director" : "Joseph Pevney",
	"cast" : [
		"Jeff Chandler",
		"Evelyn Keyes",
		"Stephen McNally",
		"Rock Hudson"
	],
	"plot" : "An ambitious coal miner is talked into becoming a boxer by his gambler brother.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c8f99d4ee0af9e5ccfc"),
	"title" : "Iron Man",
	"year" : 2008,
	"imdbId" : "tt0371746",
	"mpaaRating" : "PG-13",
	"genre" : "Action, Adventure, Sci-Fi",
	"viewerRating" : 7.9,
	"viewerVotes" : 615059,
	"runtime" : 126,
	"director" : "Jon Favreau",
	"cast" : [
		"Robert Downey Jr.",
		"Terrence Howard",
		"Jeff Bridges",
		"Gwyneth Paltrow"
	],
	"plot" : "After being held captive in an Afghan cave, an industrialist creates a unique weaponized suit of armor to fight evil.",
	"language" : "English, Persian, Urdu, Arabic, Hungarian"
}
{
	"_id" : ObjectId("58c59ccf99d4ee0af9ee3f51"),
	"title" : "Iron Man",
	"year" : 2013,
	"imdbId" : "tt3062350",
	"genre" : "Animation, Short, Sci-Fi",
	"runtime" : 7,
	"director" : "Abdolreza Salehi"
}
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

El lenguaje de consulta MongoDB nos permite especificar que queremos hacer coincidir los elementos del array que ocurren en una posición específica en un array.

Para consultar las películas en las que Jeff Bridges aparece primero entre los miembros del elenco, podemos usar la notación de puntos y especificar un índice de array.

Para esto, será un poco más fácil ver la diferencia si volvemos a Compass.

Entonces, aquí en Compass, una consulta para el elenco Jeff Bridges `{cast: "Jeff Bridges"}` nos da 114 documentos.

<img src="/images/c2/19-compass-jeff.png">

Pero si consultamos a Jeff Bridges en la posición 0 para el campo de conversión, entonces solo tenemos 56 documentos, y podemos ver que en todos y cada uno de estos resultados devueltos, Jeff Bridges aparece primero en el array de conversión.

<img src="/images/c2/19-compass-jeff0.png">

Ahora, nuevamente, solo para recordarle, si queremos hacer este tipo de consulta, **será necesario encerrar nuestra clave entre comillas** tanto en Compass como en el Shell.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ "cast.0": "Jeff Bridges" }).pretty()
...
{
	"_id" : ObjectId("58c59c7599d4ee0af9e24d19"),
	"title" : "The Vanishing",
	"year" : 1993,
	"imdbId" : "tt0108473",
	"mpaaRating" : "R",
	"genre" : "Drama, Horror, Mystery",
	"viewerRating" : 6.3,
	"viewerVotes" : 17291,
	"runtime" : 109,
	"director" : "George Sluizer",
	"cast" : [
		"Jeff Bridges",
		"Kiefer Sutherland",
		"Nancy Travis",
		"Sandra Bullock"
	],
	"plot" : "The boyfriend of an abducted woman never gives up the search as the abductor looks on.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7599d4ee0af9e24f2d"),
	"title" : "Blown Away",
	"year" : 1994,
	"imdbId" : "tt0109303",
	"mpaaRating" : "R",
	"genre" : "Action, Crime, Drama",
	"viewerRating" : 6.1,
	"viewerVotes" : 21856,
	"runtime" : 121,
	"director" : "Stephen Hopkins",
	"cast" : [
		"Jeff Bridges",
		"Tommy Lee Jones",
		"Suzy Amis",
		"Lloyd Bridges"
	],
	"plot" : "An Irish bomber escapes from prison and targets a member of the Boston bomb squad.",
	"language" : "English, Irish"
}
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y de nuevo, aquí podemos ver rápidamente que Jeff Bridges aparece en primer lugar para todos estos.

**Hemos analizado:**

* **Las coincidencias de igualdad en los arrays con todo el array.**
* **Las coincidencias de cualquier elemento del array.**
* **Las coincidencias basadas en un elemento específico del array.**

Ahora debe tener una sólida comprensión de la sintaxis para las operaciones de lectura más comunes en los arrays.

## 20. Examen Reading Documents: Array Fields

**Problem:**

Explore the movieDetails collection that you loaded into your Atlas sandbox cluster and then issue a query to answer the following question. How many documents list just two writers: "Ethan Coen" and "Joel Coen", in that order?

You will find the [`count()`](https://docs.mongodb.com/manual/reference/method/cursor.count/) method useful in answering this question using the mongo shell.

Choose the best answer:


* 1 :+1:

* 3

* 7

* 12

* 20

## 21. Práctica de laboratorio 2.4: Consultas en campos de array, Parte 1

Lab 2.4: Queries on Array Fields, Part 1

**Problem:**

Explore the movieDetails collection that you loaded into your Atlas sandbox cluster and then issue a query to answer the following question. How many movies in the movieDetails collection list "Family" among its genres?

You will find the [`count()`](https://docs.mongodb.com/manual/reference/method/cursor.count/) method useful in answering this question using the mongo shell.

Choose the best answer:

* 20

* 57

* 124 :+1:

* 200

* 277

## 22. Práctica de laboratorio 2.5: Consultas en campos de array, Parte 2

Lab 2.5: Queries on Array Fields, Part 2

**Problem:**

Explore the movieDetails collection that you loaded into your Atlas sandbox cluster and then issue a query to answer the following question. How many movies in the movieDetails collection list "Western" second among its genres?

You will find the [`count()`](https://docs.mongodb.com/manual/reference/method/cursor.count/) method useful in answering this question using the mongo shell.

Choose the best answer:

* 7

* 14 :+1:

* 80

* 93

* 102

## 23. Tema: Cursores

### Notas de lectura

Más información sobre [cursor iteration in the mongo shell](https://docs.mongodb.com/manual/tutorial/iterate-a-cursor/index.html) y el comando [getMore](https://docs.mongodb.com/manual/reference/command/getMore/#dbcmd.getMore).

### Transcripción

El método `find` devuelve un cursor.

**Un cursor es esencialmente un puntero a la ubicación actual en un conjunto de resultados**.

Para consultas que devuelven más que unos pocos documentos, MongoDB devolverá los resultados en lotes a nuestro cliente.

Y recuerda que el shell mongo es un cliente.

Usamos el cursor en nuestro cliente para recorrer los resultados.

```
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ "cast.0": "Jeff Bridges" }).pretty()
...
{
	"_id" : ObjectId("58c59c7599d4ee0af9e24d19"),
	"title" : "The Vanishing",
	"year" : 1993,
	"imdbId" : "tt0108473",
	"mpaaRating" : "R",
	"genre" : "Drama, Horror, Mystery",
	"viewerRating" : 6.3,
	"viewerVotes" : 17291,
	"runtime" : 109,
	"director" : "George Sluizer",
	"cast" : [
		"Jeff Bridges",
		"Kiefer Sutherland",
		"Nancy Travis",
		"Sandra Bullock"
	],
	"plot" : "The boyfriend of an abducted woman never gives up the search as the abductor looks on.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c7599d4ee0af9e24f2d"),
	"title" : "Blown Away",
	"year" : 1994,
	"imdbId" : "tt0109303",
	"mpaaRating" : "R",
	"genre" : "Action, Crime, Drama",
	"viewerRating" : 6.1,
	"viewerVotes" : 21856,
	"runtime" : 121,
	"director" : "Stephen Hopkins",
	"cast" : [
		"Jeff Bridges",
		"Tommy Lee Jones",
		"Suzy Amis",
		"Lloyd Bridges"
	],
	"plot" : "An Irish bomber escapes from prison and targets a member of the Boston bomb squad.",
	"language" : "English, Irish"
}
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> it
{
	"_id" : ObjectId("58c59c7699d4ee0af9e26312"),
	"title" : "Wild Bill",
	"year" : 1995,
	"imdbId" : "tt0114938",
	"mpaaRating" : "R",
	"genre" : "Action, Biography, Western",
	"viewerRating" : 5.9,
	"viewerVotes" : 4520,
	"runtime" : 98,
	"director" : "Walter Hill",
	"cast" : [
		"Jeff Bridges",
		"Ellen Barkin",
		"John Hurt",
		"Diane Lane"
	],
	"plot" : "The early career of legendary lawman Wild Bill Hickock is telescoped and culminates in his relocation in Deadwood and a reunion with Calamity Jane.",
	"language" : "English, Sioux, Cantonese"
}
...
{
	"_id" : ObjectId("58c59c9e99d4ee0af9e79b07"),
	"title" : "A Dog Year",
	"year" : 2009,
	"imdbId" : "tt0808232",
	"genre" : "Comedy, Drama",
	"viewerRating" : 6.1,
	"viewerVotes" : 1972,
	"runtime" : 80,
	"director" : "George LaVoo",
	"cast" : [
		"Jeff Bridges",
		"Lauren Ambrose",
		"Lois Smith",
		"Domhnall Gleeson"
	],
	"plot" : "A guy suffering from a midlife crisis takes in a dog that's crazier than he is.",
	"language" : "English"
}
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

En el shell, el cursor se itera automáticamente hasta 20 veces para imprimir un conjunto inicial de resultados de búsqueda, como vemos aquí en respuesta a esta consulta.

Entonces, cuando se emitió esta consulta, el servidor devolvió un cursor al shell, y el shell solicitó el siguiente elemento del conjunto de resultados del cursor 20 veces.

Entonces, lo que veremos arriba aquí, si nos desplazáramos hacia arriba, son los primeros 20 resultados de esta consulta.

Ahora, a medida que iteramos a través de un cursor y llegamos al final de un lote de resultados de la consulta, si hay más resultados, iterar el cursor activará una operación `getMore` para recuperar el siguiente lote de resultados.

De esta manera, podemos iterar a través de un conjunto completo de resultados de búsqueda.

Ahora, en este caso particular, el shell proporciona una conveniencia, y es que después de que muestra los primeros 20 resultados, tenemos la oportunidad de ver más de ellos escribiéndo `it`, que es la abreviatura de iterar.

Y eso nos dará los próximos 20 resultados en nuestro conjunto de resultados.

Y podemos continuar haciendo esto hasta agotar el cursor.

De esta manera, podemos iterar a través de un conjunto completo de resultados de búsqueda.

## 24. Tema: Proyecciones

### Transcripción

Las proyecciones reducen la sobrecarga de la red y los requisitos de procesamiento al limitar los campos que se devuelven en los documentos de resultados.

Por defecto, MongoDB devuelve todos los campos en todos los documentos coincidentes para consultas.

Puede definir una proyección como el segundo argumento del método `find`.

Si quiero limitar mis documentos de resultados para que solo contengan un título, puedo hacerlo usando la siguiente sintaxis, o casi.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ genre: "Action, Adventure"}, {title: 1})
{ "_id" : ObjectId("58c59c6a99d4ee0af9e0d117"), "title" : "Who Will Marry Mary?" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0d9c2"), "title" : "The New Exploits of Elaine" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0db62"), "title" : "The Ventures of Marguerite" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0dde5"), "title" : "The Iron Claw" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0dc25"), "title" : "Beatrice Fairfax" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0e28d"), "title" : "The Great Secret" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0e482"), "title" : "The Seven Pearls" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0e5f3"), "title" : "The Brass Bullet" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0e459"), "title" : "The Red Ace" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0e977"), "title" : "Wolves of Kultur" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0e8f3"), "title" : "Tarzan of the Apes" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0e71f"), "title" : "The Iron Test" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0e884"), "title" : "The Romance of Tarzan" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0ec74"), "title" : "Perils of Thunder Mountain" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0ea8d"), "title" : "The Fatal Fortune" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0f083"), "title" : "The Revenge of Tarzan" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0efa9"), "title" : "The Lost City" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0f0b7"), "title" : "The Screaming Shadow" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0f0cf"), "title" : "The Son of Tarzan" }
{ "_id" : ObjectId("58c59c6b99d4ee0af9e0f1a1"), "title" : "The Adventures of Tarzan" }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

```

Entonces, usando esta sintaxis, puedo crear una proyección donde los resultados de esta consulta devolverán solo el título y, en realidad, el `_id` y el `title`.

Y hablaremos de eso en solo un segundo.

Entonces, nuevamente, use el filtro como de costumbre `{ genre: "Action, Adventure"}`.

Pero en este caso, debido a que nos gustaría tener el título, también estamos agregando una proyección como el segundo argumento que pasamos para encontrar `{title: 1}`.

Ahora, obtengo el `_id` y el `title`.

Obtengo el campo `_id` porque **el `_id` se devuelve de forma predeterminada para todas las proyecciones**, a menos que lo excluya explícitamente de la proyección.

La sintaxis de proyección me permite incluir explícitamente campos en los documentos devueltos.

También puedo excluir explícitamente los campos.

**Incluyo explícitamente los campos con un uno y excluyo con un cero.**

Si no quiero ver el campo `_id`, entonces, para mi proyección, necesito agregar  `_id: 0`.

Vamos a correr esto.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ genre: "Action, Adventure"}, {title: 1, _id: 0})
{ "title" : "Who Will Marry Mary?" }
{ "title" : "The New Exploits of Elaine" }
{ "title" : "The Ventures of Marguerite" }
{ "title" : "The Iron Claw" }
{ "title" : "Beatrice Fairfax" }
{ "title" : "The Great Secret" }
{ "title" : "The Seven Pearls" }
{ "title" : "The Brass Bullet" }
{ "title" : "The Red Ace" }
{ "title" : "Wolves of Kultur" }
{ "title" : "Tarzan of the Apes" }
{ "title" : "The Iron Test" }
{ "title" : "The Romance of Tarzan" }
{ "title" : "Perils of Thunder Mountain" }
{ "title" : "The Fatal Fortune" }
{ "title" : "The Revenge of Tarzan" }
{ "title" : "The Lost City" }
{ "title" : "The Screaming Shadow" }
{ "title" : "The Son of Tarzan" }
{ "title" : "The Adventures of Tarzan" }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y ahora podemos ver que, de hecho, solo tenemos títulos.

Ahora, si tenemos la situación inversa, donde realmente queremos excluir explícitamente un par de campos, podemos hacerlo de la siguiente manera.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ genre: "Action, Adventure"}, {viewerRating: 0, viewerVotes: 0, runtime: 0, _id: 0}).pretty()
{
	"title" : "Who Will Marry Mary?",
	"year" : 1913,
	"imdbId" : "tt0003545",
	"genre" : "Action, Adventure",
	"director" : "Walter Edwin",
	"cast" : [
		"Mary Fuller",
		"Ben F. Wilson",
		"Richard Tucker",
		"Harry Beaumont"
	],
	"language" : "English"
}
{
	"title" : "The New Exploits of Elaine",
	"year" : 1915,
	"imdbId" : "tt0005808",
	"genre" : "Action, Adventure",
	"director" : "Louis J. Gasnier, Leopold Wharton, Theodore Wharton",
	"cast" : [
		"Pearl White",
		"Creighton Hale",
		"Arnold Daly",
		"Edwin Arden"
	]
}
{
	"title" : "The Ventures of Marguerite",
	"year" : 1915,
	"imdbId" : "tt0006213",
	"genre" : "Action, Adventure",
	"director" : "Robert Ellis, John Mackin, Hamilton Smith",
	"cast" : [
		"Marguerite Courtot",
		"Richard Purdon",
		"Edward Roseman",
		"Paula Sherman"
	],
	"plot" : "As heiress to a large fortune, Marguerite is able to satisfy her love for beautiful clothes and a taste for adventure, while confronted by a multitude of schemers and gangsters bent on reducing her to poverty.",
	"language" : "English"
}
{
	"title" : "The Iron Claw",
	"year" : 1916,
	"imdbId" : "tt0006867",
	"genre" : "Action, Adventure",
	"director" : "Edward Josè, George B. Seitz",
	"cast" : [
		"Pearl White",
		"Creighton Hale",
		"Sheldon Lewis",
		"Harry L. Fraser"
	],
	"language" : "English"
}
{
	"title" : "Beatrice Fairfax",
	"year" : 1916,
	"imdbId" : "tt0006409",
	"genre" : "Action, Adventure",
	"director" : "Leopold Wharton, Theodore Wharton",
	"cast" : [
		"Harry Fox",
		"Grace Darling",
		"Allan Murnane",
		"Nigel Barrie"
	],
	"language" : "English"
}
{
	"title" : "The Great Secret",
	"year" : 1917,
	"imdbId" : "tt0008031",
	"genre" : "Action, Adventure",
	"director" : "Christy Cabanne",
	"cast" : [
		"Francis X. Bushman",
		"Beverly Bayne",
		"Fred R. Stanton",
		"Edward Connelly"
	],
	"plot" : "A wealthy young athlete comes to the aid of a beautiful heiress, whose fortune is being threatened by two arch villains, The Great Master and Doctor Zulph.",
	"language" : "English"
}
{
	"title" : "The Seven Pearls",
	"year" : 1917,
	"imdbId" : "tt0008562",
	"genre" : "Action, Adventure",
	"director" : "Louis J. Gasnier, Donald MacKenzie",
	"cast" : [
		"Mollie King",
		"Creighton Hale",
		"L�on Bary",
		"John J. Dunn"
	],
	"language" : "English"
}
{
	"title" : "The Brass Bullet",
	"year" : 1918,
	"imdbId" : "tt0008919",
	"genre" : "Action, Adventure",
	"director" : "Ben F. Wilson",
	"cast" : [
		"Juanita Hansen",
		"Jack Mulhall",
		"Charles Hill Mailes",
		"Joseph W. Girard"
	],
	"language" : "English"
}
{
	"title" : "The Red Ace",
	"year" : 1917,
	"imdbId" : "tt0008502",
	"genre" : "Action, Adventure",
	"director" : "Jacques Jaccard",
	"cast" : [
		"Marie Walcamp",
		"Lawrence Peyton",
		"L.M. Wells",
		"Bobbie Mack"
	],
	"language" : "English"
}
{
	"title" : "Wolves of Kultur",
	"year" : 1918,
	"imdbId" : "tt0009825",
	"genre" : "Action, Adventure",
	"director" : "Joseph A. Golden",
	"cast" : [
		"Leah Baird",
		"Charles Hutchison",
		"Sheldon Lewis",
		"Betty Howe"
	],
	"language" : "English"
}
{
	"title" : "Tarzan of the Apes",
	"year" : 1918,
	"imdbId" : "tt0009682",
	"genre" : "Action, Adventure",
	"director" : "Scott Sidney",
	"cast" : [
		"Elmo Lincoln",
		"Enid Markey",
		"True Boardman",
		"Kathleen Kirkham"
	],
	"plot" : "The plot follows the novel more closely than does any other Tarzan movie. John and Alice Clayton take ship for Africa. Mutineers maroon them. After his parents die the newborn Tarzan is ..."
}
{
	"title" : "The Iron Test",
	"year" : 1918,
	"imdbId" : "tt0009233",
	"genre" : "Action, Adventure",
	"director" : "Robert N. Bradbury, Paul Hurst",
	"cast" : [
		"Antonio Moreno",
		"Carol Holloway",
		"Barney Furey",
		"Chet Ryan"
	]
}
{
	"title" : "The Romance of Tarzan",
	"year" : 1918,
	"imdbId" : "tt0009560",
	"genre" : "Action, Adventure",
	"director" : "Wilfred Lucas",
	"cast" : [
		"Elmo Lincoln",
		"Enid Markey",
		"Thomas Jefferson",
		"Cleo Madison"
	],
	"plot" : "Tarzan and Jane are to sail for England. They are attacked by natives and Tarzan is believed to have been killed. The Greystoke relatives return to England, the Porters (Jane's family) goes...",
	"language" : "English"
}
{
	"title" : "Perils of Thunder Mountain",
	"year" : 1919,
	"imdbId" : "tt0010558",
	"genre" : "Action, Adventure",
	"director" : "Robert N. Bradbury, W.J. Burman",
	"cast" : [
		"Antonio Moreno",
		"Carol Holloway",
		"Kate Price",
		"Jack Waltemeyer"
	],
	"language" : "English"
}
{
	"title" : "The Fatal Fortune",
	"year" : 1919,
	"imdbId" : "tt0010114",
	"genre" : "Action, Adventure",
	"director" : "Donald MacKenzie, Frank Wunderlee",
	"cast" : [
		"Helen Holmes",
		"Jack Levering",
		"Leslie King",
		"William Black"
	],
	"plot" : "A young newspaperwoman travels to a South Seas island to search for buried treasure.",
	"language" : "English"
}
{
	"title" : "The Revenge of Tarzan",
	"year" : 1920,
	"imdbId" : "tt0011624",
	"genre" : "Action, Adventure",
	"director" : "Harry Revier, George M. Merrick",
	"cast" : [
		"Gene Pollar",
		"Karla Schramm",
		"Estelle Taylor",
		"Armand Cortes"
	],
	"plot" : "Tarzan and Jane are sailing for France in answer to a call for help from Countess de Coude who is being persecuted by her brother Rokoff. After a duel with the Countess' jealous husband, ..."
}
{
	"title" : "The Lost City",
	"year" : 1920,
	"imdbId" : "tt0011413",
	"genre" : "Action, Adventure",
	"director" : "E.A. Martin",
	"cast" : [
		"Juanita Hansen",
		"George Chesebro",
		"Frank Clark",
		"Hector Dion"
	],
	"language" : "English"
}
{
	"title" : "The Screaming Shadow",
	"year" : 1920,
	"imdbId" : "tt0011667",
	"genre" : "Action, Adventure",
	"director" : "Ben F. Wilson, Duke Worne",
	"cast" : [
		"Ben F. Wilson",
		"Neva Gerber",
		"Frances Terry",
		"Howard Crampton"
	],
	"language" : "English"
}
{
	"title" : "The Son of Tarzan",
	"year" : 1920,
	"imdbId" : "tt0011717",
	"genre" : "Action, Adventure",
	"director" : "Arthur J. Flaven, Harry Revier",
	"cast" : [
		"Kamuela C. Searle",
		"P. Dempsey Tabler",
		"Nita Martan",
		"Karla Schramm"
	],
	"plot" : "The scenario follows the book closely. Tarzan's son Jack (Korak to the apes) is kidnapped from England by Tarzan's old enemy Paulovich. He escapes into the African jungle with the help of ..."
}
{
	"title" : "The Adventures of Tarzan",
	"year" : 1921,
	"imdbId" : "tt0011908",
	"mpaaRating" : "PASSED",
	"genre" : "Action, Adventure",
	"director" : "Robert F. Hill, Scott Sidney",
	"cast" : [
		"Elmo Lincoln",
		"Louise Lorraine",
		"Scott Pembroke",
		"Frank Whitson"
	],
	"plot" : "Tarzan spurns the love of La, Queen of Opar. When he isn't trying to keep the Bolshevik Rokoff and Clayton (pretender to the Greystoke estate) from reaching Opar, he is attacked ...",
	"language" : "English"
}
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Entonces, digamos que en lugar de incluir el título, voy a excluir la calificación del espectador, los votos del espectador y el tiempo de ejecución (viewerRating, viewerVotes, runtime).

Ahora mis resultados contienen todos los campos excepto los que excluí explícitamente aquí.

Tenga en cuenta que no hay calificación de espectador, ni votos de espectador, ni tiempo de ejecución en estos documentos.

Y solo por el bien de la comparación, si abandono la proyección, entonces vemos que la calificación del espectador, los votos del espectador y el tiempo de ejecución regresan para aquellos documentos que tienen un tiempo de ejecución, como este.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movies.find({ genre: "Action, Adventure"}).pretty()
...
	],
	"plot" : "A young newspaperwoman travels to a South Seas island to search for buried treasure.",
	"language" : "English"
}
{
	"_id" : ObjectId("58c59c6b99d4ee0af9e0f083"),
	"title" : "The Revenge of Tarzan",
	"year" : 1920,
	"imdbId" : "tt0011624",
	"genre" : "Action, Adventure",
	"viewerRating" : 4.1,
	"viewerVotes" : 14,
	"runtime" : 90,
	"director" : "Harry Revier, George M. Merrick",
	"cast" : [
		"Gene Pollar",
		"Karla Schramm",
		"Estelle Taylor",
		"Armand Cortes"
	],
	"plot" : "Tarzan and Jane are sailing for France in answer to a call for help from Countess de Coude who is being persecuted by her brother Rokoff. After a duel with the Countess' jealous husband, ..."
}
{
	"_id" : ObjectId("58c59c6b99d4ee0af9e0efa9"),
	"title" : "The Lost City",
...	
```

Por lo tanto, **si necesita limitar los campos devueltos por sus consultas, la proyección es la herramienta que debe utilizar**.

Tiene **ceros para excluir explícitamente los campos** que no necesita ver en su conjunto de resultados.

Y use **uno para incluir explícitamente campos y excluir todos los demás**.

## 25. Tema: Actualización de documentos: updateOne ()

### Notas de lectura

Para seguir este tema, debe conectarse al clúster de sandbox en el que ha importado los datos.

### Transcripción

Ahora echemos un vistazo a la actualización de documentos.

Antes de comenzar, me gustaría ver el conjunto de datos movieDetails en Compass con un poco más de detalle.

<img src="/images/c2/25-compass-all-details.png">

Este conjunto de datos incluye los campos que puede esperar, como el title, year, y el MPAA rating.

Pero también incluye campos que son importantes para construir un sitio web diseñado para aficionados al cine y otros que solo tienen un interés genuino en las películas.

Hay un campo para los países en los que se filmó la película, quiénes fueron los escritores, los actores, por supuesto, e incluso un campo para el póster de la película para proporcionar una forma de acceder a algunas imágenes.

Además, tenemos los datos de **Rotten Tomatoes** para las calificaciones de los espectadores y los premios que una película determinada puede haber ganado.

Entonces, para el sitio web de una película, podemos usar estos datos para proporcionar una imagen realmente agradable de lo que se hizo para hacer esta película.

Como mencioné, aunque no todas las películas en este conjunto de datos incluyen este campo, muchas incluyen un campo de póster.

Echemos un vistazo a uno que en realidad no incluye un póster.

<img src="/images/c2/25-compass-the-martian.png">

Así que aquí tenemos la película The Martian

Y como puede ver en un escaneo rápido de los campos para este documento, no hay un campo de póster (poster).

Y de hecho, tampoco hay campo de premios (awards).

Ahora, no pasaremos demasiado tiempo en esto, pero la diferencia entre The Martian y otras películas que hemos visto ilustra algo que diferencia a MongoDB de las bases de datos relacionales.

**Es posible que los documentos de la misma colección tengan diferentes esquemas**.

Y aprovechamos eso en MongoDB de varias maneras.

Esta es una diferencia importante con respecto a las tablas de bases de datos relacionales en las que todos los registros deben tener exactamente el mismo conjunto de columnas.

Una de las formas más simples en que usamos esto en una base de datos MongoDB es que para los documentos para los cuales no tenemos datos para un campo, no necesitamos incluir el campo en absoluto.

Yendo un poco más allá, muchas películas no han recibido premios.

La flexibilidad del modelo de datos de MongoDB nos permite modelar fácilmente los premios solo para aquellas películas que los tienen sin crear una segunda tabla o una colección que luego debemos unir en consultas para obtener todos los datos necesarios para una película.

Ahora, de hecho, existe un póster de película para The Martian.

Y The Martian ganó varios premios.

Entonces, como ejemplo de actualización de documentos en MongoDB, vamos a arreglar este documento agregando un campo de póster(poster) y un campo de premios(awards).

Ahora, una aplicación podría hacer este tipo de actualización a medida que el póster esté disponible en forma electrónica para su uso en el sitio o incluso en respuesta al contenido moderado y aportado por el usuario.

En el lenguaje de consulta MongoDB, el método para actualizar un solo documento se llama **`updateOne`**.

Ahora, por supuesto, podríamos agregar este campo en Compass simplemente editando el documento.

<img src="/images/c2/25-compass-edit.png">

Pero para la mayoría de los casos de uso de actualizaciones, va a escribir actualizaciones como parte de la lógica de su aplicación y no realizará actualizaciones a mano, como lo haríamos en una herramienta GUI como Compass.

Así que aquí está nuestro llamado a `updateOne`.

```sh
db.movieDetails.updateOne(
  { title: "The Martian" }, 
  { $set: {
    poster: "https://m.media-amazon.com/images/M/MV5BMTc2MTQ3MDA1Nl5BMl5BanBnXkFtZTgwODA3OTI4NjE@._V1_SY1000_CR0,0,675,1000_AL_.jpg"
  }
})
```

Hablemos brevemente sobre la sintaxis.

La idea básica aquí con `updateOne` y los otros dos métodos de actualización es que primero especifique un filtro.

Al igual que con `find`, esto `{ title: "The Martian" }` identificará el documento o documentos que queremos actualizar.

`updateOne` simplemente actualizará el primer documento que coincida con nuestro filtro.

Por ejemplo, si hubiéramos puesto algo aquí como el año 1999, habría docenas de documentos que coinciden en esta colección.

El primero recuperado por MongoDB sería el que se actualizó.

En este caso, solo hay una película que se titula The Martian.

Ahora, por supuesto, en una aplicación web, script de actualización u otra aplicación de software, estaríamos utilizando un identificador único como `_id` para especificar el documento que se actualizará.

Estamos usando el título aquí solo para que sea obvio exactamente qué película estamos actualizando.

El segundo argumento para `updateOne` especifica cómo queremos actualizar el documento.

Debe aplicar un operador de actualización de algún tipo.

En este caso, estamos utilizando el operador `$set`.

`$set` toma un documento como argumento.

Se espera un documento que tenga uno o más campos listados.

`$set` actualiza el documento que coincide con el filtro, de modo que todos los pares de valores clave en el documento actualizado se reflejan en la nueva versión del documento que estamos actualizando.

**Para esta llamada a `updateOne`, `$set` agregará un campo llamado poster, con esta URL como valor**.

**Si hubiera un campo de póster existente en el documento, esto modificaría su valor**.

Ahora vamos a copiar este comando y ejecutarlo en nuestro shell.

En este shell, estamos conectados a nuestro clúster de sandbox Atlas y estamos utilizando la base de datos de video para que podamos acceder a la colección movieDetails.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use video
switched to db video

MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.updateOne(
...   { title: "The Martian" }, 
...   { $set: {
...     poster: "https://m.media-amazon.com/images/M/MV5BMTc2MTQ3MDA1Nl5BMl5BanBnXkFtZTgwODA3OTI4NjE@._V1_SY1000_CR0,0,675,1000_AL_.jpg"
...   }
... })
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

La respuesta que recibimos nos dice que la base de datos reconoció la actualización.

Igualamos un solo documento `"matchedCount" : 1`, como se esperaba.

Si hubiéramos igualado más, obtendríamos esa cuenta aquí.

Y luego la respuesta indica cuántos documentos fueron modificados `"modifiedCount" : 1`.

Para `updateOne`, este valor siempre debe ser 1 o 0.

Echemos un vistazo al documento ahora para ver qué cambios se hicieron.

<img src="/images/c2/25-compass-add-poster.png">

Y si actualizo el documento, puedo ver que ahora, de hecho, hay un campo de póster (el último campo).

Los operadores de actualización, como puede imaginar, no se limitan a las actualizaciones escalares, como acabamos de realizar.

Podemos actualizar campos con cualquier valor legal.

Como ejemplo rápido, avancemos y suministremos el campo de premios para The Martian.

```sh
db.movieDetails.updateOne(
  { title: "The Martian" }, 
  { $set: {
      "awards": {
        "wins": 8,
        "nominations": 14,
        "text": "Nominated for 3 Golden Globes. Another 8 wins & 14 nominations."
      }
  }
})
```

Ahora, podemos seguir adelante y ejecutar esta llamada para `updateOne`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.updateOne(
...   { title: "The Martian" }, 
...   { $set: {
...       "awards": {
...         "wins": 8,
...         "nominations": 14,
...         "text": "Nominated for 3 Golden Globes. Another 8 wins & 14 nominations."
...       }
...   }
... })
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y tenga en cuenta que aquí, estamos haciendo algo muy similar a lo que hicimos en nuestra llamada anterior para `updateOne`.

Pero en este caso, nuestro operador `$set` establecerá un campo `awards` de modo que el valor de la clave `awards`sea este documento.

Ahora, si seguimos adelante y ejecutamos esto, vemos que, nuevamente, emparejamos un documento y modificamos un documento `"matchedCount" : 1, "modifiedCount" : 1`.

Volviendo atrás y mirando este documento en Compass, podemos ver que nuestro campo de premios está aquí, con los valores especificados en nuestra llamada a `updateOne`.

<img src="/images/c2/25-compass-add-awards.png">

Y con eso, hemos cubierto los fundamentos del uso de `updateOne`.

## 26. Tema: Operadores de Actualización

### Notas de lectura

Para seguir este tema, debe conectarse al clúster de sandbox en el que ha importado los datos.

### Transcripción

Lo que me gustaría hacer ahora es darle una idea de los diferentes tipos de operadores de actualización de campo que tenemos disponibles para nosotros.

Utilizamos **`$set`**. 

Este reemplaza o agrega completamente cada campo especificado en su parámetro como vimos en ejemplos anteriores.

Echemos un vistazo a las otras opciones que tenemos para los operadores de actualización.

Y para hacer esto, estoy mirando la página del operador de actualización en la documentación de MongoDB.

[Update Operators](https://docs.mongodb.com/manual/reference/operator/update/index.html)

Además de `$set`, también hay `$unset`.

Esto **eliminará por completo el campo que especificamos de un documento**.

Otros aquí incluyen `$min` y `$max`.

Estos nos permiten actualizar un campo basado en la comparación con otro valor tomando el mínimo de los dos valores o el máximo de los dos valores.

Podemos ver que hay otros operadores.

Como ejercicio, me gustaría pedirle que visite la documentación de MongoDB sobre operadores de actualización y pruebe algunos de estos.

Todos se explican por sí mismos y hay buena documentación sobre cada uno de ellos.

Pero antes de hacerlo, echemos un vistazo a un par más de estos operadores a medida que continuamos avanzando a través de nuestros ejemplos aquí.

Ahora, las actualizaciones tienen varios casos de uso diferentes.

Se utilizan para corregir errores y, con el tiempo, mantener nuestros datos actualizados.

Para los datos de películas, gran parte de lo que hay es estático.

<img src="/images/c2/25-compass-all-details.png">

Tenemos directores, escritores, actores, etc.

Sin embargo, será necesario actualizar otro contenido, como los comentarios y las calificaciones, a medida que los usuarios tomen medidas para crear nuevos comentarios o calificaciones.

Podríamos usar `$set` para hacer este tipo de actualizaciones.

Pero ese es un enfoque propenso a errores.

Es demasiado fácil hacer la aritmética de forma incorrecta o cometer otros tipos de errores.

En cambio, tenemos una serie de operadores que admiten actualizaciones numéricas de datos.

Ahora, como mencioné, tenemos `$min` y `$max`.

Pero también hay `$inc`.

Esto incrementa el valor almacenado en el campo que le pasamos.

Veamos un ejemplo del uso del operador `$inc` para actualizar las revisiones.

```sh
db.movieDetails.updateOne({ 
  title: "The Martian"
}, {
  $inc: {
    "tomato.reviews": 3,
    "tomato.userReviews": 25
  }
})
```

Aquí, lo que vamos a hacer es, nuevamente trabajando con "The Martian", incrementar el `tomate.reviews` en tres y el `tomate.userReviews` en 25.

Y en caso de que no esté familiarizado con **Rotten Tomatoes**, en cada uno de nuestros documentos de detalles de la película hay un campo `tomate`.

Y el campo `tomate` refleja la calificación para esta película de Rotten Tomatoes.

**Rotten Tomatoes es un agregador de clasificaciones de usuarios para películas**.

Entonces, las personas que han visto una película caen en una calificación.

Esos se agrupan en una clasificación de tomate.

Así que aquí estamos simulando la situación en la que hemos recibido tres revisiones adicionales para "The Martian".

Si ejecutamos esto y echamos un vistazo a la salida, como se esperaba, nuestra actualización fue exitosa.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.updateOne({ 
...   title: "The Martian"
... }, {
...   $inc: {
...     "tomato.reviews": 3,
...     "tomato.userReviews": 25
...   }
... })
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y si miramos el documento antes de la actualización de `tomato.reviews` y `tomato.userReviews` y luego actualizamos, vemos que `tomato.reviews` es 283.

Y las opiniones de los usuarios superaron los 105,000.

<img src="/images/c2/26-compass-the-martian-inc.png">

Ahora pasemos a los campos de array.

Como se puede imaginar, hay una serie de situaciones en las que queremos actualizar los valores de un array.

MongoDB proporciona muchos operadores de actualización para arrays.

[Update Operators for Arrays](https://docs.mongodb.com/manual/reference/operator/update/index.html#array)

`$addToSet` actualizará un array con nuevos valores solo si el valor aún no está contenido en el array.

Con `$pullAll` puedo extraer el primer o el último elemento de un array según mis parámetros.

Con `Spop` puedo eliminar todos los valores que coinciden con algunos criterios con `$pullAll`.

Y, con `$push`, puedo ingresar nuevos valores.

Ahora, nuevamente, lo aliento a que eche un vistazo a la documentación para los operadores de actualización, en la sección de los operadores de actualización de array.

Pero echemos un vistazo a un par de ejemplos en los que necesitaremos actualizar los campos del array.

Para la mayoría de las aplicaciones web, deseamos estructurar nuestros datos de tal manera que podamos obtener todos los datos que necesitamos para representar una página individual con la menor cantidad posible de consultas a la base de datos.

Para revisiones, comentarios y otras contribuciones de los usuarios, recomendamos que una parte de sus revisiones, a menudo las más recientes o más útiles, se incluyan como documentos incrustados.

Siguiendo esta mejor práctica, lo que podríamos hacer para las reseñas de películas es algo como lo siguiente.

Ahora puede ignorar en gran medida esta parte además del hecho de que es una revisión.

Pero déjame llamar tu atención sobre la llamada al método `updateOne`.

```sh
let reviewText1 = [
  "The Martian could have been a sad drama film, instead it was a ",
  "hilarious film with a little bit of drama added to it. The Martian is what ",
  "everybody wants from a space adventure. Ridley Scott can still make great ",
  "movies and this is one of his best."
].join()
db.movieDetails.updateOne({
  title: "The Martian"
}, {
  $push: {
    reviews: {
      rating: 4.5,
      date: ISODate("2016-01-12T09:00:00Z"),
      reviewer: "Spencer H.",
      text: reviewText1
    }
  }
})
```

Esto, llamado `updateOne`, creará una clave llamada `reviews`, creará un array como el valor de esta clave y luego empujará esta revisión (`reviews`) al array.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> let reviewText1 = [
...   "The Martian could have been a sad drama film, instead it was a ",
...   "hilarious film with a little bit of drama added to it. The Martian is what ",
...   "everybody wants from a space adventure. Ridley Scott can still make great ",
...   "movies and this is one of his best."
... ].join()
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.updateOne({
...   title: "The Martian"
... }, {
...   $push: {
...     reviews: {
...       rating: 4.5,
...       date: ISODate("2016-01-12T09:00:00Z"),
...       reviewer: "Spencer H.",
...       text: reviewText1
...     }
...   }
... })
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Recuerde que nuestro documento "The Martian" en realidad no tiene un campo llamado `reviews`.

La razón por la que esto funciona es porque `push` crea un array si aún no existe uno.

Echemos un vistazo al documento ahora.

<img src="/images/c2/26-compass-the-martian-reviews.png">

Aquí podemos ver nuestro campo `reviews` agregado y las revisiones son un array.

Y este array tiene un elemento, que es la revisión que especificamos en nuestra actualización.

Ahora agreguemos algunas reseñas más.

```sh
let reviewText2 = [
  "There have been better movies made about space, and there are ",
  "elements of the film that are borderline amateur, such as weak dialogue, an uneven tone, ",
  "and film cliches."
].join()

let reviewText3 = [
  "The Martian Review: There are some movies you know going into them that they're going to be great.",
  "The Martian was not only great but exceeded all my expectations as well. A return to form from director Ridley Scott ",
  "(Alien, Blade Runner) who directed last year's abysmal 'Exodus: Gods and Kings (which I adeptly gave 1/5 because I ",
  "was happy that it ended)"
].join()

let reviewText4 = [
  "An astronaut/botanist is stranded on Mars and must rely upon ingenuity to survive. It's hard to divorce my opinions ",
  "about this film from my opinions of the book, for after the film I thought that they had adapted the book but left ",
  "out all the good parts. The cut that bleeds most is the final chapter, which gives the whole story a philosophical ",
  "significance that is both poignant and naively inspiring."
].join()

let reviewText5 = [
  "This novel-adaptation is humorous, intelligent and captivating in all its visual-grandeur. The Martian highlights ",
  "an impeccable Matt Damon, power-stacked ensemble and Ridley Scott's masterful direction, which is back in full form."
].join()

let reviewText6 = [
  "Personable sci-fi flick from Ridley Scott that updates the old Robinson Crusoe stuck on a desert isle (and how he ",
  "learns to survive) trope. First off, make that a deserted planet. Mars. And the background and the hope this film ",
  "presents is grounded on is our quest to reach beyond the limits of Earth's gravity (and so it loudly parades that ",
  "we'll overcome every adversity)." 
].join()

let reviewText7 = [
  "A declaration of love for the potato, science and the indestructible will to survive. While it clearly is the Matt ",
  "Damon show (and he is excellent), the supporting cast may be among the strongest seen on film in the last 10 years. ",
  "An engaging, exciting, funny and beautifully filmed adventure thriller no one should miss."
].join()

db.movieDetails.updateOne({
  title: "The Martian"
}, {
  $push: {
    reviews: {
      $each: [{
        rating: 2.5,
        date: ISODate("2016-01-12T07:00:00Z"),
        reviewer: "Matthew Samuel",
        text: reviewText2
      }, {
        rating: 5.0,
        date: ISODate("2016-01-13T09:00:00Z"),
        reviewer: "Jarrad C",
        text: reviewText3
      }, {
        rating: 3.0,
        date: ISODate("2016-01-14T08:00:00Z"),
        reviewer: "hunterjt13",
        text: reviewText4
      }, {
        rating: 4.5,
        date: ISODate("2016-01-15T09:00:00Z"),
        reviewer: "Eugene B",
        text: reviewText5
      }, {
        rating: 3.5,
        date: ISODate("2016-01-16T10:00:00Z"),
        reviewer: "Kevin M. W",
        text: reviewText6
      }, {
        rating: 4.5,
        date: ISODate("2016-01-17T11:00:00Z"),
        reviewer: "Drake T",
        text: reviewText7
      }
    ]}
  }
}) 
```

Nuevamente estoy creando texto de revisión.

Y en este caso estoy creando varios de ellos.

Pero echemos un vistazo a nuestra `updateOne`.

De nuevo, estamos usando `$push`.

Y como se esperaba, estamos pushing en el reviews array.

Pero aquí estamos usando el modificador `$each` para `push`.

Algunos de nuestros operadores de actualización, particularmente aquellos que tienen que ver con arrays, tienen modificadores asociados con ellos.

Este modificador significa que `$push` agregará cada uno de estos documentos como elementos individuales al array de reviews.

Si no usamos `$each` aquí, entonces todo el array especificado en nuestro `$push` se agregaría como un solo elemento en ese array.

En algunas circunstancias, eso podría ser lo que queremos, pero no en este caso.

Así que ejecutemos este comando y luego miremos nuestro documento "The Martian".

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> let reviewText2 = [
...   "There have been better movies made about space, and there are ",
...   "elements of the film that are borderline amateur, such as weak dialogue, an uneven tone, ",
...   "and film cliches."
... ].join()
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
MongoDB Enterprise Cluster0-shard-0:PRIMARY> let reviewText3 = [
...   "The Martian Review: There are some movies you know going into them that they're going to be great.",
...   "The Martian was not only great but exceeded all my expectations as well. A return to form from director Ridley Scott ",
...   "(Alien, Blade Runner) who directed last year's abysmal 'Exodus: Gods and Kings (which I adeptly gave 1/5 because I ",
...   "was happy that it ended)"
... ].join()

let reviewText4 = [
  "An astronaut/botanist is stranded on Mars and must rely upon ingenuity to survive. It's hard to divorce my opinions ",
  "about this film from my opinions of the book, for after the film I thought that they had adapted the book but left ",
  "out all the good parts. The cut that bleeds most is the final chapter, which gives the whole story a philosophical ",
  "significance that is both poignant and naively inspiring."
].join()

let reviewText5 = [
  "This novel-adaptation is humorous, intelligent and captivating in all its visual-grandeur. The Martian highlights ",
  "an impeccable Matt Damon, power-stacked ensemble and Ridley Scott's masterful direction, which is back in full form."
].join()

let reviewText6 = [
  "Personable sci-fi flick from Ridley Scott that updates the old Robinson Crusoe stuck on a desert isle (and how he ",
  "learns to survive) trope. First off, make that a deserted planet. Mars. And the background and the hope this film ",
  "presents is grounMongoDB Enterprise Cluste
MongoDB Enterprise Cluster0-shard-0:PRIMARY> let reviewText4 = [
...   "An astronaut/botanist is stranded on Mars and must rely upon ingenuity to survive. It's hard to divorce my opinions ",
...   "about this film from my opinions of the book, for after the film I thought that they had adapted the book but left ",
...   "out all the good parts. The cut that bleeds most is the final chapter, which gives the whole story a philosophical ",
...   "significance that is both poignant and naively inspiring."
... ].join()
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
let reviewText5 = [
  "This novel-adaptation is humorous, intelligent and captivating in all its visual-grandeur. The Martian highlights ",
  "an impeccable Matt Damon, power-stacked ensemble and Ridley Scott's masterful direction, which is back in full form."
].join()

let reviewText6 = [
  "Personable sci-fi flick from Ridley Scott that updates the old Robinson Crusoe stuck on a desert isle (and how he ",
  "learns to survive) trope. First off, make that a deserted planet. Mars. And the background and the hope this film ",
  "presents is grounded on is our quest to reach beyond the limits of Earth's gravity (and so it loudly parades that ",
  "we'll overcome every adversity)." 
].join()

let reviewText7 = [
  "A declaration of love for the potato, science and the indestructible will to survive. While it clearly is the Matt ",
  "Damon show (and he is excellent), the supporting cast may be among the strongest seen on film in the last 10 years. ",
  "An engaging, exciting, funny and beautifullet reviewText5 = [
...   "This novel-adaptation is humorous, intelligent and captivating in all its visual-grandeur. The Martian highlights ",
...   "an impeccable Matt Damon, power-stacked ensemble and Ridley Scott's masterful direction, which is back in full form."
... ].join()
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
let reviewText6 = [
  "Personable sci-fi flick from Ridley Scott that updates the old Robinson Crusoe stuck on a desert isle (and how he ",
  "learns to survive) trope. First off, make that a deserted planet. Mars. And the background and the hope this film ",
  "presents is grounded on is our quest to reach beyond the limits of Earth's gravity (and so it loudly parades that ",
  "we'll overcome every adversity)." 
].join()

let reviewText7 = [
  "A declaration of love for the potato, science and the indestructible will to survive. While it clearly is the Matt ",
  "Damon show (and he is excellent), the supporting cast may be among the strongest seen on film in the last 10 years. ",
  "An engaging, exciting, funny and beautifully filmed adventure thriller no one should miss."
].join()

db.movieDetails.updateOne({
  title: "The Martian"
}, {
  $push: {
    reviews: {
      $each: [{
        rating: 2.5,
        date: ISODate("2016-01-12T07:00:00Z"),
        reviewer: "Matthew Samuel",
    MongoDB Enterprise Cluster0-shard-0:PRIMAlet reviewText6 = [
...   "Personable sci-fi flick from Ridley Scott that updates the old Robinson Crusoe stuck on a desert isle (and how he ",
...   "learns to survive) trope. First off, make that a deserted planet. Mars. And the background and the hope this film ",
...   "presents is grounded on is our quest to reach beyond the limits of Earth's gravity (and so it loudly parades that ",
...   "we'll overcome every adversity)." 
... ].join()
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
MongoDB Enterprise Cluster0-shard-0:PRIMARY> let reviewText7 = [
...   "A declaration of love for the potato, science and the indestructible will to survive. While it clearly is the Matt ",
...   "Damon show (and he is excellent), the supporting cast may be among the strongest seen on film in the last 10 years. ",
...   "An engaging, exciting, funny and beautifully filmed adventure thriller no one should miss."
... ].join()

db.movieDetails.updateOne({
  title: "The Martian"
}, {
  $push: {
    reviews: {
      $each: [{
        rating: 2.5,
        date: ISODate("2016-01-12T07:00:00Z"),
        reviewer: "Matthew Samuel",
        text: reviewText2
      }, {
        rating: 5.0,
        date: ISODate("2016-01-13T09:00:00Z"),
        reviewer: "Jarrad C",
        text: reviewText3
      }, {
        rating: 3.0,
        date: ISODate("2016-01-14T08:00:00Z"),
        reviewer: "hunterjt13",
        text: reviewText4
      }, {
        rating: 4.5,
        date: ISODate("2016-01-15T09:00:00Z"),
        reviewer: "Eugene B",
        text: reviewText5
      }, {
        rating: 3.5,
        date: ISODate("2016-01-16T10:00:00Z"),
        reviewer: "Kevin M. W",
        text: reviewText6
      }, {
        rating: 4.5,
        date: ISODate("2016-01-17T11:00:00Z"),
        reviewer: "Drake T",
        text: reviewText7
      }
    ]}
  }
}) MongoDB Enterprise Cluster0-shard-0:PRIMAR
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.movieDetails.updateOne({
...   title: "The Martian"
... }, {
...   $push: {
...     reviews: {
...       $each: [{
...         rating: 2.5,
...         date: ISODate("2016-01-12T07:00:00Z"),
...         reviewer: "Matthew Samuel",
...         text: reviewText2
...       }, {
...         rating: 5.0,
...         date: ISODate("2016-01-13T09:00:00Z"),
...         reviewer: "Jarrad C",
...         text: reviewText3
...       }, {
...         rating: 3.0,
...         date: ISODate("2016-01-14T08:00:00Z"),
...         reviewer: "hunterjt13",
...         text: reviewText4
...       }, {
...         rating: 4.5,
...         date: ISODate("2016-01-15T09:00:00Z"),
...         reviewer: "Eugene B",
...         text: reviewText5
...       }, {
...         rating: 3.5,
...         date: ISODate("2016-01-16T10:00:00Z"),
...         reviewer: "Kevin M. W",
...         text: reviewText6
...       }, {
...         rating: 4.5,
...         date: ISODate("2016-01-17T11:00:00Z"),
...         reviewer: "Drake T",
...         text: reviewText7
...       }
...     ]}
...   }
... }) 
{ "acknowledged" : true, "matchedCount" : 1, "modifiedCount" : 1 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Y aquí podemos ver que, de hecho, hemos agregado siete revisiones adicionales.

<img src="/images/c2/26-compass-the-martian-reviews-7.png">

Y con eso, hemos concluido nuestra visión general de los operadores de actualización.

Revisamos algunos de los operadores que están disponibles y trabajamos con algunos ejemplos.

No dudes en experimentar por ti mismo con la colección de detalles de la película.

Te animo a que pruebes tantos operadores de actualización como puedas utilizando la documentación de MongoDB como guía.

## 27. Examen

## 28. Tema: Actualización de documentos: updateMany ()

### Transcripción

## 29. Tema: Upserts

### Transcripción

## 30. Tema: Actualización de documentos: replaceOne ()

### Transcripción

## 31. Laboratorio 2.6: Operadores de actualización

## 32. Tema: Eliminar documentos

### Transcripción
