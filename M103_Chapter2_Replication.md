# Capítulo 2: Replicación

### 30 Items

Conceptos básicos de replicación y administración de conjuntos de réplicas

## Lecciones

1. Tema: ¿Qué es la replicación?
2. Examen
3. Tema: Conjunto de réplicas MongoDB
4. Examen
5. Tema: Configuración de un conjunto de réplicas
6. Examen
7. Laboratorio: iniciar un conjunto de réplicas localmente
8. Tema: Documento de configuración de replicación
9. Examen
10. Tema: Comandos de replicación
11. Examen
12. Tema: DB local: Parte 1
13. Tema: DB local: Parte 2
14. Examen
15. Tema: reconfigurar un conjunto de réplicas en ejecución
16. Examen
17. Laboratorio: eliminar y volver a agregar un nodo
18. Tema: Lee y escribe en un conjunto de réplicas
19. Examen
20. Tema: conmutación por error y elecciones
21. Examen
22. Tema: Escribir inquietudes: Parte 1
23. Tema: Escribir inquietudes: Parte 2
24. Examen
25. Laboratorio: escribe con conmutación por error
26. Tema: Leer preocupaciones
27. Examen
28. Tema: Leer preferencias
29. Examen
30. Laboratorio - Preocupación de lectura y preferencias de lectura

## 1. Tema: ¿Qué es la replicación?

### Transcripción

En esta lección, cubriremos la replicación y cómo hace que sus datos sean más duraderos.

MongoDB utiliza asíncrona, replicación basada en sentencias porque es independiente de la plataforma y permite una mayor flexibilidad dentro de un **replica set**(conjunto de réplicas).

Pero primero, hablemos sobre qué es la replicación.

<img src="images/m103/c2/2-1-replication-0.png">

**La replicación es el concepto de mantener múltiples copias de sus datos.**

<img src="images/m103/c2/2-1-replication.png">

Este es un concepto realmente importante en MongoDB, pero realmente en cualquier sistema de base de datos.

La razón principal por la que es necesaria la replicación es porque nunca puede asumir que todos sus servidores siempre estarán disponibles.

<img src="images/m103/c2/2-1-servers.png">

Si tiene que realizar tareas de mantenimiento en un centro de datos o un desastre borra sus datos por completo, sus servidores experimentarán tiempo de inactividad en algún momento.

<img src="images/m103/c2/2-1-servers-mantenimiento.png">

<img src="images/m103/c2/2-1-servers-desastre.png">

El punto de replicación es asegurarse de que en caso de que su servidor se caiga, aún puede acceder a sus datos.

Este concepto se llama **disponibilidad**(availability).

<img src="images/m103/c2/2-1-availability.png">

Una base de datos que no usa replicación solo tiene un único servidor de base de datos, y nos referimos a estos como nodos independientes (standalone nodes).

<img src="images/m103/c2/2-1-standalone.png">

En una configuración independiente, las bases de datos pueden atender lecturas y escrituras solo mientras ese nodo único está en funcionamiento.

Pero si el nodo se cae, perdemos todo el acceso a esos datos.

<img src="images/m103/c2/2-1-standalone-fallo.png">

Nuestras lecturas y escrituras no llegarán al servidor.

Ahora en una solución replicada, tenemos un par de nodos adicionales a la mano, y contienen copias de nuestros datos.

En MongoDB, un grupo de nodos que tienen copias de los mismos datos se denomina **replica set** conjunto de réplicas.

<img src="images/m103/c2/2-1-replicaset.png">

Y en un replica set, todos los datos se manejan de manera predeterminada en uno de los nodos, 

<img src="images/m103/c2/2-1-replicaset-1.png">

y depende de los nodos restantes del conjunto sincronizarse con él y replicar cualquier dato nuevo que se haya escrito a través de un mecanismo asíncrono.

<img src="images/m103/c2/2-1-replicaset-2.png">

El nodo donde se envían los datos se denomina **nodo primario**, y todos los demás nodos se denominan **nodos secundarios**.

<img src="images/m103/c2/2-1-replicaset-3.png">

El objetivo aquí es que todos los nodos se mantengan consistentes entre sí.

Entonces, si nuestra aplicación está usando la base de datos y el nodo primario se cae, 

<img src="images/m103/c2/2-1-replicaset-4.png">

uno de los nodos secundarios puede tomar su lugar como primario en un proceso conocido como **failover** (conmutación por error).

<img src="images/m103/c2/2-1-replicaset-5.png">

<img src="images/m103/c2/2-1-failover.png">

Los nodos deciden específicamente qué secundaria se convertirá en primaria a través de una **elección**.

<img src="images/m103/c2/2-1-election.png">

Y este nombre no es una coincidencia.

Los nodos secundarios en realidad votan entre sí para decidir qué secundaria se convertirá en la primaria.

<img src="images/m103/c2/2-1-vote.png">

En un replica set duradero, la **failover** (conmutación por error) puede tener lugar rápidamente, de modo que no se pierdan datos, y las aplicaciones que usan los datos continuarán comunicándose con el replica set como si nunca hubiera pasado nada.

Y una vez que el nodo vuelve a funcionar, y es seguro que puede ponerse al día y sincronizarse en la última copia de los datos, se unirá al replica set automáticamente.

<img src="images/m103/c2/2-1-replicaset-6.png">

La **disponibilidad** y la **redundancia de datos** son propiedades típicas de una solución de base de datos duradera.

La replicación de datos puede tomar una de dos formas.

<img src="images/m103/c2/2-1-replication-type.png">

Hay replicación binaria y replicación basada en sentencias (binary replication and statement-based replication).

Echaremos un vistazo a las diferencias entre estos dos enfoques, y comenzaremos con la replicación binaria.

<img src="images/m103/c2/2-1-binary.png">

Digamos que insertamos este documento en nuestra base de datos.

<img src="images/m103/c2/2-1-document.png">

Una vez completada la escritura, tenemos unos pocos bytes en el disco que se escribieron para contener algunos datos nuevos.

<img src="images/m103/c2/2-1-data.png">

La forma en que funciona la replicación binaria es examinando los bytes exactos que cambiaron en los archivos de datos y registrando esos cambios en un **binary log** (registro binario).

<img src="images/m103/c2/2-1-data-2.png">

Los nodos secundarios reciben una copia del **binary log** (registro binario) y escriben los datos especificados que cambiaron a las ubicaciones exactas de bytes que se especifican en el **binary log** (registro binario).

La replicación de datos de esta manera es bastante fácil en las secundarias porque obtienen instrucciones realmente específicas sobre qué bytes cambiar y a qué cambiarlos.

Y, de hecho, los secundarios ni siquiera son conscientes de las declaraciones que están replicando.

Esto puede ser bueno porque no hay contexto sobre los datos necesarios para replicar una escritura.

Sin embargo, el uso de la binary replication (replicación binaria) supone que el sistema operativo es coherente en todo el conjunto de réplicas.

<img src="images/m103/c2/2-1-windows.png">

Por ejemplo, si nuestro nodo primario ejecuta Windows, los secundarios no pueden usar el mismo registro binario si ejecutan Linux.

<img src="images/m103/c2/2-1-windows-linux.png">

Y si tienen el mismo sistema operativo, todas las máquinas en el conjunto de réplicas deben tener el mismo conjunto de instrucciones.

Entonces, Windows x86 o x64 y la misma versión del servidor de base de datos que se ejecuta en cada máquina.

<img src="images/m103/c2/2-1-versiones.png">

En otras palabras, el uso de la binary replication (replicación binaria) requiere una consistencia muy estricta en todas las máquinas que se ejecutan en un conjunto de réplicas.

Incluso olvidarse de actualizar el servidor de la base de datos en uno de los nodos podría generar datos corruptos.

<img src="images/m103/c2/2-1-corrupted-data.png">

La Statement-based replication (replicación basada en declaraciones) es más o menos lo que parece.

<img src="images/m103/c2/2-1-statement.png">

Después de que se completa una escritura en el nodo primario, la declaración de escritura en sí misma se almacena en el **oplog** (registro de operaciones), 

<img src="images/m103/c2/2-1-oplog.png">

y los secundarios luego sincronizan sus **oplog** (registro de operaciones) con el **oplog** (registro de operaciones) primario y reproducen cualquier declaración nueva en sus propios datos.

<img src="images/m103/c2/2-1-statement-2.png">

Este enfoque funciona independientemente del sistema operativo o el conjunto de instrucciones de los nodos en el conjunto de réplica.

<img src="images/m103/c2/2-1-statement-3.png">

MongoDB utiliza la statement-based replication (replicación basada en instrucciones), pero los comandos correctos en realidad sufren una pequeña transformación antes de almacenarse en el **oplog** (registro de operaciones).

Y el objetivo aquí de la transformación es asegurarse de que las declaraciones almacenadas en el oplog se puedan aplicar un número indefinido de veces sin dejar de generar el mismo estado de datos.

Esta propiedad se llama **idempotencia**.

<img src="images/m103/c2/2-1-idempotence.png">

Por ejemplo, supongamos que tenemos una declaración que incrementó las paid views (vistas pagas) en un sitio web en 1.

<img src="images/m103/c2/2-1-inc.png">

El primario ya aplicó esta declaración a sus datos, por lo que sabe que después de incrementar el uso de la página en 1, el total de visitas de la página pasó de 1,000 a 1,001.

<img src="images/m103/c2/2-1-inc-2.png">

En realidad, transformaría esta declaración en una declaración que establece vistas de página en 1.001 y luego la almacena en el oplog registro de operaciones.

Cuando las declaraciones se replican de esta manera, podemos reproducir el registro de operaciones tantas veces como queramos sin preocuparnos por la consistencia de los datos.

<img src="images/m103/c2/2-1-manytimes.png">

Ahora echemos un vistazo a los pros y los contras de la replicación binaria y basada en sentencias.

El enfoque binario requiere que se almacenen menos datos en el registro binario, lo que significa que se pasan menos datos del primario al secundario.

La replicación binaria puede ser mucho más rápida que la replicación basada en sentencias porque los secundarios requieren menos trabajo al replicar realmente desde el registro binario.

Los datos que deben cambiarse se escriben directamente en el registro en ese caso.

Por otro lado, la replicación basada en sentencias en MongoDB escribe los comandos reales de MongoDB en el oplog, por lo que el oplog es un poco más grande.

Sin embargo, las declaraciones no están vinculadas a un sistema operativo específico ni a ninguna dependencia a nivel de máquina.

Por lo tanto, existen pocas restricciones en las máquinas en un replica set en MongoDB.

Esto es valioso para cualquier solución multiplataforma que requiera múltiples sistemas operativos en el mismo conjunto de réplicas.

<img src="images/m103/c2/2-1-resumen.png">

## 2. Examen What is Replication?

**Problem:**

Which of the following are true about binary replication and statement-based replication?

Check all answers that apply:

* Statement-based replication is platform independent. :+1:

* MongoDB uses statement-based replication, not binary replication. :+1:

* Binary replication is more accurate than statement-based replication.

**Correct answers:**

**Statement-based replication is platform independent.**

Statement-based replication is agnostic of operating system, because statements do not depend on a specific byte makeup or instruction set.

**MongoDB uses statement-based replication, not binary replication.**

MongoDB uses a small variation of statement-based replication which reduces statements to idempotent versions so they can be repeated.

**Incorrect answer:**

**Binary replication is more accurate than statement-based replication.**

Both methods of replication are accurate; however they do vary in speed and variability across operating systems.


## 3. Tema: Conjunto de réplicas MongoDB

### Notas de lectura

Lea más sobre el [Simple Raft Protocol](http://thesecretlivesofdata.com/raft/) y el  [Raft Consensus Algorithm](https://raft.github.io/).

### Transcripción

Ahora que hemos visto por qué la replicación es importante, profundicemos rápidamente en los detalles de los conjuntos de réplicas.

Conjuntos de réplicas o grupos de mongods que comparten copias de la misma información entre ellos.

Los miembros del conjunto de réplicas pueden tener uno de dos roles diferentes.

El nodo puede ser primario donde este nodo sirve todas las lecturas y todas las escrituras.

<img src="images/m103/c2/2-3-replicaset.png">

O nodo secundario donde la responsabilidad de este nodo es replicar toda la información, y luego servir como una alta disponibilidad para el nodo en caso de falla del primario.

Los secundarios obtendrán los datos del primario a través de un mecanismo de replicación asincrónica.

Cada vez que una aplicación escribe algunos datos en el replica set, ese derecho lo maneja el nodo primario.

<img src="images/m103/c2/2-3-client.png">

Y luego los datos se replican en los nodos secundarios.

<img src="images/m103/c2/2-3-client-2.png">

Ahora este mecanismo de replicación se basa en un protocolo que gestiona la forma en que los secundarios deben leer los datos del primario.

En MongoDB, este protocolo de replicación síncrona puede tener diferentes versiones.

<img src="images/m103/c2/2-3-client-3.png">

Tenemos **PV1** y **PV0**.

Las diferentes versiones del protocolo de replicación variarán ligeramente en la forma en que la durabilidad y la disponibilidad se verán forzadas en todo el conjunto.

Actualmente, la versión 1 del protocolo, o PV1, es la versión predeterminada.

<img src="images/m103/c2/2-3-client-4.png">

Este protocolo se basa en el protocolo **RAFT**.

Si no está familiarizado con el protocolo RAFT, en las notas de esta lección, encontrará información detallada sobre RAFT.

Solo tenga en cuenta por ahora que las versiones anteriores de MongoDB usaban la versión anterior del protocolo PV0, y que podría haber algunos detalles de configuración entre ambos protocolos.

Por ahora, nos centraremos en PV1.

En el corazón de este mecanismo de replicación está nuestro registro de operaciones, u **oplog** para abreviar.

<img src="images/m103/c2/2-3-oplog.png">

El oplog es un bloqueo basado en segmentos que realiza un seguimiento de todas las operaciones de escritura reconocidas por los conjuntos de réplica.

Cada vez que una escritura se aplica con éxito al nodo primario, se registrará en el oplog en su forma idempotente.

<img src="images/m103/c2/2-3-oplog-2.png">

Analizaremos los detalles de idempotencia más adelante.

Pero tenga en cuenta que una operación idempotente se puede aplicar varias veces.

Y el resultado final de esa operación siempre dará como resultado el mismo resultado final.

Más sobre esto más adelante.

Además de un rol primario o secundario, un miembro del conjunto de réplicas también se puede configurar como **árbitro**.

<img src="images/m103/c2/2-3-arbiter.png">

Un árbitro es un miembro que no tiene datos.

Su mera existencia es servir como un desempate entre los secundarios en una elección.

Y, obviamente, si no tiene datos, nunca puede convertirse en primario.

<img src="images/m103/c2/2-3-arbiter-2.png">

Los replica sets son resistentes a fallas.

Eso significa que tienen un mecanismo de conmutación por error (failover) que requiere que la mayoría de los nodos en un replica set estén disponibles para que se elija una primaria.

En este caso particular, supongamos que perdemos el acceso a nuestro primario.

<img src="images/m103/c2/2-3-replicaset-failure.png">

Si no tenemos un primario, no podremos escribir, y eso no es bueno.

Entonces, necesitamos despejar entre los nodos restantes del conjunto, ¿cuál podría convertirse en el nuevo primario?

Eso es a través de una elección, que se incrusta en los detalles de la versión política.

<img src="images/m103/c2/2-3-election.png">

Cómo se elige una primaria o por qué esto: un nodo particular se convierte en primario en lugar de otro.

Está fuera de alcance por ahora, pero tenga en cuenta que los detalles de estos estarán relacionados con la versión de protocolo que pueda tener su sistema.

Por ahora, solo tenga en cuenta que existe un mecanismo de conmutación por error (failover mechanism).

Es importante tener en cuenta que siempre debe tener al menos un **número impar de nodos en su replica set**.

En caso de un número par de nodos, asegúrese de que la mayoría esté constantemente disponible.

<img src="images/m103/c2/2-3-replicaset-four.png">

En esta forma de conjunto de réplicas, necesitará tener al menos tres nodos para estar disponibles.

La lista de los miembros del conjunto de réplica en sus opciones de configuración define la topología del conjunto de réplica.

<img src="images/m103/c2/2-3-replicaset-four-2.png">

Cualquier cambio de topología desencadenará una elección.

Agregar miembros al conjunto, fallas en miembros o cambiar cualquiera de los aspectos de configuración del conjunto de réplica se percibirá como un cambio de topología.

La topología de un replica set se define en la configuración del replica set.

La configuración del replica set se define en uno de los nodos y luego se comparte entre todos los miembros a través del mecanismo de replicación.

Examinaremos los documentos de configuración de replicación en detalle más adelante.

En este caso, tenemos cuatro miembros y necesito llamar su atención sobre una situación específica.

<img src="images/m103/c2/2-3-replicaset-four-3.png">

Esta topología ofrece exactamente el mismo número de fallas que un replica set de tres nodos.

Solo puede permitirse perder un miembro.

En caso de perder dos de ellos, no tendremos mayoría disponible fuera de los sets.

<img src="images/m103/c2/2-3-replicase-four-4.png">

¿Por qué?

Bueno, la mayoría de 4 es 3.

Por lo tanto, los dos nodos restantes no podrán elegir un primario entre ellos.

Tener ese nodo adicional no proporcionará disponibilidad adicional del servicio.

Solo otra copia redundante de nuestros datos, lo cual es bueno, pero no necesariamente por razones de disponibilidad.

Ahora, los replica sets pueden llegar hasta 50 miembros.

<img src="images/m103/c2/2-3-replicaset-50.png">

Y esto podría ser útil, especialmente para la distribución geográfica de nuestros datos donde queremos copias de nuestros datos más cerca de nuestros usuarios y aplicaciones, o simplemente en múltiples ubicaciones para la redundancia.

<img src="images/m103/c2/2-3-replicaset-50-2.png">

Pero solo un máximo de siete de esos miembros pueden ser miembros con derecho a voto.

<img src="images/m103/c2/2-3-replicaset-7-voting.png">

Más de siete miembros pueden hacer que las rondas electorales tomen demasiado tiempo, con poco o ningún beneficio por motivos de disponibilidad y coherencia.

Entonces, entre esos siete nodos, uno de ellos se convertirá en el primario y los restantes serán elegibles como primarios si en caso de que cambie su política, o en caso de que se active una nueva elección.

Ahora, si por alguna razón no podemos o no queremos tener un nodo con datos, pero aún así podemos realizar una conmutación por error entre nodos, podemos agregar un miembro del conjunto de réplicas como árbitro.

<img src="images/m103/c2/2-3-arbiter-3.png">

Dicho esto, **los árbitros causan importantes problemas de consistencia en los sistemas de datos distribuidos**.

Por lo tanto, le recomendamos que los use con cuidado.

<img src="images/m103/c2/2-3-arbiter-4.png">

En mi opinión personal, el uso de árbitros es una opción muy sensible y potencialmente dañina en muchas implementaciones.

Así que desanimo ociosamente el uso de árbitros.

Dentro de los nodos secundarios, estos también se pueden configurar para que tengan propiedades específicas o especiales definidas.

Podemos definir nodos ocultos, por ejemplo.

<img src="images/m103/c2/2-3-hidden.png">

El propósito de un nodo oculto es proporcionar cargas de trabajo específicas de solo lectura, o tener copias sobre sus datos que están ocultos de la aplicación.

Los nodos ocultos también se pueden configurar con un retraso en su proceso de replicación.

Llamamos a estos nodos **delayed** (retrasados).

<img src="images/m103/c2/2-3-delayed.png">

El propósito de tener nodos delayed es permitir la resistencia a la corrupción a nivel de aplicación, sin depender de archivos de copia de seguridad en frío para recuperarse de tal evento.

Si tenemos un nodo delayed, digamos una hora, y si su DBA pierde accidentalmente una colección, tenemos una hora para recuperar todos los datos del nodo delayed sin necesidad de volver al archivo de copia de seguridad para recuperar en cualquier momento que se creó una copia de seguridad.

Permitiéndonos tener copias de seguridad en caliente.

Recapitulemos lo que acabamos de aprender en esta conferencia.

<img src="images/m103/c2/2-3-resumen.png">

Los creplica sets son grupos de procesos mongod que comparten los mismos datos entre todos los miembros del conjunto.

Proporcionan un mecanismo de alta disponibilidad y conmutación por error (failover) para nuestra aplicación, lo que hace que el servicio no caiga en caso de falla.

La conmutación por error(failover) es compatible con la mayoría de los nodos que eligen entre ellos quién debería ser el nuevo nodo primario en cada momento.

Los replica sets son un sistema dinámico, significa que los miembros pueden tener diferentes roles en diferentes momentos y pueden configurarse para abordar un propósito funcional específico, como abordar la lectura en las cargas de trabajo, o configurarse para retrasarse a tiempo para permitir copias de seguridad en caliente.

## 4. Examen MongoDB Replica Set

**Problem:**

Which of the following are true for replica sets in MongoDB?

Check all answers that apply:

* We can have up to 50 voting members in a replica set.

* Replica set members have a fixed role assigned.

* We should always use arbiters.

* Replica sets provide high availability. :+1:


## 5. Tema: Configuración de un conjunto de réplicas

### Notas de lectura

Lea más sobre los [comandos VI](http://www.lagmonster.org/docs/vi.html).

**Errata**

En el minuto 1:08, Matt muestra cómo cambiar los permisos del archivo de clave para permitir que el proceso `mongod` lea el archivo usando el modo demasiado permisivo, 600. Un conjunto de permisos más correcto sería usar **400**. Hemos reflejado eso en el instrucciones de lectura a continuación.

**Instrucciones de lectura**

*Nota: En el video, utilizamos el antiguo nombre de host* `"m103.mongodb.university"` que se ha cambiado a `"m103"`. *Hemos actualizado todos los siguientes comandos en consecuencia*.

El archivo de configuración para el primer nodo (`node1.conf`):

```sh
storage:
  dbPath: /var/mongodb/db/node1
net:
  bindIp: 192.168.103.100,localhost
  port: 27011
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node1/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

Crear el archivo de claves y establecer permisos en él:

```sh
sudo mkdir -p /var/mongodb/pki/
sudo chown vagrant:vagrant /var/mongodb/pki/
openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
chmod 400 /var/mongodb/pki/m103-keyfile
```

Crear el dbpath para **node1**:

```sh
mkdir -p /var/mongodb/db/node1
```

Iniciar una `mongod` con `node1.conf`:

```sh
mongod -f node1.conf
```

Copiando `node1.conf` a `node2.conf` y `node3.conf`:

```sh
cp node1.conf node2.conf
cp node2.conf node3.conf
```

Editar `node2.conf` usando `vi`:

```sh
vi node2.conf
```

Guardar el archivo y salir de `vi`:

```sh
:wq
```

`node2.conf`, después de cambiar `dbpath`, `port` y `logpath`:

```sh
storage:
  dbPath: /var/mongodb/db/node2
net:
  bindIp: 192.168.103.100,localhost
  port: 27012
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node2/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

`node3.conf`, después de cambiar `dbpath`, `port`, y `logpath`:

```sh
storage:
  dbPath: /var/mongodb/db/node3
net:
  bindIp: 192.168.103.100,localhost
  port: 27013
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node3/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

Crear los directorios de datos para `node2` y `node3`:

```sh
mkdir /var/mongodb/db/{node2,node3}
```

Inicio de procesos mongod con `node2.conf` y `node3.conf`:

```sh
mongod -f node2.conf
mongod -f node3.conf
```

Cononectarse a `node1`:

```sh
mongo --port 27011
```

Inicializando el replica set:

```sh
rs.initiate()
```

Crear un user:

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

Saliendo del Shell Mongo y conectándose a todo el replica set:

```sh
exit
mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin"
-p "m103-pass" --authenticationDatabase "admin"
```

Obtención del estado del replica set:

```sh
rs.status()
```

Agregar otros miembros al replica set:

```sh
rs.add("m103:27012")
rs.add("m103:27013")
```

Obtención de una descripción general de la topología del replica set:

```sh
rs.isMaster()
```

Dar de baja el primario actual:

```sh
rs.stepDown()
```

Comprobación de la descripción general del replica set después de la elección:

```sh
rs.isMaster()
```

### Transcripción

Muy bien, así que en esta lección, vamos a iniciar un replica set local.

Comenzaremos lanzando independientemente tres procesos mongod y no podrán comunicarse entre sí hasta que los conectemos, en ese momento podrán replicar datos para nosotros.

<img src="images/m103/c2/2-5-replicaset.png">

Entonces este es el archivo de configuración para el nodo independiente.


```sh
storage:
  dbPath: /var/mongodb/db/node1
net:
  bindIp: 192.168.103.100,localhost
  port: 27011
security:
  authorization: enabled  
systemLog:
  destination: file
  path: /var/mongodb/db/node1/mongod.log
  logAppend: true
processManagement:
  fork: true
```

Lo hemos llamado node 1.

Y esta configuración debería resultarle bastante familiar si ha seguido las lecciones anteriores.

En realidad, no necesitamos cambiar ninguna de estas configuraciones para habilitar la replicación, solo necesitamos agregar algunas líneas.

Entonces, esta línea `keyFile: /var/mongodb/pki/m103-keyfile` permite la autenticación de archivos clave en nuestro clúster, lo que exige que todos los miembros del replica set se autentiquen entre sí utilizando un archivo clave que creamos aquí.

```sh
storage:
  dbPath: /var/mongodb/db/node1
net:
  bindIp: 192.168.103.100,localhost
  port: 27011
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node1/mongod.log
  logAppend: true
processManagement:
  fork: true
```

Y crearemos este en un minuto.

Esto se suma a la autenticación del cliente que habilitamos en la línea anterior `authorization: enabled`.

Entonces creamos este archivo de clave usando OpenSSL, y lo colocamos en el directorio que especificamos en nuestro archivo de configuración.

```sh
vagrant@m103:~$ sudo mkdir -p /var/mongodb/pki/
vagrant@m103:~$ sudo chown vagrant:vagrant /var/mongodb/pki/
vagrant@m103:~$ openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
vagrant@m103:~$ chmod 400 /var/mongodb/pki/m103-keyfile
```

Pero en este momento, nuestros procesos mongod en realidad no pueden usar este archivo de clave porque no tienen los permisos para leerlo.

Entonces, lo que vamos a hacer es cambiar los permisos usando `chmod` para permitirles leer el archivo.

600 (400) aquí solo especifica nuevos permisos.

Entonces, habilitar la autenticación de archivos clave aquí `keyFile: /var/mongodb/pki/m103-keyfile` habilita implícitamente la autenticación del cliente que habilitamos en la línea anterior `authorization: enabled`, pero voy a dejar ambos aquí por el momento solo por claridad.


```sh
storage:
  dbPath: /var/mongodb/db/node1
net:
  bindIp: 192.168.103.100,localhost
  port: 27011
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node1/mongod.log
  logAppend: true
processManagement:
  fork: true
```

Este es un recordatorio de que, además de autenticarse con el cliente, nuestros nodos también se autentican entre sí.

Entonces esta es la última línea que tenemos que agregar a nuestro archivo de configuración para permitir la replicación en este nodo.

```sh
replication:
  replSetName: m103-example
```

Y todo lo que hace es especificar el nombre del conjunto de réplicas del que este nodo formará parte.

```sh
storage:
  dbPath: /var/mongodb/db/node1
net:
  bindIp: 192.168.103.100,localhost
  port: 27011
security:
  authorization: enabled
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node1/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

Ahora todo lo que tenemos que hacer es crear la ruta de DB que nombramos aquí `dbPath: /var/mongodb/db/node1`.

Y en realidad podemos usar este archivo para iniciar un mongod.

Así que aquí solo estoy creando mi ruta de DB, y ahora puedo iniciar el mongod usando nuestro archivo de configuración.

```sh
vagrant@m103:~$ mkdir -p /var/mongodb/db/node1
vagrant@m103:~$ sudo chown vagrant:vagrant /var/mongodb/db/node1/
vagrant@m103:~$ mongod -f ./node1.conf
about to fork child process, waiting until server is ready for connections.
forked process: 2404
child process started successfully, parent exiting
vagrant@m103:~$ 
```

Y hemos comenzado con éxito nuestro primer nodo.

Así que ahora tenemos un nodo y solo nos quedan dos más.

Entonces, este comando simplemente está copiando el archivo que acabamos de crear en un nuevo archivo llamado `node2.conf` porque los otros dos nodos tendrán configuraciones muy similares.

```sh
vagrant@m103:~$ cp node1.conf node2.conf
```

Básicamente podemos copiar este, cambiar tres líneas y lanzar un nuevo nodo.

Nunca subestimes el poder de copiar y pegar.

Voy a hacer lo mismo para nuestro tercer nodo aquí

```sh
vagrant@m103:~$ cp node2.conf node3.conf
```

, y luego editaré el segundo.


```sh
vi node2.conf
```

Entonces, las tres cosas que necesitamos cambiar en este archivo son la ruta de la base de datos, el número de puerto y la ruta del registro.

```sh
storage:
  dbPath: /var/mongodb/db/node2
net:
  bindIp: 192.168.103.100,localhost
  port: 27012
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node2/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

Guardar el archivo y salir de vi:

```sh
:wq
```

Una vez que hacemos eso, en realidad estamos bien para comenzar un nuevo nodo.

Así que aquí acabo de crear la ruta para el nodo 2 y la estoy iniciando con mongod.

```sh
vagrant@m103:~$ mkdir /var/mongodb/db/node2

vagrant@m103:~$ sudo chown vagrant:vagrant /var/mongodb/db/node2

vagrant@m103:~$ mongod -f node2.conf
about to fork child process, waiting until server is ready for connections.
forked process: 2455
child process started successfully, parent exiting
vagrant@m103:~$ 
```

Y ahora tenemos dos nodos en nuestro conjunto.

Solo voy a hacer lo mismo para nuestro tercer archivo de configuración, y notaré que los tres nodos en el conjunto de réplica hacen referencia al mismo archivo de clave.

```sh
vi node3.conf

storage:
  dbPath: /var/mongodb/db/node3
net:
  bindIp: 192.168.103.100,localhost
  port: 27013
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/node3/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

```sh
vagrant@m103:~$ mkdir /var/mongodb/db/node3

vagrant@m103:~$ sudo chown vagrant:vagrant /var/mongodb/db/node2

vagrant@m103:~$ mongod -f node3.conf
about to fork child process, waiting until server is ready for connections.
forked process: 2495
child process started successfully, parent exiting
vagrant@m103:~$ 
```

Por lo general, estas instancias mongod se estarían ejecutando en diferentes máquinas, pero debido a que se están ejecutando en la misma máquina, todas compartirán el mismo archivo de clave `keyFile: /var/mongodb/pki/m103-keyfile` y usarán el mismo para autenticarse entre sí.

Normalmente, este archivo de clave se copiará en cada máquina donde se ejecuta cada mongod.

Entonces, en este punto, comenzamos tres procesos mongod que eventualmente formarán un conjunto de réplicas.

<img src="images/m103/c2/2-5-replicaset-2.png">

Pero en este momento, no pueden replicar datos.

Y, de hecho, no saben que hay otros nodos por ahí.

Son ciegos al mundo que los rodea.

Necesitamos habilitar la comunicación entre los nodos para que puedan permanecer sincronizados.

<img src="images/m103/c2/2-5-replicaset-3.png">

Así que solo voy a conectarme al nodo uno aquí.

```sh
vagrant@m103:~$ mongo --port 27011
MongoDB shell version v3.6.17
connecting to: mongodb://127.0.0.1:27011/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("e3080836-f968-458b-a416-41bb33d46053") }
MongoDB server version: 3.6.17
MongoDB Enterprise > 

```

Entonces uso este comando `rs.initiate` para iniciar el conjunto de réplicas.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.initiate()
{
	"info2" : "no configuration specified. Using a default configuration for the set",
	"me" : "192.168.103.100:27011",
	"info" : "try querying local.system.replset to see current configuration",
	"ok" : 0,
	"errmsg" : "already initialized",
	"code" : 23,
	"codeName" : "AlreadyInitialized",
	"operationTime" : Timestamp(1582567520, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582567520, 1),
		"signature" : {
			"hash" : BinData(0,"dz+tLQF1l21yDuvszF08KAkSn64="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Y en realidad necesitamos ejecutarlo en uno de los nodos.

Como lo ejecutamos aquí, solo tenemos que agregar los otros dos nodos desde este nodo.

Sin embargo, tenemos habilitada la autenticación del cliente, por lo que no podemos agregar otros nodos al conjunto hasta que creamos un usuario y luego nos conectamos como ese usuario.

Muy bien, entonces con el siguiente comando creó nuestro súper usuario `m103`, llamado `m103-admin`, que tiene acceso de root y se autentica en la base de datos `admin`.

```sh
MongoDB Enterprise m103-example:PRIMARY> use admin
switched to db admin

MongoDB Enterprise m103-example:PRIMARY> db.createUser({
...   user: "m103-admin",
...   pwd: "m103-pass",
...   roles: [
...     {role: "root", db: "admin"}
...   ]
... })
Successfully added user: {
	"user" : "m103-admin",
	"roles" : [
		{
			"role" : "root",
			"db" : "admin"
		}
	]
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Ahora voy a salir de este mongod y luego volver a iniciar sesión como ese usuario.

```sh
MongoDB Enterprise m103-example:PRIMARY> exit
bye

vagrant@m103:~$ mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
MongoDB shell version v3.6.17
connecting to: mongodb://192.168.103.100:27011/?authSource=admin&gssapiServiceName=mongodb&replicaSet=m103-example
2020-02-24T18:10:39.096+0000 I NETWORK  [thread1] Starting new replica set monitor for m103-example/192.168.103.100:27011
2020-02-24T18:10:39.097+0000 I NETWORK  [thread1] Successfully connected to 192.168.103.100:27011 (1 connections now open to 192.168.103.100:27011 with a 5 second timeout)
Implicit session: session { "id" : UUID("5d9da307-cdec-4eee-93b1-58be6e987724") }
MongoDB server version: 3.6.17
Server has startup warnings: 
2020-02-24T17:43:17.073+0000 I STORAGE  [initandlisten] 
2020-02-24T17:43:17.073+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-02-24T17:43:17.073+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-02-24T17:43:17.847+0000 I CONTROL  [initandlisten] 
2020-02-24T17:43:17.847+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-02-24T17:43:17.847+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-24T17:43:17.847+0000 I CONTROL  [initandlisten] 
2020-02-24T17:43:17.847+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-02-24T17:43:17.847+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-24T17:43:17.847+0000 I CONTROL  [initandlisten] 
MongoDB Enterprise m103-example:PRIMARY> 
```

Entonces este es el comando que vamos a usar para conectarnos al conjunto de réplicas.

Y además de autenticar aquí con una contraseña de nombre de usuario, tenemos que especificar el nombre de la réplica establecida en el nombre del host.

Esto le indicará al Shell Mongo que se conecte directamente al replica set, en lugar de solo este nodo que especificamos.

Lo que va a hacer el shell es usar este nodo para descubrir cuál es el primario actual del conjunto de réplicas y luego conectarse a ese nodo.

En este caso, obviamente solo hay un nodo en el conjunto y ese nodo es el primario.

Entonces esa es la que nos conectó el shell.

Entonces este comando, `rs.status` es una forma útil de obtener un informe de estado en nuestro conjunto de réplicas.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.status()
{
	"set" : "m103-example",
	"date" : ISODate("2020-02-24T18:12:55.931Z"),
	"myState" : 1,
	"term" : NumberLong(1),
	"syncingTo" : "",
	"syncSourceHost" : "",
	"syncSourceId" : -1,
	"heartbeatIntervalMillis" : NumberLong(2000),
	"optimes" : {
		"lastCommittedOpTime" : {
			"ts" : Timestamp(1582567970, 1),
			"t" : NumberLong(1)
		},
		"readConcernMajorityOpTime" : {
			"ts" : Timestamp(1582567970, 1),
			"t" : NumberLong(1)
		},
		"appliedOpTime" : {
			"ts" : Timestamp(1582567970, 1),
			"t" : NumberLong(1)
		},
		"durableOpTime" : {
			"ts" : Timestamp(1582567970, 1),
			"t" : NumberLong(1)
		}
	},
	"members" : [
		{
			"_id" : 0,
			"name" : "192.168.103.100:27011",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 1779,
			"optime" : {
				"ts" : Timestamp(1582567970, 1),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2020-02-24T18:12:50Z"),
			"syncingTo" : "",
			"syncSourceHost" : "",
			"syncSourceId" : -1,
			"infoMessage" : "",
			"electionTime" : Timestamp(1582567469, 2),
			"electionDate" : ISODate("2020-02-24T18:04:29Z"),
			"configVersion" : 1,
			"self" : true,
			"lastHeartbeatMessage" : ""
		}
	],
	"ok" : 1,
	"operationTime" : Timestamp(1582567970, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582567970, 1),
		"signature" : {
			"hash" : BinData(0,"doEUCG+b9bZOJWbWCxB3hbFLDM0="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Mira, nos da el nombre del set `"set" : "m103-example"`.

Nos da cuánto tiempo son los intervalos entre latidos `"heartbeatIntervalMillis" : NumberLong(2000)`.

Por defecto, son 2000 milisegundos, lo que significa que los nodos están hablando entre sí cada dos segundos.

Podemos desplazarnos hacia abajo para obtener una lista de los miembros del conjunto.

En este caso, es solo un miembro, al cual estamos conectados, el primario actual.


```sh
"members" : [
		{
			"_id" : 0,
			"name" : "192.168.103.100:27011",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 1779,
			"optime" : {
				"ts" : Timestamp(1582567970, 1),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2020-02-24T18:12:50Z"),
			"syncingTo" : "",
			"syncSourceHost" : "",
			"syncSourceId" : -1,
			"infoMessage" : "",
			"electionTime" : Timestamp(1582567469, 2),
			"electionDate" : ISODate("2020-02-24T18:04:29Z"),
			"configVersion" : 1,
			"self" : true,
			"lastHeartbeatMessage" : ""
		}
	],
```

Entonces, este es el comando que usamos para agregar nuevos nodos a nuestro conjunto de réplicas, `rs.add`, y todo lo que tenemos que especificar aquí es el nombre de host, que es solo el nombre de host del cuadro Vagrant y el puerto que ese nodo está ejecutando.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.add("m103:27012")
{
	"ok" : 1,
	"operationTime" : Timestamp(1582571070, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582571070, 1),
		"signature" : {
			"hash" : BinData(0,"L+ddF1hw5wdj2EMhxP+VhQzDrac="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Ahora que funcionó.

Solo voy a hacer lo mismo para nuestro tercer nodo.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.add("m103:27013")
{
	"ok" : 1,
	"operationTime" : Timestamp(1582571128, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582571128, 1),
		"signature" : {
			"hash" : BinData(0,"LY/2N0LMnI1pj6B7bd20tOd6aZY="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Solo voy a verificar con `rs.isMaster`.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013"
	],
	"setName" : "m103-example",
	"setVersion" : 3,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "192.168.103.100:27011",
	"me" : "192.168.103.100:27011",
	"electionId" : ObjectId("7fffffff0000000000000001"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582571181, 1),
			"t" : NumberLong(1)
		},
		"lastWriteDate" : ISODate("2020-02-24T19:06:21Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582571181, 1),
			"t" : NumberLong(1)
		},
		"majorityWriteDate" : ISODate("2020-02-24T19:06:21Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-24T19:06:29.251Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582571181, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582571181, 1),
		"signature" : {
			"hash" : BinData(0,"pYBSJ9iA5h080WPeTcIGDzL5HwE="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Y podemos ver que nuestro replica set ahora tiene tres nodos.

```sh
"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013"
	],
```

Entonces, ahora que hemos agregado esos dos nodos a nuestro replica set y los hemos conectado, pueden replicar datos entre sí.

<img src="images/m103/c2/2-5-replicaset-4.png">

Una cosa que quiero señalar en este momento es que el primario actual se está ejecutando en el puerto 27011.

<img src="images/m103/c2/2-5-replicaset-5.png">

Y podríamos verificar eso a partir de la salida de `rs.isMaster`, donde dice que primario es `"primary" : "192.168.103.100:27011",`, de hecho, el nodo que se ejecuta en 27011.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013"
	],
	"setName" : "m103-example",
	"setVersion" : 3,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "192.168.103.100:27011",
	"me" : "192.168.103.100:27011",
	"electionId" : ObjectId("7fffffff0000000000000001"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582571181, 1),
			"t" : NumberLong(1)
		},
		"lastWriteDate" : ISODate("2020-02-24T19:06:21Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582571181, 1),
			"t" : NumberLong(1)
		},
		"majorityWriteDate" : ISODate("2020-02-24T19:06:21Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-24T19:06:29.251Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582571181, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582571181, 1),
		"signature" : {
			"hash" : BinData(0,"pYBSJ9iA5h080WPeTcIGDzL5HwE="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Sin embargo, podemos forzar una elección para que un nodo diferente se vuelva primario.

Y el comando que usamos para hacer eso se llama `rs.stepDown`.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.stepDown()
2020-02-24T19:09:48.251+0000 E QUERY    [thread1] Error: error doing query: failed: network error while attempting to run command 'replSetStepDown' on host '192.168.103.100:27011'  :
DB.prototype.runCommand@src/mongo/shell/db.js:168:1
DB.prototype.adminCommand@src/mongo/shell/db.js:186:16
rs.stepDown@src/mongo/shell/utils.js:1397:12
@(shell):1:1
2020-02-24T19:09:48.251+0000 I NETWORK  [thread1] Marking host 192.168.103.100:27011 as failed :: caused by :: Location40657: Last known master host cannot be reached
2020-02-24T19:09:48.251+0000 I NETWORK  [thread1] Socket closed remotely, no longer connected (idle 9 secs, remote host 192.168.103.100:27011)
2020-02-24T19:09:48.254+0000 I NETWORK  [thread1] Successfully connected to 192.168.103.100:27011 (1 connections now open to 192.168.103.100:27011 with a 5 second timeout)
2020-02-24T19:09:48.255+0000 W NETWORK  [thread1] Unable to reach primary for set m103-example
2020-02-24T19:09:48.759+0000 W NETWORK  [thread1] Unable to reach primary for set m103-example
2020-02-24T19:09:49.262+0000 W NETWORK  [thread1] Unable to reach primary for set m103-example
MongoDB Enterprise m103-example:PRIMARY> 

```
Ahora, el comando `rs.stepDown()` es lo que usamos para reducir de forma segura el primario actual a un secundario y forzar una elección.

El error que estamos obteniendo aquí es porque el shell está tratando de conectarnos con el primario, pero los secundarios todavía están en el proceso de elegir un primario, por lo que no hay un primario en este momento.

Tan pronto como uno sea elegido, el shell nos conectará con él, lo que acaba de hacer.

Si volvemos a ejecutar `rs.isMaster`, podemos verificar que ahora este nodo `"primary" : "m103:27012"` es el primario actual, a diferencia de 27011, que era el primario antes.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013"
	],
	"setName" : "m103-example",
	"setVersion" : 3,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "m103:27012",
	"me" : "m103:27012",
	"electionId" : ObjectId("7fffffff0000000000000002"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582571489, 1),
			"t" : NumberLong(2)
		},
		"lastWriteDate" : ISODate("2020-02-24T19:11:29Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582571489, 1),
			"t" : NumberLong(2)
		},
		"majorityWriteDate" : ISODate("2020-02-24T19:11:29Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-24T19:11:29.645Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582571489, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582571489, 1),
		"signature" : {
			"hash" : BinData(0,"lTtGNn5lVOQu5xV7I3QcAveGgQM="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Entonces, para resumir, hemos cubierto cómo iniciar un conjunto de réplicas, cómo puede agregar nodos al conjunto de réplicas y cómo verificar el estado del conjunto de réplicas.

<img src="images/m103/c2/2-5-resumen.png">

Usamos `rs.status` y `rs.isMaster` en esta lección, y esos comandos tienen diferentes salidas para diferentes casos de uso.

Y le instaría a explorarlos para descubrir cuál se ajusta a su caso de uso.

## 6. Examen Setting Up a Replica Set

**Problem:**

Which of the following is/are true about setting up a replica set?

Check all answers that apply:

* All nodes in a replica set must be run on the same port.

* When connecting to a replica set, the mongo shell will redirect the connection to the primary node. :+1:

* rs.initiate() must be run on every node in the replica set.

* Enabling internal authentication in a replica set implicitly enables client authentication. :+1:

**Correct answers:**

* **Enabling internal authentication in a replica set implicitly enables client authentication.**

This is true; if nodes authenticate to each other, clients must authenticate to the cluster.

* **When connecting to a replica set, the mongo shell will redirect the connection to the primary node.**

This is true; even if an election is held and there is temporarily no primary, the shell will wait to connect until a primary is elected.

**Incorrect answers:**

* **All nodes in a replica set must be run on the same port.**

This is incorrect; in fact, the nodes should be run on different ports. Ideally, they would each run on different machines.

* **`rs.initiate()` must be run on every node in the replica set.**

This is incorrect; rs.initiate() should only be run on one node in the replica set.

## 7. Laboratorio: iniciar un conjunto de réplicas localmente

### Lab - Initiate a Replica Set Locally

**Problem:**

In this lab you will launch a replica set with three members from within your Vagrant environment. To secure this replica set, you will create a keyfile for your nodes to use when communicating with each other.

For this lab, you must place this keyfile in the `/var/mongodb/pki` directory and change the permissions so only the owner of the file can read it or write to it:

```sh
sudo mkdir -p /var/mongodb/pki
sudo chown vagrant:vagrant -R /var/mongodb
openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
chmod 600 /var/mongodb/pki/m103-keyfile
```

Your three mongod processes will each have their own configuration file, and now those config files can reference the keyfile you just made. These config files will be similar to the config file from the previous lab, but with the following adjustments:


type | PRIMARY | SECONDARY	 | SECONDARY
-----|---------|-------------|----------
config filename |	mongod-repl-1.conf | mongod-repl-2.conf |	mongod-repl-3.conf
port | 27001 | 27002 | 27003
dbPath | /var/mongodb/db/1 | /var/mongodb/db/2 | /var/mongodb/db/3
logPath | /var/mongodb/db/mongod1.log | /var/mongodb/db/mongod2.log | /var/mongodb/db/mongod3.log
replSet | m103-repl | m103-repl	| m103-repl
keyFile | /var/mongodb/pki/m103-keyfile | /var/mongodb/pki/m103-keyfile |	/var/mongodb/pki/m103-keyfile
bindIP | localhost,192.168.103.100 | localhost,192.168.103.100 | localhost,192.168.103.100

Note that the mongod does **not** automatically create the `dbPath` directory. You will need to create this yourself:

```sh
mkdir /var/mongodb/db/{1,2,3}
```

Once your configuration files are complete, you can start up the replica set:

1. Start a mongod process with the first config file (on port **27001**). This mongod process will act as the primary node in your replica set (at least, until an election occurs).

2. Now use the mongo shell to connect to this node. On this node, and **only** this node, initiate your replica set with `rs.initiate()`. Remember that this will only work if you are connected from `localhost`.

3. Once you run `rs.initiate()`, the node automatically configures a default replication configuration and elects itself as a primary. Use `rs.status()` to check the status of the replica set. The shell prompt will read `PRIMARY` once the initiation process completes successfully.

4. Because the replica set uses a keyfile for internal authentication, clients must authenticate before performing any actions.

   While still connected to the primary node, create an admin user for your cluster using the `localhost` exception. As a reminder, here are the requirements for this user:

   * Role: `root` on `admin` database
   * Username: `m103-admin`
   * Password: `m103-pass`

5. Now exit the mongo shell and start the other two mongod processes with their respective configuration files.

6. Reconnect to your primary node as `m103-admin` and add the other two nodes to your replica set. Remember to use the IP address of the Vagrant box `192.168.103.100` when adding these nodes.

7. Once your other two members have been successfully added, run `rs.status()` to check that the `members` array has three nodes - one labeled `PRIMARY` and two labeled `SECONDARY`.

Now run the validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_initialize_local_replica_set
```

Enter answer here: 5a4d32f979235b109001c7bc

MIO
```sh
  241  vi mongod-repl-1.conf
  242  vi mongod-repl-2.conf
  243  cp mongod-repl-1.conf mongod-repl-2.conf
  244  cp mongod-repl-1.conf mongod-repl-3.conf
  245  vi mongod-repl-2.conf
  246  vi mongod-repl-3.conf
  247  cls
  248  clear
  249  mkdir -p /var/mongodb/pki
  250  sudo chown vagrant:vagrant -R /var/mongodb
  251  openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
  252  mkdir -p /var/mongodb/pki/
  253  sudo chown vagrant:vagrant -R /var/mongodb
  254  openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
  255  sudo chown vagrant:vagrant -R /var/mongodb/pki/
  256  openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
  257  chmod 600 /var/mongodb/pki/m103-keyfile
  258  mkdir /var/mongodb/db/{1,2,3}
  259  sudo chown vagrant:vagrant /var/mongodb/db/1/
  260  sudo chown vagrant:vagrant /var/mongodb/db/2/
  261  sudo chown vagrant:vagrant /var/mongodb/db/3/
  262  mongod -f ./mongod-repl-1.conf
  263  vi mongod-repl-1.conf
  264  vi mongod-repl-2.conf
  265  vi mongod-repl-3.conf
  266  vi mongod-repl-2.conf
  267  cls
  268  clear
  269  mongod -f mongod-repl-1.conf
  270  ls
  271  vi mongod-repl-1.conf
  272  mongod -f mongod-repl-1.conf
  273  mongo --port 27001
  274  mongod -f mongod-repl-2.conf
  275  mongod -f mongod-repl-3.conf
  276  mongo --host "m103-repl/192.168.103.100:27001" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
  277  validate_lab_initialize_local_replica_set
  278  mongo --host "m103-repl/192.168.103.100:27001" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
 
```

#### See detailed answer:

Below is an example of a valid config file for the first node in our replica set:

```sh
storage:
  dbPath: /var/mongodb/db/1
net:
  bindIp: 192.168.103.100,localhost
  port: 27001
security:
  keyFile: /var/mongodb/pki/m103-keyfile
systemLog:
  destination: file
  path: /var/mongodb/db/mongod1.log
  logAppend: true
processManagement:
  fork: true
operationProfiling:
  slowOpThresholdMs: 50
replication:
  replSetName: m103-repl
```

Once we are connected to this first node, we can initiate our replica set with `rs.initiate()`. Again, this command must be run from the same host as the mongod to use the localhost exception.

We can create our `m103-admin` user with the following commands:

```sh
rs.initiate()
use admin
db.createUser({
  user: "m103-admin",
  pwd: "m103-pass",
  roles: [
    {role: "root", db: "admin"}
  ]
})
```

After exiting the mongo shell, we can start up the other two mongod processes. These will be the secondary nodes in our replica set.

Now that the cluster has a configured user for authentication, we cannot use the localhost exception anymore. Instead, connect using the mongo shell and specify the `m103-admin` user to authenticate and connect to the cluster. As this user, we can add our other two nodes with the following commands:

```sh
rs.add("192.168.103.100:27002")
rs.add("192.168.103.100:27003")
```

We should receive a response that says `{"ok" : 1}` from each of these `rs.add()` commands. Now, running `rs.status()` should give us a `members` list with three healthy nodes.


## 8. Tema: Documento de Configuración de Replicación

### Transcripción

En esta lección, vamos a diseccionar la configuración de replicación.

En particular, analizaremos qué opciones de configuración de replica set tenemos y cómo se reflejan esas opciones en el documento de opciones de configuración.

El documento de configuración del replica set es un documento BSON simple que gestionamos utilizando una representación JSON donde la configuración o nuestros replica set se definen y se comparten en todos los nodos que están configurados en los conjuntos.

<img src="images/m103/c2/2-8-replication-configuration.png">

Podemos establecer cambios manualmente en este documento para configurar un replica set de acuerdo con la topología esperada y las opciones generales de replicación.

Aunque podemos hacer esto simplemente editando dichos documentos usando el shell `mongo.db`, también podemos usar un conjunto de ayudantes de shell como `rs.add`, `initiate`, `remove`, etc. 

Eso nos ayudará a facilitar la configuración y administración de esta misma configuración.

Hay una buena cantidad de opciones de configuración diferentes a nuestra disposición, como puede ver en el documento de opciones de configuración de línea de base.

<img src="images/m103/c2/2-8-configuration-options.png">

Esto puede sonar un poco desalentador, pero en realidad, es un conjunto de opciones bastante sencillo.

Y para esta lección, en particular, vamos a ver solo un conjunto de estas opciones de configuración: las básicas y fundamentales y las que vamos a utilizar a lo largo del curso.

<img src="images/m103/c2/2-8-configuration-set.png">

Todas las otras opciones están fuera del alcance de este curso.

Pero comencemos con el campo **`_id`**.

<img src="images/m103/c2/2-8-id.png">

Este campo se establece con el nombre del replica set.

Este es un valor de cadena que coincide con el replica set definido por el servidor.

Cada vez que comenzamos nuestro mongoD y proporcionamos un nombre `--replSet` a nuestro mongoD, lo que significa que este mongoD pertenecerá al conjunto, o estableciendo ese mismo nombre en el archivo de configuración, por ejemplo, nuestro archivo `etc/mongodb.conf`

<img src="images/m103/c2/2-8-id-2.png">

Estamos estableciendo un valor específico para ser utilizado como un nombre de replica set.

El mismo valor debe coincidir con el campo `_id` de nuestro documento de configuración del replica set.

<img src="images/m103/c2/2-8-id-3.png">

En caso de que tengamos valores diferentes de la configuración `_id` y el nombre del replica set definidas, terminamos con un mensaje de error.

<img src="images/m103/c2/2-8-id-4.png">

Obtenemos una configuración de replica set incorrecta, indicando que estamos intentando iniciar el replica set con un nombre diferente desde el que se ha establecido como `--replSet` o en el archivo de configuración.

<img src="images/m103/c2/2-8-id-5.png">

Esta es una protección contra configuraciones incorrectas o la adición incorrecta del servidor a los replica sets incorrectos.

El siguiente campo es la **`version`**.

<img src="images/m103/c2/2-8-version.png">

Ahora, una versión es solo un número entero que se incrementa cada vez que cambia la configuración actual de nuestro replica set.

<img src="images/m103/c2/2-8-version-2.png">

Si, por ejemplo, agregamos un nodo a nuestro replica set y si nuestra versión solía ser la número 1, incrementamos el valor.

<img src="images/m103/c2/2-8-version-3.png">

Cada vez que cambiamos una topología, cambiamos la configuración de un replica set o hacemos algo como cambiar el número de votos de un host dado, eso incrementará automáticamente el número de versión.

<img src="images/m103/c2/2-8-version-4.png">

El siguiente campo en línea es **members**.

Y los members es donde se define la topología de nuestro replica set.

<img src="images/m103/c2/2-8-members.png">

Cada elemento del array de miembros es un subdocumento que contiene los miembros del nodo del replica set.

Cada uno tiene un host compuesto por el host name y port.

En este caso, por ejemplo, tenemos `m103:27017`.

<img src="images/m103/c2/2-8-members-2.png">

Luego tenemos un conjunto de indicadores que determinan el papel de los nodos dentro del replica set.

El `arbiterOnly` se explica por sí mismo.

Esto significa que el nodo no retendrá ningún dato, y su contribución al conjunto es asegurar el quórum en las elecciones.

<img src="images/m103/c2/2-8-members-3.png">

`hidden--` es otro indicador que establece el nodo en función oculta.

Un nodo oculto no es visible para la aplicación, lo que significa que cada vez que emitimos algo como un `RS` es un comando maestro, este nodo no aparecerá en la lista.

Los nodos `int` son útiles para situaciones en las que queremos que un nodo particular admita operaciones específicas.

No están relacionados con la naturaleza operativa de su aplicación.

Por ejemplo, tener un nodo que maneje todos los informes o lecturas de BI.

Ambos indicadores están configurados en `false` de forma predeterminada.

**`priority`** la prioridad es un valor entero que nos permite establecer una jerarquía dentro del replica set.

<img src="images/m103/c2/2-8-priority.png">

Podemos establecer prioridades entre 0 y 1,000.

Los miembros con mayor prioridad tienden a ser elegidos en las primarias con mayor frecuencia.

Un cambio en la prioridad de un nodo desencadenará una elección porque se percibirá como un cambio de topología.

Establecer la prioridad en 0 efectivamente excluye a ese miembro de convertirse en primario.

En el caso, estamos configurando un miembro para que sea solo árbitro, eso implica que la prioridad debe establecerse en 0.

<img src="images/m103/c2/2-8-priority-2.png">

Lo mismo se aplicaría para hidden.

La prioridad aquí también debe convertirse en 0.

<img src="images/m103/c2/2-8-priority-3.png">

Si la nota está oculta, nunca puede volverse primaria porque la aplicación no la verá.

De lo contrario, se producirá un error en el que una nueva configuración del replica set es incompatible.

<img src="images/m103/c2/2-8-priority-4.png">

La prioridad debe ser 0 cuando oculto es igual a verdadero.

Y finalmente, tenemos **`slaveDelay`**.

<img src="images/m103/c2/2-8-slaved.png">

slaveDelay es un valor entero que determina un intervalo de demora de replicación en segundos.

El valor predeterminado es 0.

Esta opción habilitará delayed nodes.

Estos miembros retrasados mantienen una copia de los datos que reflejan un estado en algún momento en el pasado, aplicando ese retraso en segundos.

Por ejemplo, si tenemos nuestra opción slaveDelay a 3.600 segundos, lo que significa 1 hora, eso significará que dicho miembro replicará datos de los otros nodos en los conjuntos que ocurrieron hace 1 hora.

<img src="images/m103/c2/2-8-slaved-2.png">

Al configurar esta opción como esclavo, también implica que su nodo estará oculto y la prioridad se establecerá en 0.

<img src="images/m103/c2/2-8-slaved-3.png">

Ah sí, y casi lo olvido.

También tenemos el campo `_id` dentro del subdocumento de miembros.

<img src="images/m103/c2/2-8-id-6.png">

Esto es solo un identificador único de cada elemento en el array.

Es un campo entero simple que se establece una vez que tenemos un miembro para el replica set.

Una vez establecido, este valor no se puede cambiar.

Ahora, de nuevo, hay mucho más que podemos configurar dentro de los documentos de configuración del replica set.

<img src="images/m103/c2/2-8-all.png">

**`settings`** en la que podemos definir varios atributos de protocolo de replicación diferentes 

<img src="images/m103/c2/2-8-all-2.png">

o cosas como la **`protocolVersion`** y **`configsvr`** que se verán más adelante en este curso.

<img src="images/m103/c2/2-8-all-3.png">

Pero los usos reales de estas opciones para el curso básico de administración están fuera de alcance.

Recapitulemos.

<img src="images/m103/c2/2-8-resumen.png">

El documento de configuración de replicación se utiliza para configurar nuestros replica set.

Aquí es donde se definen las propiedades de nuestros replica set, y el documento se comparte entre todos los miembros del conjunto.

El campo de miembros es donde se determinará un montón de nuestra configuración básica: qué nodos son parte del conjunto, qué roles tienen y qué tipo de topología queremos definir se establece en este campo.

Hay una gran cantidad de otras opciones de configuración que tratan con mecanismos de replicación internos o configuraciones generales de los conjuntos.

Estamos fuera de alcance en eso, pero tenga en cuenta que hay mucho más en el documento de replica set que puede configurar.

## 9. Examen Replication Configuration Document

**Problem:**

Which of the following fields are included in the replica set configuration document?

Check all answers that apply:

* `_id` :+1:

* `version` :+1:

* `members` :+1:

## 10. Tema: Comandos de replicación

### Instrucción de este tema

Comandos cubiertos en este tema:

```sh
rs.status()
rs.isMaster()
db.serverStatus()['repl']
rs.printReplicationInfo()
```

### Transcripción

En esta lección, cubriremos algunos de los comandos que usamos para recopilar información sobre un replica set.

<img src="images/m103/c2/2-10-replicaset.png">

Probablemente haya notado que hay muchas formas diferentes de verificar el estado de un replica set porque cada nodo emite mucha información.

Cada comando de replicación proporciona un subconjunto diferente de esta información.

#### El primero que vamos a cubrir es `rs.status`.

<img src="images/m103/c2/2-10-status.png">

`rs.status` se usa para informar sobre el estado general de cada nodo en el conjunto.

<img src="images/m103/c2/2-10-status-2.png">

Los datos que obtiene del `heartbeat` (latido) enviado entre nodos intermedios en el conjunto.

Debido a que depende de los `heartbeat` para estos datos, en realidad puede estar unos segundos desactualizado.

Este comando nos brinda la mayor cantidad de información para cada nodo específico.

```sh
MongoDB Enterprise m103-repl:PRIMARY> rs.status()
{
	"set" : "m103-repl",
	"date" : ISODate("2020-02-24T22:02:58.124Z"),
	"myState" : 1,
	"term" : NumberLong(1),
	"syncingTo" : "",
	"syncSourceHost" : "",
	"syncSourceId" : -1,
	"heartbeatIntervalMillis" : NumberLong(2000),
	"optimes" : {
		"lastCommittedOpTime" : {
			"ts" : Timestamp(1582581776, 1),
			"t" : NumberLong(1)
		},
		"readConcernMajorityOpTime" : {
			"ts" : Timestamp(1582581776, 1),
			"t" : NumberLong(1)
		},
		"appliedOpTime" : {
			"ts" : Timestamp(1582581776, 1),
			"t" : NumberLong(1)
		},
		"durableOpTime" : {
			"ts" : Timestamp(1582581776, 1),
			"t" : NumberLong(1)
		}
	},
	"members" : [
		{
			"_id" : 0,
			"name" : "192.168.103.100:27001",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 7989,
			"optime" : {
				"ts" : Timestamp(1582581776, 1),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2020-02-24T22:02:56Z"),
			"syncingTo" : "",
			"syncSourceHost" : "",
			"syncSourceId" : -1,
			"infoMessage" : "",
			"electionTime" : Timestamp(1582573984, 2),
			"electionDate" : ISODate("2020-02-24T19:53:04Z"),
			"configVersion" : 3,
			"self" : true,
			"lastHeartbeatMessage" : ""
		},
		{
			"_id" : 1,
			"name" : "m103:27002",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 7262,
			"optime" : {
				"ts" : Timestamp(1582581776, 1),
				"t" : NumberLong(1)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1582581776, 1),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2020-02-24T22:02:56Z"),
			"optimeDurableDate" : ISODate("2020-02-24T22:02:56Z"),
			"lastHeartbeat" : ISODate("2020-02-24T22:02:56.698Z"),
			"lastHeartbeatRecv" : ISODate("2020-02-24T22:02:56.699Z"),
			"pingMs" : NumberLong(0),
			"lastHeartbeatMessage" : "",
			"syncingTo" : "192.168.103.100:27001",
			"syncSourceHost" : "192.168.103.100:27001",
			"syncSourceId" : 0,
			"infoMessage" : "",
			"configVersion" : 3
		},
		{
			"_id" : 2,
			"name" : "m103:27003",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 7250,
			"optime" : {
				"ts" : Timestamp(1582581776, 1),
				"t" : NumberLong(1)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1582581776, 1),
				"t" : NumberLong(1)
			},
			"optimeDate" : ISODate("2020-02-24T22:02:56Z"),
			"optimeDurableDate" : ISODate("2020-02-24T22:02:56Z"),
			"lastHeartbeat" : ISODate("2020-02-24T22:02:56.699Z"),
			"lastHeartbeatRecv" : ISODate("2020-02-24T22:02:56.698Z"),
			"pingMs" : NumberLong(0),
			"lastHeartbeatMessage" : "",
			"syncingTo" : "192.168.103.100:27001",
			"syncSourceHost" : "192.168.103.100:27001",
			"syncSourceId" : 0,
			"infoMessage" : "",
			"configVersion" : 3
		}
	],
	"ok" : 1,
	"operationTime" : Timestamp(1582581776, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582581776, 1),
		"signature" : {
			"hash" : BinData(0,"1yVYWpLfu4uqhn5m+z0Z2oeZdGs="),
			"keyId" : NumberLong("6797103509075394561")
		}
	}
}
MongoDB Enterprise m103-repl:PRIMARY> 
```

Podemos ver que para un nodo dado obtenemos el estado del nodo `"stateStr" : "PRIMARY",`.

En este caso, es el primario.

Tenemos el `"uptime" : 7989` tiempo de actividad, que es el número de segundos que este nodo se ha estado ejecutando.

Y tenemos el `"optime"`

```sh
	"optime" : {
		"ts" : Timestamp(1582581776, 1),
		"t" : NumberLong(1)
	},
```

, que es la última vez que este nodo aplicó una operación desde su `oplog`.

También hay estadísticas de `heartbeat` latidos para cada nodo, pero no para el nodo en el que ejecutamos este comando.

Esto se debe a que las estadísticas de `heartbeat` latidos son relativas al lugar donde se ejecutó `rs.status`.

Sabemos que se ejecutó a partir de esto porque la marca propia en este nodo es verdadera `"self" : true,`.

Podemos desplazarnos hacia abajo a uno de los otros nodos y ver que tenemos algunas estadísticas de heartbeats aquí abajo.

```sh
"lastHeartbeat" : ISODate("2020-02-24T22:02:56.698Z"),
"lastHeartbeatRecv" : ISODate("2020-02-24T22:02:56.699Z"),
```

Porque sabemos que el primario era desde donde se ejecutaba este comando.

Sabemos que el último latido (`lastHeartbeat`) se refiere a la última vez que este nodo recibió con éxito un latido del primario.

Por el contrario, el último heartbeat recibido(`lastHeartbeatRecv`) se refiere a la última vez que el primario recibió con éxito un `heartbeat` de esta nota.

Podemos ver la frecuencia real de `heartbeat` en este conjunto a través de `"heartbeatIntervalMillis" : NumberLong(2000)`.

En este caso, este conjunto se ejecuta con el valor predeterminado de 2.000 milisegundos, lo que significa que los nodos se hablan entre sí cada dos segundos.

#### El siguiente comando que vamos a cubrir es `rs.isMaster`

<img src="images/m103/c2/2-10-ismaster.png">

Este describe el rol del nodo desde donde ejecutamos este comando.

Y también nos da información sobre el replica set.

<img src="images/m103/c2/2-10-ismaster-2.png">

La salida es mucho más fácil de leer que `rs.status` solo porque su salida es mucho más corta.

```sh
MongoDB Enterprise m103-repl:PRIMARY> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27001",
		"m103:27002",
		"m103:27003"
	],
	"setName" : "m103-repl",
	"setVersion" : 3,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "192.168.103.100:27001",
	"me" : "192.168.103.100:27001",
	"electionId" : ObjectId("7fffffff0000000000000001"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582581866, 1),
			"t" : NumberLong(1)
		},
		"lastWriteDate" : ISODate("2020-02-24T22:04:26Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582581866, 1),
			"t" : NumberLong(1)
		},
		"majorityWriteDate" : ISODate("2020-02-24T22:04:26Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-24T22:04:32.907Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582581866, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582581866, 1),
		"signature" : {
			"hash" : BinData(0,"qqTq/k/BNGbYECMhBAeDFngwrwg="),
			"keyId" : NumberLong("6797103509075394561")
		}
	}
}
MongoDB Enterprise m103-repl:PRIMARY> 

```

Como podemos ver, la lista de hosts es mucho más corta en este comando, y podemos verificar fácilmente si este nodo es secundario.

En este caso, no es porque el secundario es falso `"secondary" : false`.

O si este es el principal, en `"ismaster" : true,` en este caso.

También nos da el nombre del nodo primario en el conjunto, independientemente de dónde ejecutamos este comando.

En este caso, obviamente, lo ejecutamos desde el primario, por lo que esta bandera primaria `"primary" : "192.168.103.100:27001",` solo va a decir lo mismo que yo.

Entonces, solo quiero señalar aquí que cuando escribimos este comando con paréntesis después, llamamos a este método.

Pero podemos eliminar los paréntesis para ver qué se está ejecutando realmente en segundo plano.

```sh
MongoDB Enterprise m103-repl:PRIMARY> rs.isMaster
function () {
    return db.isMaster();
}
MongoDB Enterprise m103-repl:PRIMARY> 
```

Y podemos ver que `rs.isMaster` es en realidad solo un contenedor alrededor de una función llamada `db.isMaster`.

**Notarás que muchos de los comandos `rs.commands` en Mongo Shell son solo envoltorios alrededor de los `db.commands`.**

Como nota al margen, este es el comando que utilizan los controladores para descubrir el rol de cada nodo en el replica set.

Para obtener más información al respecto, puede seguir la referencia en las notas de la conferencia.

#### El siguiente comando es **`db.serverStatus`**.

<img src="images/m103/c2/2-10-serverstatus.png">

Este comando nos da mucha información sobre el proceso MongoD, pero solo veremos la sección llamada `repl`.

<img src="images/m103/c2/2-10-serverstatus-2.png">

La salida de este comando será muy similar a la salida de `rs.isMaster`.

```sh
MongoDB Enterprise m103-repl:PRIMARY> db.serverStatus()['repl']
{
	"hosts" : [
		"192.168.103.100:27001",
		"m103:27002",
		"m103:27003"
	],
	"setName" : "m103-repl",
	"setVersion" : 3,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "192.168.103.100:27001",
	"me" : "192.168.103.100:27001",
	"electionId" : ObjectId("7fffffff0000000000000001"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582582066, 1),
			"t" : NumberLong(1)
		},
		"lastWriteDate" : ISODate("2020-02-24T22:07:46Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582582066, 1),
			"t" : NumberLong(1)
		},
		"majorityWriteDate" : ISODate("2020-02-24T22:07:46Z")
	},
	"rbid" : 1
}
MongoDB Enterprise m103-repl:PRIMARY> 
```

Como podemos ver, la salida de este comando es muy similar a la salida de `rs.isMaster` con la excepción de un campo aquí.

El `"rbid" : 1` no está incluido en `rs.isMaster`.

Y todo lo que hace es contar el número de rollbacks que se han producido en este nodo.

#### El último comando que vamos a cubrir es `rs.printReplicationInfo`.

<img src="images/m103/c2/2-10-print.png">

Este comando solo tiene datos sobre el registro de operaciones y específicamente solo el registro de operaciones para el nodo al que estamos conectados actualmente.

Nos dará marcas de tiempo exactas para el primer y último evento que ocurrió en el registro de operaciones para ese nodo.

<img src="images/m103/c2/2-10-print-2.png">

Así que este es un informe rápido sobre la duración actual del registro de operaciones en el tiempo y en megabytes.

```sh
MongoDB Enterprise m103-repl:PRIMARY> rs.printReplicationInfo()
configured oplog size:   1722.11328125MB
log length start to end: 8162secs (2.27hrs)
oplog first event time:  Mon Feb 24 2020 19:53:04 GMT+0000 (UTC)
oplog last event time:   Mon Feb 24 2020 22:09:06 GMT+0000 (UTC)
now:                     Mon Feb 24 2020 22:09:13 GMT+0000 (UTC)
MongoDB Enterprise m103-repl:PRIMARY> 
```

Y recuerde que el primer evento en el registro de operaciones está sujeto a cambios porque el registro de operaciones es una colección limitada, y periódicamente se vacía para dejar espacio para nuevos datos.

En esta lección, hemos aprendido que hay varias formas diferentes de verificar el estado de un replica set, y cada una es importante por derecho propio.

<img src="images/m103/c2/2-10-resumen.png">

## 11. Examen Replication Commands

**Problem:**

What information can be obtained from running `rs.printReplicationInfo()`?

Check all answers that apply:

* The time of the earliest entry in the oplog. :+1:

* The earliest statement entered in the oplog.

* The time of the latest entry in the oplog. :+1:

* The current primary in the replica set.

* The last statement entered in the oplog.

**Correct answers:**

**The time of the earliest entry in the oplog.**

**The time of the latest entry in the oplog.**

`rs.printReplicationInfo()` gives us the times of the earliest and latest entries in the oplog.

**Incorrect answers:**

**The earliest statement entered in the oplog.**

**The last statement entered in the oplog.**

`rs.printReplicationInfo()` will only return timestamps for oplog statements; the statements themselves can be found in local.oplog.rs.

**The current primary in the replica set.**

`rs.printReplicationInfo()` only contains information pertaining to the node where the command was run.

## 12. Tema: DB local: Parte 1

### Notas de lectura

Instrucciones de lectura

Hacer un directorio de datos e iniciar un proceso mongod para un standalone node(nodo independiente):

```sh
mkdir allbymyselfdb
mongod --dbpath allbymyselfdb
```

Todas las instancias de MongoDB comienzan con dos bases de datos predeterminadas, **admin** y **local**:

```sh
mongo
show dbs
```

Mostrar colecciones de la base de datos local (esto muestra más colecciones de un conjunto de réplicas que de un nodo independiente):

```sh
use local
show collections
```

Consultando el oplog después de conectarse a un conjunto de réplicas:

```sh
use local
db.oplog.rs.find()
```

Obteniendo información sobre el oplog. Recuerde que el oplog es una colección limitada (capped), lo que significa que puede crecer hasta un tamaño preconfigurado antes de que comience a sobrescribir las entradas más antiguas con las más nuevas. Lo siguiente determinará si una colección está capped, cuál es el tamaño y cuál es el tamaño máximo.

Almacenar estadísticas de oplog como una variable llamada `stats`:

```sh
var stats = db.oplog.rs.stats()
```

Verificando que esta colección esté capada (crecerá a un tamaño preconfigurado antes de que comience a sobrescribir las entradas más antiguas con las más nuevas):

```sh
stats.capped
```

Obteniendo el tamaño actual del oplog:

```sh
stats.size
```

Obteniendo el límite de tamaño del oplog:

```sh
stats.maxSize
```

Obtención de datos de registro de operaciones actuales (incluidos el primer y último horario de eventos y el tamaño de registro de operaciones configurado):

```sh
rs.printReplicationInfo()
```

### Transcripción

En esta lección, vamos a echar un vistazo rápido a una base de datos fundamental y al antiguo proceso de replicación: nuestro querida y dulce BD Local.

En el momento en que comenzamos nuestro MongoDB, como lo estoy haciendo ahora, hay un nodo independiente (standalone node).

```sh
vagrant@m103:~$ 
vagrant@m103:~$ mkdir allbymyselfdb
vagrant@m103:~$ mongod --dbpath allbymyselfdb
```

Y un segunco shell me conecte a ese nodo, 

```sh
vagrant@m103:~$ mongo
MongoDB shell version v3.6.17
connecting to: mongodb://127.0.0.1:27017/?gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("9be42300-e732-4d0e-9052-b832d83ee4ba") }
MongoDB server version: 3.6.17
Server has startup warnings: 
2020-02-25T10:26:49.922+0000 I STORAGE  [initandlisten] 
2020-02-25T10:26:49.922+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-02-25T10:26:49.922+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-02-25T10:26:50.630+0000 I CONTROL  [initandlisten] 
2020-02-25T10:26:50.630+0000 I CONTROL  [initandlisten] ** WARNING: Access control is not enabled for the database.
2020-02-25T10:26:50.631+0000 I CONTROL  [initandlisten] **          Read and write access to data and configuration is unrestricted.
2020-02-25T10:26:50.631+0000 I CONTROL  [initandlisten] 
2020-02-25T10:26:50.632+0000 I CONTROL  [initandlisten] ** WARNING: This server is bound to localhost.
2020-02-25T10:26:50.632+0000 I CONTROL  [initandlisten] **          Remote systems will be unable to connect to this server. 
2020-02-25T10:26:50.633+0000 I CONTROL  [initandlisten] **          Start the server with --bind_ip <address> to specify which IP 
2020-02-25T10:26:50.633+0000 I CONTROL  [initandlisten] **          addresses it should serve responses from, or with --bind_ip_all to
2020-02-25T10:26:50.633+0000 I CONTROL  [initandlisten] **          bind to all interfaces. If this behavior is desired, start the
2020-02-25T10:26:50.634+0000 I CONTROL  [initandlisten] **          server with --bind_ip 127.0.0.1 to disable this warning.
2020-02-25T10:26:50.634+0000 I CONTROL  [initandlisten] 
2020-02-25T10:26:50.634+0000 I CONTROL  [initandlisten] 
2020-02-25T10:26:50.634+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-02-25T10:26:50.634+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-25T10:26:50.634+0000 I CONTROL  [initandlisten] 
2020-02-25T10:26:50.635+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-02-25T10:26:50.635+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-25T10:26:50.635+0000 I CONTROL  [initandlisten] 
MongoDB Enterprise > 

```
Una vez ya conectado escribo el comando:

```sh
MongoDB Enterprise > show dbs
admin   0.000GB
config  0.000GB
local   0.000GB
MongoDB Enterprise > 
```

podemos ver dos(3) espacios de nombres (name spaces) diferentes, o bases de datos, si lo prefiere.

Tenemos **admin**, que comprende todos los datos de administración y donde la mayoría de nuestros comandos de administración como `db.shutdownServer`, por ejemplo, deben ejecutarse o se ejecutarán.

Y tenemos **local**.

Así que usemos **local** por un segundo.


```sh
MongoDB Enterprise > use local
switched to db local
MongoDB Enterprise > 
```

Y mostremos sus colecciones:


```sh
MongoDB Enterprise > show collections
startup_log
MongoDB Enterprise > 
```

La BD local tiene solo una colección: `startup_log`.

Nada extraordinario hasta el momento, ya que este `startup_log` solo contiene el registro de inicio de este nodo en particular.

Entonces, conectemos a nuestro replica set y veamos cómo funcionan estos locales una vez que estemos en la tierra del replica set.

#### Conectarme al Replica Set Creado Localmente 

```sh
vagrant@m103:~$ mongod -f ./node1.conf
about to fork child process, waiting until server is ready for connections.
forked process: 6018
ERROR: child process failed, exited with error number 51
To see additional information in this output, start without the "--fork" option.
vagrant@m103:~$ 

vagrant@m103:~$ mongod -f ./node2.conf
about to fork child process, waiting until server is ready for connections.
forked process: 6025
ERROR: child process failed, exited with error number 51
To see additional information in this output, start without the "--fork" option.
vagrant@m103:~$ 

vagrant@m103:~$ mongod -f ./node3.conf
about to fork child process, waiting until server is ready for connections.
forked process: 6033
ERROR: child process failed, exited with error number 51
To see additional information in this output, start without the "--fork" option.
vagrant@m103:~$ 

vagrant@m103:~$ mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
MongoDB shell version v3.6.17
connecting to: mongodb://192.168.103.100:27011/?authSource=admin&gssapiServiceName=mongodb&replicaSet=m103-example
2020-02-25T10:43:22.529+0000 I NETWORK  [thread1] Starting new replica set monitor for m103-example/192.168.103.100:27011
2020-02-25T10:43:22.531+0000 I NETWORK  [thread1] Successfully connected to 192.168.103.100:27011 (1 connections now open to 192.168.103.100:27011 with a 5 second timeout)
2020-02-25T10:43:22.532+0000 I NETWORK  [thread1] Successfully connected to m103:27012 (1 connections now open to m103:27012 with a 5 second timeout)
2020-02-25T10:43:22.532+0000 I NETWORK  [thread1] changing hosts to m103-example/192.168.103.100:27011,m103:27012,m103:27013 from m103-example/192.168.103.100:27011
2020-02-25T10:43:22.534+0000 I NETWORK  [ReplicaSetMonitor-TaskExecutor-0] Successfully connected to m103:27013 (1 connections now open to m103:27013 with a 5 second timeout)
Implicit session: session { "id" : UUID("f09597ad-6816-4d9c-9121-abaca9c88343") }
MongoDB server version: 3.6.17
Server has startup warnings: 
2020-02-24T17:56:47.029+0000 I STORAGE  [initandlisten] 
2020-02-24T17:56:47.029+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-02-24T17:56:47.029+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] 
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] 
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] 
MongoDB Enterprise m103-example:PRIMARY> 
```

#### Conectarme al Replica Set del Curso

```sh
vagrant@m103:~$ mongo "mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/test?replicaSet=Cluster0-shard-0" --authenticationDatabase admin --ssl --username m001-student --password m001-mongodb-basics
MongoDB shell version v3.6.17
connecting to: mongodb://cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017/test?authSource=admin&gssapiServiceName=mongodb&replicaSet=Cluster0-shard-0
2020-02-25T10:47:01.252+0000 I NETWORK  [thread1] Starting new replica set monitor for Cluster0-shard-0/cluster0-shard-00-00-jxeqq.mongodb.net:27017,cluster0-shard-00-01-jxeqq.mongodb.net:27017,cluster0-shard-00-02-jxeqq.mongodb.net:27017
2020-02-25T10:47:01.787+0000 I NETWORK  [thread1] Successfully connected to cluster0-shard-00-02-jxeqq.mongodb.net:27017 (1 connections now open to cluster0-shard-00-02-jxeqq.mongodb.net:27017 with a 5 second timeout)
2020-02-25T10:47:01.788+0000 I NETWORK  [ReplicaSetMonitor-TaskExecutor-0] Successfully connected to cluster0-shard-00-00-jxeqq.mongodb.net:27017 (1 connections now open to cluster0-shard-00-00-jxeqq.mongodb.net:27017 with a 5 second timeout)
2020-02-25T10:47:02.358+0000 I NETWORK  [ReplicaSetMonitor-TaskExecutor-0] Successfully connected to cluster0-shard-00-01-jxeqq.mongodb.net:27017 (1 connections now open to cluster0-shard-00-01-jxeqq.mongodb.net:27017 with a 5 second timeout)
Implicit session: session { "id" : UUID("e46722c5-10c4-41c2-8f46-c61ac1a6ae24") }
MongoDB server version: 3.6.17
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Ahora que estoy conectado a un replica set, y puede ver aquí desde el mensaje que estoy conectado a una réplica M103 y a su primario.

Si uso local y muestro las colecciones, entonces aquí tenemos un poco más de información, o al menos un poco más de colecciones en este escenario.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use local
switched to db local
MongoDB Enterprise Cluster0-shard-0:PRIMARY> show collections
clustermanager
me
oplog.rs
replset.election
replset.minvalid
replset.oplogTruncateAfterPoint
startup_log
system.replset
system.rollback.id
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Genial, pero ¿para qué son estos?

<img src="images/m103/c2/2-12-collections.png">

La mayoría de estas colecciones, como `me`, `startup_log`, `system.replset`, `system.rollback.id`, o `replset.election`, y  `replset.minvalid` son colecciones mantenidas internamente por el servidor.

Realmente no varían tanto, y la información que contienen son datos de configuración simples, nada demasiado interesante allí.

Pero donde las cosas se ponen realmente interesantes es con una colección en particular: `oplog.rs`.

**`oplog.rs` es el punto central de nuestro mecanismo de replicación.**

Esta es la colección de `oplog` que hará un seguimiento de todas las declaraciones que se replican en nuestro conjunto de réplicas.

Cada pieza de información y operaciones que necesitan ser replicadas se registrarán en esta colección.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.oplog.rs.find().pretty()
{
	"ts" : Timestamp(1560827583, 1),
	"t" : NumberLong(57),
	"h" : NumberLong("-5595341604350583338"),
	"v" : 2,
	"op" : "n",
	"ns" : "",
	"wall" : ISODate("2019-06-18T03:13:03.301Z"),
	"o" : {
		"msg" : "periodic noop"
	}
}
{
	"ts" : Timestamp(1560827593, 1),
	"t" : NumberLong(57),
	"h" : NumberLong("2636313012311016051"),
	"v" : 2,
	"op" : "n",
	"ns" : "",
	"wall" : ISODate("2019-06-18T03:13:13.301Z"),
	"o" : {
		"msg" : "periodic noop"
	}
}
...
```

Hay algunas cosas sobre la colección `oplog.rs` que debes conocer.

En primer lugar, es una colección capada.

<img src="images/m103/c2/2-12-capped.png">

**Capada significa que el tamaño de esta colección está limitado a un tamaño específico.**

Si recopilamos las estadísticas de nuestra colección `oplog.rs` en esta variable `stats`, hay una bandera llamada `.capped` que nos dirá que esta colección está, de hecho, limitada.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> var stats = db.oplog.rs.stats()
MongoDB Enterprise Cluster0-shard-0:PRIMARY> stats.capped
true
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Puedes ver el tamaño de esta colección.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> stats.size
1856842408
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

También podemos ver el tamaño máximo (`maxSize`) de la colección particular.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> stats.maxSize
NumberLong(1871434496)
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Ahora, si desea ver las estadísticas en una unidad de megabytes, puede ver que esta colección aquí contiene casi 2 gigabytes de datos: 1.8 gigabytes.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> var stats = db.oplog.rs.stats(1024*1024)
MongoDB Enterprise Cluster0-shard-0:PRIMARY> stats.maxSize
1784
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Por defecto, la colección `oplog.rs` ocupará el 5% de su disco libre.

En mi caso, tengo casi 33 gigabytes de datos disponibles.

```sh
vagrant@m103:~$ df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            996M   12K  996M   1% /dev
tmpfs           201M  364K  200M   1% /run
/dev/sda1        40G  5.0G   33G  14% /
none            4.0K     0  4.0K   0% /sys/fs/cgroup
none            5.0M     0  5.0M   0% /run/lock
none           1001M     0 1001M   0% /run/shm
none            100M     0  100M   0% /run/user
none            932G  686G  246G  74% /shared
none            932G  686G  246G  74% /dataset
none            932G  686G  246G  74% /vagrant
vagrant@m103:~$ 
```

Así que 1,8 gigabytes me dan ese 5%.

El tamaño de nuestro registro de operaciones determinará nuestra ventana de replicación: la cantidad de tiempo que llevará completar su registro de operaciones.

Ahora, también podemos ver parte de esa información en `printReplicationInfo`, donde aquí, que proporciona el estado actual de mi oplog.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> rs.printReplicationInfo()
configured oplog size:   1784.739013671875MB
log length start to end: 21801542secs (6055.98hrs)
oplog first event time:  Tue Jun 18 2019 03:13:03 GMT+0000 (UTC)
oplog last event time:   Tue Feb 25 2020 11:12:05 GMT+0000 (UTC)
now:                     Tue Feb 25 2020 11:12:07 GMT+0000 (UTC)
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Estamos configurados para esos 1,784 megabytes.

La longitud del registro comienza a terminar es de aproximadamente 21801542 segundos, 6055.98 horas, una muy larga.

Y se calcula en función de la hora del primer evento y del último evento.

Ahora, como pueden ver, no he hecho mucho hasta ahora.

Y a partir de ahora, donde estamos ahora en este momento, veo que no he hecho mucho en este replica set en particular.

Por lo tanto, el tiempo calculado para completar el oplog es relativamente bajo.

Pero espera solo un segundo.

Pensé que el tamaño del oplog se midió en tamaño, por lo tanto, es megabytes, pero ¿estamos hablando de horas?

No temas, joven padawan.

Lo que está sucediendo aquí es que MongoDB le informará cuánto tiempo llevará, dada la carga de trabajo actual, cuánto tiempo llevará comenzar a sobrescribir las entradas en su oplog (registro de operaciones).

En este caso, no he estado haciendo mucho, por lo tanto, mi carga de trabajo es bastante baja.

Por lo tanto, solo tomará aproximadamente 0.1 horas llenar el oplog(registro de operaciones), lo que no es realmente predecible en este momento.

Necesitamos más datos para calcular correctamente cuánto tiempo se tarda en llenar nuestro oplog(registro de operaciones).

## 13. Tema: DB local: Parte 2

### Notas de lectura

Instrucciones de lectura

Crear nuevo namespace `m103.messages`:

```sh
use m103
db.createCollection('messages')
```

Consulta el oplog, filtra los latidos del corazón (`"periodic noop"`) y solo devuelve la última entrada:

```sh
use local
db.oplog.rs.find( { "o.msg": { $ne: "periodic noop" } } ).sort( { $natural: -1 } ).limit(1).pretty()
``` 

Insertar 100 documentos diferentes:

```sh
use m103
for ( i=0; i< 100; i++) { db.messages.insert( { 'msg': 'not yet', _id: i } ) }
db.messages.count()
```

Consultando el oplog para encontrar todas las operaciones relacionadas con `m103.messages`:

```sh
use local
db.oplog.rs.find({"ns": "m103.messages"}).sort({$natural: -1})
```

Ilustrando que una declaración de actualización puede generar muchas entradas en el oplog:

```sh
use m103
db.messages.updateMany( {}, { $set: { author: 'norberto' } } )
use local
db.oplog.rs.find( { "ns": "m103.messages" } ).sort( { $natural: -1 } )
```

Remember, even though you can write data to the local db, you should not.

Recuerde, aunque **puede** escribir datos en la base de datos `local`, no debe hacerlo.

### Transcripción

Después de iniciar nuestro nodo y agregar el nodo al replica set, se crea la colección `oplog.rs`.

<img src="images/m103/c2/2-13-oplog.png">

Por defecto, como mencioné antes, tomamos el 5% del disco disponible.

<img src="images/m103/c2/2-13-oplog-2.png">

Pero este valor también se puede configurar configurándolo a través del tamaño del registro en megabytes en la sección de replicación de nuestro archivo de configuración.

A medida que las operaciones se registran en el oplog, como insertar o eliminar o crear operaciones de tipo de colección, la colección `oplog.rs` comienza a acumular las operaciones y declaraciones, hasta que alcanza el límite de tamaño de oplog.

<img src="images/m103/c2/2-13-oplog-3.png">

Una vez que eso sucede, las primeras operaciones en nuestro oplog comienzan a sobrescribirse con las operaciones más nuevas.

<img src="images/m103/c2/2-13-oplog-4.png">

El tiempo que lleva completar completamente nuestro oplog y comenzar a reescribir las primeras declaraciones determina la ventana de replicación.

<img src="images/m103/c2/2-13-oplog-5.png">

La ventana de replicación es un aspecto importante para monitorear porque afectaremos la cantidad de tiempo que el conjunto de réplica puede permitir que un nodo esté inactivo sin requerir intervención humana para la recuperación automática.

Cada nodo en nuestro replica set tiene su propio oplog.

<img src="images/m103/c2/2-13-replicaset.png">

A medida que las escrituras y las operaciones llegan al nodo primario, se capturan en el oplog.

<img src="images/m103/c2/2-13-replicaset-2.png">

Y luego los nodos secundarios replican esos datos y los aplican en su propio oplog.

<img src="images/m103/c2/2-13-replicaset-3.png">

Ahora, si por alguna razón uno de los nodos se desconecta, ya sea porque van a realizar algún mantenimiento, o hay algún tipo de problema de red, o algún tiempo de inactividad del servidor de cualquier tipo,

<img src="images/m103/c2/2-13-replicaset-4.png">

 y el servidor sigue funcionando, 
 
 <img src="images/m103/c2/2-13-replicaset-5.png">
 
 el replica set continúa acumulándose nuevas escrituras: para que el servidor pueda ponerse al día con los nodos restantes, deberá descubrir qué es un punto común en el que todos pueden ver lo que sucedió en el pasado.

<img src="images/m103/c2/2-13-replicaset-6.png">


Básicamente, lo que sucederá es que un nodo en recuperación verificará su última entrada de oplog e intentará encontrarlo en uno de los nodos disponibles.

<img src="images/m103/c2/2-13-replicaset-7.png">

Una vez que lo encuentre, simplemente volverá a aplicar todas las operaciones desde ese punto y se pondrá al día con los nodos restantes de los conjuntos.

<img src="images/m103/c2/2-13-replicaset-8.png">

Sin embargo, si no puede encontrar la misma operación común en el oplog de los nodos restantes debido a que el oplog ya ha girado y no hay un punto fuente de sincronización como la última operación,

<img src="images/m103/c2/2-13-replicaset-9.png">

el nodo no podrá recuperarse y sincronizarse automáticamente con el resto de el conjunto, se coloca en modo de recuperación.

<img src="images/m103/c2/2-13-replicaset-10.png">

Ahora, no todos los nodos deben ser iguales.

<img src="images/m103/c2/2-13-replicaset-11.png">

Por ejemplo, las fuentes de sincronización pueden tener diferentes tamaños de registro.

Sin embargo, si nuestro tamaño de registro es mayor y puede acomodar más cambios en el sistema, podemos permitir que nuestros nodos estén inactivos por más tiempo y aún así poder volver a conectarse una vez que se vuelvan a activar.

<img src="images/m103/c2/2-13-replicaset-12.png">

Por lo tanto, el tamaño de nuestra colección `oplog.rs` es un aspecto importante a tener en cuenta.

En resumen, la ventana de replicación medida en horas será proporcional a la carga de su sistema.

<img src="images/m103/c2/2-13-oplog-6.png">

Deberías vigilar eso.

La otra cosa buena es que nuestro tamaño de `oplog.rs` se puede cambiar.

<img src="images/m103/c2/2-13-oplog-7.png">

Y tenemos una buena cantidad de buena documentación que le indica cómo hacerlo como tarea administrativa.

Otro aspecto a conocer sobre esta colección es que, dada la naturaleza idempotente de las instrucciones, una única actualización puede dar lugar a varias operaciones diferentes en esta colección.

Déjame mostrarte cómo funciona esto.

```sh
192:~ adolfodelarosa$ mongo "mongodb+srv://cluster0-3bh0e.mongodb.net/test"  --username m001-student --password m001-mongodb-basics
```

Voy a usar esta base de datos `m103`.

Voy a crear una colección llamada `'messages'`.

Una vez que creo eso, puedo ver esa colección allí creada.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use m103
switched to db m103
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.createCollection('messages')
{
	"ok" : 1,
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582740039, 1),
		"signature" : {
			"hash" : BinData(0,"Rs265mL8LZYWwZILKyDpwvC/IiM="),
			"keyId" : NumberLong("6778805621249540098")
		}
	},
	"operationTime" : Timestamp(1582740039, 1)
}
MongoDB Enterprise Cluster0-shard-0:PRIMARY> show collections
messages
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Ahora, si cambio a mi base de datos `local` y busco en nuestros `oplog.rs`, excluyendo cualquier operación `"periodic noop"` mantenida por el servidor, puedo encontrar aquí las instrucciones que crean esta colección en el oplog.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use local
switched to db local                         
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.oplog.rs.find({"o.msg": {$ne: "periodic noop"}})
                                                        .sort({$natural: -1}).limit(1).pretty()
{
	"ts" : Timestamp(1582740039, 1),
	"t" : NumberLong(6),
	"h" : NumberLong(0),
	"v" : 2,
	"op" : "c",
	"ns" : "m103.$cmd",
	"ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"),
	"wall" : ISODate("2020-02-26T18:00:39.361Z"),
	"o" : {
		"create" : "messages",
		"idIndex" : {
			"v" : 2,
			"key" : {
				"_id" : 1
			},
			"name" : "_id_",
			"ns" : "m103.messages"
		}
	}
}
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Genial, esto es realmente bueno.

Ahora volvamos a nuestra BD `m103`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use m103
switched to db m103
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

E insertemos algunos documentos solo por diversión.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> for ( i=0; i < 100; i++) { db.messages.insert({'msg': 'not yet', _id: i}) }
WriteResult({ "nInserted" : 1 })
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Ahí tienes.

Insertamos 100 documentos con el mensaje, todavía no.

Si sigo adelante y los cuento, puedo ver 100 documentos allí, genial.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.messages.count()
100
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Si vuelvo a mi base de datos local y busco esos mensajes, puedo ver que puedo encontrar esos inserts: la operación allí es un insert `"op" : "i"`.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use local
switched to db local
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.oplog.rs.find({"ns": "m103.messages"}).sort({$natural: -1 })
{ "ts" : Timestamp(1582740791, 19), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.687Z"), "o" : { "_id" : 99, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 18), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.650Z"), "o" : { "_id" : 98, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 17), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.613Z"), "o" : { "_id" : 97, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 16), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.576Z"), "o" : { "_id" : 96, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 15), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.539Z"), "o" : { "_id" : 95, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 14), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.500Z"), "o" : { "_id" : 94, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 13), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.463Z"), "o" : { "_id" : 93, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 12), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.426Z"), "o" : { "_id" : 92, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 11), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.388Z"), "o" : { "_id" : 91, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 10), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.351Z"), "o" : { "_id" : 90, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 9), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.314Z"), "o" : { "_id" : 89, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 8), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.277Z"), "o" : { "_id" : 88, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 7), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.240Z"), "o" : { "_id" : 87, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 6), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.203Z"), "o" : { "_id" : 86, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 5), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.166Z"), "o" : { "_id" : 85, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 4), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.129Z"), "o" : { "_id" : 84, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 3), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.092Z"), "o" : { "_id" : 83, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 2), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.055Z"), "o" : { "_id" : 82, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740791, 1), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:11.018Z"), "o" : { "_id" : 81, "msg" : "not yet" } }
{ "ts" : Timestamp(1582740790, 26), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "i", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "wall" : ISODate("2020-02-26T18:13:10.980Z"), "o" : { "_id" : 80, "msg" : "not yet" } }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

```

Y el objeto que se inserta es uno de los 100 documentos insertados, genial.

Pero ahora hagamos una simple actualización Muchas operaciones.

Digamos que quiero asegurarme de que estos mensajes aquí tengan un autor.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use m103
switched to db m103
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.messages.updateMany({}, {$set: {author: 'adolfo'}})
{ "acknowledged" : true, "matchedCount" : 100, "modifiedCount" : 100 }
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 
```

Así que voy a establecer un nuevo campo llamado autor con el valor llamado, bueno, mi propio nombre: Adolfo.

Una vez que haga esto, y tenga en cuenta que esta es una sola operación, una `UpdateOne`, que modificó y combinó 100 documentos diferentes.

Si volvemos a lo `local`, y si buscamos más operaciones, puedo ver que hay una operación de actualización, o varias operaciones de actualización - `"op" : "u"` significa una actualización - para todos los documentos afectados en esta colección.

```sh
MongoDB Enterprise Cluster0-shard-0:PRIMARY> use local
switched to db local
MongoDB Enterprise Cluster0-shard-0:PRIMARY> db.oplog.rs.find({"ns": "m103.messages"}).sort({$natural: -1 })
{ "ts" : Timestamp(1582741291, 100), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 99 }, "wall" : ISODate("2020-02-26T18:21:31.559Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 99), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 98 }, "wall" : ISODate("2020-02-26T18:21:31.559Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 98), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 97 }, "wall" : ISODate("2020-02-26T18:21:31.559Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 97), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 96 }, "wall" : ISODate("2020-02-26T18:21:31.559Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 96), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 95 }, "wall" : ISODate("2020-02-26T18:21:31.559Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 95), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 94 }, "wall" : ISODate("2020-02-26T18:21:31.559Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 94), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 93 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 93), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 92 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 92), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 91 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 91), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 90 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 90), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 89 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 89), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 88 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 88), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 87 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 87), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 86 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 86), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 85 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 85), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 84 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 84), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 83 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 83), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 82 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 82), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 81 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
{ "ts" : Timestamp(1582741291, 81), "t" : NumberLong(6), "h" : NumberLong(0), "v" : 2, "op" : "u", "ns" : "m103.messages", "ui" : UUID("54206d01-59ed-4de1-858f-783879728e90"), "o2" : { "_id" : 80 }, "wall" : ISODate("2020-02-26T18:21:31.558Z"), "o" : { "$v" : 1, "$set" : { "author" : "adolfo" } } }
Type "it" for more
MongoDB Enterprise Cluster0-shard-0:PRIMARY> 

```

Entonces, una sola instrucción, - un `updateMany` -, en nuestro nodo primario, produjo 100 operaciones diferentes en nuestro oplog.

Ahora esta es la magia de la **idempotencia**.

Se consciente de esto.

<img src="images/m103/c2/2-13-oplog-8.png">

A veces es fácil descartar el hecho de que la idempotencia podría generar muchas más operaciones en nuestro registro de operaciones que la cantidad de comandos realmente emitidos por el cliente.

Un último punto que quiero llamar su atención es que, por favor, no cambie ninguna de esta información presente en ninguna de esta colección.

<img src="images/m103/c2/2-13-collections.png">

Contrariamente a lo que MC Hammer solía decir, de hecho, puede tocar esto, dado el conjunto correcto de permisos.

Pero por favor no lo hagas.

Esto podría afectar enormemente la forma en que funciona su réplica.

Así que trata de no perder el tiempo con esta colección.

Dicho esto, y para demostrarle que puede hacer algún daño, intentemos escribir en esta base de datos local.

Voy a seguir adelante y en mi colección local, inserte un mensaje que diga que no puede tocar esto.

<img src="images/m103/c2/2-13-code-1.png">

Y si lo buscamos en nuestros `oplog.rs`, no lo encontraremos.

<img src="images/m103/c2/2-13-code-2.png">

Debe tener en cuenta que estos datos que acabo de escribir en esta base de datos en particular, -- ahí está, `mylocalcollection` --, están escritos en la base de datos local.

<img src="images/m103/c2/2-13-code-3.png">

Eso significa que es local para esta instancia en particular.

Los datos escritos en esta colección no se replicarán.

La base de datos local es como Las Vegas.

Lo que sucede en local, se queda en local.

Ningún otro nodo en el conjunto verá esos datos, excepto obviamente por los `oplog.rs`, que están leyendo y aplicándolos por su cuenta.

Cualquier otra cosa que desee mantener local, puede escribir aquí.

Pero no le recomendamos que haga nada de eso a menos que realmente sepa lo que está haciendo.

Resumamos rápidamente lo que acabamos de aprender.

<img src="images/m103/c2/2-13-resumen.png">

La base de datos local contiene información muy importante y no debe ser desordenada.

Cambiar los datos en su oplog o en cualquiera de las colecciones de configuración afectará la configuración en el mecanismo de replicación.

`oplog.rs` es fundamental para nuestro mecanismo de replicación.

Todo lo que necesita ser replicado será almacenado y registrado en el blog de manera idempotente.

El tamaño de nuestro oplog afectará la ventana de replicación y debe ser monitoreado de cerca.

Cualquier dato escrito en la base de datos local que no esté escrito en `oplog.rs` o que cambie cualquiera de las colecciones de configuración del sistema permanecerá allí y no se replicará.

## 14. Examen Local DB: Part 2


**Problem:**

Which of the following is true?

Check all answers that apply:

* The `oplog.rs` collection contains all operations that will be replicated. :+1:

* You cannot write to the `local` database.

* The `local` database does not allow the creation of other collections.

* We should drop the `oplog.rs` collection from time to time to avoid it becoming too big.

* The local database will not be replicated. :+1:

**Correct answers:**

**The** local **database will not be replicated**.

Any data written to this database will not be replicated across the different nodes of the set.

**The `oplog.rs` collection contains all operations that will be replicated**.

The `oplog.rs` collection holds all the statements that get replicated across the different replica set members.

**Incorrect answers:**

**You cannot write to the `local` database.**

Given the correct permissions, an authorized user can write data to the `local` db. That said, we strongly advise against that.

**The `local` database does not allow the creation of other collections.**

Although we discourage it, you can write new collections to the local database.

**We should drop the `oplog.rs` collection from time to time to avoid it becoming too big.**

We cap the `oplog.rs` collection instead of dropping it entirely.

## 15. Tema: reconfigurar un conjunto de réplicas en ejecución

### Notas de lectura

Instrucciones de lectura

*Nota: En el video de este tema, utilizamos el antiguo nombre de host* `"m103.mongodb.university"` *que se ha cambiado a * `"m103"`. *Hemos actualizado todos los siguientes comandos en consecuencia*.

`node4.conf:`

```sh
storage:
  dbPath: /var/mongodb/db/node4
net:
  bindIp: 192.168.103.100,localhost
  port: 27014
systemLog:
  destination: file
  path: /var/mongodb/db/node4/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

`arbiter.conf:`

```sh
storage:
  dbPath: /var/mongodb/db/arbiter
net:
  bindIp: 192.168.103.100,localhost
  port: 28000
systemLog:
  destination: file
  path: /var/mongodb/db/arbiter/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

Inicio de procesos mongod para nuestro cuarto nodo y árbitro:

```sh
mongod -f node4.conf
mongod -f arbiter.conf
```

Desde el Mongo Shell del replica set, agregando el nuevo secundario y el nuevo árbitro:

```sh
rs.add("m103:27014")
rs.addArb("m103:28000")
```

Verificación del replica set después de agregar dos nuevos nodos:

```sh
rs.isMaster()
```

Eliminando el árbitro de nuestro replica set:

```sh
rs.remove("m103:28000")
```

Asignando la configuración actual a una variable de shell que podemos editar, para reconfigurar el conjunto de réplicas:

```sh
cfg = rs.conf()
```

Edición de nuestra nueva variable `cfg` para cambiar la topología, específicamente, modificando `cfg.members`:

```sh
cfg.members[3].votes = 0
cfg.members[3].hidden = true
cfg.members[3].priority = 0
```

Actualización de nuestro replica set para usar la nueva configuración `cfg`:

```sh
rs.reconfig(cfg)
```

### Transcripción

Muy bien, hasta este punto, hemos cubierto conceptos básicos de replicación y cómo implementar un replica set.

Pero en esta lección, vamos a cubrir cómo reconfigurar un replica set mientras aún se está ejecutando.

Entonces, supongamos que nuestro replica set que contiene nuestros datos está funcionando con un nodo primario y dos nodos secundarios.

<img src="images/m103/c2/2-15-replicaset.png">

Nuestro supervisor nos dice que quiere agregar dos nodos más, uno secundario y un árbitro.

<img src="images/m103/c2/2-15-replicaset-2.png">

Esta será la tipología de nuestro replica set una vez que hayamos agregado nuestros dos nodos.

Y nuestro supervisor quería agregar un secundario y un árbitro en lugar de solo dos secundarios, porque un árbitro es un nodo mucho más barato de mantener.

No necesita copiar o replicar todos los datos del primario, pero aún así nos proporciona un número impar de números de votación en el conjunto.

Así que aquí estoy conectado a nuestro replica set, 

```sh
vagrant@m103:~$ mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
...
MongoDB Enterprise m103-example:PRIMARY> 
```

y solo voy a `rs.isMaster` para ver esa tipología realmente rápido.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013"
	],
	"setName" : "m103-example",
	"setVersion" : 3,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "m103:27012",
	"me" : "m103:27012",
	"electionId" : ObjectId("7fffffff0000000000000002"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582819021, 1),
			"t" : NumberLong(2)
		},
		"lastWriteDate" : ISODate("2020-02-27T15:57:01Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582819021, 1),
			"t" : NumberLong(2)
		},
		"majorityWriteDate" : ISODate("2020-02-27T15:57:01Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-27T15:57:10.513Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582819021, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582819021, 1),
		"signature" : {
			"hash" : BinData(0,"5dP2I6LYsArrA/XcK2LWRV+1CZk="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Y podemos ver que nuestro replica set actualmente tiene tres nodos.

Para añadir el cuarto nodo:

Copiamos la configuración del nodo 3 a l nodo 4:

```sh
vagrant@m103:~$ cp node3.conf node4.conf
```

Entonces, editamos el archivo `node4.conf`:

```sh
vagrant@m103:~$ vi node4.conf

storage:
  dbPath: /var/mongodb/db/node4
net:
  bindIp: 192.168.103.100,localhost
  port: 27014
systemLog:
  destination: file
  path: /var/mongodb/db/node4/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

Guardar el archivo y salir de vi:

```sh
:wq
```

Debemos crear la carpeta para `node4` y dar los permisos necesarios:

```sh
vagrant@m103:~$ mkdir /var/mongodb/db/node4

vagrant@m103:~$ sudo chown vagrant:vagrant /var/mongodb/db/node4
```

Así que aquí voy a lanzar un mongod usando el archivo de configuración para nuestro cuarto nodo.

```sh
vagrant@m103:~$ mongod -f node4.conf
about to fork child process, waiting until server is ready for connections.
forked process: 15236
child process started successfully, parent exiting
vagrant@m103:~$ 
```

Para añadir el nodo arbotro:

Copiamos la configuración del nodo 4 al nodo arbitro:

```sh
vagrant@m103:~$ cp node4.conf arbiter.conf
```

Entonces, editamos el archivo `arbiter.conf`:

```sh
vagrant@m103:~$ vi arbiter.conf

storage:
  dbPath: /var/mongodb/db/arbiter
net:
  bindIp: 192.168.103.100,localhost
  port: 28000
systemLog:
  destination: file
  path: /var/mongodb/db/arbiter/mongod.log
  logAppend: true
processManagement:
  fork: true
replication:
  replSetName: m103-example
```

Guardar el archivo y salir de vi:

```sh
:wq
```

Debemos crear la carpeta para `arbiter` y dar los permisos necesarios:

```sh
vagrant@m103:~$ mkdir /var/mongodb/db/arbiter

vagrant@m103:~$ sudo chown vagrant:vagrant /var/mongodb/db/arbiter
```

Entonces, una vez que comenzamos nuestro nodo4, podemos iniciar nuestro árbitro.

```sh
vagrant@m103:~$ mongod -f arbiter.conf
about to fork child process, waiting until server is ready for connections.
forked process: 15306
child process started successfully, parent exiting
vagrant@m103:~$ 
```

Y ahora tenemos los dos nodos que necesitamos.

Y solo tenemos que agregarlos al replica set, entramos a nuestro Mongo Shell.

```sh
vagrant@m103:~$ mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
....
MongoDB Enterprise m103-example:PRIMARY> 
```

Así que aquí solo estamos agregando el cuarto nodo a nuestro conjunto.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.add("m103:27014")
{
	"ok" : 1,
	"operationTime" : Timestamp(1582820887, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582820887, 1),
		"signature" : {
			"hash" : BinData(0,"qXjueg5mSoSh69J90BSnELmjc3Q="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Entonces, cuando agregamos nuestro árbitro, usamos un comando especial llamado `addArb`.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.addArb("m103:28000")
{
	"ok" : 1,
	"operationTime" : Timestamp(1582820960, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582820960, 1),
		"signature" : {
			"hash" : BinData(0,"kCABCim+zcVWA7VXd5+iS9zE0LQ="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Y parece que esto se completó.

Y solo voy a verificar `rs.isMaster` para asegurarme de eso.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013",
		"m103:27014"
	],
	"arbiters" : [
		"m103:28000"
	],
	"setName" : "m103-example",
	"setVersion" : 5,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "m103:27012",
	"me" : "m103:27012",
	"electionId" : ObjectId("7fffffff0000000000000002"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582821011, 1),
			"t" : NumberLong(2)
		},
		"lastWriteDate" : ISODate("2020-02-27T16:30:11Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582821011, 1),
			"t" : NumberLong(2)
		},
		"majorityWriteDate" : ISODate("2020-02-27T16:30:11Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-27T16:30:16.160Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582821011, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582821011, 1),
		"signature" : {
			"hash" : BinData(0,"BAImHUTmbbKTiNSRa6RIgZvKr5w="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Y podemos ver que nuestro replica set ahora tiene cuatro nodos y un árbitro.

<img src="images/m103/c2/2-15-replicaset-2.png">

Entonces, nuestro replica set se ejecuta con un nodo primario, tres secundarios y un nodo árbitro.

Pero nuestro jefe de ingeniería simplemente nos dijo que tenemos que matar el nodo árbitro porque no tenemos el presupuesto para ello.

Entonces, usamos el comando `remove`, simplemente pasamos el puerto donde se ejecutaba nuestro árbitro.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.remove("m103:28000")
{
	"ok" : 1,
	"operationTime" : Timestamp(1582821297, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582821297, 1),
		"signature" : {
			"hash" : BinData(0,"3wMb32vRLU3AkBwgKEhSSS2kTG0="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Y lo hemos eliminado con éxito.

Sin embargo, en este momento nuestro replica set está configurada solo tiene cuatro miembros.

Podemos verificar eso desde `rs.isMaster`.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013",
		"m103:27014"
	],
	"setName" : "m103-example",
	"setVersion" : 6,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "m103:27012",
	"me" : "m103:27012",
	"electionId" : ObjectId("7fffffff0000000000000002"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582821331, 1),
			"t" : NumberLong(2)
		},
		"lastWriteDate" : ISODate("2020-02-27T16:35:31Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582821331, 1),
			"t" : NumberLong(2)
		},
		"majorityWriteDate" : ISODate("2020-02-27T16:35:31Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-27T16:35:36.127Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582821331, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582821331, 1),
		"signature" : {
			"hash" : BinData(0,"qvF7DOdvAFejHtoSQik5H8l1Gzo="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 

```

Nuestra lista de hosts solo tiene cuatro nodos.

<img src="images/m103/c2/2-15-replicaset-3.png">

Entonces, para remediar el problema de tener un número par de miembros con derecho a voto en el conjunto, no tenemos que eliminar nuestro secundario por completo.

Solo tenemos que revocar sus privilegios de votación para que nos deje con tres miembros con derecho a voto.

Nuestro jefe de ingeniería también ha estado hablando sobre el uso de un nodo oculto para almacenar copias de seguridad.

Entonces decidimos ser un poco inteligentes.

Además de no ser votante, este secundario también será un nodo oculto.

<img src="images/m103/c2/2-15-replicaset-4.png">

Por lo tanto, podemos reconfigurar este nodo para que esté oculto y sin votación sin eliminarlo o reiniciar el nodo.

Usamos nuestro `rs.conf` para recuperar una configuración de replica set.


```sh
MongoDB Enterprise m103-example:PRIMARY> rs.conf()
{
	"_id" : "m103-example",
	"version" : 6,
	"protocolVersion" : NumberLong(1),
	"members" : [
		{
			"_id" : 0,
			"host" : "192.168.103.100:27011",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 1,
			"host" : "m103:27012",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 2,
			"host" : "m103:27013",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 3,
			"host" : "m103:27014",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		}
	],
	"settings" : {
		"chainingAllowed" : true,
		"heartbeatIntervalMillis" : 2000,
		"heartbeatTimeoutSecs" : 10,
		"electionTimeoutMillis" : 10000,
		"catchUpTimeoutMillis" : -1,
		"catchUpTakeoverDelayMillis" : 30000,
		"getLastErrorModes" : {
			
		},
		"getLastErrorDefaults" : {
			"w" : 1,
			"wtimeout" : 0
		},
		"replicaSetId" : ObjectId("5e54102d7969353676a7b43b")
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Esto nos da una configuración completa del replica set, incluido el nombre de host y el puerto de cada nodo, así como si el nodo está oculto o es un árbitro.

También nos da la cantidad de votos que tiene cada nodo.

Así que aquí solo estoy almacenando la configuración para el conjunto en una variable llamada `cfg`.

```sh
MongoDB Enterprise m103-example:PRIMARY> cfg = rs.conf()
{
	"_id" : "m103-example",
	"version" : 6,
	"protocolVersion" : NumberLong(1),
	"members" : [
		{
			"_id" : 0,
			"host" : "192.168.103.100:27011",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 1,
			"host" : "m103:27012",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 2,
			"host" : "m103:27013",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 3,
			"host" : "m103:27014",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		}
	],
	"settings" : {
		"chainingAllowed" : true,
		"heartbeatIntervalMillis" : 2000,
		"heartbeatTimeoutSecs" : 10,
		"electionTimeoutMillis" : 10000,
		"catchUpTimeoutMillis" : -1,
		"catchUpTakeoverDelayMillis" : 30000,
		"getLastErrorModes" : {
			
		},
		"getLastErrorDefaults" : {
			"w" : 1,
			"wtimeout" : 0
		},
		"replicaSetId" : ObjectId("5e54102d7969353676a7b43b")
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Entonces, los miembros aquí son una sección de la configuración que tiene una lista de los nodos en el conjunto.

Y he elegido el nodo en la posición de índice 3 en esa lista.

Y he cambiado el número de votos que tiene a 0.

```sh
MongoDB Enterprise m103-example:PRIMARY> cfg.members[3].votes = 0
0
MongoDB Enterprise m103-example:PRIMARY> 
```

Esto nos dejará con un número impar de miembros votantes en el set.

Y aquí estoy configurando la variable oculta de ese nodo en true para ocultar esta nota de cualquier aplicación cliente.

```sh
MongoDB Enterprise m103-example:PRIMARY> cfg.members[3].hidden = true
true
MongoDB Enterprise m103-example:PRIMARY> 
```

Y **los nodos ocultos nunca pueden convertirse en primarios**, por lo que para que este sea un nodo oculto, debemos establecer la prioridad en 0.

```sh
MongoDB Enterprise m103-example:PRIMARY> cfg.members[3].priority = 0
0
MongoDB Enterprise m103-example:PRIMARY> 
```

Entonces, este es un nuevo comando, `rs.reconfig`, que usamos para reconfigurar un conjunto de réplicas en ejecución.

Pasamos nuestra configuración actualizada como argumento, y necesitamos especificar todo este documento.

Es por eso que tomamos una copia de la configuración original y luego la modificamos según lo necesitábamos.

Tenga en cuenta que la reconfiguración podría desencadenar una elección dependiendo de lo que haya en la nueva configuración.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.reconfig(cfg)
{
	"ok" : 1,
	"operationTime" : Timestamp(1582822346, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582822346, 1),
		"signature" : {
			"hash" : BinData(0,"nXLrvTSHeIi9AtjLS/st2/oiy4I="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 

```

Y parecía que esto funcionaba.

Solo voy a verificar que funcionó con `rs.conf` para obtener la configuración actual del replica set.


```sh
MongoDB Enterprise m103-example:PRIMARY> rs.conf()
{
	"_id" : "m103-example",
	"version" : 7,
	"protocolVersion" : NumberLong(1),
	"members" : [
		{
			"_id" : 0,
			"host" : "192.168.103.100:27011",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 1,
			"host" : "m103:27012",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 2,
			"host" : "m103:27013",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : false,
			"priority" : 1,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 1
		},
		{
			"_id" : 3,
			"host" : "m103:27014",
			"arbiterOnly" : false,
			"buildIndexes" : true,
			"hidden" : true,
			"priority" : 0,
			"tags" : {
				
			},
			"slaveDelay" : NumberLong(0),
			"votes" : 0
		}
	],
	"settings" : {
		"chainingAllowed" : true,
		"heartbeatIntervalMillis" : 2000,
		"heartbeatTimeoutSecs" : 10,
		"electionTimeoutMillis" : 10000,
		"catchUpTimeoutMillis" : -1,
		"catchUpTakeoverDelayMillis" : 30000,
		"getLastErrorModes" : {
			
		},
		"getLastErrorDefaults" : {
			"w" : 1,
			"wtimeout" : 0
		},
		"replicaSetId" : ObjectId("5e54102d7969353676a7b43b")
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Y parece que el nodo 4 ahora tiene prioridad 0, no puede votar y está oculto.

Entonces, nuestro replica set todavía tiene cuatro nodos, que es un número par, pero solo un número impar de ellos puede votar.

<img src="images/m103/c2/2-15-replicaset-5.png">

Entonces, para recapitular, en esta lección cubrimos cómo agregar árbitros y nuevos secundarios, dijimos un poco sobre nodos ocultos y agregamos uno a nuestro conjunto.

<img src="images/m103/c2/2-15-resumen.png">

Y también cubrimos cómo reconfigurar un replica set mientras aún se está ejecutando.

## 16. Examen Reconfiguring a Running Replica Set

**Problem:**

What is true about hidden nodes?

Check all answers that apply:

* Hidden nodes are a type of arbiter.

* Hidden nodes can become primary.

* Hidden nodes vote in elections. :+1:

* Hidden nodes replicate data. :+1:

* Secondary nodes cannot become hidden nodes without going offline.

**Correct answers:**

**Hidden nodes vote in elections.**

Hidden nodes can never become the primary, but they can still vote in elections.

**Hidden nodes replicate data.**

Hidden nodes replicate data from the primary, although they are invisible to client applications.

**Incorrect answers:**

**Hidden nodes can become primary.**

Hidden nodes must have priority 0.

**Hidden nodes are a type of arbiter.**

Hidden nodes and arbiters are different types of replica set members; arbiters hold no data while hidden nodes replicate from the oplog.

**Secondary nodes must be restarted before converting to hidden nodes.**

Secondary nodes can be converted to hidden nodes while the replica set is running, using rs.reconfig().

## 17. Laboratorio: eliminar y volver a agregar un nodo

Lab - Remove and Re-Add a Node

**Problem:**

In this lab, you will make your replica set more flexible. In the previous lab, we've configured the replica set cluster using the exact IP address. However, if we configured the server with a different IP address (e.g. going from a fixed IP to DNS server for IP address resolution), this would break the replication mechanism since the nodes won't be able to reach each other with the original IP address.

In this lab, you will modify the replica set so one of the nodes uses the hostname `m103`, the virtual machine hostname, instead of the external IP address of the Vagrant box.

To correctly reconfigure this node, you will have to remove the node from the replica set, and then add it back with the correct hostname. For this lab, you only need to do this for one of the nodes in the set.

Some tips that will help the validator pass:

* The configuration of the nodes should **not** change - the hostname `m103` is already bound to the IP address `192.168.103.100`
* The nodes should still run on ports **27001**, **27002**, **27003**
* The name of your replica set should still be `m103-repl`

**Note**: Please make sure that the hostname `m103` is getting mapped to `192.168.103.100` and not `127.0.0.1`. You can verify this by running this command `ping m103` at the vagrant prompt and it should resolve to `192.168.103.100`. If this is not happening then please refer [this post]() in the discussion forum to update it.

When you're finished, run the validation script in your vagrant and outside the mongo shell and enter the validation key you receive below. If you receive an error, it should give you some idea of what went wrong.

```sh
vagrant@m103:~$ validate_lab_remove_readd_node
```

Enter answer here: 5a4fff19c0324e9feb9f60b9  (Fuera de Tiempo)

#### See detailed answer:

We don't have to change the config files of our `mongod` instances, because the current `bind_ip` resolves the IP address of **m103** hostname.

To remove a node from the replica set, simply use `rs.remove()`:

```sh
rs.remove("192.168.103.100:27003")
```

Once the node is removed, we can just add him back again, using the `hostname` instead: **m103**:

```sh
rs.add("m103:27003")
```

Below is a snippet of the output from running `rs.status()`:

```sh
"members" : [
  {
    "_id" : 8,
    "name" : "m103:27003",
    "health" : 1,
    "state" : 2,
    "stateStr" : "SECONDARY",
  }
]
```

Notice that the name of this replica set member now says `"m103:27003"`.

MIO

```sh
vagrant@m103:~$ mongo --host "m103-repl/192.168.103.100:27001" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
MongoDB shell version v3.6.17
connecting to: mongodb://192.168.103.100:27001/?authSource=admin&gssapiServiceName=mongodb&replicaSet=m103-repl
2020-02-27T17:47:40.909+0000 I NETWORK  [thread1] Starting new replica set monitor for m103-repl/192.168.103.100:27001
2020-02-27T17:47:40.911+0000 I NETWORK  [thread1] Successfully connected to 192.168.103.100:27001 (1 connections now open to 192.168.103.100:27001 with a 5 second timeout)
2020-02-27T17:47:40.912+0000 I NETWORK  [thread1] Successfully connected to m103:27002 (1 connections now open to m103:27002 with a 5 second timeout)
2020-02-27T17:47:40.913+0000 I NETWORK  [thread1] changing hosts to m103-repl/192.168.103.100:27001,m103:27002,m103:27003 from m103-repl/192.168.103.100:27001
2020-02-27T17:47:40.914+0000 I NETWORK  [ReplicaSetMonitor-TaskExecutor-0] Successfully connected to m103:27003 (1 connections now open to m103:27003 with a 5 second timeout)
Implicit session: session { "id" : UUID("3c022cee-4ab4-486b-ad5c-a3350906d82a") }
MongoDB server version: 3.6.17
Server has startup warnings: 
2020-02-24T19:56:15.486+0000 I STORAGE  [initandlisten] 
2020-02-24T19:56:15.486+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-02-24T19:56:15.486+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-02-24T19:56:16.158+0000 I CONTROL  [initandlisten] 
2020-02-24T19:56:16.158+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-02-24T19:56:16.158+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-24T19:56:16.158+0000 I CONTROL  [initandlisten] 
2020-02-24T19:56:16.158+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-02-24T19:56:16.158+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-24T19:56:16.158+0000 I CONTROL  [initandlisten] 
MongoDB Enterprise m103-repl:PRIMARY> rs.remove("192.168.103.100:27003")
error: couldn't find 192.168.103.100:27003 in [
	{
		"_id" : 0,
		"host" : "192.168.103.100:27001",
		"arbiterOnly" : false,
		"buildIndexes" : true,
		"hidden" : false,
		"priority" : 1,
		"tags" : {
			
		},
		"slaveDelay" : NumberLong(0),
		"votes" : 1
	},
	{
		"_id" : 1,
		"host" : "m103:27002",
		"arbiterOnly" : false,
		"buildIndexes" : true,
		"hidden" : false,
		"priority" : 1,
		"tags" : {
			
		},
		"slaveDelay" : NumberLong(0),
		"votes" : 1
	},
	{
		"_id" : 2,
		"host" : "m103:27003",
		"arbiterOnly" : false,
		"buildIndexes" : true,
		"hidden" : false,
		"priority" : 1,
		"tags" : {
			
		},
		"slaveDelay" : NumberLong(0),
		"votes" : 1
	}
]
MongoDB Enterprise m103-repl:PRIMARY> rs.add("m103:27003")
{
	"ok" : 0,
	"errmsg" : "Found two member configurations with same host field, members.2.host == members.3.host == m103:27003",
	"code" : 103,
	"codeName" : "NewReplicaSetConfigurationIncompatible",
	"operationTime" : Timestamp(1582825712, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582825712, 1),
		"signature" : {
			"hash" : BinData(0,"E8Z3mR/UvcF+w6vekf7eFYLECqE="),
			"keyId" : NumberLong("6797103509075394561")
		}
	}
}
MongoDB Enterprise m103-repl:PRIMARY> rs.status()
{
	"set" : "m103-repl",
	"date" : ISODate("2020-02-27T17:48:45.703Z"),
	"myState" : 1,
	"term" : NumberLong(2),
	"syncingTo" : "",
	"syncSourceHost" : "",
	"syncSourceId" : -1,
	"heartbeatIntervalMillis" : NumberLong(2000),
	"optimes" : {
		"lastCommittedOpTime" : {
			"ts" : Timestamp(1582825722, 1),
			"t" : NumberLong(2)
		},
		"readConcernMajorityOpTime" : {
			"ts" : Timestamp(1582825722, 1),
			"t" : NumberLong(2)
		},
		"appliedOpTime" : {
			"ts" : Timestamp(1582825722, 1),
			"t" : NumberLong(2)
		},
		"durableOpTime" : {
			"ts" : Timestamp(1582825722, 1),
			"t" : NumberLong(2)
		}
	},
	"members" : [
		{
			"_id" : 0,
			"name" : "192.168.103.100:27001",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 251210,
			"optime" : {
				"ts" : Timestamp(1582825722, 1),
				"t" : NumberLong(2)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1582825722, 1),
				"t" : NumberLong(2)
			},
			"optimeDate" : ISODate("2020-02-27T17:48:42Z"),
			"optimeDurableDate" : ISODate("2020-02-27T17:48:42Z"),
			"lastHeartbeat" : ISODate("2020-02-27T17:48:43.750Z"),
			"lastHeartbeatRecv" : ISODate("2020-02-27T17:48:44.698Z"),
			"pingMs" : NumberLong(0),
			"lastHeartbeatMessage" : "",
			"syncingTo" : "m103:27002",
			"syncSourceHost" : "m103:27002",
			"syncSourceId" : 1,
			"infoMessage" : "",
			"configVersion" : 3
		},
		{
			"_id" : 1,
			"name" : "m103:27002",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 251550,
			"optime" : {
				"ts" : Timestamp(1582825722, 1),
				"t" : NumberLong(2)
			},
			"optimeDate" : ISODate("2020-02-27T17:48:42Z"),
			"syncingTo" : "",
			"syncSourceHost" : "",
			"syncSourceId" : -1,
			"infoMessage" : "",
			"electionTime" : Timestamp(1582721555, 1),
			"electionDate" : ISODate("2020-02-26T12:52:35Z"),
			"configVersion" : 3,
			"self" : true,
			"lastHeartbeatMessage" : ""
		},
		{
			"_id" : 2,
			"name" : "m103:27003",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 251198,
			"optime" : {
				"ts" : Timestamp(1582825722, 1),
				"t" : NumberLong(2)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1582825722, 1),
				"t" : NumberLong(2)
			},
			"optimeDate" : ISODate("2020-02-27T17:48:42Z"),
			"optimeDurableDate" : ISODate("2020-02-27T17:48:42Z"),
			"lastHeartbeat" : ISODate("2020-02-27T17:48:43.750Z"),
			"lastHeartbeatRecv" : ISODate("2020-02-27T17:48:43.748Z"),
			"pingMs" : NumberLong(0),
			"lastHeartbeatMessage" : "",
			"syncingTo" : "m103:27002",
			"syncSourceHost" : "m103:27002",
			"syncSourceId" : 1,
			"infoMessage" : "",
			"configVersion" : 3
		}
	],
	"ok" : 1,
	"operationTime" : Timestamp(1582825722, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582825722, 1),
		"signature" : {
			"hash" : BinData(0,"OY1832QCW//1rhWbvWZhbiOcM+U="),
			"keyId" : NumberLong("6797103509075394561")
		}
	}
}
MongoDB Enterprise m103-repl:PRIMARY> 

```

## 18. Tema: Lee y escribe en un conjunto de réplicas

### Notas de lectura

**Instrucciones del tema**

*Nota: En el video conferencia, utilizamos el antiguo nombre de host* `"m103.mongodb.university"` que se ha cambiado a `"m103"`. *Hemos actualizado todos los siguientes comandos en consecuencia*.

Conexión al conjunto de réplicas:

```sh
mongo --host "m103-example/m103:27011" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
```

Comprobación de la topología del replica set:

```sh
rs.isMaster()
```

Insertar un documento en una nueva colección:

```sh
use newDB
db.new_collection.insert( { "student": "Matt Javaly", "grade": "A+" } )
```

Conexión directa a un nodo secundario (¡este nodo puede no ser secundario en su replica set!):

```sh
mongo --host "m103:27012" -u "m103-admin" -p "m103-pass"
--authenticationDatabase "admin"
```

Intentando ejecutar un comando de lectura en un nodo secundario (esto debería fallar):

```sh
show dbs
```

Habilitar comandos de lectura en un nodo secundario:

```sh
rs.slaveOk()
```

Lectura desde un nodo secundario:

```sh
use newDB
db.new_collection.find()
```

Intentando escribir datos directamente en un nodo secundario (esto debería fallar, porque no podemos escribir datos directamente en un nodo secundario):

```sh
db.new_collection.insert( { "student": "Norberto Leite", "grade": "B+" } )
```

Apagar el servidor (en ambos nodos secundarios)

```sh
use admin
db.shutdownServer()
```

Conectando directamente al último nodo saludable en nuestro conjunto:

```sh
mongo --host "m103:27011" -u "m103-admin" -p "m103-pass"
--authenticationDatabase "admin"
```

Verificando que el último nodo bajó para convertirse en secundario cuando la mayoría de los nodos del conjunto no estaban disponibles:

```sh
rs.isMaster()
```

### Transcripción

Entonces, en esta lección, vamos a echar un vistazo a las lecturas y escrituras en un replica set para tener una idea de cómo funciona el mecanismo de replicación MongoDB.

Así que solo voy a usar este comando para conectarme a nuestro replica set de tres nodos llamado `m103-example`.

```sh
vagrant@m103:~$ mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
...
MongoDB Enterprise m103-example:PRIMARY> 
```

Y para averiguar qué nodo en este conjunto es actualmente el principal, voy a ejecutar este comando `rs.isMaster()`.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013"
	],
	"setName" : "m103-example",
	"setVersion" : 7,
	"ismaster" : true,
	"secondary" : false,
	"primary" : "m103:27012",
	"me" : "m103:27012",
	"electionId" : ObjectId("7fffffff0000000000000002"),
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582827921, 1),
			"t" : NumberLong(2)
		},
		"lastWriteDate" : ISODate("2020-02-27T18:25:21Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582827921, 1),
			"t" : NumberLong(2)
		},
		"majorityWriteDate" : ISODate("2020-02-27T18:25:21Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-27T18:25:29.804Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582827921, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582827921, 1),
		"signature" : {
			"hash" : BinData(0,"WOtVY6nGi1Tu+MrWM7L1e1CBt1o="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY> 
```

Y esta salida en realidad nos da mucha información.

Nos da el primario actual, que es el nodo que se ejecuta en el puerto `27012`.

Y también nos da el nodo al que estamos conectados actualmente, `"me"`.


```sh
"primary" : "m103:27012",
"me" : "m103:27012",
```

En este caso, es lo mismo que el primario, porque estamos conectados a la réplica establecida a través del shell.

También nos da los otros nodos en este replica set.

```sh
"hosts" : [
	"192.168.103.100:27011",
	"m103:27012",
	"m103:27013"
],
```

Así que voy a crear una nueva base de datos aquí llamada `newDB`.

```sh
MongoDB Enterprise m103-example:PRIMARY> use newDB
switched to db newDB
MongoDB Enterprise m103-example:PRIMARY> 
```

E inserte un nuevo documento en una nueva colección llamada `new_collection` en `newDB`.

```sh
MongoDB Enterprise m103-example:PRIMARY> db.new_collection.insert({ "student": "Matt Javaly", "grade": "A+" })
WriteResult({ "nInserted" : 1 })
MongoDB Enterprise m103-example:PRIMARY> 
```

Y me voy a conectar a un secundario para asegurarme de que también reciban la escritura.

Podemos desplazarnos por la salida `rs.isMaster` para descubrir cuál de estos nodos es secundario.

Y sabemos que `m103:27012` es el principal, por lo que solo tenemos que conectarnos a uno de los otros dos nodos.

Este es el comando que vamos a usar para conectarnos directamente a un nodo secundario en nuestro replica set.

```sh
vagrant@m103:~$ mongo --host "m103:27011" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
...
MongoDB Enterprise m103-example:SECONDARY> 
```

Observe que hemos cambiado el puerto de nodo que hemos seleccionado en nuestro nombre de host.

Y tampoco hemos especificado el nombre del replica set.

**Porque si tuviéramos que especificar el nombre del replica set, el shell nos dirigiría automáticamente al primario.**

Y en este caso, en realidad queremos conectarnos directamente a un secundario.

Y como podemos ver, el indicador de shell cambia para reflejar que ahora estamos conectados a un nodo secundario.

Entonces podemos comenzar a ejecutar comandos de shell en el nodo secundaria, ¿verdad?

```sh
MongoDB Enterprise m103-example:SECONDARY> show dbs
2020-02-28T10:06:35.382+0000 E QUERY    [thread1] Error: listDatabases failed:{
	"operationTime" : Timestamp(1582884390, 1),
	"ok" : 0,
	"errmsg" : "not master and slaveOk=false",
	"code" : 13435,
	"codeName" : "NotMasterNoSlaveOk",
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582884390, 1),
		"signature" : {
			"hash" : BinData(0,"a91PPVLOG0bEANPM5aU+8DoIuKg="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
} :
_getErrorWithCode@src/mongo/shell/utils.js:25:13
Mongo.prototype.getDBs@src/mongo/shell/mongo.js:67:1
shellHelper.show@src/mongo/shell/utils.js:860:19
shellHelper@src/mongo/shell/utils.js:750:15
@(shellhelp2):1:1
MongoDB Enterprise m103-example:SECONDARY> 
```

No, en realidad no podemos.

Cuando estamos conectados a un nodo secundario, solo podemos ejecutar comandos de lectura después de decirle a MongoDB que estamos seguros de que eso es lo que queremos hacer.

Esto se debe a que MongoDB se equivoca por el lado de la coherencia.

Dado que queremos asegurarnos de que siempre tenga una vista coherente de sus datos, debe decir explícitamente lo contrario si desea leer en las secundarias.

Entonces, este es el comando que vamos a ejecutar para habilitar las operaciones de lectura en el nodo secundario.

```sh
MongoDB Enterprise m103-example:SECONDARY> rs.slaveOk()
MongoDB Enterprise m103-example:SECONDARY> 
```

Y ahora, nuestro comando `show dbs` debería funcionar realmente.

```sh
MongoDB Enterprise m103-example:SECONDARY> show dbs
admin   0.000GB
config  0.000GB
local   0.001GB
newDB   0.000GB
MongoDB Enterprise m103-example:SECONDARY> 
```

Y lo hace

Y podemos ver que el comando de escritura fue replicado en el nodo secundario.

```sh
MongoDB Enterprise m103-example:SECONDARY> use newDB
switched to db newDB
MongoDB Enterprise m103-example:SECONDARY> show collections
new_collection
MongoDB Enterprise m103-example:SECONDARY> db.new_collection.find()
{ "_id" : ObjectId("5e580afbe038a84ad59ad0c9"), "student" : "Matt Javaly", "grade" : "A+" }
MongoDB Enterprise m103-example:SECONDARY> 
```

Así que aquí he intentado insertar un documento en un nodo secundario.

```sh
MongoDB Enterprise m103-example:SECONDARY> db.new_collection.insert({ "student": "Adolfo de la Rosa", "grade": "B+" })
WriteResult({ "writeError" : { "code" : 10107, "errmsg" : "not master" } })
MongoDB Enterprise m103-example:SECONDARY> 
```

Y como se esperaba, solo podemos hacer lecturas en este secundario.

Nunca podremos escribir en un nodo secundario.

El propósito de esto es hacer cumplir una fuerte consistencia en nuestro clúster.

El shell Mongo nos permite saber que no podemos escribir en la secundaria.

Hasta ahora hemos cubierto cómo funcionan las lecturas y escrituras en un replica set cuando está en buen estado.

En el interés de aprender cómo los replica set manejan la crisis, vamos a romper algunas cosas.

Primero, vamos a cerrar este nodo.

```
MongoDB Enterprise m103-example:SECONDARY> use admin
switched to db admin
MongoDB Enterprise m103-example:SECONDARY> db.shutdownServer()
server should be down...
2020-02-28T10:15:41.090+0000 I NETWORK  [thread1] trying reconnect to m103:27011 (192.168.103.100) failed
2020-02-28T10:15:41.091+0000 W NETWORK  [thread1] Failed to connect to 192.168.103.100:27011, in(checking socket for error after poll), reason: Connection refused
2020-02-28T10:15:41.091+0000 I NETWORK  [thread1] reconnect m103:27011 (192.168.103.100) failed failed 
2020-02-28T10:15:41.094+0000 I NETWORK  [thread1] trying reconnect to m103:27011 (192.168.103.100) failed
2020-02-28T10:15:41.094+0000 W NETWORK  [thread1] Failed to connect to 192.168.103.100:27011, in(checking socket for error after poll), reason: Connection refused
2020-02-28T10:15:41.094+0000 I NETWORK  [thread1] reconnect m103:27011 (192.168.103.100) failed failed 
MongoDB Enterprise > 
MongoDB Enterprise > ^C
bye
2020-02-28T10:16:24.712+0000 I NETWORK  [thread1] trying reconnect to m103:27011 (192.168.103.100) failed
2020-02-28T10:16:24.712+0000 W NETWORK  [thread1] Failed to connect to 192.168.103.100:27011, in(checking socket for error after poll), reason: Connection refused
2020-02-28T10:16:24.712+0000 I NETWORK  [thread1] reconnect m103:27011 (192.168.103.100) failed failed 
2020-02-28T10:16:24.712+0000 I QUERY    [thread1] Failed to end session { id: UUID("1aa78899-1e1a-43d6-bf68-a09f66d83f26") } due to SocketException: socket exception [CONNECT_ERROR] for couldn't connect to server m103:27011, connection attempt failed
vagrant@m103:~$ 
```

Ahora, cuando nos conectamos de nuevo al replica set, ejecutamos `rs.status()` podemos ver que el nodo que cerramos ya no es accesible desde el primario `"stateStr" : "(not reachable/healthy)",`.

```sh
MongoDB Enterprise m103-example:PRIMARY> rs.status()
{
	"set" : "m103-example",
	"date" : ISODate("2020-02-28T10:18:11.060Z"),
	"myState" : 1,
	"term" : NumberLong(2),
	"syncingTo" : "",
	"syncSourceHost" : "",
	"syncSourceId" : -1,
	"heartbeatIntervalMillis" : NumberLong(2000),
	"optimes" : {
		"lastCommittedOpTime" : {
			"ts" : Timestamp(1582885091, 1),
			"t" : NumberLong(2)
		},
		"readConcernMajorityOpTime" : {
			"ts" : Timestamp(1582885091, 1),
			"t" : NumberLong(2)
		},
		"appliedOpTime" : {
			"ts" : Timestamp(1582885091, 1),
			"t" : NumberLong(2)
		},
		"durableOpTime" : {
			"ts" : Timestamp(1582885091, 1),
			"t" : NumberLong(2)
		}
	},
	"members" : [
		{
			"_id" : 0,
			"name" : "192.168.103.100:27011",
			"health" : 0,
			"state" : 8,
			"stateStr" : "(not reachable/healthy)",
			"uptime" : 0,
			"optime" : {
				"ts" : Timestamp(0, 0),
				"t" : NumberLong(-1)
			},
			"optimeDurable" : {
				"ts" : Timestamp(0, 0),
				"t" : NumberLong(-1)
			},
			"optimeDate" : ISODate("1970-01-01T00:00:00Z"),
			"optimeDurableDate" : ISODate("1970-01-01T00:00:00Z"),
			"lastHeartbeat" : ISODate("2020-02-28T10:18:10.243Z"),
			"lastHeartbeatRecv" : ISODate("2020-02-28T10:15:40.186Z"),
			"pingMs" : NumberLong(0),
			"lastHeartbeatMessage" : "Connection refused",
			"syncingTo" : "",
			"syncSourceHost" : "",
			"syncSourceId" : -1,
			"infoMessage" : "",
			"configVersion" : -1
		},
		{
			"_id" : 1,
			"name" : "m103:27012",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 318085,
			"optime" : {
				"ts" : Timestamp(1582885091, 1),
				"t" : NumberLong(2)
			},
			"optimeDate" : ISODate("2020-02-28T10:18:11Z"),
			"syncingTo" : "",
			"syncSourceHost" : "",
			"syncSourceId" : -1,
			"infoMessage" : "",
			"electionTime" : Timestamp(1582571388, 1),
			"electionDate" : ISODate("2020-02-24T19:09:48Z"),
			"configVersion" : 7,
			"self" : true,
			"lastHeartbeatMessage" : ""
		},
		{
			"_id" : 2,
			"name" : "m103:27013",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 163546,
			"optime" : {
				"ts" : Timestamp(1582885081, 1),
				"t" : NumberLong(2)
			},
			"optimeDurable" : {
				"ts" : Timestamp(1582885081, 1),
				"t" : NumberLong(2)
			},
			"optimeDate" : ISODate("2020-02-28T10:18:01Z"),
			"optimeDurableDate" : ISODate("2020-02-28T10:18:01Z"),
			"lastHeartbeat" : ISODate("2020-02-28T10:18:10.185Z"),
			"lastHeartbeatRecv" : ISODate("2020-02-28T10:18:10.385Z"),
			"pingMs" : NumberLong(0),
			"lastHeartbeatMessage" : "",
			"syncingTo" : "m103:27012",
			"syncSourceHost" : "m103:27012",
			"syncSourceId" : 1,
			"infoMessage" : "",
			"configVersion" : 7
		},
		{
			"_id" : 3,
			"name" : "m103:27014",
			"health" : 0,
			"state" : 6,
			"stateStr" : "(not reachable/healthy)",
			"uptime" : 0,
			"optime" : {
				"ts" : Timestamp(0, 0),
				"t" : NumberLong(-1)
			},
			"optimeDurable" : {
				"ts" : Timestamp(0, 0),
				"t" : NumberLong(-1)
			},
			"optimeDate" : ISODate("1970-01-01T00:00:00Z"),
			"optimeDurableDate" : ISODate("1970-01-01T00:00:00Z"),
			"lastHeartbeat" : ISODate("2020-02-28T10:18:10.274Z"),
			"lastHeartbeatRecv" : ISODate("1970-01-01T00:00:00Z"),
			"pingMs" : NumberLong(0),
			"lastHeartbeatMessage" : "",
			"authenticated" : false,
			"syncingTo" : "",
			"syncSourceHost" : "",
			"syncSourceId" : -1,
			"infoMessage" : "",
			"configVersion" : -1
		}
	],
	"ok" : 1,
	"operationTime" : Timestamp(1582885091, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582885091, 1),
		"signature" : {
			"hash" : BinData(0,"kr1oysJBl/4YQJMd48ABY0zT5XA="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:PRIMARY>
```

Ahora solo voy a cerrar este otro nodo `m103:27013`.


```sh
vagrant@m103:~$ mongo --host "m103:27013" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
...
MongoDB Enterprise m103-example:SECONDARY> use admin
switched to db admin
MongoDB Enterprise m103-example:SECONDARY> db.shutdownServer()
server should be down...
2020-02-28T10:23:49.970+0000 I NETWORK  [thread1] trying reconnect to m103:27013 (192.168.103.100) failed
2020-02-28T10:23:49.970+0000 W NETWORK  [thread1] Failed to connect to 192.168.103.100:27013, in(checking socket for error after poll), reason: Connection refused
2020-02-28T10:23:49.970+0000 I NETWORK  [thread1] reconnect m103:27013 (192.168.103.100) failed failed 
2020-02-28T10:23:49.974+0000 I NETWORK  [thread1] trying reconnect to m103:27013 (192.168.103.100) failed
2020-02-28T10:23:49.974+0000 W NETWORK  [thread1] Failed to connect to 192.168.103.100:27013, in(checking socket for error after poll), reason: Connection refused
2020-02-28T10:23:49.974+0000 I NETWORK  [thread1] reconnect m103:27013 (192.168.103.100) failed failed 
MongoDB Enterprise > ^C
bye
2020-02-28T10:23:55.778+0000 I NETWORK  [thread1] trying reconnect to m103:27013 (192.168.103.100) failed
2020-02-28T10:23:55.779+0000 W NETWORK  [thread1] Failed to connect to 192.168.103.100:27013, in(checking socket for error after poll), reason: Connection refused
2020-02-28T10:23:55.780+0000 I NETWORK  [thread1] reconnect m103:27013 (192.168.103.100) failed failed 
2020-02-28T10:23:55.780+0000 I QUERY    [thread1] Failed to end session { id: UUID("701a68a5-9006-4845-829e-9b424f035b0f") } due to SocketException: socket exception [CONNECT_ERROR] for couldn't connect to server m103:27013, connection attempt failed
vagrant@m103:~$ 
```

Entonces, ahora que hemos apagado dos de los nodos en nuestro replica set, solo queda uno.

Y uno de cada tres no forma una mayoría.

Por lo tanto, en realidad no podremos conectarnos al primario, porque el primario actual, que se estaba ejecutando en este nodo, se ha reducido para convertirse en secundario.

```sh
vagrant@m103:~$ mongo --host "m103:27012" -u "m103-admin" -p "m103-pass" --authenticationDatabase "admin"
MongoDB shell version v3.6.17
connecting to: mongodb://m103:27012/?authSource=admin&gssapiServiceName=mongodb
Implicit session: session { "id" : UUID("9afe76ef-5fc4-43c0-863c-af29314b9dc8") }
MongoDB server version: 3.6.17
Server has startup warnings: 
2020-02-24T17:56:47.029+0000 I STORAGE  [initandlisten] 
2020-02-24T17:56:47.029+0000 I STORAGE  [initandlisten] ** WARNING: Using the XFS filesystem is strongly recommended with the WiredTiger storage engine
2020-02-24T17:56:47.029+0000 I STORAGE  [initandlisten] **          See http://dochub.mongodb.org/core/prodnotes-filesystem
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] 
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/enabled is 'always'.
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] 
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] ** WARNING: /sys/kernel/mm/transparent_hugepage/defrag is 'always'.
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] **        We suggest setting it to 'never'
2020-02-24T17:56:47.712+0000 I CONTROL  [initandlisten] 
MongoDB Enterprise m103-example:RECOVERING> 
```

Así que aquí no he especificado el nombre del replica set, porque quiero conectarme directamente a este nodo.

Y como podemos ver, ese nodo ha dejado de ser el primario, ni tampoco secundario es `RECOVERING`.

Podemos verificar eso ejecutando `rs.isMaster()`.

```sh
MongoDB Enterprise m103-example:RECOVERING> rs.isMaster()
{
	"hosts" : [
		"192.168.103.100:27011",
		"m103:27012",
		"m103:27013"
	],
	"setName" : "m103-example",
	"setVersion" : 7,
	"ismaster" : false,
	"secondary" : false,
	"me" : "m103:27012",
	"lastWrite" : {
		"opTime" : {
			"ts" : Timestamp(1582885431, 1),
			"t" : NumberLong(2)
		},
		"lastWriteDate" : ISODate("2020-02-28T10:23:51Z"),
		"majorityOpTime" : {
			"ts" : Timestamp(1582885421, 1),
			"t" : NumberLong(2)
		},
		"majorityWriteDate" : ISODate("2020-02-28T10:23:41Z")
	},
	"maxBsonObjectSize" : 16777216,
	"maxMessageSizeBytes" : 48000000,
	"maxWriteBatchSize" : 100000,
	"localTime" : ISODate("2020-02-28T10:27:27.845Z"),
	"logicalSessionTimeoutMinutes" : 30,
	"minWireVersion" : 0,
	"maxWireVersion" : 6,
	"readOnly" : false,
	"ok" : 1,
	"operationTime" : Timestamp(1582885431, 1),
	"$clusterTime" : {
		"clusterTime" : Timestamp(1582885431, 1),
		"signature" : {
			"hash" : BinData(0,"DWPn4jM0ZRuMrWsVNBBghG3hORw="),
			"keyId" : NumberLong("6797075527363461121")
		}
	}
}
MongoDB Enterprise m103-example:RECOVERING> 

```

Entonces, como podemos ver, todavía estamos conectados al mismo nodo que antes `"me" : "m103:27012"`.

Pero ese nodo tampoco es un nodo secundario `"secondary" : false,`.

Aunque el nodo primario nunca cayó, perdimos el último secundario que nos dio la mayoría.

Si el replica set ya no puede llegar a la mayoría de los nodos, todos los nodos restantes en el conjunto se convierten en secundarios (`RECOVERING`).

Y debido a que son secundarias, no podemos escribir nada en el replica set, porque no hay primario.

Este es solo otro mecanismo seguro utilizado por el replica set MongoDB para garantizar la coherencia de los datos.

Así que solo para recapitular.

<img src="images/m103/c2/2-18-resumen.png">

En esta lección, cubrimos los datos que se replican en un secundario, cómo funciona la lectura de los nodos secundarios y cómo escribir en un replica set cuando la mayoría no está disponible, es decir, no podemos.

## 19. Examen Reads and Writes on a Replica Set

**Problem:**

Which of the following is true about reading and writing from secondaries?

Check all answers that apply:

* We have to run rs.slaveOk() before we can read from secondary nodes. :+1:

* Running rs.slaveOk() allows us to read and write from secondaries.

* Connecting to the replica set will automatically connect to a secondary node.

**See detailed answer**

**Correct answers:**

**We have to run `rs.slaveOk()` before we can read from secondary nodes.**

Reads should typically go through the primary node, but rs.slaveOk() allows us to read from secondaries.

**Incorrect answers:**

**Connecting to the replica set will automatically connect to a secondary node.**

This is incorrect; By default, MongoDB will always redirect a Replica Set connection to the primary node. In case of Read Preference being defined in the connection string, that determines other behavior, then the connection might be redirected to connect to a Secondary node, only for read workload. Writes are always sent to the Primary node of the Replica Set cluster.

**Running `rs.slaveOk()` allows us to read and write from secondaries.**

This is incorrect; `rs.slaveOk` only determines that the mongo shell can be set to read data if connected to a Secondary node.

## 20. Tema: Failover (Conmutación por Error) y Elecciones

### Notas de lectura

Instrucciones de lectura

Almacenar la configuración del replica set como una variable `cfg`:

```sh
cfg = rs.conf()
```

Establecer la prioridad de un nodo a 0, por lo que no puede convertirse en primario (haciendo que el nodo sea "passive"):

```sh
cfg.members[2].priority = 0
```

Actualización de nuestro conjunto de réplicas para usar la nueva configuración `cfg`:

```sh
rs.reconfig(cfg)
```

Comprobando la nueva topología de nuestro replica set:

```sh
rs.isMaster()
```

Forzar una elección en este replica set (aunque en este caso, manipulamos la elección para que solo un nodo pueda convertirse en primario):

```sh
rs.stepDown()
```

Verificando la topología de nuestro replica set después de las elecciones:

```sh
rs.isMaster()
```

### Transcripción

En esta lección, vamos a discutir cómo funcionan las failovers y las elecciones en MongoDB.

El nodo primario es el primer punto de contacto para cualquier cliente que se comunique con la base de datos.

Incluso si los secundarios caen, el cliente continuará comunicándose con el nodo que actúa como primario hasta que el primario no esté disponible.

Pero, ¿qué causaría que una primaria no esté disponible?

Una razón común es el mantenimiento.

Entonces, digamos que estamos haciendo una actualización continua en un replica set de tres nodos.

Una actualización continua solo significa que estamos actualizando un servidor a la vez, comenzando con los secundarios.

Y eventualmente, actualizaremos el primario.

Las actualizaciones continuas son excelentes porque nos permiten realizar actualizaciones al tiempo que maximizamos la disponibilidad para cualquier cliente que use la base de datos.

Comenzaremos deteniendo el proceso de MongoDB en un secundario y luego volviendo a ponerlo en marcha con la nueva versión de la base de datos.

Y podemos hacer lo mismo para actualizar nuestra otro secundario.

Cuando se hayan actualizado todos nuestros secundarios, es hora de actualizar el primario.

Pero reiniciar el servidor de base de datos en el primario desencadenaría una elección de todos modos, por lo que vamos a iniciar una elección de forma segura con `rs.stepDown()`.

Una vez que se complete la elección, el último servidor de base de datos que ejecuta la versión anterior de la base de datos será un nodo secundario.

Entonces podemos bajarlo y luego volver a subirlo con el nuevo servidor de base de datos sin afectar la disponibilidad de todo el replica set.

Hablemos un poco sobre cómo funcionan las elecciones.

Las elecciones tienen lugar cada vez que hay un cambio en la topología.

La reconfiguración de un replica set siempre desencadenará una elección que puede elegir o no un nuevo primario.

Pero definitivamente verá un nuevo primario elegido en dos casos: cada vez que el nodo primario actual no esté disponible, o cuando el nodo primario actual deje de ser secundario.

El método para determinar qué secundaria se postulará para las elecciones comienza con prioridad, que discutiremos en un minuto; y cualquier nodo que tenga la última copia de los datos.

Entonces, supongamos que todos los nodos de nuestro replica set tienen la misma prioridad, que es la predeterminada, a menos que hayamos establecido prioridades para los nodos de nuestro conjunto.

Y este nodo tiene la última copia de los datos.

Por lo tanto, se postulará para las elecciones y luego votará automáticamente por sí mismo.

Luego les pedirá a los otros dos nodos su apoyo en las elecciones.

Y van a decir, está bien, tienes una copia bastante reciente de los datos, pareces un buen candidato, entonces también se comprometerán a apoyarlos.

Este nodo será elegido primario.

También existe la pequeña posibilidad de que dos nodos se postulen para elecciones simultáneamente.

Pero en un replica set con un número impar de nodos, esto no importa.

Estos dos nodos se ejecutarán, lo que significa que ambos votarán por sí mismos.

Y luego este nodo esencialmente decidirá cuál de estos nodos se vuelve primario en virtud de un desempate.

Esto se convierte en un problema cuando tenemos un número par de miembros con derecho a voto en un conjunto.

Si dos secundarias se postulan para elecciones simultáneamente y hay un número par de nodos restantes en el conjunto, existe la posibilidad de que dividan el voto y haya un empate.

Ahora un empate no es el fin del mundo, porque los nodos simplemente comenzarán de nuevo y celebrarán otras elecciones.

El problema de repetir las elecciones una y otra vez es que cualquier aplicación que acceda a los datos tendrá que pausar toda la actividad y esperar hasta que se elija un primario.

Un número par de nodos aumenta las posibilidades de que se repita una elección, por lo que generalmente intentamos mantener un número impar en nuestros replica set.

Otro aspecto importante de las elecciones es la prioridad asignada a cada nodo en un conjunto.

La prioridad es esencialmente la probabilidad de que un nodo se convierta en el principal durante una elección.

El primario predeterminado para un nodo es 1, y cualquier nodo con prioridad 1 o superior puede ser elegido primario.

Podemos aumentar la prioridad de un nodo si queremos que sea más probable en este nodo se convierte en primario.

Pero cambiar este valor solo no garantiza eso.

También podemos establecer la prioridad del nodo en 0 si nunca queremos que ese nodo se convierta en primario.

**Un nodo de prioridad 0 aún puede votar en las elecciones, pero no puede postularse para las elecciones.**

Echemos un vistazo a cómo cambiar la prioridad de un nodo.

Así que aquí solo estoy almacenando un documento de configuración en una variable.

Así que aquí solo estoy estableciendo la prioridad de uno de nuestros secundarios a 0 para que nunca pueda convertirse en el nodo primario.

Y aquí, solo estoy guardando la nueva configuración en un replica set.

Entonces, ahora que hemos reconfigurado nuestro replica set, voy a ejecutar `rs.isMaster()` a través de la nueva topología.

Entonces, en MongoDB, los nodos que no son elegibles para convertirse en primarios se definen como nodos pasivos.

Y podemos ver que el nodo que reconfiguramos para tener prioridad 0 ha aparecido en nuestra lista de pasivos.

Los otros dos nodos siguen siendo elegibles para convertirse en los primarios.

Algo que quiero señalar aquí.

Cuando llamamos a `rs.stepDown()`, siempre trata de elegir un nuevo nodo primario.

Pero en este caso, aparte del primario actual, solo hay un nodo que es elegible para convertirse en el primario.

Lo que significa que si tuviéramos que convocar una elección en este momento, este nodo tendría que convertirse en el nodo primario.

Por cierto, al cambiar la prioridad del nodo, hemos manipulado las elecciones a favor de este nodo.

Entonces, solo para probar nuestra teoría, convocaremos una elección en este conjunto de réplicas.

Solo tenemos que esperar a que se complete la elección.

Muy bien, así que terminó, y solo voy a verificar cuál es el primario actual.

Y podemos ver que teníamos razón.

Este nodo se convirtió en el nodo primario porque era el único primario elegible en esa elección.

Así que una última cosa a tener en cuenta.

Si el primario actual no puede llegar a la mayoría de los otros nodos en el conjunto, entonces se reducirá automáticamente para convertirse en secundario.

En un replica set de tres nodos, la mayoría son dos nodos.

Entonces, si dos nodos se desactivan, incluso si el primario aún está disponible, se reducirá.

En este punto, una elección no puede tener lugar hasta que se vuelvan a generar suficientes nodos para formar una mayoría, y los clientes podrán conectarse a todo el replica set porque no hay un primario.

Entonces, para resumir, hemos cubierto cómo se mantiene la disponibilidad durante las elecciones, la prioridad efectiva en las elecciones y el comportamiento de un replica set cuando la mayoría de los nodos no están disponibles.

## 21. Examen Failover and Elections

**Problem:**

Which of the following is true about elections?

Check all answers that apply:

* Elections can take place anytime while the primary is available.

* Nodes with priority 0 cannot be elected primary. :+1:

* Nodes with higher priority are more likely to be elected primary. :+1:

* All nodes have an equal chance to become primary.

**See detailed answer**

**Correct answers:**

**Nodes with priority 0 cannot be elected primary.**

Setting a node's priority to 0 guarantees that node will never become primary.

**Nodes with higher priority are more likely to be elected primary.**

Raising a node's priority doesn't guarantee that node will be elected primary, but it does increase the likelihood.

**Incorrect answers:**

**Elections can take place anytime while the primary is available.**

If a majority of nodes are unavailable, elections cannot take place.

**All nodes have an equal chance to become primary.**

Priority and recency of a node's oplog dictates which nodes are more likely to become primary.


## 22. Tema: Escribir Concerns (Inquietudes): Parte 1

### Transcripción

La preocupación de escritura (write concern) de MongoDB es un mecanismo de reconocimiento que los desarrolladores pueden agregar a las operaciones de escritura.

Los niveles más altos de reconocimiento producen una garantía de durabilidad más fuerte.

La durabilidad significa que la escritura se ha propagado al número de nodos miembros del replica set especificados en la preocupación de escritura.

Cuantos más miembros del replica set reconozcan el éxito de una escritura, más probable es que la escritura sea duradera en caso de falla.

La mayoría aquí se define como una mayoría simple de miembros del replica set.

Entonces divide por dos y redondea.

La compensación con una mayor durabilidad es el tiempo.

Más durabilidad requiere más tiempo para lograr la garantía de durabilidad especificada, ya que debe esperar esos reconocimientos.

Repasemos los niveles de preocupación de escritura disponibles.

Una preocupación de escritura de cero significa que la aplicación no espera ningún reconocimiento.

La escritura puede tener éxito o fallar.

La aplicación realmente no le importa.

Solo comprueba que se puede conectar con éxito al nodo.

Piense en esto como una estrategia de disparar y olvidar: muy rápido, pero sin controles de seguridad.

La preocupación de escritura predeterminada es una.

Eso significa que la aplicación espera un acuse de recibo de un solo miembro del replica set, específicamente, el primario.

Esta es una garantía básica de éxito.

Escribir inquietudes superiores a uno aumenta el número de reconocimientos para incluir uno o más miembros secundarios.

Los niveles más altos de preocupación de escritura corresponden a una garantía más fuerte de durabilidad de escritura.

Por ejemplo, puedo establecer una preocupación de escritura de tres para requerir el reconocimiento de los tres miembros del replica set.

La mayoría es una palabra clave que se traduce en la mayoría de los miembros del replica set.

Es una mayoría simple.

Entonces divida el número de miembros entre dos y redondee.

Entonces, este replica set de tres miembros tiene una mayoría de dos.

Un replica set de cinco miembros tendría una mayoría de tres, y así sucesivamente.

Lo bueno de la mayoría es que no tiene que actualizar su problema de escritura si aumenta el tamaño de su replica set.

También hay un nivel de preocupación de escritura para las etiquetas de replica set.

Eso es un pequeño avance para esta serie de cursos, pero puede consultar nuestra documentación sobre preocupación por escrito para obtener más información.

Solo estamos hablando de replica set en esta lección.

Pero MongoDB admite la preocupación de escritura tanto para clústeres independientes como fragmentados.

Para los grupos fragmentados en particular, la preocupación de escritura se reduce al nivel de fragmento.

Finalmente, quiero dejar muy claro que, sin importar la preocupación de escritura que especifique, MongoDB siempre replica los datos en todos los nodos portadores de datos del clúster.

La preocupación de escritura está ahí para que tenga una forma de rastrear la durabilidad de los datos insertados.

Hay dos opciones adicionales de preocupación de escritura que proporciona MongoDB.

El primero es `wtimeout`.

Esto le permite establecer una cantidad máxima de tiempo que la aplicación espera antes de marcar una operación como fallida.

Una nota importante aquí: golpear un error de tiempo de espera no significa que la operación de escritura en sí haya fallado.

Solo significa que la aplicación no obtuvo el nivel de durabilidad que solicitó.

El segundo es `j`, o `journal`.

Esta opción requiere que cada miembro del replica set reciba la escritura y se comprometa con el diario archivado para que la escritura se considere reconocida.

Comenzando con MongoDB 3.2.6, una preocupación de escritura de mayoría implica que `j` es igual a verdadero.

La ventaja de establecer `j` es igual a verdadero para cualquier preocupación de escritura dada es que tiene una garantía más sólida de que no solo se recibieron las escrituras, sino que se escribieron en un diario en el disco.

Si establece `j` en falso, o si el registro en diario está deshabilitado en el mongod, el nodo solo necesita almacenar los datos en la memoria antes de informar el éxito.

## 23. Tema: Escribir Concerns (Inquietudes): Parte 2

### Transcripción

En general, los comandos básicos de escritura admiten todos los problemas de escritura.

Ahora, hemos hablado de todo esto en un nivel bastante alto.

Write concern (Escribir preocupación) tiene mucho más sentido cuando lo ves en acción.

En una lección anterior, le mostramos cómo una aplicación escribe "behave"("comportarse") durante un evento de conmutación por error.

Volvamos a ese ejemplo ahora, pero para hablar sobre la write operation (preocupación por escrito).

Tenemos nuestra réplica de tres miembros configurada con una aplicación que inserta datos en el primario.

Como recordará, los datos se replican desde el primario al secundario.

La write operation predeterminada es una.

Entonces, aunque no establezcamos la write operation explícitamente, la aplicación le asigna implícitamente una preocupación de escritura de uno.

En este punto, sabemos que nuestro primario ha recibido la operación de escritura.

Hemos recibido un reconocimiento basado en nuestra preocupación por escrito.

Como eso coincide con la write operation que solicitamos, estamos listos para comenzar.

Pero, ¿qué pasaría si la primaria fallara en este punto antes de completar la replicación de esta escritura a las secundarias?

Los secundarios no verán la escritura.

Si bien tuvo éxito en un solo nodo, no teníamos garantía de que la escritura se hubiera propagado a los miembros restantes del conjunto de réplicas.

Aunque obtuvimos el nivel de preocupación por escritura que solicitamos, el nivel de garantía era demasiado bajo para acomodar el escenario.

En lo que respecta a nuestra aplicación, esta fue una escritura exitosa.

Pero cuando este primario vuelva a estar en línea, el derecho en realidad se revertirá.

Imaginemos el mismo escenario pero con una write operation de la mayoría.

Eso significa que ahora necesitaríamos un reconocimiento de dos miembros de nuestro replica set de tres miembros.

La aplicación espera hasta que el primario informa que al menos uno de los secundarios también ha confirmado la escritura.

Con dos reconocimientos, la aplicación puede considerar la escritura como un éxito.

Ahora, si el primario se cae, tenemos una garantía razonable de que al menos uno de los secundarios ha recibido la operación de escritura.

Si establezco la preocupación de escritura en tres, la aplicación requerirá un reconocimiento de los tres miembros antes de considerar la escritura exitosa.

En este punto, puede estar pensando, bueno, quiero garantías de durabilidad de los datos.

Entonces, ¿no debería establecer siempre mi preocupación de escritura lo más alta posible?

Bueno, un nivel más fuerte de garantía de durabilidad viene con la compensación.

Específicamente, debe esperar a que lleguen los reconocimientos.

Eso significa que sus operaciones de escritura pueden tardar más de lo requerido si solicitó un acuse de recibo menor o incluso no.

A medida que aumenta el número de nodos del replica set, las operaciones de escritura pueden tardar aún más en volver como exitosas.

Alternativamente, ¿qué sucede si uno de sus secundarios está caído?

En función de nuestra preocupación por la escritura, necesitamos un reconocimiento de tres miembros del conjunto de réplicas.

Esta escritura ahora bloquea hasta que el secundario vuelve a estar en línea, lo que puede llevar más tiempo del aceptable.

Aquí es donde la opción de tiempo de espera `W` es útil.

Puede configurarlo en un tiempo razonable en el que la aplicación deja de esperar y avanza arrojando un error relacionado con la preocupación de escritura.

Recuerde, el error de tiempo de espera no significa que la escritura falló.

Podemos ver aquí que la primaria y al menos una secundaria sí lo reconocieron.

Pero debido a que se agotó el tiempo de espera para la inquietud de escritura solicitada, en lo que respecta a la solicitud, no recibió el nivel de garantía que solicitó.

En general, establecer una `W` de mayoría es un buen punto medio entre escrituras rápidas y garantías de durabilidad.

En resumen, la preocupación de escritura permite que sus aplicaciones soliciten un cierto número de confirmaciones para una operación de escritura para el clúster MongoDB.

Estos reconocimientos representan garantías de durabilidad cada vez mayores para determinadas operaciones de escritura.

La preocupación de escritura viene con un intercambio de velocidad de escritura.

Cuanto mayor sea la garantía que necesita de que un derecho sea duradero, más tiempo requiere la operación de escritura general para completarse.

MongoDB admite una preocupación de escritura en todos los tipos de implementación.

Eso significa que los replica set y clústeres fragmentados independientes son compatibles con la preocupación de escritura.

## 24. Examen Write Concerns: Part 2

**Problem:**

Consider a 3-member replica set, where one secondary is offline. Which of the following write concern levels can still return successfully?

Check all answers that apply:

* 3

* all

* majority :+1:

* online

**See detailed answer**

Majority is the correct answer. "all" and "online" are not valid write concerns. "3" would block indefinitely if one out of three members was offline.

## 25. Laboratorio: Escribe con Failovers (Conmutación por Error)

Lab - Writes with Failovers

**Problem:**

In this lab, you will attempt to write data with a `writeConcern` to a replica set where one node has failed.

In order to simulate a node failure within your replica set, you will connect to the node individually and shut it down. Connecting back to the replica set and running `rs.status()` should show the failing node with a description like this:

```sh
{
  "name" : "m103:27001",
  "health" : 0,
  "stateStr" : "(not reachable/healthy)",
  "lastHeartbeatMessage" : "Connection refused",
  "configVersion" : -1
}
```

With one of your nodes down, attempt to insert a document in your replica set by running the following commands:

```sh
use testDatabase
db.new_data.insert({"m103": "very fun"}, { writeConcern: { w: 3, wtimeout: 1000 }})
```

This will attempt to insert one record into a collection called `testDatabase.new_data`, while verifying that 3 nodes registered the write. It should return an error, because only 2 nodes are healthy.

Given the output of the insert command, and your knowledge of `writeConcern`, check all that apply:

Attempts Remaining:3 Attempts left

Check all answers that apply:

* When a `writeConcernError` occurs, the document is still written to the healthy nodes. :+1:

* `w: "majority"` would also cause this write operation to return with an error.

* The write operation will always return with an error, even if `wtimeout` is not specified.

* The unhealthy node will be receiving the inserted document when it is brought back online. :+1:

#### See detailed answer

**Correct:**

* When a `writeConcernError` occurs, the document is still written to the healthy nodes.

The `WriteResult` object simply tells us whether the `writeConcern` was successful or not - it will not undo successful writes from any of the nodes.

* The unhealthy node will have the inserted document when it is brought back online.

When the unhealthy node comes back online, it rejoins the replica set and its oplog is compared with the other nodes' oplogs. Any missing operations will be replayed on the newly healthy node.

**Incorrect:**

* `w: "majority"` would also cause this write operation to return with an error.

`w: "majority"` requests acknowledgement that a majority of nodes in a replica set have registered the write. In a three-node replica set, only two nodes are required for a majority, so the two healthy nodes are sufficient to satisfy this `writeConcern`.

* The write operation will always return with an error, even if `wtimeout` is not specified.

If `wtimeout` is not specified, the write operation will be retried for an indefinite amount of time until the `writeConcern` is successful. If the `writeConcern` is impossible, like in this example, it may never return anything to the client.


## 26. Tema: Leer Concerns (Preocupaciones)

### Notas de lectura

Instrucciones de lectura

Nota: A la 1:04, el instructor quería decir: "para documentos **devueltos** por una operación de lectura". A las 3:05, el instructor quiso decir: "no admite la preocupación de **lectura** de la mayoría".

### Transcripción

La preocupación de lectura es un complemento de la preocupación de escritura, un mecanismo de reconocimiento para las operaciones de lectura donde los desarrolladores pueden dirigir su aplicación para realizar operaciones de lectura donde los documentos devueltos cumplen con la garantía de durabilidad solicitada.

Considere un replica set donde las aplicaciones insertan un documento, que es reconocido por el primario.

Antes de que el documento pueda replicarse en las secundarias, la aplicación lee ese documento.

De repente, la primaria falla.

El documento que hemos leído aún no se ha replicado en las secundarias.

Cuando el antiguo primario vuelva a estar en línea, esos datos se revertirán como parte del proceso de sincronización.

Si bien puede acceder a los datos retrotraídos manualmente, la aplicación efectivamente tiene un dato que ya no existe en el clúster.

Esto puede causar problemas en la aplicación, dependiendo de su arquitectura.

La preocupación de lectura proporciona una forma de abordar el problema de la durabilidad de los datos durante un evento de conmutación por error.

Al igual que la preocupación de escritura le permite especificar qué tan duraderas deberían ser las operaciones de escritura, la preocupación de lectura le permite a su aplicación especificar una garantía de durabilidad para los documentos escritos por una operación de lectura.

La operación de lectura solo devuelve datos reconocidos como escritos en un número de miembros del replica set especificados en el asunto de lectura.

Puede elegir entre devolver los datos más recientes en un clúster o los datos recibidos por la mayoría de los miembros del clúster.

Una nota realmente importante.

Un documento que no cumple con la preocupación de lectura especificada no es un documento que se garantiza que se perderá.

Simplemente significa que en el momento de la lectura, los datos no se habían propagado a nodos suficientes para cumplir con la garantía de durabilidad solicitada.

Hay cuatro niveles de preocupación de lectura.

Local devuelve los datos más recientes en el clúster.

Cualquier información recién escrita en el primario califica para la preocupación de lectura local.

No hay garantías de que los datos estarán seguros durante un evento de conmutación por error.

Las lecturas locales son las predeterminadas para las operaciones de lectura contra las primarias.

Las inquietudes de lectura disponibles son las mismas que las inquietudes de lectura locales para las implementaciones de replica set.

La preocupación de lectura disponible es el valor predeterminado para operaciones de lectura contra miembros secundarios.

Las lecturas secundarias son un aspecto de la preferencia de lectura de MongoDB, que se trata en una lección posterior.

La principal diferencia entre la preocupación de lectura local y disponible está en el contexto de agrupaciones fragmentadas.

Vamos a hablar más sobre grupos fragmentados más adelante.

Es suficiente saber que la preocupación de lectura disponible tiene un comportamiento especial en grupos fragmentados.

La preocupación de lectura mayoritaria solo devuelve datos que han sido reconocidos como escritos a la mayoría de los miembros del replica set.

Entonces, aquí en nuestro replica set de tres miembros, nuestras operaciones de lectura solo devolverían aquellos documentos escritos tanto en el primario como en el secundario.

La única forma en que los documentos devueltos por una operación de lectura mayoritaria de preocupación de lectura podrían perderse es si la mayoría de los miembros del replica set cayeron y los documentos no se replicaron a los miembros restantes del replica set, lo cual es una situación muy poco probable, dependiendo de su implementación arquitectura.

La preocupación por la lectura mayoritaria proporciona la garantía más fuerte en comparación con las escrituras locales o disponibles.

Pero la compensación es que es posible que no obtenga los datos más recientes o más recientes en su clúster.

Como nota especial, el motor de almacenamiento MMAPv1 no admite la preocupación de escritura de la mayoría.

La preocupación de lectura linealizable se agregó en MongoDB 3.4 y tiene un comportamiento especial.

Al igual que la preocupación de lectura mayoritaria, también solo devuelve datos que se han comprometido mayoritariamente.

Linearizeable va más allá de eso y proporciona funciones de lectura y escritura.

Entonces, ¿cuándo deberías usar lo que lees?

Realmente depende de los requisitos de su aplicación.

Datos rápidos, seguros o más recientes en su clúster.

Elige dos.

Esa es la idea general.

Si desea lecturas rápidas de los últimos datos, la preocupación por la lectura local o disponible debería ser adecuada para usted.

Pero perderá su garantía de durabilidad.

Si desea lecturas rápidas de los datos más seguros, la preocupación de lectura mayoritaria es un buen término medio.

Pero, de nuevo, es posible que no reciba los últimos datos escritos en su clúster.

Si desea lecturas seguras de los últimos datos en el momento de la operación de lectura, entonces linealizar es una buena opción, pero es más probable que sea más lento.

Y es solo un documento de solo lectura.

Una cosa para ser realmente clara es que la preocupación por la lectura no impide eliminar datos mediante una operación CRUD, como eliminar.

Solo estamos hablando de la durabilidad de los datos devueltos por una operación de lectura en el contexto de un evento de conmutación por error.

En una lección posterior, hablaremos sobre la preferencia de lectura, donde puede enrutar sus operaciones de lectura a miembros secundarios.

La preferencia de lectura para los secundarios le permite especificar la preocupación de lectura local o disponible, pero perderá la garantía de obtener los datos más recientes.

Así que recuerda que con las lecturas secundarias, la preocupación por la lectura local y disponible es realmente rápida, pero no necesariamente la última.

Para recapitular, la preocupación de lectura utiliza un mecanismo de reconocimiento para las operaciones de lectura donde las aplicaciones pueden solicitar solo aquellos datos que cumplan con la garantía de durabilidad solicitada.

Hay cuatro opciones de lectura disponibles.

Local y disponible son idénticos para replica set.

Su comportamiento es más distinto en grupos fragmentados.

La preocupación de lectura mayoritaria es su punto medio entre la durabilidad y las lecturas rápidas, pero puede devolver datos obsoletos.

Linearizeable tiene las garantías de durabilidad más sólidas y siempre devuelve los datos más recientes en el contexto de una operación de lectura a cambio de operaciones de lectura potencialmente lentas, así como las restricciones y requisitos generales para usarlo.

Finalmente, puede usar la preocupación de lectura junto con la preocupación de escritura para especificar garantías de durabilidad tanto en lecturas como en escrituras.

Recuerde que los dos trabajan juntos, pero no dependen el uno del otro.

Puede usar preocupación de lectura con preocupación de escritura o preocupación de lectura sin preocupación de escritura.

## 27. Examen Read Concerns

**Problem:**

Which of the following read concerns only return data from write operations that have been committed to a majority of nodes?

Check all answers that apply:

* linearizable :+1:

* local

* majority :+1:

* available

**See detailed answer**

None of the read concerns require you to specify a write concern. However, reads with the read concern `majority` and `linearizable` will only return data that has been replicated to a majority of nodes in the replica set.

The difference between `majority` and `linearizable` lies in sense of [causal consistency](https://en.wikipedia.org/wiki/Causal_consistency) that `linearizable` enforces.

Let us look into this in detail:

When reading from a replica set with readConcern `majority` all documents that have been majority committed by the replica set will be returned to the application.

In the following diagram we have a set of operations taking place:

* W1 : first write operation

* W2 : second write operation

* RM : read with read concern majority.

<img src="images/m103/c2/readconcern+majority.png">

**RM** will return every document, matching the query selector, that has been majority committed, by the time the server receives the **RM**. In this case, **W1** would be returned.

Read concern `linearizable` will wait for all prior writes to be replicated to a majority of nodes before it returns a response.

In the following diagram we have:

* W1 : first write operation

* W2 : second write operation

* RL : read with read concern linearizable

<img src="images/m103/c2/readconcern+linearizable.png">

The response for the read operation will wait until all writes, received by the server prior to **RL**, are majority committed before returning the document to the client. In this case, both **W1** and **W2** would be available to be returned to the client.

## 28. Tema: Leer preferencias

### Transcripción

La preferencia de lectura permite a sus aplicaciones enrutar operaciones de lectura a miembros específicos de un replica set.

La preferencia de lectura es principalmente una configuración del lado del conductor.

Asegúrese de consultar la documentación del controlador para obtener instrucciones completas sobre cómo especificar una preferencia de lectura para sus operaciones de lectura.

Tome este replica set de tres miembros, por ejemplo.

Por defecto, sus aplicaciones leen y escriben datos en el primario.

Con los replica set, los datos se replican a todos los miembros con datos.

Por lo tanto, ambos secundarios tendrían eventualmente copias de los datos primarios.

¿Qué sucede si, en cambio, quisiéramos que nuestra aplicación prefiriera leer de un miembro secundario?

Con la preferencia de lectura, podemos dirigir la aplicación para enrutar su consulta a un miembro secundario del conjunto de réplica, en lugar del primario.

Hay cinco modos de preferencia de lectura compatibles.

La preferencia de lectura primaria enruta todas las operaciones de lectura solo a la primaria.

Esta es la preferencia de lectura predeterminada.

Primaria Las rutas preferidas leen operaciones a la primaria.

Pero si el primario no está disponible, como durante una elección o un evento de conmutación por error, la aplicación puede enrutar las lecturas a un miembro secundario disponible.

Las rutas secundarias leen operaciones solo a los miembros secundarios en el replica set.

Las rutas preferidas secundarias leen operaciones a los miembros secundarios.

Pero si no hay miembros secundarios disponibles, la operación se dirige a la primaria.

Las rutas más cercanas leen operaciones al miembro del replica set con la menor latencia de red al host, independientemente del tipo de miembros.

Esto generalmente admite operaciones de lectura geográficamente locales.

Con lecturas secundarias, siempre tenga en cuenta que, dependiendo de la cantidad de latencia de replicación en un replica set, puede recibir datos obsoletos.

Por ejemplo, supongamos que este replica set recibe una operación de escritura que actualiza este documento, cuyo nombre es Mongo 101, para tener un estado de publicado.

En este momento, el secundario todavía tiene la versión anterior de este documento donde el estado está pendiente.

Si publico una lectura secundaria, al usar la preferencia de lectura más cercana se selecciona el miembro secundario para leer, obtendré una versión anterior de este documento.

Esta tabla le da una idea de algunos de los escenarios en los que usaría una preferencia de lectura determinada.

La gran conclusión aquí es que las lecturas secundarias siempre tienen la posibilidad de devolver datos obsoletos.

La importancia de los datos depende de la demora que haya entre su primario y sus secundarios.

Los replica set distribuidas geográficamente tienen más probabilidades de sufrir lecturas obsoletas, por ejemplo, que un replica set donde todos los miembros están en la misma región geográfica, o incluso en el mismo centro de datos.

Para resumir, la preferencia de lectura le permite elegir a qué miembros del replica set enrutar las operaciones de lectura.

El gran inconveniente de usar una preferencia de lectura, que no sea primaria, es el potencial de las operaciones de lectura obsoleta.

Y la preferencia de lectura más cercana es más útil si desea admitir lecturas geográficamente locales.

Solo recuerde que puede venir con el potencial de leer datos obsoletos.

## 29. Examen Read Preferences

**Problem:**

Which of the following read preference options may result in stale data?

Check all answers that apply:

* secondaryPreferred :+1:

* secondary :+1:

* primaryPreferred :+1:

* nearest :+1:

* primary

**See detailed answer**

When using the default read preference `primary`, data will always be read from the primary node, and will therefore be the most recent copy.

When using any other read preference, data may be read from a secondary node, which could result in reading slightly stale data.

## 30. Laboratorio - Preocupación de lectura y preferencias de lectura

Lab - Read Concern and Read Preferences

**Problem:**

In this lab, you will take advantage of different read preferences to increase the availability of your replica set.

To begin, load the dataset in your Vagrant box into your replica set:

```sh
mongoimport --drop \
--host m103-repl/192.168.103.100:27002,192.168.103.100:27001,192.168.103.100:27003 \
-u "m103-admin" -p "m103-pass" --authenticationDatabase "admin" \
--db applicationData --collection products /dataset/products.json
```

You can check that you've loaded the entire dataset by verifying that there are exactly 516784 documents in the `applicationData.products` collection:

```sh
use applicationData
db.products.count()
```

Once the dataset is fully imported into your replica set, you will simulate a node failure. This is similar to the previous lab, but this time you will shut down **two** nodes.

When two of your nodes are unresponsive, you will not be able to connect to the replica set. You will have to connect to the third node, which should be the only healthy node in the cluster.

Which of these `readPreferences` will allow you to read data from this node?

Check all answers that apply:

* secondaryPreferred :+1:

* secondary :+1:

* primaryPreferred :+1:

* primary

* nearest :+1:

#### See detailed answer

The key concept to understand here is that when two nodes go down in a three-node replica set, the third node becomes a secondary regardless of whether it started as a primary.

Therefore, connecting to the third node is the same as connecting to a secondary node, and any `readPreference` will work except for `primary`, which requires all operations to read from the primary node.
