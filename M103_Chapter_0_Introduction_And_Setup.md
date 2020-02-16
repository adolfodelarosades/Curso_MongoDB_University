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

<img src="/images/1-ss.png">

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

## 4. Examen

## 5. Laboratorio: Configuración del entorno Vagrant

## 6. Tema: Solución de problemas del entorno Vagrant
