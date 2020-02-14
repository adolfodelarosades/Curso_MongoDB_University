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

Voy a insertar algunos documentos en videos, películas de punto y cero.

Esto es solo para que pueda hacer un poco de noodling por aquí demostrando este comando, y luego deshacerme de esta colección sin corromper los datos que realmente quiero mantener, con respecto a las películas (movies).

Consulte los Handouts de esta lección para descargar los ejemplos aquí para que pueda probarlos usted mismo.

Ahora, en lugar de pasar un solo documento como primer argumento, como hicimos para Insert One, con Insert Many, pasamos una array.

En este ejemplo, el array comienza aquí y termina aquí.

Y puede ver que cada elemento es un documento que nos gustaría insertar en esta colección.

Ahora hay opciones adicionales para Insert Many,.

Y los veremos un poco más adelante.

Por ahora, ejecutemos este comando.

Solo voy a copiarlo y pegarlo en nuestro shell.

En este shell, estoy conectado a mi Sandbox Atlas cluster.

Y podemos ver que lo que sucede aquí se trata de lo que esperaría, dada nuestra comprensión de cómo funciona la inserción de documentos en MongoDB.

Echemos un vistazo más de cerca a esta operación.

Entonces nuestro comando es Insert Many.

Y tenemos 1, 2, 3, 4, 5 documentos que estamos intentando insertar.

Tenga en cuenta, sin embargo, que estos dos documentos son idénticos.

Con una inserción masiva, es posible que se produzca un error o una excepción para uno de los documentos que está insertando, especialmente si estamos haciendo algo como una operación de limpieza de datos donde hay mucho ruido en los datos o errores de algún tipo en nuestro conjunto de datos.

Para manejar estos problemas con gracia, hay un par de opciones diferentes para Insert Many que debe conocer.

Puede elegir entre inserciones ordenadas (ordered) o inserciones no ordenadas (unordered).

El valor predeterminado es inserciones ordenadas.

Esto proporciona un buen ejemplo, ya que estamos usando valores IMDB como nuestro `_id` para cada documento en esta llamada Insert Many.

De nuevo, estos dos documentos son idénticos.

Pero lo más importante, son sus valores de `_id` los que son idénticos.

Y sabemos que en una colección individual, los valores de `_id` deben ser únicos.

Entonces, si nos desplazamos hacia abajo y vemos qué sucedió con este comando, podemos ver que tenemos un error de clave duplicada.

Y el error está en la película Wrath of Khan.

También podemos ver, si miramos un poco más abajo en la salida, que el número de documentos insertados fue dos, a pesar de que tenemos cinco documentos en esta llamada Insert Many.

Y si vamos a Compass y nos refrescamos, vemos que hay una película de Star Trek y una película de Wrath of Khan en esta colección.

Entonces, de hecho, solo dos documentos se insertaron de los cinco que intentamos insertar.

Ahora volvamos a nuestro shell.

Como mencioné, Insert Many hace una inserción ordenada, lo que significa que tan pronto como encuentre un error, dejará de insertar documentos.

Debido a que encontró un error aquí, solo estos dos documentos se insertaron en esta colección con esta llamada.

Ahora, en muchas aplicaciones, es posible que simplemente queramos que nuestra aplicación continúe si encuentra un error, porque estamos bien con los documentos que erraron al no insertarse, o tenemos un proceso separado para tratarlos de otra manera.

Para admitir este caso de uso, podemos especificar un segundo argumento para nuestro método Insert Many.

Entonces, volviendo a nuestro editor de texto, el array es el primer argumento para Insert Many.

Como segundo argumento, puedo proporcionar un documento que especifique un valor de `false` para la clave ordenada.

Este documento de Opciones nos permite cambiar el comportamiento predeterminado.

Y en este caso, en lugar de hacer una inserción ordenada, voy a hacer una inserción no ordenada.

Avancemos y ejecutemos esto ahora.

Ahora aquí, en realidad vemos tres errores de escritura.

Hay tres errores de clave duplicados: uno para Wrath of Khan, uno para Star Trek y otro para Wrath of Khan.

Echemos un vistazo a nuestra consulta nuevamente.

Entonces, estas tres inserciones causaron un error de clave duplicada (duplicate key error).

¿Porqué es eso?

Bueno, si recuerdas, estos dos ya estaban en la colección de nuestro intento anterior de usar Insert Many.

Entonces, un intento de insertar este y este nuevamente obtuvo errores de clave duplicados debido a esa inserción anterior.

Éste, por supuesto, tiene exactamente la misma idea que esta, por lo que falla por la misma razón que este documento.

Lo interesante aquí es que, en lugar de simplemente eliminar el error y dejar de ejecutar inserciones, Insert Many continuó e insertó estos dos documentos: Star Trek Into Darkness y Star Trek First Contact.

Si vamos a Compass y actualizamos nuestra vista de colección, podemos ver que ahora hay cuatro documentos en esta colección: Wrath of Khan, Star Trek, Star Trek into Darkness y Star Trek First Contact.

Entonces, al especificar `false` para ordenado, aunque haya, en este caso, tres errores de clave duplicados, Insert Many continua insertando documentos.

Y terminá insertando esos dos últimos en nuestra lista de documentos que pasamos a Insert Many.

En esta lección, vimos Insert Many y las versiones ordenadas y desordenadas de una operación Insert Many.

Insert One e Insert Many son dos formas principales en las que podemos crear documentos.

Hay un par de otras formas en que podemos crear documentos mediante el uso de operaciones de actualización.

Los comandos de actualización pueden provocar la inserción de documentos.

Llamamos a estas operaciones **Upserts**.

Discutimos Upserts en detalle en otra lección.

Con esto, hemos discutido, con cierto detalle, las formas principales en las que creamos documentos en MongoDB e insinuamos la tercera forma en que los datos se pueden insertar en una colección de MongoDB.

Y con eso, hemos abordado la parte Create (Crear) de nuestras operaciones CRUD.

## 15. Laboratorio 2.2: ¿Cuántos insertados?

## 16. Tema: Lectura de documentos: campos escalares

### Transcripción

## 17. Examen

## 18. Laboratorio 2.3: Consultas en campos escalares

## 19. Tema: Lectura de documentos: campos de matriz

### Transcripción



## 20. Examen

## 21. Práctica de laboratorio 2.4: Consultas en campos de array, Parte 1

## 22. Práctica de laboratorio 2.5: Consultas en campos de array, Parte 2

## 23. Tema: Cursores

### Transcripción

## 24. Tema: Proyecciones

### Transcripción

## 25. Tema: Actualización de documentos: updateOne ()

## 26. Conferencia: Operadores de actualización

### Transcripción

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
