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

<img src="/images/m103/c1/1-1-objetivos.png">

Aprenderá qué es el mongod, cómo funciona a un alto nivel y qué es un demonio (daemon), la disponibilidad del mongod y las opciones que puede usar para configurarlo.

Mongod es el principal proceso de demonio para MongoDB.

<img src="/images/m103/c1/1-1-mongod.png">

Es la unidad central de la base de datos, que maneja las conexiones, las solicitudes y la parte más importante de esta, persistiendo sus datos o escribiendo sus datos en un disco.

Entonces, ¿qué es un demonio?

<img src="/images/m103/c1/1-1-daemon.png">

**Un daemon es un programa, un proceso que debe ejecutarse y no interactuar de manera directa**.

Por convención, los demonios tienen un **d** añadido al final de su nombre, de ahí el nombre **mongod**.

Cuando corremos mongod, no interactuamos con él directamente.

En cambio, nuestra aplicación utiliza un controlador(driver) para comunicarse con el mongod.

La interacción del usuario es a nivel de aplicación.

Y el driver maneja todos los detalles esenciales de la comunicación con el mongod.

MongoDB está disponible en numerosas arquitecturas de 64 bits, incluidas las populares, como Ubuntu, Windows y otras.

<img src="/images/m103/c1/1-1-arquitecturas.png">

Para fines de desarrollo y demostración, también puede instalarlo en un Mac OS.

Todo bien.

Entonces, ahora que tenemos una idea de qué es mongod, discutamos su uso.

**Mongod se inicia más fácilmente ejecutando el comando `mongod`**.

```sh
vagrant@m103:~$ mongod
2020-02-17T18:09:33.728+0000 I CONTROL  [initandlisten] MongoDB starting : pid=2226 port=27017 dbpath=/data/db 64-bit host=m103
2020-02-17T18:09:33.729+0000 I CONTROL  [initandlisten] db version v3.6.17
2020-02-17T18:09:33.730+0000 I CONTROL  [initandlisten] git version: 3d6953c361213c5bfab23e51ab274ce592edafe6
2020-02-17T18:09:33.730+0000 I CONTROL  [initandlisten] OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
2020-02-17T18:09:33.731+0000 I CONTROL  [initandlisten] allocator: tcmalloc
2020-02-17T18:09:33.731+0000 I CONTROL  [initandlisten] modules: enterprise 
2020-02-17T18:09:33.732+0000 I CONTROL  [initandlisten] build environment:
2020-02-17T18:09:33.732+0000 I CONTROL  [initandlisten]     distmod: ubuntu1404
2020-02-17T18:09:33.733+0000 I CONTROL  [initandlisten]     distarch: x86_64
2020-02-17T18:09:33.733+0000 I CONTROL  [initandlisten]     target_arch: x86_64
2020-02-17T18:09:33.734+0000 I CONTROL  [initandlisten] options: {}
2020-02-17T18:09:33.735+0000 I STORAGE  [initandlisten] 
2020-02-17T18:09:33.736+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-02-17T18:09:33.736+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-02-17T18:09:33.737+0000 I STORAGE  [initandlisten] wiredtiger_open config: create,cache_size=488M,cache_overflow=(file_max=0M),session_max=20000,eviction=(threads_min=4,threads_max=4),config_base=false,statistics=(fast),compatibility=(release="3.0",require_max="3.0"),log=(enabled=true,archive=true,path=journal,compressor=snappy),file_manager=(close_idle_time=100000),statistics_log=(wait=0),verbose=(recovery_progress),
2020-02-17T18:09:34.658+0000 I STORAGE  [initandlisten] WiredTiger message [1581962974:658120][2226:0x7f983f818ac0], txn-recover: Set global recovery timestamp: 0
2020-02-17T18:09:34.680+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.681+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2020-02-17T18:09:34.681+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2020-02-17T18:09:34.682+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.682+0000 I CONTROL  [initandlisten] ** WARNING: This server is bound to localhost.
2020-02-17T18:09:34.683+0000 I CONTROL  [initandlisten] **          Remote systems will be unable to connect to this server. 
2020-02-17T18:09:34.683+0000 I CONTROL  [initandlisten] **          Start the server with --bind_ip <address> to specify which IP 
2020-02-17T18:09:34.683+0000 I CONTROL  [initandlisten] **          addresses it should serve responses from, or with --bind_ip_all to
2020-02-17T18:09:34.684+0000 I CONTROL  [initandlisten] **          bind to all interfaces. If this behavior is desired, start the
2020-02-17T18:09:34.684+0000 I CONTROL  [initandlisten] **          server with --bind_ip 127.0.0.1 to disable this warning.
2020-02-17T18:09:34.684+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.685+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.685+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-02-17T18:09:34.685+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-17T18:09:34.686+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.686+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-02-17T18:09:34.687+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-17T18:09:34.687+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.796+0000 I STORAGE  [initandlisten] createCollection: admin.system.version with provided UUID: 9260dafa-0288-46da-9a1a-4c27213a57ac
2020-02-17T18:09:34.828+0000 I COMMAND  [initandlisten] setting featureCompatibilityVersion to 3.6
2020-02-17T18:09:34.839+0000 I STORAGE  [initandlisten] createCollection: local.startup_log with generated UUID: 2d47d4be-20e9-4602-9350-4a5062d6edd1
2020-02-17T18:09:34.846+0000 I FTDC     [initandlisten] Initializing full-time diagnostic data capture with directory '/data/db/diagnostic.data'
2020-02-17T18:09:34.846+0000 I NETWORK  [initandlisten] listening via socket bound to 127.0.0.1
2020-02-17T18:09:34.846+0000 I NETWORK  [initandlisten] listening via socket bound to /tmp/mongodb-27017.sock
2020-02-17T18:09:34.846+0000 I NETWORK  [initandlisten] waiting for connections on port 27017
2020-02-17T18:09:34.859+0000 I STORAGE  [LogicalSessionCacheRefresh] createCollection: config.system.sessions with generated UUID: ee39bfbd-0de8-4c79-8592-55e1c474aeab
2020-02-17T18:09:34.872+0000 I INDEX    [LogicalSessionCacheRefresh] build index on: config.system.sessions properties: { v: 2, key: { lastUse: 1 }, name: "lsidTTLIndex", ns: "config.system.sessions", expireAfterSeconds: 1800 }
2020-02-17T18:09:34.872+0000 I INDEX    [LogicalSessionCacheRefresh] 	 building index using bulk method; build may temporarily use up to 500 megabytes of RAM
2020-02-17T18:09:34.874+0000 I INDEX    [LogicalSessionCacheRefresh] build index done.  scanned 0 total records. 0 secs

```

Vemos un montón de resultados aquí, pero la información más importante está en la parte superior e inferior.

`MongoDB starting : pid=2226 port=27017 dbpath=/data/db 64-bit host=m103`

Ahora, para que podamos ver aquí, mongod nos ha dicho la identificación del proceso de este proceso mongod, 2226, el puerto en el que se está ejecutando, 27017, el dbpath /data/db 

Y ambos son valores predeterminados, eso es porque no especificamos nada más.

También nos da el nombre de host de la caja vagrant, m103.

Y podemos ver, en la parte inferior aquí, que está escuchando conexiones en el puerto 27017.

`waiting for connections on port 27017`

Tenga en cuenta que nuestro terminal ya no interactúa.

Por ejemplo, si escribimos, no pasa nada.

La terminal nos da una nueva línea.

Tenemos que abrir una ventana y terminal diferente para conectarnos a mongo aquí.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ vagrant ssh
Welcome to Ubuntu 14.04.6 LTS (GNU/Linux 3.13.0-170-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Mon Feb 17 17:24:52 UTC 2020

  System load:  0.0               Processes:           85
  Usage of /:   5.1% of 39.34GB   Users logged in:     0
  Memory usage: 6%                IP address for eth0: 10.0.2.15
  Swap usage:   0%                IP address for eth1: 192.168.103.100

  Graph this data and manage this system at:
    https://landscape.canonical.com/

New release '16.04.6 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Last login: Mon Feb 17 17:24:53 2020 from 10.0.2.2
vagrant@m103:~$ 
```

Así que vamos a abrir una segunda ventana de terminal aquí y escribir el comando mongo para conectarse a mongo.

```sh
vagrant@m103:~$ mongo
MongoDB shell version v3.6.17
connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("3107a574-0174-4614-a665-e5e2c324ff51") }
MongoDB server version: 3.6.17
Welcome to the MongoDB shell.
For interactive help, type "help".
For more comprehensive documentation, see
	http://docs.mongodb.org/
Questions? Try the support group
	http://groups.google.com/group/mongodb-user
Server has startup warnings: 
2020-02-17T18:09:33.735+0000 I STORAGE  [initandlisten] 
2020-02-17T18:09:33.736+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-02-17T18:09:33.736+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-02-17T18:09:34.680+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.681+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2020-02-17T18:09:34.681+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2020-02-17T18:09:34.682+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.682+0000 I CONTROL  [initandlisten] ** WARNING: This server is bound to localhost.
2020-02-17T18:09:34.683+0000 I CONTROL  [initandlisten] **          Remote systems will be unable to connect to this server. 
2020-02-17T18:09:34.683+0000 I CONTROL  [initandlisten] **          Start the server with --bind_ip <address> to specify which IP 
2020-02-17T18:09:34.683+0000 I CONTROL  [initandlisten] **          addresses it should serve responses from, or with --bind_ip_all to
2020-02-17T18:09:34.684+0000 I CONTROL  [initandlisten] **          bind to all interfaces. If this behavior is desired, start the
2020-02-17T18:09:34.684+0000 I CONTROL  [initandlisten] **          server with --bind_ip 127.0.0.1 to disable this warning.
2020-02-17T18:09:34.684+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.685+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.685+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-02-17T18:09:34.685+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-17T18:09:34.686+0000 I CONTROL  [initandlisten] 
2020-02-17T18:09:34.686+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-02-17T18:09:34.687+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-17T18:09:34.687+0000 I CONTROL  [initandlisten] 
MongoDB Enterprise > 

```

Esto se conectará en el puerto predeterminado, que es 27017.

Y estamos conectados.

Ahora, ejecutemos el comando `show dbs` desde el shell para obtener una lista de bases de datos.

```sh
MongoDB Enterprise > show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
MongoDB Enterprise > 
```
Vemos admin y local.

Tenga en cuenta que si ya ha estado usando MongoDB localmente, es posible que vea más bases de datos que esta.

Entonces, mirando hacia atrás en la salida del terminal de mongod, podemos ver exactamente cuándo nos conectamos.

Las líneas con la información "conexión aceptada" nos pueden decir exactamente cuándo ocurrió una conexión y de dónde vino.

```sh
connection accepted from 127.0.0.1:59213 #1 (1 connection now open)
```

En este caso solo fuimos nosotros conectando a través del shell de mongo.

Bien, hemos terminado con este mongod por ahora, así que vamos a apagar el servidor y salir del shell mongo.

Para hacerlo, volveremos a la terminal con el shell mongo, usaremos la base de datos de administración y luego ejecutaremos el comando de apagado `db.shutdownServer()`.

```sh
MongoDB Enterprise > use admin
switched to db admin
MongoDB Enterprise > db.shutdownServer()
server should be down...
2020-02-17T18:24:50.324+0000 I NETWORK  [thread1] trying reconnect to 127.0.0.1:27017 (127.0.0.1) failed
2020-02-17T18:24:50.325+0000 W NETWORK  [thread1] Failed to connect to 127.0.0.1:27017, in(checking socket for error after poll), reason: Connection refused
2020-02-17T18:24:50.325+0000 I NETWORK  [thread1] reconnect 127.0.0.1:27017 (127.0.0.1) failed failed 
MongoDB Enterprise > 
```

Si vemos nuestro servidor mongod tenemos:

```
020-02-17T18:24:50.227+0000 I FTDC     [conn1] Shutting down full-time diagnostic data capture
2020-02-17T18:24:50.229+0000 I STORAGE  [conn1] WiredTigerKVEngine shutting down
2020-02-17T18:24:50.247+0000 I STORAGE  [conn1] shutdown: removing fs lock...
2020-02-17T18:24:50.248+0000 I CONTROL  [conn1] now exiting
2020-02-17T18:24:50.248+0000 I CONTROL  [conn1] shutting down with code:0
vagrant@m103:~$ 
```
No ha sacado del servidor

Finalmente, saldremos del shell mongo con el comando salir.

Así que solo voy a usar la base de datos de administración aquí, y luego voy a ejecutar el comando `db.shutdownServer`.

Esto va a cerrar el proceso mongod.

Y si miramos la salida mongod, podemos ver que está apagada.

Para salir del shell mongo, simplemente teníamos que escribir exit.

```sh
MongoDB Enterprise > exit
bye
2020-02-17T18:27:47.286+0000 I NETWORK  [thread1] trying reconnect to 127.0.0.1:27017 (127.0.0.1) failed
2020-02-17T18:27:47.287+0000 W NETWORK  [thread1] Failed to connect to 127.0.0.1:27017, in(checking socket for error after poll), reason: Connection refused
2020-02-17T18:27:47.287+0000 I NETWORK  [thread1] reconnect 127.0.0.1:27017 (127.0.0.1) failed failed 
2020-02-17T18:27:47.287+0000 I QUERY    [thread1] Failed to end session { id: UUID("3107a574-0174-4614-a665-e5e2c324ff51") } due to SocketException: socket exception [CONNECT_ERROR] for couldn't connect to server 127.0.0.1:27017, connection attempt failed
vagrant@m103:~$ 
```

Podemos ver la salida de mongod, que cuando se apagó, eliminó los archivos de socket y, en general, simplemente se limpió después de sí mismo, solo para ser un buen proceso en nuestra computadora y no ocupar demasiado espacio mientras no corriendo.

Así que ahora hablemos sobre cómo configurar el mongod.

El mongod toma una miríada de banderas, y podemos verlas todas escribiendo el comando `mongod --help`.

```sh
vagrant@m103:~$ mongod --help
Options:

General options:
  -h [ --help ]                         show this usage information
  --version                             show version information
  -f [ --config ] arg                   configuration file specifying 
                                        additional options
  -v [ --verbose ] [=arg(=v)]           be more verbose (include multiple times
                                        for more verbosity e.g. -vvvvv)
  --quiet                               quieter output
  --port arg                            specify port number - 27017 by default
  --bind_ip arg                         comma separated list of ip addresses to
                                        listen on - localhost by default
  --bind_ip_all                         bind to all ip addresses
  --ipv6                                enable IPv6 support (disabled by 
                                        default)
  --listenBacklog arg (=128)            set socket listen backlog size
  --maxConns arg                        max number of simultaneous connections 
                                        - 1000000 by default
  --logpath arg                         log file to send write to instead of 
                                        stdout - has to be a file, not 
                                        directory
  --syslog                              log to system's syslog facility instead
                                        of file or stdout
  --syslogFacility arg                  syslog facility used for mongodb syslog
                                        message
  --logappend                           append to logpath instead of 
                                        over-writing
  --logRotate arg                       set the log rotation behavior 
                                        (rename|reopen)
  --timeStampFormat arg                 Desired format for timestamps in log 
                                        messages. One of ctime, iso8601-utc or 
                                        iso8601-local
  --redactClientLogData                 Redact client data written to the 
                                        diagnostics log
  --pidfilepath arg                     full path to pidfile (if not set, no 
                                        pidfile is created)
  --timeZoneInfo arg                    full path to time zone info directory, 
                                        e.g. /usr/share/zoneinfo
  --keyFile arg                         private key for cluster authentication
  --noauth                              run without security
  --setParameter arg                    Set a configurable parameter
  --transitionToAuth                    For rolling access control upgrade. 
                                        Attempt to authenticate over outgoing 
                                        connections and proceed regardless of 
                                        success. Accept incoming connections 
                                        with or without authentication.
  --clusterAuthMode arg                 Authentication mode used for cluster 
                                        authentication. Alternatives are 
                                        (keyFile|sendKeyFile|sendX509|x509)
  --nounixsocket                        disable listening on unix sockets
  --unixSocketPrefix arg                alternative directory for UNIX domain 
                                        sockets (defaults to /tmp)
  --filePermissions arg                 permissions to set on UNIX domain 
                                        socket file - 0700 by default
  --fork                                fork server process
  --networkMessageCompressors [=arg(=disabled)] (=snappy)
                                        Comma-separated list of compressors to 
                                        use for network messages
  --auth                                run with security
  --clusterIpSourceWhitelist arg        Network CIDR specification of permitted
                                        origin for `__system` access.
  --slowms arg (=100)                   value of slow for profile and console 
                                        log
  --slowOpSampleRate arg (=1)           fraction of slow ops to include in the 
                                        profile and console log
  --profile arg                         0=off 1=slow, 2=all
  --cpu                                 periodically show cpu and iowait 
                                        utilization
  --sysinfo                             print some diagnostic system 
                                        information
  --noIndexBuildRetry                   don't retry any index builds that were 
                                        interrupted by shutdown
  --noscripting                         disable scripting engine
  --notablescan                         do not allow table scans
  --shutdown                            kill a running server (for init 
                                        scripts)

Replication options:
  --oplogSize arg                       size to use (in MB) for replication op 
                                        log. default is 5% of disk space (i.e. 
                                        large is good)

Master/slave options (old; use replica sets instead):
  --master                              master mode
  --slave                               slave mode
  --source arg                          when slave: specify master as 
                                        <server:port>
  --only arg                            when slave: specify a single database 
                                        to replicate
  --slavedelay arg                      specify delay (in seconds) to be used 
                                        when applying master ops to slave
  --autoresync                          automatically resync if slave data is 
                                        stale

Replica set options:
  --replSet arg                         arg is <setname>[/<optionalseedhostlist
                                        >]
  --replIndexPrefetch arg               specify index prefetching behavior (if 
                                        secondary) [none|_id_only|all]
  --enableMajorityReadConcern [=arg(=1)] (=1)
                                        enables majority readConcern

Sharding options:
  --configsvr                           declare this is a config db of a 
                                        cluster; default port 27019; default 
                                        dir /data/configdb
  --shardsvr                            declare this is a shard db of a 
                                        cluster; default port 27018

SSL options:
  --sslOnNormalPorts                    use ssl on configured ports
  --sslMode arg                         set the SSL operation mode 
                                        (disabled|allowSSL|preferSSL|requireSSL
                                        )
  --sslPEMKeyFile arg                   PEM file for ssl
  --sslPEMKeyPassword arg               PEM file password
  --sslClusterFile arg                  Key file for internal SSL 
                                        authentication
  --sslClusterPassword arg              Internal authentication key file 
                                        password
  --sslCAFile arg                       Certificate Authority file for SSL
  --sslClusterCAFile arg                CA used for verifying remotes during 
                                        outbound connections
  --sslCRLFile arg                      Certificate Revocation List file for 
                                        SSL
  --sslDisabledProtocols arg            Comma separated list of TLS protocols 
                                        to disable [TLS1_0,TLS1_1,TLS1_2]
  --sslWeakCertificateValidation        allow client to connect without 
                                        presenting a certificate
  --sslAllowConnectionsWithoutCertificates 
                                        allow client to connect without 
                                        presenting a certificate
  --sslAllowInvalidHostnames            Allow server certificates to provide 
                                        non-matching hostnames
  --sslAllowInvalidCertificates         allow connections to servers with 
                                        invalid certificates
  --sslFIPSMode                         activate FIPS 140-2 mode at startup

Storage options:
  --storageEngine arg                   what storage engine to use - defaults 
                                        to wiredTiger if no data files present
  --dbpath arg                          directory for datafiles - defaults to 
                                        /data/db
  --directoryperdb                      each database will be stored in a 
                                        separate directory
  --noprealloc                          disable data file preallocation - will 
                                        often hurt performance
  --nssize arg (=16)                    .ns file size (in MB) for new databases
  --quota                               limits each database to a certain 
                                        number of files (8 default)
  --quotaFiles arg                      number of files allowed per db, implies
                                        --quota
  --smallfiles                          use a smaller default file size
  --syncdelay arg (=60)                 seconds between disk syncs (0=never, 
                                        but not recommended)
  --upgrade                             upgrade db if needed
  --repair                              run repair on all dbs
  --repairpath arg                      root directory for repair files - 
                                        defaults to dbpath
  --journal                             enable journaling
  --nojournal                           disable journaling (journaling is on by
                                        default for 64 bit)
  --journalOptions arg                  journal diagnostic options
  --journalCommitInterval arg           how often to group/batch commit (ms)

Auditing Options:
  --auditDestination arg                Destination of audit log output.  
                                        (console/syslog/file)
  --auditFormat arg                     Format of the audit log, if logging to 
                                        a file.  (BSON/JSON)
  --auditPath arg                       full filespec for audit log file
  --auditFilter arg                     filter spec to screen audit records

SNMP Module Options:
  --snmp-subagent                       run snmp subagent
  --snmp-master                         run snmp as master

WiredTiger options:
  --wiredTigerCacheSizeGB arg           maximum amount of memory to allocate 
                                        for cache; defaults to 1/2 of physical 
                                        RAM
  --wiredTigerJournalCompressor arg (=snappy)
                                        use a compressor for log records 
                                        [none|snappy|zlib]
  --wiredTigerDirectoryForIndexes       Put indexes and data in different 
                                        directories
  --wiredTigerMaxCacheOverflowFileSizeGB arg (=0)
                                        Maximum amount of disk space to use for
                                        cache overflow; Defaults to 0 
                                        (unbounded)
  --wiredTigerCollectionBlockCompressor arg (=snappy)
                                        block compression algorithm for 
                                        collection data [none|snappy|zlib]
  --wiredTigerIndexPrefixCompression arg (=1)
                                        use prefix compression on row-store 
                                        leaf pages

InMemory options:
  --inMemorySizeGB arg                  maximum amount of memory to allocate 
                                        for InMemory data; defaults to 50% of 
                                        physical RAM less 1GB

Encryption at rest options:
  --enableEncryption                    Enable encryption at rest
  --encryptionKeyFile arg               File path for encryption key file
  --encryptionCipherMode arg            Cipher mode to use for encryption at 
                                        rest
  --kmipRotateMasterKey                 Rotate master encryption key
  --kmipKeyIdentifier arg               KMIP unique identifier for existing key
                                        to use
  --kmipServerName arg                  KMIP server host name
  --kmipPort arg                        KMIP server port (defaults to 5696)
  --kmipClientCertificateFile arg       Client certificate for authenticating 
                                        to KMIP server
  --kmipClientCertificatePassword arg   Client certificate for authenticating 
                                        Mongo to KMIP server
  --kmipServerCAFile arg                CA File for validating connection to 
                                        KMIP server

LDAP Module Options:
  --ldapServers arg                     Comma separated list of LDAP servers on
                                        format  host:port
  --ldapTransportSecurity arg (=tls)    Transport security used between MongoDB
                                        and remote LDAP server(none|tls)
  --ldapBindMethod arg (=simple)        Authentication scheme to use while 
                                        connecting to LDAP. This may either be 
                                        'sasl' or 'simple'
  --ldapBindSaslMechanisms arg (=DIGEST-MD5)
                                        Comma separated list of SASL mechanisms
                                        to use while binding to the LDAP server
  --ldapTimeoutMS arg (=10000)          Timeout for LDAP queries (ms)
  --ldapQueryUser arg                   LDAP entity to bind with to perform 
                                        queries
  --ldapQueryPassword arg               Password to use while binding to the 
                                        LDAP server to perform queries
  --ldapUserToDNMapping arg (=[{match: "(.+)", substitution: "{0}"}])
                                        Tranformation from MongoDB users to 
                                        LDAP user DNs
  --ldapAuthzQueryTemplate arg          Relative LDAP query URL which will be 
                                        queried against the host to acquire 
                                        LDAP groups. The token {USER} will be 
                                        replaced with the mapped username

vagrant@m103:~$ 

```

Eso es mucha flexibilidad.

Por ahora, los que vamos a cubrir son `port`, `dbpath`, `logpath` y `fork`.

`--port` es el argumento utilizado para decirle a mongod en qué puerto escuchar.

<img src="/images/m103/c1/1-1-port.png">

Si esto no se especifica, mongod escuchará en el puerto 27017 de forma predeterminada.

El `--dbpath` es donde mongod crea los archivos que almacenarán información para nuestras bases de datos y colecciones.

<img src="/images/m103/c1/1-1-dbpath.png">

`--logpath` es uno que no especificamos en este momento, pero es donde mongod registrará mensajes informativos.

<img src="/images/m103/c1/1-1-logpath.png">

Vimos esto temprano cuando corrimos mongod.

Pero en ese caso, lo imprimió en la salida estándar o en el terminal.

En cambio, podríamos especificar un destino con un logpath y mongod generará la información en ese archivo.

`--fork` es un argumento usado para decirle a mongod que comience como un proceso en segundo plano.

<img src="/images/m103/c1/1-1-fork.png">

Podemos usar esto para que mongod no bloquee la ventana de terminal actual como acaba de hacer.

Así que vamos a probar algunas de estas opciones.

Me gustaría iniciar MongoDB en el puerto 30,000 con un dbpath a un directorio local llamado first_mongod y enviar el proceso para que no bloquee mi shell.

Primero, para especificar una ruta db diferente, tenemos que crear el directorio de antemano.

Mongod no hará eso por nosotros.

Entonces, en este comando, solo estamos creando el directorio first_mongod.

Entonces, si ejecutamos esto, nos dará un error.

```sh
vagrant@m103:~$ mkdir first_mongod
vagrant@m103:~$ mongod --port 30000 --dbpath first_mongod --fork
BadValue: --fork has to be used with --logpath or --syslog
try 'mongod --help' for more information
vagrant@m103:~$ 
```

Olvidé mencionar que si especificamos la bandera fork, también debe especificar una ruta de acceso.

Así que configuremos la ruta de acceso a un archivo llamado mongod.log en el primer directorio mongod.

Entonces, ahora que hemos especificado nuestra ruta de acceso, esto debería funcionar.

```sh
vagrant@m103:~$ mongod --port 30000 --dbpath first_mongod --logpath first_mongod/mongod.log --fork
about to fork child process, waiting until server is ready for connections.
forked process: 2514
child process started successfully, parent exiting
vagrant@m103:~$ 
```

Y lo hace

Bifurcó el proceso, nos da el nombre de la ID del proceso-- 2514, y nos dice que el proceso hijo comienza con éxito.

Entonces, debido a que bifurcamos este proceso, en realidad tenemos acceso al shell incluso después de iniciar mongod.

**Entonces podemos conectarnos a mongo en el mismo shell**.

```sh
vagrant@m103:~$ mongo
MongoDB shell version v3.6.17
connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
2020-02-17T18:48:01.569+0000 W NETWORK  [thread1] Failed to connect to 127.0.0.1:27017, in(checking socket for error after poll), reason: Connection refused
2020-02-17T18:48:01.570+0000 E QUERY    [thread1] Error: couldn't connect to server 127.0.0.1:27017, connection attempt failed :
connect@src/mongo/shell/mongo.js:263:13
@(connect):1:6
exception: connect failed
vagrant@m103:~$ 

```

Sin embargo, esto no va a funcionar, porque está intentando conectarse en el puerto 27017, pero hemos comenzado nuestro proceso en el puerto 30000.

Si especificamos eso al comando mongo.

Debería conectarnos al puerto correcto.

```sh
vagrant@m103:~$ mongo --port 30000
MongoDB shell version v3.6.17
connecting to: mongodb://127.0.0.1:30000/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("8860a539-2154-475a-84b4-617b83ffbcd0") }
MongoDB server version: 3.6.17
Server has startup warnings: 
2020-02-17T18:46:15.086+0000 I STORAGE  [initandlisten] 
2020-02-17T18:46:15.086+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-02-17T18:46:15.086+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] 
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] 
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] ** WARNING: This server is bound to localhost.
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] **          Remote systems will be unable to connect to this server. 
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] **          Start the server with --bind_ip <address> to specify which IP 
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] **          addresses it should serve responses from, or with --bind_ip_all to
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] **          bind to all interfaces. If this behavior is desired, start the
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] **          server with --bind_ip 127.0.0.1 to disable this warning.
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] 
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] 
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] 
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-17T18:46:15.751+0000 I CONTROL  [initandlisten] 
MongoDB Enterprise > 

```

Y lo hace

Simplemente podemos ejecutar un show dbs rápido para asegurarnos de que se esperan nuestras bases de datos.

```sh
MongoDB Enterprise > show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
MongoDB Enterprise > 
```

Y nuevamente, tenemos la base de datos de administración y la base de datos local.

Así que ahora voy a cerrar este como lo hicimos la última vez.

Así que cerramos nuestro servidor desde la base de datos de `admin`, y luego salimos del shell mongo.

```sh
MongoDB Enterprise > use admin
switched to db admin
MongoDB Enterprise > db.shutdownServer()
server should be down...
2020-02-17T18:51:44.727+0000 I NETWORK  [thread1] trying reconnect to 127.0.0.1:30000 (127.0.0.1) failed
2020-02-17T18:51:44.728+0000 W NETWORK  [thread1] Failed to connect to 127.0.0.1:30000, in(checking socket for error after poll), reason: Connection refused
2020-02-17T18:51:44.728+0000 I NETWORK  [thread1] reconnect 127.0.0.1:30000 (127.0.0.1) failed failed 
MongoDB Enterprise > exit
bye
2020-02-17T18:51:50.678+0000 I NETWORK  [thread1] trying reconnect to 127.0.0.1:30000 (127.0.0.1) failed
2020-02-17T18:51:50.678+0000 W NETWORK  [thread1] Failed to connect to 127.0.0.1:30000, in(checking socket for error after poll), reason: Connection refused
2020-02-17T18:51:50.678+0000 I NETWORK  [thread1] reconnect 127.0.0.1:30000 (127.0.0.1) failed failed 
2020-02-17T18:51:50.679+0000 I QUERY    [thread1] Failed to end session { id: UUID("8860a539-2154-475a-84b4-617b83ffbcd0") } due to SocketException: socket exception [CONNECT_ERROR] for couldn't connect to server 127.0.0.1:30000, connection attempt failed
vagrant@m103:~$ 
```

Así que solo como resumen.

<img src="/images/m103/c1/1-1-resumen.png">

En esta lección aprendimos que mongod es el principal proceso de demonio para MongoDB.

Interactuamos con mongod a través de un controlador, no directamente.

Mongod está disponible en muchos sistemas de 64 bits.

Y aprendimos sobre algunas de las opciones básicas de configuración de la línea de comandos.


## 2. Examen The Mongod

**Problem:**

When specifying the `--fork` argument to mongod, what must also be specified?

Check all answers that apply:


* `--loglocation`

* `--logpath` :+1:

* `--logdestination`

* `--logfile`

## 3. Laboratorio: Lanzamiento de Mongod

### Lab - Launching Mongod

**Problem:**

In this lab, you're going to launch your own mongod with a few basic command line arguments.

Applying what you've learned so far about the `mongod` process, launch a mongod instance, from the command line, with the following requirements:

* run on port **27000**
* data files are stored in `/data/db/`
* listens to connections from the IP address `192.168.103.100` and localhost
* authentication is enabled

By default, a mongod that enforces authentication but has no configured users only allows connections through the `localhost`. Use the mongo shell on the Vagrant box to use the localhost exception and connect to this node.

Use the following command to connect to the Mongo shell and create the following user. **You will need this user in order to validate subsequent labs**.

```sh
mongo admin --host localhost:27000 --eval '
  db.createUser({
    user: "m103-admin",
    pwd: "m103-pass",
    roles: [
      {role: "root", db: "admin"}
    ]
  })
'
```

MIO
```sh
vagrant@m103:~$ mongo admin --host localhost:27000 --eval '
> db.createUser({
> user: "m103-admin",
> pwd: "m103-pass",
> roles: [
> {role: "root", db: "admin"}
> ]
> })
> '
MongoDB shell version v3.6.17
connecting to: mongodb://localhost:27000/admin?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("e0b6fc24-944c-477c-b391-1aa39d04a49a") }
MongoDB server version: 3.6.17
Successfully added user: {
	"user" : "m103-admin",
	"roles" : [
		{
			"role" : "root",
			"db" : "admin"
		}
	]
}
vagrant@m103:~$ 
```

The above command creates a user with the following credentials:

* Role: `root` on `admin` database
* Username: `m103-admin`
* Password: `m103-pass`

When you're finished, run the following validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_launch_mongod
```

MIO
```sh
vagrant@m103:~$ mongod --port 27000 --dbpath /data/db --bind_ip 192.168.103.100,localhost

vagrant@m103:~$ validate_lab_launch_mongod
5a21c6dd403b6546001e79c0
vagrant@m103:~$ 
```

*Hint:* You want to make sure all applicable command line options are set! Also, in case you need to restart the `mongod` daemon, you may need to `kill` the process using it's **pid**.

You can use the following command to find the pid of the process:

```sh
ps -ef | grep mongod
```

To kill the process, you can use this command:

```sh
kill <pid>
```

Enter answer here:5a21c6dd403b6546001e79c0

## 4. Tema: Archivo de Configuración

### Lecture Notes

*At 3:54 the speaker says "YAML stands for Yet Another Markup Language". This acronym has been updated to "YAML Ain't Markup Language".*

See MongoDB documentation for more information about [command line options](https://docs.mongodb.com/manual/reference/program/mongod/#options) and [configuration file options](https://docs.mongodb.com/manual/reference/configuration-options/).

#### Lecture Instructions

*These lecture instructions are not meant to be reproduced in your environment. They reflect what you will see in the lecture video, however they may point to non-existing resources and files*.

Launch mongod using default configuration:

```sh
mongod
```

Launch mongod with specified `--dbpath` and `--logpath`:


```sh
mongod --dbpath /data/db --logpath /data/log/mongod.log
```

Launch mongod and fork the process:

```sh
mongod --dbpath /data/db --logpath /data/log/mongod.log --fork
```

Launch mongod with many configuration options:

*Note that all* "ssl" *options have been edited to use* "tls" *instead. As of MongoDB 4.2, options using* "ssl" *have been deprecated*.

```sh
mongod --dbpath /data/db --logpath /data/log/mongod.log --fork --replSet "M103" --keyFile /data/keyfile --bind_ip "127.0.0.1,192.168.103.100" --tlsMode requireTLS --tlsCAFile "/etc/tls/TLSCA.pem" --tlsCertificateKeyFile "/etc/tls/tls.pem"
```

Example configuration file, with the same configuration options as above:

```sh
storage:
  dbPath: "/data/db"
systemLog:
  path: "/data/log/mongod.log"
  destination: "file"
replication:
  replSetName: M103
net:
  bindIp : "127.0.0.1,192.168.103.100"
tls:
  mode: "requireTLS"
  certificateKeyFile: "/etc/tls/tls.pem"
  CAFile: "/etc/tls/TLSCA.pem"
security:
  keyFile: "/data/keyfile"
processManagement:
  fork: true
 
```

### Transcripción

El archivo de configuración de MongoDB es una forma de organizar las opciones que necesita para ejecutar el proceso MongoD o MongoS en un archivo YAML fácil de analizar.

Para la mayoría de los casos de uso fuera del desarrollo o evaluación más básico, debe usar un archivo de configuración para almacenar sus opciones de inicio MongoD o MongoS.

Antes de entrar en más detalles sobre el archivo de configuración, comencemos con una opción de MongoD.

Si ha interactuado con un programa en un entorno de terminal o shell, es probable que ya esté familiarizado con el funcionamiento de las opciones de línea de comandos.

Por lo general, tiene su ejecutable principal, por ejemplo, MongoD.

Luego encadena las opciones de la línea de comandos.

Algunos de ellos les gusta tomar un valor, como `dbpath` y `logpath`.

Otros funcionan como una bandera y no requieren un valor para dirigir el comportamiento, como `fork`.

Al final, tiene su comando completo en la línea de comando con potencialmente docenas de opciones y valores.

```sh
vagrant@m103:~$ mongod --dbpath /data/db --logpath /data/log/mongod.log --fork --replSet "M103" --keyFile /data/keyfile --bind_ip "127.0.0.1, 192.168.0.100" --sslMode requireSSL --sslCAFile "/etc/ssl/SSLCA.pem" --sslPEMKeyFile "/etc/ssl/ssl.pem"
```

Este comando es perfectamente válido y funcionaría bien en entornos de producción.

Pero hay problemas con este enfoque.

Tendríamos que volver a escribir todo esto cada vez que quisiéramos lanzar un nuevo MongoD en un servidor diferente.

Si queremos rastrear esta configuración, necesitaríamos seleccionar los servicios existentes que se ejecutan en el servidor o ejecutar un comando dentro de MongoDB.

Finalmente, es más difícil de leer y buscar opciones individuales a lo largo de esta cadena de línea de comando muy larga.

Antes de entrar en el archivo de configuración, comencemos con algunas opciones comunes de línea de comandos.

<img src="/images/m103/c1/1-4-comandos.png">

Tiene sus opciones básicas de configuración de ruta: `dbpath` y `logpath`.

A partir de 3.6, debe configurar `bind ip` para incluir un adaptador de red en el host que proporciona acceso a la red.

De lo contrario, MongoD solo puede aceptar conexiones en ese mismo host.

Al configurar las opciones `replSet` y `keyFile` se inicia MongoD en modo de replicación con la seguridad de autenticación básica entre clústeres y la autenticación de usuario habilitada.

Estas son opciones muy comunes, y es probable que vea al menos una de estas en cualquier implementación de MongoDB.

Las opciones de SSL están relacionadas con el cifrado de transporte tls ssl.

No necesita saber mucho sobre estas opciones en detalle para este curso.

Eche un vistazo a M310 aquí en la Universidad MongoDB para un curso en profundidad sobre seguridad de clúster, que incluye tsl ssl.

Alternativamente, eche un vistazo a nuestra documentación.

Finalmente, tenemos `fork`, que simplemente le dice a MongoD que se ejecute como un demonio en lugar de estar atado a una ventana de terminal.

Entonces, ¿cuáles son las contrapartes del archivo de configuración para estas opciones de línea de comandos?

<img src="/images/m103/c1/1-4-file-options.png">

A la derecha, tengo la ruta completa a la opción de configuración.

Eso significa que cada clave en la ruta al valor final es el padre del archivo YAML.

Por lo tanto, la opción de configuración de nombre `replSet` se encuentra bajo la clave principal de replicación `replication.replSetName`.

Archivos de configuración para admitir anidamiento más profundo también.

Eche un vistazo a estas dos opciones: sslPEMKey y sslCA file.

Ambos están bajo el padre SSL, que a su vez están bajo el padre neto.

El padre neto también captura una ip de enlace.

Las tres opciones están relacionadas con el padre neto, pero como sslPEMKey y sslCAKey son específicas de net.ssl.

```sh
net.ssl.sslPEMKey
net.ssl.sslCAKey
net.ssl.sslCAKey
```

Ahora, ¿cómo llegué a estas asignaciones?

Aquí es donde nuestro fantástico manual viene al rescate.

[mongod](https://docs.mongodb.com/manual/reference/program/mongod/index.html)

[Configuration File Options](https://docs.mongodb.com/manual/reference/configuration-options/index.html)

Todas nuestras opciones de línea de comandos y nuestras opciones de archivo de configuración están bien documentadas en estos dos enlaces.

Te invito a que les eches un vistazo para descubrir qué opciones de línea de comandos u opciones de archivos de configuración están disponibles y cómo mapear entre las dos.

Ahora tomemos nuestra lista de opciones de archivo de configuración con la ruta completa a la opción y traduzcamos eso a nuestro archivo YAML de configuración.

YAML significa **Y**et **A**nother **M**arkup **L**anguage.

Tienes tus pares de valores clave.

La clave de nivel superior y un archivo de configuración MongoDB representan una agrupación lógica de opciones.

Cada elemento anidado debajo de una clave de nivel superior representa una opción relacionada con esa clave de nivel superior.

<img src="/images/m103/c1/1-4-yaml.png">

Así que aquí vemos que dbPath es una opción de almacenamiento.

Recuerde, anteriormente esto figuraba como storage.dbPath.

La opción de línea de comando era dash dash dbPath.

Una clave puede tener múltiples valores de pares de claves incrustados, cada uno de los cuales representa una opción relacionada con la clave de nivel superior.

Así que aquí tenemos nuestra familia de opciones de registro del sistema donde estoy especificando la ruta al archivo de registro y el tipo de archivo.

Notarás que nuestra única opción, ruta de registro, se convirtió en dos.

A veces, una opción de archivo de configuración tendrá una o más opciones encadenadas requeridas.

La documentación siempre indicará claramente estas relaciones.

También es más fácil ver distintos grupos de opciones relacionadas.

Puedo distinguir claramente las opciones de almacenamiento de las opciones de registro del sistema de las opciones de replicación.

Incluso he agregado un comentario para mejorar la legibilidad y la comprensión.

Las opciones del archivo de configuración tienen el mismo efecto que las opciones de la línea de comandos, pero como puede ver, el formato YAML ofrece ventajas significativas.

Estas son todas las opciones de nuestro ejemplo inicial.

<img src="/images/m103/c1/1-4-yaml2.png">

El efecto en MongoD es el mismo, pero la organización y la legibilidad se han mejorado enormemente.

Ahora, ¿cómo usamos un archivo de configuración?

Tendremos que usar al menos una opción de línea de comando para que esto funcione.

<img src="/images/m103/c1/1-4-config.png">

```sh
mongod --conf "/etc/mongod.conf"
mongod -f "/etc/mongod.conf"
```

Especifique la `--config` o `-f` junto con una ruta al archivo de configuración.

Para muchas distribuciones de Linux, al instalar MongoDB a través de un administrador de paquetes, encontrará un archivo de configuración predeterminado en `etc/ mongod.conf`.

Siéntase libre de modificar esto o apunte a su propio archivo de configuración.

Solo necesita asegurarse de que el proceso MongoD pueda acceder a la ubicación del archivo y leer el archivo.

Puede encontrar la lista completa de opciones de archivo de configuración y cómo usarlas en nuestra documentación en línea.

La documentación también incluye ejemplos estructurales, así como una descripción de cómo funcionan las opciones y cuáles son los valores esperados.

En resumen, las opciones del archivo de configuración proporcionan la misma funcionalidad que nuestras opciones de línea de comandos.

<img src="/images/m103/c1/1-4-resumen.png">

Mejoran la legibilidad de nuestros ajustes de configuración, y puede usar la documentación para facilitar la asignación de una opción de línea de comando a una opción de archivo de configuración.

## 5. Examen Configuration File

#### Problem:

Consider the following:

```sh
mongod --dbpath /data/db --logpath /data/logs --replSet M103 --bind_ip '127.0.0.1,192.168.103.100' --keyFile /data/keyfile --fork
```

Which of the following represents a configuration file equivalent to the command line options?


Check all answers that apply:

:+1+
```sh
storage:
  dbPath: "/data/db"
systemLog:
  destination: file
  path: "/data/logs"
replication:
  replSetName: "M103"
net:
  bindIp: "127.0.0.1,192.168.103.100"
security:
  keyFile: "/data/keyfile"
processManagement:
  fork: true
```

```sh
storage.dbPath: /data/db
systemLog.destination: "/data/logs"
replication.replSetName: "M103"
net.bindIp: "127.0.0.1,192.168.103.100"
security.keyFile: "/data/keyfile"
processManagement.fork: true
```

```sh
storage
  dbPath="/data/db"
systemLog
  destination="/data/logs"
replication
  replSetName="M103"
net
  bindIp="127.0.0.1,192.168.103.100"
security
  keyFile="/data/keyfile"
processManagement
  fork=true
```

## 6. Laboratorio: Archivo de Configuración

Lab - Configuration File

**Problem:**

In this lab, you're going to launch a mongod instance in the vagrant environment with the same exact settings as the previous lab. However this time, those settings will be defined in a configuration file instead of the command line. It is required for this lab that you use the [YAML format](https://docs.mongodb.com/manual/reference/configuration-options/index.html#file-format) to construct this config file.

As a reminder, here are the requirements of your mongod instance:

* run on port **27000**
* data files are stored in `/data/db/`
* listens to connections from the IP address `192.168.103.100` and `localhost`
* authentication is enabled

If you created the user from the previous lab, you don't need to create any new users. If you did not create the user, do so now. Here are the requirements for that user:

* Role: `root` on `admin` database
* Username: `m103-admin`
* Password: `m103-pass`.

When your config file is complete, launch mongod with the `--config <config_filepath>` command line option with the filepath to your config file in place of `config_filepath` (you can also use the `-f` option instead of `--config`).

When you're finished, run the following validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_configuration_file
```

## 7. Tema: Estructura de Archivo

### Lecture Notes

#### Lecture Instructions

List `--dbpath` directory:

```sh
ls -l /data/db
```

List diagnostics data directory:

```sh
ls -l /data/db/diagnostic.data
```

List journal directory:

```sh
ls -l /data/db/journal
```

List socket file:

```sh
ls /tmp/mongodb-27017.sock
```

### Transcripción

## 8. Examen File Structure

**Problem:**

Which of the following files in the MongoDB data directory can you access to view collection data?

Check all answers that apply:

* The collection.wt file

* The storage.bson file

* The WiredTiger.wt file

* None of the above :+1:

## 9. Laboratorio: Cambiar la Ruta de Acceso a la Base de Datos Predeterminada

Lab - Change the Default DB Path

**Problem:**

In this lab, you're going to edit the config file from the previous lab to include a different DB path than the default path `/data/db`. Mongod will now store data files in this new directory instead.

Using what you know about the configuration file and Linux user groups, please complete the following:

* create a new folder `/var/mongodb/db/` and allow mongod to write files to this directory
   * create this directory with `sudo`, because `/var` is owned by `root`
   * use `chown` to change the owner of this directory to `vagrant:vagrant`
* edit your config file to use this new directory as the `dbpath`

Here are the updated requirements for your mongod instance:

* runs on port **27000**
* stores its data files in `/var/mongodb/db/`
* listens to connections from the IP address `192.168.103.100` and `localhost`
* uses authentication

Now that your config file has changed, you have to restart mongod so the server will reflect those changes. As a reminder, here is the way to safely shutdown from the `mongo` shell:

```sh
use admin
db.shutdownServer()
quit()
```

Once your `mongod` is safely stopped, you can launch it again with your new config file.

We could have kept the previous DB and users, however in this lab we will start with a new empty database directory, meaning empty database.

Let's recreate the user `m103-admin` with the same requirements as earlier, as we will need this user for the validation scripts and later tasks.

* Role: `root` on `admin` database
* Username: `m103-admin`
* Password: `m103-pass`

When you're finished, run the following validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_change_dbpath
```

Enter answer here:

## 10. Tema: Comandos Básicos

### Lecture Notes

The `explain()` output has changed in MongoDB 4.2. Specifically, a new section called `explain.queryPlanner.optimizedPipeline` has been added to the output. You can read about it in the [optimizedPipeline docs](https://docs.mongodb.com/manual/reference/explain-results/#explain.queryPlanner.optimizedPipeline).

#### Lecture Instructions

User management commands:

```sh
db.createUser()
db.dropUser()
```

Collection management commands:

```sh
db.<collection>.renameCollection()
db.<collection>.createIndex()
db.<collection>.drop()
```

Database management commands:

```sh
db.dropDatabase()
db.createCollection()
```

Database status command:

```sh
db.serverStatus()
```

Creating index with Database Command:

```sh
db.runCommand(
  { "createIndexes": <collection> },
  { "indexes": [
    {
      "key": { "product": 1 }
    },
    { "name": "name_index" }
    ]
  }
)
```

Creating index with Shell Helper:

```sh
db.<collection>.createIndex(
  { "product": 1 },
  { "name": "name_index" }
)
```

Introspect a Shell Helper:

```sh
db.<collection>.createIndex
```


### Transcripción

## 11. Examen

Basic Commands

**Problem:**

Which of the following methods executes a database command?

Check all answers that apply:

* db.command( { <COMMAND> } )

* db.executeCommand( { <COMMAND> } )

* db.runCommand( { <COMMAND> } ) :+1:

* db.runThisCommand( { <COMMAND> } )

## 12. Tema: Conceptos Básicos de Registro

### Lecture Notes

The logging output has changed in MongoDB 4.2. If you're interested, you can find more information in the 4.2 release notes for [Logging & Diagnostics](https://docs.mongodb.com/manual/release-notes/4.2/#logging-and-diagnostics).

**Lecture Instructions**

Get the logging components:

```sh
mongo admin --host 192.168.103.100:27000 -u m103-admin -p m103-pass --eval '
  db.getLogComponents()
'
```

Change the logging level:

```sh
mongo admin --host 192.168.103.100:27000 -u m103-admin -p m103-pass --eval '
  db.setLogLevel(0, "index")
'
```

View the logs through the Mongo shell:

```sh
db.adminCommand({ "getLog": "global" })
```

View the logs through the command line:

```sh
tail -f /data/db/mongod.log
```

Update a document:

```sh
mongo admin --host 192.168.103.100:27000 -u m103-admin -p m103-pass --eval '
  db.products.update( { "sku" : 6902667 }, { $set : { "salePrice" : 39.99} } )
'
```

Look for instructions in the log file with grep:

```sh
grep -i 'update' /data/db/mongod.log
```

### Transcripción

## 13. Examen Logging Basics

**Problem:**

Which of the following operations can be used to access the logs?

Check all answers that apply:

* Running db.adminCommand({ "getLog": "global" }) from the Mongo shell :+1:

* Running tail -f <path-to-log-file> from the command line :+1:

* Running db.getLogComponents() from the Mongo shell

## 14. Tema: Perfil de la Base de Datos

### Lecture Notes

**Lecture Instructions**

**Note: The command ``show collections`` no longer lists the system.* collections. It changed after version 4.0.**

To list all of the collection names you can run this command:

```sh
db.runCommand({listCollections: 1})
```

Get profiling level:

```sh
mongo newDB --host 192.168.103.100:27000 -u m103-admin -p m103-pass --authenticationDatabase admin --eval '
  db.getProfilingLevel()
'
```

Set profiling level:

```sh
mongo newDB --host 192.168.103.100:27000 -u m103-admin -p m103-pass --authenticationDatabase admin --eval '
  db.setProfilingLevel(1)
'
```

Show collections:

```sh
mongo newDB --host 192.168.103.100:27000 -u m103-admin -p m103-pass --authenticationDatabase admin --eval '
  db.getCollectionNames()
'
```

Note: *show collections* only works from within the shell

Set `slowms` to 0:

```sh
mongo newDB --host 192.168.103.100:27000 -u m103-admin -p m103-pass --authenticationDatabase admin --eval '
  db.setProfilingLevel( 1, { slowms: 0 } )
'
```

Insert one document into a new collection:

```sh
mongo newDB --host 192.168.103.100:27000 -u m103-admin -p m103-pass --authenticationDatabase admin --eval '
  db.new_collection.insert( { "a": 1 } )
'
```

Get profiling data from `system.profile`:

```sh
mongo newDB --host 192.168.103.100:27000 -u m103-admin -p m103-pass --authenticationDatabase admin --eval '
  db.system.profile.find().pretty()
'
```

### Transcripción

## 15. Examen Profiling the Database

**Problem:**

What events are captured by the profiler?

Check all answers that apply:

* CRUD operations :+1:

* Administrative commands :+1:

* Network timeouts

* WiredTiger storage data

* Cluster configuration operations :+1:

## 16. Laboratorio: Inicio de Sesión en una Instalación Diferente

Lab - Logging to a Different Facility

**Problem:**

By default, mongod sends diagnostic logging information to the standard output, occupying the terminal window where mongod was started. In this lab, you will tell mongod to send those logs to a file so you can fork the mongod process and continue to use the terminal window where mongod was started. You'll also change some of the default log settings.

Your task for this lab is to change the config file such that:

* mongod sends logs to `/var/mongodb/db/mongod.log`
* mongod is forked and run as a daemon (this will not work without a logpath)
* any query that takes 50ms or longer is logged (remember to specify this in the configuration file!)

**Note:** Using `db.setProfilingLevel()` will not work for this lab, because this command will not persist data in the config file - if the mongod process shuts down, such as during a maintenance procedure, the profiling level set with `db.setProfilingLevel()` will be lost.

All of the other information in your config file should stay the same as in the previous lab.

When you're finished, run the following validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_different_logpath
```

Enter answer here:

## 17. Tema: Seguridad Básica: Parte 1

### Transcripción

## 18. Tema: Seguridad Básica: Parte 2

### Lecture Notes

**Lecture Instructions**

Print configuration file:

```sh
cat /etc/mongod.conf
```

Launch standalone mongod:

```sh
mongod -f /etc/mongod.conf
```

Connect to mongod:

```sh
mongo --host 127.0.0.1:27017
```

Create new user with the root role (also, named root):

```sh
use admin
db.createUser({
  user: "root",
  pwd: "root123",
  roles : [ "root" ]
})
```

Connect to mongod and authenticate as root:

```sh
mongo --username root --password root123 --authenticationDatabase admin
```

Run DB stats:

```sh
db.stats()
```

Shutdown the server:

```sh
use admin
db.shutdownServer()
```

### Transcripción

## 19. Examen Basic Security: Part 2

**Problem:**

When should you deploy a MongoDB deployment with security enabled?

Check all answers that apply:

* When deploying your staging environment :+1:

* When deploying an evaluation environment :+1:

* When deploying your production environment :+1:

* When deploying a development environment :+1:

## 20. Tema: Roles Incorporados: Parte 1

### Lecture Notes

[M310 - MongoDB Security](https://university.mongodb.com/courses/M310/about)


### Transcripción

## 21. Tema: Roles Incorporados: Parte 2

### Lecture Notes

**Lecture Instructions**

Authenticate as `root` user:

```sh
mongo admin -u root -p root123
```

Create security officer:

```sh
db.createUser(
  { user: "security_officer",
    pwd: "h3ll0th3r3",
    roles: [ { db: "admin", role: "userAdmin" } ]
  }
)
```

Create database administrator:

```sh
db.createUser(
  { user: "dba",
    pwd: "c1lynd3rs",
    roles: [ { db: "admin", role: "dbAdmin" } ]
  }
)
```

Grant role to user:

```sh
db.grantRolesToUser( "dba",  [ { db: "playground", role: "dbOwner"  } ] )
```

Show role privileges:

```sh
db.runCommand( { rolesInfo: { role: "dbOwner", db: "playground" }, showPrivileges: true} )
```

### Transcripción

## 22. Examen Built-In Roles: Part 2

**Problem:**

Which of the following are Built-in Roles in MongoDB?

Check all answers that apply:

* dbAdminAnyDatabase :+1:

* read :+1:

* readWriteUpdate

* dbAdminAllDatabases

* rootAll

## 23. Laboratorio: Creación del Primer Usuario de la Aplicación

Lab - Creating First Application User

**Problem:**

In the first lab, you created a user with the root role on the admin database. The requirements are listed here:

* run on port **27000**
* data files are stored in `/var/mongodb/db/`
* listens to connections from the IP address `192.168.103.100` and `localhost`
* authentication is enabled
* root user on *admin* database with username: `m103-admin` and password: `m103-pass`

The `root` role is one of the most powerful roles in a Mongo cluster, and has many privileges which are never used by a typical application. In this lab, you will create a new user for an application that has the `readWrite` role, because the application does not need to monitor the cluster or create users - it only needs to read and write data.

The requirements for this new user are:

* Role: `readWrite` on `applicationData` database
* Authentication source: `admin`
* Username: `m103-application-user`
* Password: `m103-application-pass`

You don't need to make any changes to your `mongod` configuration, but it **must be running with authentication enabled**. If your configuration does not use authentication, this lab may fail to validate.

When you're finished, run the following validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_first_application_user
```

Enter answer here:

## 24. Tema: Descripción General de las Herramientas del Servidor

### Lecture Notes

**Lecture Instructions**

List mongodb binaries:

```sh
find /usr/bin/ -name "mongo*"
```

Create new dbpath and launch mongod:

```sh
mkdir -p ~/first_mongod
mongod --port 30000 --dbpath ~/first_mongod --logpath ~/first_mongod/mongodb.log --fork
```

Use mongostat to get stats on a running mongod process:

```sh
mongostat --help
mongostat --port 30000
```

Use mongodump to get a BSON dump of a MongoDB collection:

```sh
mongodump --help
mongodump --port 30000 --db applicationData --collection products
ls dump/applicationData/
cat dump/applicationData/products.metadata.json
```

Use mongorestore to restore a MongoDB collection from a BSON dump:

```sh
mongorestore --drop --port 30000 dump/
```

Use mongoexport to export a MongoDB collection to JSON or CSV (or stdout!):

```sh
mongoexport --help
mongoexport --port 30000 --db applicationData --collection products
mongoexport --port 30000 --db applicationData --collection products -o products.json
```

Tail the exported JSON file:

```sh
tail products.json
```

Use mongoimport to create a MongoDB collection from a JSON or CSV file:

```sh
mongoimport --port 30000 products.json
```

### Transcripción

## 25. Examen Server Tools Overview

**Problem:**

Which of the following are true differences between mongoexport and mongodump?

Check all answers that apply:

* Mongodump can create a data file and a metadata file, but mongoexport just creates a data file. :+1:

* Mongoexport is typically faster than mongodump.

* Mongoexport outputs BSON, but mongodump outputs JSON.

* Mongodump outputs BSON, but mongoexport outputs JSON. :+1:

* By default, mongoexport sends output to standard output, but mongodump writes to a file. :+1:


## 26. Laboratorio: Importación de un Conjunto de Datos

Lab - Importing a Dataset

**Problem:**

Now that you have some background about MongoDB's server tools, use `mongoimport` to import a JSON dataset into MongoDB. You can find the dataset inside the Vagrant box in `/dataset/products.json`, or in the lesson handout.

Import the whole dataset with your application's user `m103-application-user` into a collection called `products`, in the database `applicationData`.

When you're finished, run the following validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_import_dataset
```

Enter answer here:
