# Capítulo 0: Introducción y configuración

### 6 Artículos

Descripción general del curso, configuración del entorno de desarrollo

## Lecciones

1. Tema: Introducción a la replicación y fragmentación
2. Tema: requisitos previos
3. Tema: Configuración del entorno vagabundo
4. Examen
5. Laboratorio: Configuración del entorno vagabundo
6. Tema: Solución de problemas del entorno vagabundo


## 1. Tema: Introducción a la replicación y fragmentación

### Transcripción

Hola, y bienvenidos a M103, administración básica de clústeres.

Mi nombre es Matt, y voy a ser uno de tus instructores para este curso.

Juntos, aprenderemos sobre la implementación de MongoDB en varias arquitecturas y cómo usar las herramientas administrativas básicas a su disposición.

En el primer capítulo, analizaremos Mongod, que es el proceso central de la base de datos que maneja las solicitudes de datos y administra el acceso a los datos.

<img src="/images/1-mongod.png">

También cubriremos aspectos importantes de la implementación de un solo proceso de Mongod, como habilitar la autenticación y explorar los registros de la base de datos.

<img src="/images/1-mongod-2.png">

En el segundo capítulo, discutiremos cómo MongoDB admite alta disponibilidad al replicar nuestros datos.

Con múltiples copias de datos en diferentes servidores de bases de datos, la replicación proporciona un nivel de tolerancia a fallas frente a la pérdida de un único servidor de bases de datos.

<img src="/images/1-mongod-3.png">

En este curso, veremos la arquitectura general y el comportamiento de un conjunto de réplicas, y las diferentes estrategias de implementación que puede utilizar para adaptarse mejor a su aplicación.

<img src="/images/1-arquitectura.png">

En el capítulo final, discutiremos la escalabilidad y cómo MongoDB escala horizontalmente a través del fragmentación.

<img src="/images/1-ss.png">

En este curso, cubriremos temas como la arquitectura de un clúster fragmentado, cómo se manejan las consultas y cómo elegir la forma en que se distribuyen sus datos.

<img src="/images/1-3-temas.png">

Este curso tiene tres capítulos.

Y con cada capítulo, habrá un nuevo conjunto de videos de lecciones.

Después de cada lección, habrá un cuestionario para evaluar su aprendizaje del material.

Los ejercicios de laboratorio están dispersos a lo largo de cada capítulo para que pueda aplicar lo que aprendió.

Así que repasemos algunas logísticas del curso muy rápidamente.

¿Quién es el público objetivo?

Este es un curso básico.

Por lo tanto, es adecuado para personas en equipos de desarrollo y administración.

Se espera que ya tenga algún conocimiento básico o comprensión de las bases de datos o una familiaridad básica con MongoDB.

Por ejemplo, si tomaste M001 en la Universidad MongoDB y te sientes cómodo con lo que aprendiste en esa clase, entonces estás en el lugar correcto.

¿Cómo funciona la calificación?

Hay cuestionarios, laboratorios y un examen final como entregables de su parte durante el curso.

La mitad de su calificación en la clase estará determinada por su desempeño en los laboratorios, y la otra mitad estará determinada por su desempeño en el examen final.

Los cuestionarios no están calificados y existen principalmente para ayudarlo a rastrear su propia comprensión del material.

Los estudiantes con una calificación de 65% o más recibirán una calificación aprobatoria y un certificado de finalización del curso.

A lo largo del curso, le recomendamos que participe en el foro de discusión.

Proporcionamos asistentes de enseñanza expertos que están allí para ayudar a responder sus preguntas.

Pero también tienes compañeros de clase que a menudo son recursos muy útiles, ya que toman el curso junto a ti.

Al final de este curso, debe estar familiarizado con las diferentes herramientas y técnicas utilizadas para administrar las implementaciones básicas de MongoDB.

Podrá identificar diferentes arquitecturas MongoDB y usar el shell MongoDB para configurarlas.

Entonces, sin más preámbulos, gracias por decidir aprender con nosotros.

Y te deseamos la mejor de las suertes.


## 2. Tema: Requisitos Previos

## 3. Tema: Configuración del Entorno Vagabundo

## 4. Examen

## 5. Laboratorio: Configuración del entorno vagabundo

## 6. Tema: Solución de problemas del entorno vagabundo
