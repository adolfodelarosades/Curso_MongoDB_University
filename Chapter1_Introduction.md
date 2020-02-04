# Chapter 1: Introduction

## Lecture:  Welcome!

## Lecture: Granding and Logistics

A medida que comenzamos con este curso, me gustaría hablar sobre la calificación y algunos otros aspectos logísticos solo para asegurarme de que sepa cómo funciona este curso.

Verá tres tipos diferentes de ejercicios en este curso: cuestionarios, laboratorios, que también llamamos tarea, y un examen final.

Verá cuestionarios después de la mayoría de los videos de lecciones en este curso.

Todas las pruebas no están calificadas.

Están allí para permitirle evaluar su comprensión del material de la lección.

Verá laboratorios, o tareas, como a veces los llamamos, a lo largo de cada capítulo.

Los puntajes que obtiene en sus laboratorios representan el 50% de su calificación final.

Luego, al final del curso, hay un examen final.

Esto representa el otro 50% de su calificación.

En términos de un horario del curso, en la primera semana de este curso, lanzamos el capítulo uno.

Lanzamos el capítulo dos en la segunda semana, y luego en la tercera semana del curso, lanzamos tanto el capítulo tres como el examen final.

Y no te preocupes, estos dos juntos son lo suficientemente cortos como para que puedas completarlos todos en la semana tres.

Asegúrese de revisar el programa del curso para conocer las fechas exactas de lanzamiento de las semanas uno, dos y tres.

En el plan de estudios, también encontrará las fechas de vencimiento de cuándo deben entregarse los laboratorios o la tarea para cada semana.

Una vez que hayamos aprobado la fecha límite para el examen final, si ha logrado una calificación de 65% o más, recibirá una declaración de finalización de este curso.

Y nuevamente, generamos las declaraciones de finalización después de la fecha de vencimiento para el examen final.

Eso es todo para el curso esencial de logística.

Nos alegra que estés aprendiendo con nosotros y buena suerte.

## Lecture: Are you Behind a Firewall?

### ¿Estás detrás de un firewall?

Para continuar en este curso, debe poder realizar solicitudes salientes desde su computadora a los servidores de bases de datos que hemos configurado en MongoDB Atlas. Esos servidores se ejecutan en el puerto 27017 en Amazon AWS.

**Confirme que el puerto 27017 no está bloqueado haciendo clic en [http://portquiz.net:27017](http://portquiz.net:27017).**

Si tiene éxito, verá una carga de página que indica que puede realizar solicitudes salientes en el puerto 27017.

Si una página no se carga y su solicitud finalmente agota el tiempo, el tráfico saliente al puerto 27017 probablemente esté bloqueado en su red local. Si este es el caso, comuníquese con su departamento de TI para ver si hay una solución o intente realizar la solicitud desde otra ubicación.

Otras pruebas útiles para realizar si parece que tiene dificultades para conectarse serían:

1. Conéctese al host y al puerto del clúster Atlas con:

```sh
telnet cluster0-shard-00-00-jxeqq.mongodb.net 27017
```

2. Conéctese al host y al puerto del clúster Atlas con:

```sh
ping cluster0-shard-00-00-jxeqq.mongodb.net
```

