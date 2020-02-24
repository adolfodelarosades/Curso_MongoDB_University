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
sudo mkdir -p /var/mongodb/pki/
sudo chown vagrant:vagrant /var/mongodb/pki/
openssl rand -base64 741 > /var/mongodb/pki/m103-keyfile
chmod 400 /var/mongodb/pki/m103-keyfile
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
$ mkdir -p /var/mongodb/db/node1
$ mongod -f node1.conf
```

Y hemos comenzado con éxito nuestro primer nodo.

Así que ahora tenemos un nodo y solo nos quedan dos más.

Entonces, este comando simplemente está copiando el archivo que acabamos de crear en un nuevo archivo llamado `node2.conf` porque los otros dos nodos tendrán configuraciones muy similares.

```sh
cp node1.conf node2.conf
```

Básicamente podemos copiar este, cambiar tres líneas y lanzar un nuevo nodo.

Nunca subestimes el poder de copiar y pegar.

Voy a hacer lo mismo para nuestro tercer nodo aquí

```sh
cp node2.conf node3.conf
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
$ mkdir /var/mongodb/db/node2
$ mongod -f node2.conf

MAS TEXTO
```

Y ahora tenemos dos nodos en nuestro conjunto.

Solo voy a hacer lo mismo para nuestro tercer archivo de configuración, y notaré que los tres nodos en el conjunto de réplica hacen referencia al mismo archivo de clave.

```sh
vi node3.conf
```

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
$ mkdir /var/mongodb/db/node3
$ mongod -f node3.conf

MAS TEXTO
```

Por lo general, estas instancias mongod se estarían ejecutando en diferentes máquinas, pero debido a que se están ejecutando en la misma máquina, todas compartirán el mismo archivo de clave y usarán el mismo para autenticarse entre sí.

```sh
keyFile: /var/mongodb/pki/m103-keyfile
```

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
$ mongo --port 27011
```

Entonces uso este comando `rs.initiate` para iniciar el conjunto de réplicas.

```sh
$ rs.initiate()
```

Y en realidad necesitamos ejecutarlo en uno de los nodos.

Como lo ejecutamos aquí, solo tenemos que agregar los otros dos nodos desde este nodo.

Sin embargo, tenemos habilitada la autenticación del cliente, por lo que no podemos agregar otros nodos al conjunto hasta que creamos un usuario y luego nos conectamos como ese usuario.

Muy bien, entonces este comando creó nuestro súper usuario `m103`, llamado `m103-admin`, que tiene acceso de root y se autentica en la base de datos de administración.

```sh
$ use admin
$ db.createUser({
  user: "m103-admin",
  pwd: "m103-pass",
  roles: [
    {role: "root", db: "admin"}
  ]
})
```

Ahora voy a salir de este mongod y luego volver a iniciar sesión como ese usuario.


```sh
exit
mongo --host "m103-example/192.168.103.100:27011" -u "m103-admin"
-p "m103-pass" --authenticationDatabase "admin"
```


Entonces este es el comando que vamos a usar para conectarnos al conjunto de réplicas.

Y además de autenticar aquí con una contraseña de nombre de usuario, tenemos que especificar el nombre de la réplica establecida en el nombre del host.

Esto le indicará al shell mongo que se conecte directamente al conjunto de réplicas, en lugar de solo este nodo que especificamos.

Lo que va a hacer el shell es usar este nodo para descubrir cuál es el primario actual del conjunto de réplicas y luego conectarse a ese nodo.

En este caso, obviamente solo hay un nodo en el conjunto y ese nodo es el primario.

Entonces esa es la que nos conectó el shell.

Entonces este comando, `rs.status` es una forma útil de obtener un informe de estado en nuestro conjunto de réplicas.

```sh
$ rs.status()
```

Mira, nos da el nombre del set.

Nos da cuánto tiempo son los intervalos entre latidos `heartbeatIntervalMillis`.

Por defecto, son 2000 milisegundos, lo que significa que los nodos están hablando entre sí cada dos segundos.

Podemos desplazarnos hacia abajo para obtener una lista de los miembros del conjunto.

En este caso, es solo un miembro, al cual estamos conectados, el primario actual.


```sh

```


Entonces, este es el comando que usamos para agregar nuevos nodos a nuestro conjunto de réplicas, `rs.add`, 

y todo lo que tenemos que especificar aquí es el nombre de host, que es solo el nombre de host del cuadro Vagrant y el puerto que ese nodo está ejecutando.


```sh
rs.add("m103:27012")
```
Ahora que funcionó.

Solo voy a hacer lo mismo para nuestro tercer nodo.

```sh
rs.add("m103:27013")
```

Solo voy a verificar `rs.isMaster`.

```sh
rs.isMaster()
```

Y podemos ver que nuestro replica set ahora tiene tres nodos.

Entonces, ahora que hemos agregado esos dos nodos a nuestro replica set y los hemos conectado, pueden replicar datos entre sí.

<img src="images/m103/c2/2-5-replicaset-4.png">

Una cosa que quiero señalar en este momento es que el primario actual se está ejecutando en el puerto 27011.

<img src="images/m103/c2/2-5-replicaset-5.png">

Y podríamos verificar eso a partir de la salida de `rs.isMaster`, donde dice que primario es, de hecho, el nodo que se ejecuta en 27011.

```sh
rs.isMaster()
```

Sin embargo, podemos forzar una elección para que un nodo diferente se vuelva primario.

Y el comando que usamos para hacer eso se llama `rs.stepDown`.

```sh
rs.stepDown()
```

Ahora, el comando `stepDown` es lo que usamos para reducir de forma segura el primario actual a un secundario y forzar una elección.

El error que estamos obteniendo aquí es porque el shell está tratando de conectarnos con el primario, pero los secundarios todavía están en el proceso de elegir un primario, por lo que no hay un primario en este momento.

```sh
ERROR
```

Tan pronto como uno sea elegido, el shell nos conectará con él, lo que acaba de hacer.

```sh
NO ERROR
```

Si volvemos a ejecutar `rs.isMaster`, podemos verificar que ahora este nodo `CUAL` es el primario actual, a diferencia de 27011, que era el primario antes.

```sh
rs.isMaster()
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

## 8. Tema: Documento de configuración de replicación

### Transcripción

## 9. Examen

## 10. Tema: Comandos de replicación

### Transcripción

## 11. Examen

## 12. Tema: DB local: Parte 1

### Transcripción

## 13. Tema: DB local: Parte 2

### Transcripción

## 14. Examen

## 15. Tema: reconfigurar un conjunto de réplicas en ejecución

### Transcripción

## 16. Examen

## 17. Laboratorio: eliminar y volver a agregar un nodo

## 18. Tema: Lee y escribe en un conjunto de réplicas

### Transcripción

## 19. Examen

## 20. Tema: conmutación por error y elecciones

### Transcripción

## 21. Examen

## 22. Tema: Escribir inquietudes: Parte 1

### Transcripción

## 23. Tema: Escribir inquietudes: Parte 2

### Transcripción

## 24. Examen

## 25. Laboratorio: escribe con conmutación por error

## 26. Tema: Leer preocupaciones

### Transcripción

## 27. Examen

## 28. Tema: Leer preferencias

### Transcripción

## 29. Examen

## 30. Laboratorio - Preocupación de lectura y preferencias de lectura

