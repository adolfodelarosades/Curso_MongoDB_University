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

<img src="/images/m103/c1/1-mongod.png">

También cubriremos aspectos importantes de la implementación de un solo proceso de Mongod, como habilitar la autenticación y explorar los registros de la base de datos.

<img src="/images/m103/c1/1-mongod-2.png">

En el segundo capítulo, discutiremos cómo MongoDB admite alta disponibilidad al replicar nuestros datos.

Con múltiples copias de datos en diferentes servidores de bases de datos, la replicación proporciona un nivel de tolerancia a fallas frente a la pérdida de un único servidor de bases de datos.

<img src="/images/m103/c1/1-mongod-3.png">

En este curso, veremos la arquitectura general y el comportamiento de un conjunto de réplicas, y las diferentes estrategias de implementación que puede utilizar para adaptarse mejor a su aplicación.

<img src="/images/m103/c1/1-arquitectura.png">

En el capítulo final, discutiremos la escalabilidad y cómo MongoDB escala horizontalmente a través del fragmentación.

<img src="/images/m103/c1/1-ss.png">

En este curso, cubriremos temas como la arquitectura de un clúster fragmentado, cómo se manejan las consultas y cómo elegir la forma en que se distribuyen sus datos.

<img src="/images/m103/c1/1-3-temas.png">

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

<img src="/images/m103/c1/1-3-tools.png">

Entonces, sin más preámbulos, gracias por decidir aprender con nosotros.

Y te deseamos la mejor de las suertes.


## 2. Tema: Requisitos Previos

Prerrequisitos

Este curso cubrirá una serie de temas importantes en la administración de la base de datos MongoDB. Sin embargo, no cubre los conceptos básicos del lenguaje de consulta MongoDB.

Para estar completamente preparado para este curso, esperamos que tenga una familiaridad básica con las bases de datos, colecciones y documentos de MongoDB. Si no se siente seguro en estos temas, le recomendamos encarecidamente que tome [`M001: MongoDB Basics`](https://university.mongodb.com/courses/M001/about) primero.

## 3. Tema: Configuración del Entorno Vagrant (Vagabundo)

### Lecture Notes

**Lecture Instructions**

**Note:** For detailed instructions on Downloading and Installing VirtualBox and Vagrant, refer to the Handouts in this lecture for your Operating System. It also contain instructions to setup Vagrant environment in your system.

Throughout this course we'll be using Vagrant as our environment for homework exercises.

Vagrant enables us to have a consistent deployment environment for all students. This makes validating the correctness of a homework solution a lot easier. It will also aid you when asking and answering questions on the discussion forum as you know all of your peers have a near identical environment.

Vagrant requires an hypervisor or virtual environment provider to operate. For this purpose we use VirtualBox.

Below you'll find instructions on how to install [Vagrant](https://www.vagrantup.com/) and [VirtualBox](https://www.virtualbox.org/) on Windows, OSX and Linux.

**Note:** We have recently observed that the latest build of VirtualBox i.e. version 6.1 is having some compatibility issue with Vagrant and some of our users are running into error while setting up their virtual environment.

If in case you have downloaded the version 6.1 then please **uninstall** it and **download** the [VirtualBox version 6.0.14](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0).

**General Notes**

* The installation of the Vagrant environment takes, in a fast network (49Mbps download, 194Mbps upload), around 2:33 minutes to complete. Account for a considerably longer time on slower networks!
* The video instructions might not reflect the exact commands required for your workstation operating system.
* The [Vagrant](https://www.vagrantup.com/) environment files are made available as a downloadable zip file.
* Windows OS 7 and 32-bit systems are not supported.

#### Installing Vagrant on OSX or Linux

1. Install VirtualBox

   Download and installation should be straightforward. Go to the [VirtualBox downloads page](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0), download the setup binary for your appropriate OS, and run the installer.

   If you run into issues installing VirtualBox on a recent version of MacOS, you may want to look at this [knowledge article](https://apple.stackexchange.com/questions/300510/virtualbox-5-1-8-installation-didnt-install-kernel-extensions-how-do-i-fix-thi).

2. Install Vagrant

   After VirtualBox has been installed you can go ahead and [download](https://www.vagrantup.com/downloads.html) and [install](https://www.vagrantup.com/docs/installation/) Vagrant.

#### Installing Vagrant on Windows

1. Install VirtualBox

   Download and installation should be pretty straightforward. Go to the [VirtualBox downloads page](https://www.virtualbox.org/wiki/Download_Old_Builds_6_0), download the setup binary for Windows hosts, and run the installer.

2. Install MinGW

   Vagrant requires two important command-line tools: `rsync` and `ssh`. These command-line tools allow us to connect to our virtual machine and to sync files between our host and virtual machines. MinGW is a set of GNU utilities for software development on Windows. We'll use MinGW to install both `rysnc` and `ssh` onto our Windows machine.

   You should download and run the [MinGW Installer (mingw-get-setup.exe)](https://sourceforge.net/projects/mingw/files/).

3. Install rsync and ssh

   The last step of the installation of MinGW is to specify the packages you want to install. From here you can mark rsync and ssh for installation like so:

   <img src="/images/m103/c3/mingw-rsync.png">
   
   <img src="/images/m103/c3/mingw-ssh.png">
   
   After marking the packages for installation you can go ahead and apply the changes:

   <img src="/images/m103/c3/mingw-apply.png">
   
   Assuming you've kept the default installation path `rsync` and `ssh` should be installed to the `C:\MinGW\msys\1.0\bin` directory. In order for these tools to be accessible via the command-line (and Vagrant) you'll need to add this directory to your PATH environment variable.

   First go to Advanced system settings under System Properties and click "Environment Variables...":

   <img src="/images/m103/c3/system-properties.png">
   
   From there select the "Path" variable and click "Edit...".

   <img src="/images/m103/c3/env-vars.png">
   
   Directories in the PATH environment variable are deliminated by semicolons. You can now append the C:\MinGW\msys\1.0\bin directory to the end of the "Variable value."

   <img src="/images/m103/c3/edit-path.png">

   You can confirm that `rsync` and `ssh` are both successfully installed and accessible via your PATH by launching the command prompt (`cmd`) and running `rsync` and `ssh` respectively. Both commands should output usage information. If you receive an error about not being recognized as an internal or external command then you'll need revisit the steps above.

4. Install Vagrant

   After all of the above prerequisites have been installed you can go ahead and download and install Vagrant. This will require a restart of your machine.

After downloading the Handouts (provided in the next Lab), extract the zip folder in your current working directory.

From the directory `m103/m103-vagrant-env`, we can bring up the Vagrant environment:

```sh
m103-vagrant-env$ vagrant up
```
 
After bringing up the environment, we can provision Vagrant (to download datasets, validation scripts, etc.):

```sh
m103-vagrant-env$ vagrant provision
```
 
Once the environment is provisioned, we can connect with `ssh`:

```sh
m103-vagrant-env$ vagrant ssh
```
 
As you complete labs in this course, you will be asked to run validation scripts that check your work. These validators are stored inside the VM. You can run these validators from anywhere within the Vagrant environment. Do not run these validators from within a Mongo shell.

If you need to re-download these scripts, run the following command (from Vagrant):

```sh
vagrant@m103:~$ download_validators
```

### Transcripción

Entonces los laboratorios en este curso se completarán dentro de una máquina virtual.

Y esta lección trata sobre cómo configurar la máquina virtual en su máquina host.

Pero primero, ¿por qué estamos usando uno?

La primera es que es para proteger el medio ambiente.

Queremos darle un entorno que pueda usar sin alterar la configuración existente en su máquina local.

Esto también nos permite evitar la dependencia y la resolución de problemas del sistema.

Entonces, todas las dependencias se descargan por usted.

Y puedes concentrarte en el aprendizaje.

Tercero, también nos permite brindar un apoyo más consistente a nuestros estudiantes a lo largo del curso.

Si sabemos que todos están ejecutando el mismo sistema operativo, podemos diagnosticar y resolver más rápidamente los problemas que tenga.

¿Entonces, cómo funciona?

Arrancas una máquina virtual Vagrant en tu computadora.

Y utiliza la memoria, el almacenamiento y la CPU de su computadora para funcionar como una computadora real.

Y mediante el uso de un proceso llamado SSH o Secure Shell, puede conectarse a la máquina virtual.

Una vez que esté conectado, puede tratarlo como una computadora normal.

Y puede conectarlo con su terminal o símbolo del sistema.

En este curso, lanzará procesos mongod y conjuntos de réplicas dentro de su máquina virtual.

Pero para hacer eso, le pediremos que instale dos dependencias.

El primero es VirtualBox.

VirtualBox como un hipervisor de código abierto que nos permite ejecutar máquinas virtuales en su máquina host.

El segundo es Vagrant, que es una herramienta de código abierto que nos permite construir y mantener estos entornos de software virtual muy rápidamente.

Más específicamente, Vagrant hace que sea realmente fácil copiar archivos y compartir archivos desde su máquina host a su máquina virtual.

Por ejemplo, si tenía un folleto de ejercicios o algún código que escribió en su máquina local, puede tomar esos archivos y copiarlos directamente en la máquina virtual.

Veremos más sobre esto en un minuto.

Como parte de las notas de la conferencia, encontrará las instrucciones para instalar estas dos herramientas de software para Windows.

Así que no pasaré mucho tiempo en este video con respecto a eso.

Pero una vez que los tenga instalados, comenzaremos a configurar el entorno de clase virtual.

Primero, vamos a crear un directorio para este curso llamado m103 y un directorio principal llamado universidad.

Este -p aquí crea el directorio principal de la universidad en caso de que no exista.

Así que aquí tenemos una copia recursiva, que es anotada por -r, que se usa para copiar directorios completos en otra ubicación.

En este caso, estamos copiando el directorio m103-vagrant-env en la carpeta m103.

Entonces, ahora que estamos dentro de la carpeta del entorno m103 vagabundo dentro de la carpeta del curso m103, podemos comenzar a mirar el folleto real para ver lo que acabamos de descargar.

Como podemos ver, tenemos un archivo Vagrant y un archivo de aprovisionamiento.

El archivo Vagrant especifica algunos detalles de imagen del sistema operativo de referencia.

Como puede ver, tenemos el sistema operativo que estamos ejecutando, Ubuntu.

Y solo quiero llamar su atención rápidamente sobre esta carpeta que creamos llamada Shared.

Esta carpeta conectará nuestra máquina virtual a nuestra máquina host.

Entonces, en cualquier momento, si desea copiar un archivo de su máquina host en la máquina virtual, ya sea un folleto para un laboratorio o algún código que escribió en su máquina host, todo lo que debe hacer es copiarlo la carpeta compartida dentro del entorno virtual y la copiaría en la máquina virtual.

Aquí hay algunos otros detalles, como el nombre de la caja, mongod-m103 y la cantidad de memoria, que es de poco más de dos gigabytes.

También hay una dirección IP externa para el cuadro Vagrant, 192.168.103.100, que en realidad tendrá que usar en algunos laboratorios posteriores.

Así que ahora podemos echar un vistazo al archivo de aprovisionamiento y ver qué hay dentro.

Este archivo es bastante más largo que el archivo Vagrant.

Y está escrito en el script de Shell porque necesita descargar algunas cosas para que pueda completar los materiales del curso.

El archivo de aprovisionamiento en general especifica las dependencias y todo el software necesario que se requerirá para completar los laboratorios.

Y también descargará los scripts de validación que ejecutará para verificar su trabajo.

Aquí tenemos algunos comandos que definen qué versión de MongoDB instalar y dónde colocar algunos archivos y configuraciones y cualquier software de terceros.

Realmente no necesitas saber nada de esto.

Pero en caso de que algo salga mal con el entorno virtual durante el curso, podríamos pedirle que cambie algunas de las configuraciones y variables dentro de estos archivos.

Sin embargo, las posibilidades de eso son muy pequeñas.

Entonces, en general, trate de no perder el tiempo con estos archivos.

La razón por la que estamos usando Vagrant es para que pueda concentrarse en el material del curso y no en las dependencias o en cualquier otra cosa.

Así que ahora que hemos echado un vistazo a algunos de los archivos que recibió en el folleto, en realidad sacamos Vagrant y repasamos algunos de los comandos que puede usar para configurar el cuadro Vagrant una vez que se está ejecutando.

Lo primero que vamos a aprender se llama vagabundo.

Esto realmente abrirá nuestra caja como se especifica en el archivo Vagrant y el archivo de aprovisionamiento.

Esto puede tardar un tiempo en ejecutarse, especialmente si es la primera vez que lo ejecuta.

Así que solo espera unos minutos y debería completarse.

Entonces, una vez que este cuadro se está ejecutando, podemos verificar el estado y el estado del cuadro utilizando el estado de comando vagabundo.

Esto nos dice que nuestra caja mongod-m103 está funcionando.

Y también nos da algunos comandos que podemos usar para apagarlo con un alto vagabundo, suspenderlo con un vagabundo suspendido y volver a iniciarlo con vagabundo arriba.

Sin más preámbulos, vamos a SSH a la caja.

Entonces aquí podemos conectarnos a la caja usando el SSH con el comando vagrant ssh.

Esto reconocerá el archivo Vagrant y el archivo de aprovisionamiento y lo usará para conectarse al VirtualBox correcto.

Entonces, una vez que tengamos SSHed, puede notar que el indicador de Shell ha cambiado para decir vagabundo y m103.

Y esto es algo bueno.

Significa que ya no estamos conectados a nuestra máquina host y en realidad estamos conectados directamente a la máquina virtual.

Y podemos ejecutar comandos para explorar realmente nuestra máquina virtual.

Solo echaremos un vistazo a la versión mongod que está instalada en nuestra caja.

Y podemos ver su versión 3.6, que es la última.

Esto se debe a que el archivo de aprovisionamiento fue y obtuvo la última versión de MongoDB para nosotros.

Así que no tuvimos que descubrir cómo hacerlo.

También se instaló en la ubicación correcta para que pueda saltar y comenzar a ejecutar comandos Mongo.

Si queremos salir de la caja, todo lo que tenemos que hacer es escribir exit y estamos conectados de nuevo a nuestra máquina host.

Si queremos detener la caja, podemos ejecutar una detención vagabunda.

Y esto apagará con gracia nuestra máquina virtual para que el estado permanezca igual.

Y si corremos vagabundo nuevamente, lo traerá de nuevo.

Podemos verificar esto ejecutando el estado vagabundo nuevamente.

Y como podemos ver, la caja se está ejecutando.

Para resumir, esto es lo que se espera de usted.

Descargue el folleto que contiene el entorno Vagrant.

Descomprima el folleto y colóquelo en su nueva carpeta m103 para este curso.

Puede activar el entorno virtual con vagabundo, y se conectan con SSH.

Para verificar que el script de aprovisionamiento se ejecutó correctamente, puede verificar la versión de MongoDB que está instalada dentro de la máquina virtual.

Si lo desea, puede salir de la caja y poner nuestro sistema en reposo, y luego volver a activarlo para que pueda usarlo.

Todo bien.

Gracias por su atención.

Buena suerte al configurar su propio entorno virtual.


## 4. Examen Setting Up the Vagrant Environment

**Problem:**

Why are we using a Vagrant environment?

Check all answers that apply:

* Labs can be completed without interfering with existing settings on your host machine. :+1:

* Troubleshooting is easier when all students are using the same VM. :+1:

* Vagrant makes it easy to copy files from your host machine to the VM. :+1:

* The VM comes with all dependencies necessary for this course. :+1:

## 5. Laboratorio: Configuración del entorno Vagrant

## 6. Tema: Solución de problemas del entorno Vagrant
