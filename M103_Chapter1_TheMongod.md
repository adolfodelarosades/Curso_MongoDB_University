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


## 2. Examen

## 3. Laboratorio: Lanzamiento de Mongod

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
