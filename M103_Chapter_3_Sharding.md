# Capítulo 3: Sharding

### 28 Items

Asignaciones calificadas

## Contenido

1. Tema: ¿Qué es Sharding?
2. Tema: Cuándo fragmentar
3. Examen
4. Tema: Arquitectura de fragmentación
5. Examen
6. Tema: Configuración de un grupo fragmentado
7. Examen
8. Laboratorio: configurar un clúster fragmentado
9. Tema: Config DB
10. Examen
11. Tema: Shard Keys
12. Examen
13. Tema: Escoger una buena clave de fragmento
14. Examen
15. Tema: Hashed Shard Keys
16. Examen
17. Lab - Shard a Collection
18. Tema: trozos
19. Examen
20. Laboratorio - Documentos en trozos
21. Tema: Equilibrio
22. Examen
23. Tema: Consultas en un grupo fragmentado
24. Examen
25. Tema: Consultas enrutadas vs Scatter Gather: Parte 1
26. Tema: Consultas enrutadas vs Scatter Gather: Parte 2
27. Examen
28. Laboratorio: detección de consultas de recopilación de dispersión


## 1. Tema: ¿Qué es Sharding?

### Transcripción

Entonces, hasta este punto, hemos aprendido sobre las implementaciones de MongoDB de tamaños pequeños y medios.

Por lo tanto, es factible almacenar un conjunto de datos completo en un servidor.

<img src="images/m103/c3/3-1-one-server.png">

En un replica set, tenemos más de un servidor en nuestra base de datos.

<img src="images/m103/c3/3-1-tres-servers.png">

Pero cada servidor todavía tiene que contener todo el conjunto de datos.

A medida que nuestro conjunto de datos crece hasta el punto en que nuestras máquinas no pueden atender adecuadamente las aplicaciones de los clientes, una de nuestras opciones es mejorar las máquinas.

Podríamos aumentar la capacidad de las máquinas individuales para que tengan más RAM, o espacio en disco, o tal vez una CPU más potente.

<img src="images/m103/c3/3-1-recursos.png">

Esto se conoce como **escala vertical**.

<img src="images/m103/c3/3-1-vertical-scaling.png">

Pero esto podría llegar a ser muy costoso.

<img src="images/m103/c3/3-1-costoso.png">

Y además, los proveedores basados en la nube no nos permitirán escalar verticalmente para siempre.

<img src="images/m103/c3/3-1-finito.png">

Eventualmente pondrán un límite a las posibles configuraciones de hardware, lo que limitaría efectivamente nuestra capa de almacenamiento.

**En MongoDB, el escalado se realiza horizontalmente,**

<img src="images/m103/c3/3-1-hs-1.png">

**lo que significa que, en lugar de mejorar las máquinas individuales, **

<img src="images/m103/c3/3-1-hs-2.png">

**simplemente agregamos más máquinas**

<img src="images/m103/c3/3-1-hs-3.png">

**y luego distribuimos el conjunto de datos entre esas máquinas.**

<img src="images/m103/c3/3-1-hs-4.png">

La forma en que distribuimos datos en MongoDB se llama **Sharding**.

<img src="images/m103/c3/3-1-sharding-1.png">

Y Sharding nos permite hacer crecer nuestro conjunto de datos sin preocuparnos de poder almacenarlo todo en un servidor.

En su lugar, dividimos el conjunto de datos en partes 

<img src="images/m103/c3/3-1-sharding-2.png">

y luego las distribuimos en tantos fragmentos como queramos.

<img src="images/m103/c3/3-1-sharding-3.png">

<img src="images/m103/c3/3-1-sharding-4.png">

Juntos, los fragmentos forman un Sharded Cluster (clúster fragmentado).

<img src="images/m103/c3/3-1-sharding-5.png">

Para garantizar una alta disponibilidad en nuestro Sharded Cluster, implementamos cada fragmento como un replica set.

<img src="images/m103/c3/3-1-sharding-6.png">

<img src="images/m103/c3/3-1-sharding-7.png">

De esta manera, podemos garantizar un nivel de tolerancia a fallos contra cada pieza de datos, independientemente de qué fragmento contenga realmente esos datos.

Entonces, con nuestros datos distribuidos en varios servidores, las consultas pueden volverse un poco complicadas.

Consultamos nuestra base de datos buscando un documento específico.

<img src="images/m103/c3/3-1-search.png">

Al principio no es obvio dónde buscarlo.

Entonces, entre un Sharded Cluster (clúster fragmentado) y sus clientes, configuramos un tipo de proceso de enrutador que acepta consultas de los clientes y luego determina qué fragmento debe recibir esa consulta.

Ese proceso de enrutador se llama **Mongos**.

<img src="images/m103/c3/3-1-mongos-1.png">

Y los clientes se conectan a Mongos en lugar de conectarse a cada fragmento individualmente.

Y tenemos cualquier cantidad de procesos de Mongos para que podamos atender muchas solicitudes o solicitudes diferentes al mismo Sharded Cluster(Clúster fragmentado).

<img src="images/m103/c3/3-1-mongos-2.png">

¿Entonces Mongos debe ser bastante pequeño, correcto, para saber dónde está cada dato en un momento dado en un Sharded Cluster masivo?

Pero en realidad, Mongos no sabe nada.

Utiliza los metadatos sobre qué datos están contenidos en cada fragmento.

<img src="images/m103/c3/3-1-metadata-1.png">

Y esos metadatos se almacenan en los servidores de configuración.

Pero los datos en los Servidores de Configuración son utilizados muy a menudo por Mongos.

<img src="images/m103/c3/3-1-metadata-2.png">

Por lo tanto, debemos asegurarnos de que los datos permanezcan altamente disponibles.

Y probablemente pueda adivinar cómo garantizamos la alta disponibilidad aquí.

Sí, usamos replicación.

<img src="images/m103/c3/3-1-replication-1.png">

Replicamos los datos en los servidores de configuración.

Entonces, en lugar de un único Config Server (servidor de configuración), implementamos un Replica Set del Config Server (servidor de configuración).

<img src="images/m103/c3/3-1-replication-2.png">

Así que esa es una descripción general de alto nivel de Sharding en MongoDB: el Cluster Sharded contiene los fragmentos donde residen los datos; 

<img src="images/m103/c3/3-1-resumen-1.png">

los Config Servers (servidores de configuración), que contienen los metadatos de cada fragmento; 

<img src="images/m103/c3/3-1-resumen-2.png">

y los Mongos, que encamina las consultas a los fragmentos correctos.

<img src="images/m103/c3/3-1-resumen-3.png">

## 2. Tema: Cuándo Fragmentar

### Transcripción

## 3. Examen

## 4. Tema: Arquitectura de fragmentación

### Transcripción

## 5. Examen

## 6. Tema: Configuración de un grupo fragmentado

### Transcripción

## 7. Examen

## 8. Laboratorio: configurar un clúster fragmentado

## 9. Tema: Config DB

### Transcripción

## 10. Examen

## 11. Tema: Shard Keys

### Transcripción

## 12. Examen

## 13. Tema: Escoger una buena clave de fragmento

### Transcripción

## 14. Examen

## 15. Tema: Hashed Shard Keys

### Transcripción

## 16. Examen

## 17. Lab - Shard a Collection

## 18. Tema: trozos

### Transcripción

## 19. Examen

## 20. Laboratorio - Documentos en trozos

## 21. Tema: Equilibrio

### Transcripción

## 22. Examen

## 23. Tema: Consultas en un grupo fragmentado

### Transcripción

## 24. Examen

## 25. Tema: Consultas enrutadas vs Scatter Gather: Parte 1

### Transcripción

## 26. Tema: Consultas enrutadas vs Scatter Gather: Parte 2

### Transcripción

## 27. Examen

## 28. Laboratorio: detección de consultas de recopilación de dispersión


