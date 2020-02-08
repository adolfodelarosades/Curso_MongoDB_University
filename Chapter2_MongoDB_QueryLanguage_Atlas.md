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
?replicaSet=Cluster0-shard-0" --authenticationDatabase 
admin --ssl --username m001-student --password m001-mongodb-basics
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
