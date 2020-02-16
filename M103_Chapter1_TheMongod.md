# Capítulo 1: El Mongod

### 26 Items.

Standalone node configuration and setup
Configuración y configuración de nodos independientes


## Lecciones

1. Tema: El Mongod
2. Examen
3. Laboratorio: Lanzamiento de Mongod
4. Tema: Archivo de Configuración
5. Examen
6. Laboratorio: Archivo de Configuración
7. Tema: Estructura de Archivo
8. Examen
9. Laboratorio: Cambiar la Ruta de Acceso a la Base de Datos Predeterminada
10. Tema: Comandos Básicos
11. Examen
12. Tema: Conceptos Básicos de Registro
13. Examen
14. Tema: Perfil de la Base de Datos
15. Examen
16. Laboratorio: Inicio de Sesión en una Instalación Diferente
17. Conferencia: Seguridad Básica: Parte 1
18. Conferencia: Seguridad Básica: Parte 2
19. Examen
20. Conferencia: Roles Incorporados: Parte 1
21. Conferencia: Roles Incorporados: Parte 2
22. Examen
23. Laboratorio: Creación del Primer Usuario de la Aplicación
24. Tema: Descripción General de las Herramientas del Servidor
25. Examen
26. Laboratorio: Importación de un Conjunto de Datos


## 1. Tema: El Mongod

### Lecture Notes

You can read more about the MongoDB server in the [daemon documentation](https://docs.mongodb.com/manual/reference/program/mongod/).

Note: The `--fork` option is not available on the Windows operating system.

**Lecture Instructions**

Connect to the vagrant box:

```sh
cd m103-vagrant-env
vagrant ssh
```

Launch mongod in our first shell:

```sh
mongod
```

Connect to the Mongo shell in our second shell:

```sh
mongo
```

Shutdown mongod from our second shell:

```sh
mongo admin --eval 'db.shutdownServer()'
```

Find command line options for mongod:

```sh
mongod --help
```

Create new data directory and try to launch mongod with a new `port` and `dbpath`, and also `fork` the process:

```sh
mkdir first_mongod
mongod --port 30000 --dbpath first_mongod --fork
```

The above command will fail without a `logpath` - so we add one and then successfully launch mongod:

```sh
mongod --port 30000 --dbpath first_mongod --logpath first_mongod/mongod.log --fork
```

Try to connect back to Mongo shell, without specifying a port:

```sh
mongo
```

We need to add a port, because our mongod is running on port 30000, not the default 27017:

```sh
mongo --port 30000
```

Shutdown the new server:

```sh
mongo admin --port 30000 --eval 'db.shutdownServer()'
```

### Transcripción

Todo bien.

Entonces, en esta lección, vamos a discutir el mongod.

Aprenderá qué es el mongod, cómo funciona a un alto nivel y qué es un demonio, la disponibilidad del mongod y las opciones que puede usar para configurarlo.

Mongod es el principal proceso de demonio para MongoDB.

Es la unidad central de la base de datos, que maneja las conexiones, las solicitudes y la parte más importante de esta, persistiendo sus datos o escribiendo sus datos en un disco.

Entonces, ¿qué es un demonio?

Un daemon es un programa, un proceso que debe ejecutarse y no interactuar de manera directa.

Por convención, los demonios tienen un d añadido al final de su nombre, de ahí el nombre mongod.

Cuando corremos

mongod, no interactuamos con él directamente.

En cambio, nuestra aplicación utiliza un controlador para comunicarse con el mongod.

La interacción del usuario es a nivel de aplicación.

Y el conductor maneja todos los detalles esenciales de la comunicación con el mongod.

MongoDB está disponible en numerosas arquitecturas de 64 bits, incluidas las populares, como Ubuntu, Windows y otras.

Para fines de desarrollo y demostración, también puede instalarlo en un Mac OS.

Todo bien.

Entonces, ahora que tenemos una idea de qué es mongod, discutamos su uso.

Mongod se inicia más fácilmente ejecutando el comando mongod.

Vemos un montón de resultados aquí, pero la información más importante está en la parte superior e inferior.

Todo bien.

Ahora, para que podamos ver aquí, mongod nos ha dicho la identificación del proceso de este proceso mongod, 5838, el puerto en el que se está ejecutando, 27017, el dbpath.

Y ambos son valores predeterminados, eso es porque no especificamos nada más.

También nos da el nombre de host de la caja vagabunda, m103.

Y podemos ver, en la parte inferior aquí, que está escuchando conexiones en el puerto 27017.

Tenga en cuenta que nuestro terminal ya no interactúa.

Por ejemplo, si escribimos, no pasa nada.

La terminal nos da una nueva línea.

Tenemos que abrir una ventana y terminal diferente para conectarnos a mongo aquí.

Así que vamos a abrir una segunda ventana de terminal aquí y escribir el comando mongo para conectarse a mongo.

Esto se conectará en el puerto predeterminado, que es 27017.

Y estamos conectados.

Ahora, ejecutemos el comando show dbs desde el shell para obtener una lista de bases de datos.

Vemos admin y local.

Y hubo hacer.

Tenga en cuenta que si ya ha estado usando MongoDB localmente, es posible que vea más bases de datos que esta.

Entonces, mirando hacia atrás en la salida del terminal de mongod, podemos ver exactamente cuándo nos conectamos.

Las líneas con la información "conexión aceptada" nos pueden decir exactamente cuándo ocurrió una conexión y de dónde vino.

En este caso solo fuimos nosotros conectando a través de la concha de mongo.

Bien, hemos terminado con este mongod por ahora, así que vamos a apagar el servidor y salir del shell mongo.

Para hacerlo, volveremos a la terminal con el shell mongo, usaremos la base de datos de administración y luego ejecutaremos el comando de apagado.

Finalmente, saldremos del shell mongo con el comando de salida.

Así que solo voy a usar la base de datos de administración aquí, y luego voy a ejecutar el comando db.shutdownServer.

Esto va a cerrar el proceso mongod.

Y si miramos la salida mongod, podemos ver que está apagada.

Para salir del shell mongo, simplemente teníamos que escribir exit.

Podemos ver la salida de mongod, que cuando se apagó, eliminó los archivos de socket y, en general, simplemente se limpió después de sí mismo, solo para ser un buen proceso en nuestra computadora y no ocupar demasiado espacio mientras no corriendo.

Todo bien.

Así que ahora hablemos sobre cómo configurar el mongod.

El mongod toma una miríada de banderas, y podemos verlas todas escribiendo el comando mongod --help.

Eso es mucha flexibilidad.

Por ahora, los que vamos a cubrir son port, dbpath, logpath y fork.

--port es el argumento utilizado para decirle a mongod en qué puerto escuchar.

Si esto no se especifica, mongod escuchará en el puerto 27017 de forma predeterminada.

El --dbpath es donde mongod crea los archivos que almacenarán información para nuestras bases de datos y colecciones.

--Logpath es uno que no especificamos en este momento, pero es donde mongod registrará mensajes informativos.

Vimos esto temprano cuando corrimos mongod.

Pero en ese caso, lo imprimió en la salida estándar o en el terminal.

En cambio, podríamos especificar un destino con un logpath y mongod generará la información en ese archivo.

--Fork es un argumento usado para decirle a mongod que comience como un proceso en segundo plano.

Podemos usar esto para que mongod no bloquee la ventana de terminal actual como acaba de hacer.

Todo bien.

Así que vamos a probar algunas de estas opciones.

Me gustaría iniciar MongoDB en el puerto 30,000 con un dbpath a un directorio local llamado first_mongod y enviar el proceso para que no bloquee mi shell.

Primero, para especificar una ruta db diferente, tenemos que crear el directorio de antemano.

Mongod no hará eso por nosotros.

Entonces, en este comando, solo estamos creando el directorio first_mongod.

Entonces, si ejecutamos esto, nos dará un error.

Olvidé mencionar que si especificamos la bandera fork, también debe especificar una ruta de acceso.

Así que configuremos la ruta de acceso a un archivo llamado mongod.log en el primer directorio mongod.

Todo bien.

Entonces, ahora que hemos especificado nuestra ruta de acceso, esto debería funcionar.

Y lo hace

Bifurcó el proceso, nos da el nombre de la ID del proceso-- 6114, y nos dice que el proceso hijo comienza con éxito.

Entonces, debido a que bifurcamos este proceso, en realidad tenemos acceso al shell incluso después de iniciar mongod.

Entonces podemos conectarnos a mongo en el mismo shell.

Sin embargo, esto no va a funcionar, porque está intentando conectarse en el puerto 27017, pero hemos comenzado nuestro proceso en el puerto 30,000.

Si especificamos eso al comando mongo.

Debería conectarnos al puerto correcto.

Y lo hace

Simplemente podemos ejecutar un show dbs rápido para asegurarnos de que se esperan nuestras bases de datos.

Y nuevamente, tenemos la base de datos de administración y la base de datos local.

Así que ahora voy a cerrar este como lo hicimos la última vez.

Así que cerramos nuestro servidor desde la base de datos de administración, y luego salimos del shell mongo.

Todo bien.

Así que solo como resumen.

En esta lección aprendimos que mongod es el principal proceso de demonio para MongoDB.

Interactuamos con mongod a través de un controlador, no directamente.

Mongod está disponible en muchos sistemas de 64 bits.

Y aprendimos sobre algunas de las opciones básicas de configuración de la línea de comandos.


## 2. Examen The Mongod

**Problem:**

When specifying the `--fork` argument to mongod, what must also be specified?

Check all answers that apply:


* `--loglocation`

* `--logpath`

* `--logdestination`

* `--logfile`

## 3. Laboratorio: Lanzamiento de Mongod

### Lab - Launching Mongod

**Problem:**

In this lab, you're going to launch your own mongod with a few basic command line arguments.

Applying what you've learned so far about the `mongod process, launch a mongod instance, from the command line, with the following requirements:

run on port 27000
data files are stored in /data/db/
listens to connections from the IP address 192.168.103.100 and localhost
authentication is enabled
By default, a mongod that enforces authentication but has no configured users only allows connections through the localhost. Use the mongo shell on the Vagrant box to use the localhost exception and connect to this node.

Use the following command to connect to the Mongo shell and create the following user. You will need this user in order to validate subsequent labs.

mongo admin --host localhost:27000 --eval '
  db.createUser({
    user: "m103-admin",
    pwd: "m103-pass",
    roles: [
      {role: "root", db: "admin"}
    ]
  })
'
 COPY
The above command creates a user with the following credentials:

Role: root on admin database
Username: m103-admin
Password: m103-pass
When you're finished, run the following validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

vagrant@m103:~$ validate_lab_launch_mongod
 COPY
Hint: You want to make sure all applicable command line options are set! Also, in case you need to restart the mongod daemon, you may need to kill the process using it's pid.

You can use the following command to find the pid of the process:

ps -ef | grep mongod
 COPY
To kill the process, you can use this command:

kill <pid>
 COPY
Attempts Remaining:3 Attempts left

Enter answer here:

## 4. Tema: Archivo de Configuración

### Transcripción

## 5. Examen

## 6. Laboratorio: Archivo de Configuración

## 7. Tema: Estructura de Archivo

### Transcripción

## 8. Examen

## 9. Laboratorio: Cambiar la Ruta de Acceso a la Base de Datos Predeterminada

## 10. Tema: Comandos Básicos

### Transcripción

## 11. Examen

## 12. Tema: Conceptos Básicos de Registro

### Transcripción

## 13. Examen

## 14. Tema: Perfil de la Base de Datos

### Transcripción

## 15. Examen

## 16. Laboratorio: Inicio de Sesión en una Instalación Diferente

## 17. Tema: Seguridad Básica: Parte 1

### Transcripción

## 18. Tema: Seguridad Básica: Parte 2

### Transcripción

## 19. Examen

## 20. Tema: Roles Incorporados: Parte 1

### Transcripción

## 21. Tema: Roles Incorporados: Parte 2

### Transcripción

## 22. Examen

## 23. Laboratorio: Creación del Primer Usuario de la Aplicación

## 24. Tema: Descripción General de las Herramientas del Servidor

### Transcripción

## 25. Examen

## 26. Laboratorio: Importación de un Conjunto de Datos
