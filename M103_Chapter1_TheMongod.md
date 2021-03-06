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

#### See detailed answer

In order to complete this lab, we first must properly configure our mongod. Here is a command that fulfills the requirements of the lab:

```sh
mongod --port 27000 --dbpath /data/db --auth --bind_ip 192.168.103.100,127.0.0.1
```

Let's go through these command line options:

* `--port 27000` tells mongod to run on port 27000, which means we have to connect to mongo on that port.
* `--dbpath /data/db` tells mongod to store data files in the `/data/db` directory. This directory must exist before we start mongod, or we will receive an error.
* `--auth` enables access control on our deployment. This requires users to identify themselves before connecting - we will learn how to create users below.
* `--bind_ip 192.168.103.100,127.0.0.1` tells mongod to bind to the IP addresses `192.168.103.100` and `127.0.0.1` when listening for connections. `--bind_ip 192.168.103.100,localhost` would also work, as the name `localhost` resolves to `127.0.0.1`.

After configuring mongod, we can connect to mongo by simply specifying a port:

```sh
mongo --port 27000
```

We don't have to specify a `--host` here, because the default host is `127.0.0.1`, or `localhost`. We will need to connect to localhost in order to complete the next part of the lab!

Once our mongod is running with the correct settings, we must create the first user on our database, as instructed. Because we don't have any users yet, but access control is enabled, we leverage the [localhost exception](https://docs.mongodb.com/manual/core/security-users/#localhost-exception) to create our first user. This user must be able to create other users because we can only use this exception once. Luckily, our user has the role `root` and can create other users:

```sh
use admin
db.createUser({
  user: "m103-admin",
  pwd: "m103-pass",
  roles: [
    {role: "root", db: "admin"}
  ]
})
```

Notice that we can only use the localhost exception if we're connected to the `admin` database. If the user creation was successful, we should receive a message that says `Successfully added user`.

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
MIO
```sh
storage:
  dbPath: "/data/db"
net:
  bindIp: "127.0.0.1,192.168.103.100"
  port: 27000
security:
  authorization: "enabled"
```
Enter answer here: 5a2f0e41ae3c4e2f7427ee8f

#### See detailed answer

The configuration for this mongod is the same as the previous lab, but the settings should be passed in a configuration file instead of the command line. Here is an example of a valid config file for this lab:

```sh
storage:
  dbPath: /data/db
net:
  bindIp: 192.168.103.100,localhost
  port: 27000
security:
  authorization: enabled
```

Again, `localhost` is an alias for `127.0.0.1`, so they can be used interchangeably.

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

En esta lección, cubriremos la estructura de archivos de un servidor independiente MongoDB.

Esta es una lista de los archivos que puede encontrar en un directorio de datos de un servidor MongoDB o en un proceso independiente.

<img src="/images/m103/c1/1-7-estructure.png">

Por lo general, nunca necesita interactuar con los archivos de esta carpeta de datos a menos que lo indique el personal de soporte de MongoDB o mediante un procedimiento detallado en nuestra documentación.

Ninguno de estos archivos está diseñado para el acceso o modificación del usuario, y modificarlos puede causar bloqueos o pérdida de datos.

Si desea explorar, tómese el tiempo para asegurarse de que solo realiza funciones de lectura.

Echemos un vistazo a una implementación real de MongoDB.

Este grupo de archivos aquí está relacionado con la forma en que el motor de almacenamiento WiredTiger realiza un seguimiento de información como metadatos de clúster y opciones de configuración específicas de WiredTiger.

<img src="/images/m103/c1/1-7-estructure2.png">

El archivo WiredTiger.lock actúa como una seguridad.

Si ejecutó un segundo proceso MongoDB simultáneo y señaló esta carpeta, el archivo de bloqueo ayuda a evitar que se inicie ese segundo proceso MongoDB.

Si experimenta un apagado sucio, como que la máquina host pierde energía o se bloquea de algún modo, es posible que no pueda iniciar MongoD debido a este archivo de bloqueo.

Es posible que se le indique que elimine los archivos de bloqueo antes de reiniciar MongoD.

Recuerde que si no está guiado por el soporte de MongoDB o un procedimiento documentado, no interactúe con ninguno de estos archivos.

El siguiente grupo de archivos que termina en .wt está relacionado con la recopilación e indexación de los datos en sí.

Estos son sus datos de recopilación y estos son sus datos de índice.

MongoDB WiredTiger almacena datos de índice como una estructura separada de los datos de recopilación.

Cada colección de un índice obtiene su propio archivo.

Incluso en una nueva implementación de MongoDB, generalmente tiene algunas bases de datos y colecciones de forma predeterminada, por lo que siempre debe ver alguna colección en los archivos index .wt.

Puede intentar introspectar estos archivos de datos utilizando un programa como Strings, pero aquí no hay muchos datos legibles por humanos.

Estos archivos están diseñados para interactuar a través del proceso del servidor MongoDB, en lugar de una herramienta de terceros.

La modificación de estas herramientas puede provocar la pérdida de datos y bloqueos.

Ahora esta carpeta diagnostic.data se ve bastante interesante.

<img src="/images/m103/c1/1-7-data.png">

Echemos un vistazo rápido.

Estos datos contienen datos de diagnóstico capturados para uso específico por el soporte de MongoDB.

Para ser muy claros, no estamos capturando ninguno de sus datos privados reales.

Los datos de diagnóstico son capturados por nuestro módulo de captura de datos a tiempo completo o FTDC.

FTDC recopila datos de los siguientes comandos.

<img src="/images/m103/c1/1-7-ftdc.png">

Si intenta echar un vistazo a los datos producidos por el módulo FTDC utilizando algo como Strings, encontrará que no es legible para los humanos.

Estos datos solo son utilizados con fines de diagnóstico por los ingenieros de soporte de MongoDB.

Y solo pueden ver esos datos si los proporciona explícitamente.

En el futuro, echemos un vistazo a nuestros archivos de diario.

Cada uno de estos archivos de diario forma parte del sistema de registro en diario WiredTiger.

<img src="/images/m103/c1/1-7-jornal.png">

Hablemos de eso brevemente.

Con MongoDB WiredTiger, las operaciones de escritura se almacenan en la memoria intermedia y se vacían cada 60 segundos, creando un punto de control de datos.

WiredTiger también utiliza un sistema de registro de escritura anticipada en un archivo de diario en disco.

<img src="/images/m103/c1/1-7-write.png">

Las entradas del diario se almacenan primero en la memoria intermedia y luego WiredTiger sincroniza el diario en el disco cada 50 milisegundos.

Cada archivo de diario está limitado a 100 megabytes de tamaño.

WiredTiger utiliza un método de rotación de archivos para sincronizar datos con el disco.

<img src="/images/m103/c1/1-7-write2.png">

En caso de falla, WiredTiger puede usar el diario para recuperar datos que ocurrieron entre puntos de control.

Por ejemplo, durante las operaciones normales, WiredTiger transfiere datos al disco cada 60 segundos, o cuando el archivo de diario tiene 2 gigabytes de datos.

Estos enjuagues nuevamente crean un punto de control duradero.

<img src="/images/m103/c1/1-7-write3.png">

Si el MongoD se bloquea entre los puntos de control, existe la posibilidad de que los datos no se hayan escrito de forma segura y completa.

Cuando MongoDB vuelve a estar en línea, WiredTiger puede verificar si hay alguna recuperación por hacer.

<img src="/images/m103/c1/1-7-write4.png">

En caso de que haya algunas escrituras incompletas, WiredTiger mira los archivos de datos existentes para encontrar el identificador del último punto de control.

Luego busca en los archivos del diario el registro que coincida con el identificador del último punto de control.

Finalmente, aplica operaciones en los archivos de diario desde el último punto de control.

<img src="/images/m103/c1/1-7-write5.png">

Al final, el servidor MongoDB puede reanudar la ejecución normal.

Echemos un vistazo al último grupo de archivos.

<img src="/images/m103/c1/1-7-files.png">

El archivo mongod.lock tiene una función similar al archivo WiredTiger.lock.

Si este archivo no está vacío, significa que un proceso MongoDB está actualmente activo en este directorio.

Cualquier otro proceso de MongoDB que intente acceder a este directorio no se iniciará en ese caso.

Si este archivo está vacío, entonces todo está claro.

En algunas situaciones inusuales, como un cierre impuro, el archivo mongod.lock no estará vacío, aunque MongoD ya no se esté ejecutando.

Es posible que deba eliminar el archivo mongod.lock si así lo solicita el soporte o nuestra documentación.

Estos dos archivos restantes son más archivos de soporte y metadatos para WiredTiger.

Recuerde, nunca debería necesitar interactuar con cualquiera de estos archivos y modificarlos puede provocar bloqueos o pérdida de datos.

Además de los archivos contenidos aquí en el directorio de datos, también está el archivo de registro.

Vamos a repasar el inicio de sesión con más detalle en la lección posterior.

Pero solo para darle un vistazo rápido, puede ver en mi registro, no hay mucha información aquí ahora.

<img src="/images/m103/c1/1-7-log.png">

Eso es porque realmente no estoy haciendo nada con mi servidor.

A medida que utilice su servidor MongoDB, el archivo de registro se llenará con información adicional.

Estos archivos de registro son vitales para el diagnóstico posterior a la falla y también deben tratarse con cuidado.

Depende de usted si desea colocar sus archivos de registro en el mismo directorio que sus archivos de datos.

Sin embargo, no es una mala idea mantenerlos separados.

Hay un archivo más del que deberíamos hablar, pero no está en ninguno de los dos directorios de los que hemos hablado hasta ahora.

Este archivo mongodb-27017.sock es un archivo de socket utilizado por MongoDB para crear una conexión de socket en el puerto especificado.

<img src="/images/m103/c1/1-7-sock.png">

MongoDB necesita usar sockets para comunicaciones entre procesos.

Sin este archivo, MongoDB no puede funcionar.

Este archivo se crea al inicio y permite que el servidor MongoDB sea el propietario de este puerto.

Si hay un bloqueo u otro apagado sucio, puede encontrar un error en el inicio relacionado con este archivo.

Puede eliminarlo de manera segura si así lo solicita el soporte o nuestra documentación en ese caso.

Recapitulemos.

<img src="/images/m103/c1/1-7-resumen.png">

El soporte de MongoDB utiliza algunos archivos, como diagnosis.data y sus archivos de registro, para ayudarlo a resolver problemas con su base de datos.

El resto es utilizado exclusivamente por el proceso del servidor MongoDB para operaciones normales y no debe modificarse sin la dirección específica del soporte de MongoDB.

Consulte el soporte de MongoDB o nuestra documentación para obtener instrucciones sobre cómo interactuar con cualquiera de estos archivos.

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
      MIO(`vagrant@m103:/$ sudo chown -R vagrant:vagrant /var/mongodb`)
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
Implicit session: session { "id" : UUID("6223eb83-e0b6-4a9a-93f9-b900ebe52150") }
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

When you're finished, run the following validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_change_dbpath
```

Enter answer here: 5a2f973bcb6b357b57e6bf43

#### See detailed answer

Because `/var/` is owned by `root`, we have to create the subdirectory `/var/mongodb/db/` as `root`:

```sh
sudo mkdir -p /var/mongodb/db
```

However, in order to use `/var/mongodb/db/` as the dbpath, it must allow write access by the user running mongod. We can accomplish this by changing the owner of our new directory to `vagrant`:

```sh
sudo chown vagrant:vagrant /var/mongodb/db
```

Now we can edit our configuration file to use this new directory as the DB path:

```sh
storage:
  dbPath: /var/mongodb/db/
net:
  bindIp: 192.168.103.100,localhost
  port: 27000
security:
  authorization: enabled
```

Start the `mongod` process with:

```sh
mongod -f mongod.conf
```
The process of creating this user is identical to the previous labs, but we have to repeat it for our new DB path:

```sh
use admin
db.createUser({
  user: "m103-admin",
  pwd: "m103-pass",
  roles: [
    {role: "root", db: "admin"}
  ]
})
```

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

MongoDB proporciona dos funciones de registro para rastrear actividades en su base de datos.

El registro del proceso muestra actividad en la instancia de MongoDB.

El registro del proceso recopila actividad en uno de los siguientes componentes.

Cada uno de estos componentes tiene un nivel de verbosidad asociado.

Puede usar db.getLogComponents en el shell Mongo para revisar la verbosidad del componente de registro configurado actualmente.

Vamos a ver.

Estoy conectado al servidor MongoDB usando el shell mongo.

Puedo ejecutar db.getLogComponents para recuperar los componentes de registro de mi base de datos actual.

Entonces, ¿Qué significa todo esto?

Comenzando en la parte superior, el campo de verbosidad es el nivel de verbosidad predeterminado para el servidor MongoDB.

Cualquiera de los otros componentes puede heredar de este campo.

¿Ves cómo todos estos otros componentes tienen 1 negativo como su verbosidad?

Negativo 1 significa heredar del padre.

Puedes ver que tengo una verbosidad de 1, por lo que todos mis componentes heredan de eso.

Los niveles de registro 1 a 5 solo aumentan el nivel de verbosidad para incluir mensajes de depuración.

Cuanto mayor sea el número, más detallados serán sus mensajes de depuración.

Recapitulemos eso muy brevemente.

Negativo 1 significa que el componente de registro hereda su nivel de verbosidad de su padre.

Por defecto, su verbosidad es típicamente 0.

Eso significa solo mensajes informativos.

Había establecido mi nivel de verbosidad en 1 para poder ver más mensajes de depuración.

Un nivel de verbosidad más alto significa mensajes de depuración más detallados y frecuentes.

Si no está tratando de identificar y resolver un problema activamente, puede dejar la verbosidad en 0 para un nivel base de monitoreo.

Notarás que para algunos de estos componentes de registro también hay subcomponentes.

Recuerde, había tres componentes diferentes para la replicación.

Tuviste tu componente de replicación estándar, y luego tuviste un latido y un componente de reversión.

Puede ver los tres aquí debajo de la réplica principal.

Cada uno es heredero.

Los latidos y la reversión se heredan de la replicación, que en sí misma se hereda de mi campo de verbosidad de nivel superior.

Ahora, ¿cómo funciona todo esto?

Hay dos formas en que podemos mirar los registros.

El primero es mediante el uso del comando de base de datos getLogs aquí en el shell mongo.

La otra es usar una utilidad, como tail -f, para seguir el final del registro.

Comencemos con el comando getLogs.

Estoy usando db.adminCommand porque getLog debe ejecutarse en la base de datos de administración.

Estoy especificando global para decirle a getLog que me dé toda la actividad de registro.

Esto devolverá todo el registro al punto en el que ejecutamos este comando.

Puedes ver que tengo mucha actividad de índice.

Aquí hay varios comandos, incluido el comando que ejecuté cuando ejecuté db.getLog.

Mirando esto, en realidad tengo demasiada actividad de índice.

Realmente no necesito este nivel de detalle.

Cambiemos la verbosidad del registro para el componente de índice nuevamente a 0.

Estoy especificando el nivel de verbosidad al que quiero cambiar el componente con db.setLogLevel.

El resultado aquí es cuál era la configuración del nivel de registro.

Puedo volver a ejecutar db.getLogComponents para ver mi valor actualizado de índice.

Podemos ver aquí que he establecido con éxito el nivel de verbosidad del componente de registro de índice en 0.

Echemos otro vistazo al registro, esta vez usando tail -f.

Estoy especificando la ruta a mi archivo de registro a la utilidad de cola.

Y estoy especificando la bandera -f para dirigir la cola para seguir este registro.

Eso significa que constantemente recibiré actualizaciones ya que hay nueva actividad publicada en este archivo.

Dependiendo de su sistema operativo, puede haber diferentes utilidades de tail disponibles para usted que realizan la misma función básica.

Veamos específicamente este evento de registro de COMMAND.

Entonces este es el comando que acabo de identificar en el archivo de registro.

Comencemos con la marca de tiempo.

Esto nos permite saber cuándo ocurrió el evento.

A continuación, tengo el nivel de gravedad del mensaje.

Brevemente, hay cinco tipos de niveles de gravedad.

Tiene Fatal, Error, Advertencia, Informativo, que está relacionado con el nivel de detalle 0, y Depuración, que está relacionado con el nivel de detalle 1 a 5.

Este componente tiene un nivel de verbosidad de I, lo que significa que este es un mensaje informativo.

A continuación, tenemos el componente de registro real en el que se encuentra la operación.

En este caso, la operación es un comando.

También podemos ver la conexión en la que ocurrió el evento.

Las conexiones son incrementales y únicas, por lo que cualquier evento iniciado por una conexión específica es probable que provenga del mismo cliente.

Tenemos información más específica sobre el evento.

Tenemos una acción de comando que se ejecutó en la base de datos de administración.

$ Cmd indica que se trata de un comando de base de datos.

La lista completa de posibles eventos y descriptores están fuera de alcance.

Pero en general, puede esperar que lo que sigue inmediatamente a la conexión sea la operación que desencadenó el evento.

AppName indica qué cliente inició la operación, en este caso, el shell mongo.

Ahora podemos profundizar en el comando en sí.

Todo el documento es el esqueleto de***
El comando ejecutado.

Bajo el capó, tenemos un comando para establecer el parámetro que establece la verbosidad del componente de registro del componente de registro de índice en la base de datos de administración.

El comando fue generado por mí usando el método db.setLogLevel.

Finalmente, tenemos algunos metadatos relacionados con cómo se realizó la operación.

El último punto de datos es de particular interés.

Es cuánto tiempo tardó en completarse esta operación.

Hablaremos en otra lección sobre operaciones lentas y cómo identificarlas.

Mirando hacia atrás en mi registro, podemos ver que los eventos de índice se han reducido en frecuencia ya que acabamos de volver a los mensajes informativos para ese componente de registro.

Veamos qué sucede cuando emitimos una escritura.

Voy a actualizar un documento en nuestra colección de productos.

Este es un simple comando de actualización donde estoy estableciendo el precio de venta de este producto en particular.

Ahora echemos un vistazo a nuestros registros.

Ambos eventos de registro están relacionados con el comando de actualización único que emití.

Ahora, puede estar pensando, pensé que acabamos de actualizar un documento.

Bueno, una operación de actualización es esencialmente dos componentes.

Uno es el comando y el otro es la operación de escritura que se produce como resultado de ese comando.

Recapitulemos.

El registro del proceso de revisión de MongoDB admite múltiples componentes para controlar la granularidad de los eventos capturados.

Puede recuperar el registro desde el shell mongo o utilizando utilidades de línea de comandos como tail.

Finalmente, puede cambiar la verbosidad de cualquier componente de registro utilizando el shell mongo.

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

Muy bien, así que en esta lección, vamos a discutir el perfil de la base de datos y cómo se puede usar junto con los bloqueos de la base de datos.

Los servidores generan una gran cantidad de eventos.

Y un archivo de registro es excelente para capturar estos datos o subconjuntos de estos datos.

Pero el propósito de estos registros es informar sobre el estado de la base de datos, como un todo.

Los registros almacenan algunos datos en nuestros comandos, pero no hay suficientes datos aquí para comenzar a optimizar nuestras consultas.

Las líneas no contendrán ninguna estadística de ejecución, la dirección de un índice utilizado por una consulta, planes rechazados ni nada.

E incluso si pudiéramos colocar esa información en los registros, realmente no deberíamos.

Los archivos de registro están destinados a proporcionar a los administradores información operativa sobre una instancia o proceso, para que puedan marcar cualquier error, advertencia o mensaje informativo interesante.

Para depurar operaciones lentas, necesitamos ser un poco más precisos en la información que capturamos.

Para eso, confiamos en el generador de perfiles de base de datos.

Permitimos perfiladores a nivel de base de datos.

Entonces, las operaciones en cada base de datos se perfilan por separado.

Cuando está habilitado, el perfil restaurará los datos para todas las operaciones en una base de datos dada, y una nueva colección llamada perfil de puntos del sistema.

Esta recopilación contendrá datos de perfiles sobre operaciones CRUD, así como opciones administrativas y de configuración.

Tiene tres configuraciones.

El valor predeterminado es cero, lo que significa que el generador de perfiles está apagado.

Uno significa que el generador de perfiles está activado, pero solo va a las operaciones de perfil que se consideran lentas.

Por defecto, MongoDB considerará lenta cualquier operación que tarde más de 100 milisegundos.

Pero también podemos definir qué es una consulta lenta configurando el valor lento de MS, como veremos en un minuto.

Dos significa que el generador de perfiles está activado y creará un perfil de todas las operaciones en una base de datos, independientemente de cuánto tiempo demoren.

Esto es un poco peligroso porque puede generar muchos derechos sobre la colección de perfil de puntos del sistema y generar una gran carga en el sistema.

Esto no significa que las operaciones pequeñas no puedan bloquear otras, pero obtener datos sobre esas operaciones requiere más granularidad.

Muy bien, así que ahora echemos un vistazo al generador de perfiles.

Esta base de datos aún no existe, por lo que el generador de perfiles está configurado de forma predeterminada en el nivel 0.

Y podemos verificar eso ejecutando db.getprofilinglevel.

Y como puedes ver, nos da un cero.

Podemos cambiar eso a uno con db.setprofilinglevel.

Entonces, esta declaración activó el generador de perfiles, perfil de nivel 1.

Si ejecutamos este comando, podemos ver que MongoDB creó una nueva colección llamada system dot profile.

Pero no hay nada en este momento.

Y debido a que no hemos especificado una MS lenta, el generador de perfiles solo almacenará datos en consultas que demoren más de 100 milisegundos.

Muy bien, aquí, solo para tener una idea de cómo funciona el generador de perfiles y cómo se ven los datos de creación de perfiles, solo voy a configurar MS lenta a cero, para que todo se perfile en esta base de datos.

Así que solo voy a insertar un pequeño documento aquí en esta nueva colección, llamada nueva colección.

Entonces, ahora voy a ver qué hay en la colección de perfil de puntos del sistema ahora mismo, después de ejecutar esa consulta, y voy a hacer que la salida sea un poco más bonita para que sea más legible.

Muy bien, para que podamos ver nuestra declaración de derechos se registra en el generador de perfiles.

Nos da la cantidad de documentos insertados e insertados, y la cantidad de claves de índice insertadas por la operación, claves insertadas, así como el tiempo que la operación tomó en milisegundos.

Entonces también podemos perfilar las operaciones de lectura.

Aquí, tenemos un predicado de consulta realmente simple, donde solo estamos buscando documentos donde A es uno.

Y podemos ver que el generador de perfiles registró un poco más de información sobre esta consulta.

Nos dice que agotamos el cursor que estábamos usando para recuperar estos datos, y también tiene algunas estadísticas de ejecución, como el estado por el que pasamos para llegar aquí.

En este caso, fue solo un escaneo de colección.

Muy bien, solo para recapitular, hemos cubierto la diferencia entre los datos de registro y los datos de perfil, cómo configurar el generador de perfiles en su base de datos y cómo interpretar la salida del generador de perfiles dependiendo de la operación.

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

Enter answer here: 5a32e5835d7a25685155aa61

#### See detailed answer

There are three different sections to add in the config file in order to complete this lab. The first is the `systemLog` section:

```sh
systemLog:
  destination: file
  path: /var/mongodb/db/mongod.log
  logAppend: true
```

This tells mongod to send logs to a file, and specifies that file as `/var/mongodb/mongod.log.logAppend: true` tells mongod to append new entries to the existing log file when the mongod instance restarts.


The second section is `processManagement`:

```sh
processManagement:
  fork: true
```
 
This enables a daemon that runs mongod in the background, or "forks" the process. This frees up the terminal window where mongod is launched from.

The third section is `operationProfiling`:

```sh
operationProfiling:
  slowOpThresholdMs: 50
```

This sets the minimum threshold for a "slow" operation. Any operation that takes 50 milliseconds or longer will be logged as a result.

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

Enter answer here: 5a32fdd630bff1f2fcb87acf

#### See detailed answer

In order to create this new user, we have to be logged into your MongoDB server as a user with the privilege to create other users. Luckily, the `m103-admin` user from previous labs has the `root` role, and therefore has this privilege.

As a reminder, authenticating to MongoDB as this user can be done with the following command:

```sh
mongo --port 27000 -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
```

Once we are logged in, we can create our new user with the following command:

```sh
use admin
db.createUser({
  user: "m103-application-user",
  pwd: "m103-application-pass",
  roles: [
    {role: "readWrite", db: "applicationData"}
  ]
})
```

Notice that we created our user on the `admin` database, but we gave that user `readWrite` access to the `applicationData` database. These are two separate actions - the user **authenticates** against `admin` and is **authorized** on `applicationData.

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

Enter answer here: 5a383323ba6dbcf3cbcaec97

#### See detailed answer

The import can be properly completed with the following command:

```sh
mongoimport --drop --port 27000 -u "m103-application-user" \
-p "m103-application-pass" --authenticationDatabase "admin" \
--db applicationData --collection products /dataset/products.json
```
We authenticate to the database the same way with `mongoimport` as we did with `mongo`. The flags `--db` and `--collection` are used to designate a target database and collection for our data. The flag `--drop` is used to drop the existing collection, so we don't create duplicates by running this command twice.
