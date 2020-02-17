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
   
   
   ```sh
   192:~ adolfodelarosa$ vboxmanage --version
   6.0.14r133895
   192:~ adolfodelarosa$ 
   ```

2. Install Vagrant

   After VirtualBox has been installed you can go ahead and [download](https://www.vagrantup.com/downloads.html) and [install](https://www.vagrantup.com/docs/installation/) Vagrant.


   ```sh
   192:~ adolfodelarosa$ vagrant --version
   Vagrant 2.2.7
   192:~ adolfodelarosa$ 
   ```

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

<img src="/images/m103/c3/3-why.png">

* La primera es que es para proteger el medio ambiente.

   Queremos darle un entorno que pueda usar sin alterar la configuración existente en su máquina local.

* Esto también nos permite evitar la dependencia y la resolución de problemas del sistema.

   Entonces, todas las dependencias se descargan por usted.

   * Y puedes concentrarte en el aprendizaje.

* Tercero, también nos permite brindar un apoyo más consistente a nuestros estudiantes a lo largo del curso.

   Si sabemos que todos están ejecutando el mismo sistema operativo, podemos diagnosticar y resolver más rápidamente los problemas que tenga.

¿Entonces, cómo funciona?

Arrancas una máquina virtual Vagrant en tu computadora.

<img src="/images/m103/c3/3-esquema-1.png">

Y utiliza la memoria, el almacenamiento y la CPU de su computadora para funcionar como una computadora real.

Y mediante el uso de un proceso llamado SSH o Secure Shell, puede conectarse a la máquina virtual.

Una vez que esté conectado, puede tratarlo como una computadora normal.

<img src="/images/m103/c3/3-esquema-2.png">

Y puede conectarlo con su terminal o símbolo del sistema.

En este curso, lanzará procesos mongod y conjuntos de réplicas dentro de su máquina virtual.

<img src="/images/m103/c3/3-esquema-3.png">

Pero para hacer eso, le pediremos que instale dos dependencias.

<img src="/images/m103/c3/3-dosdependencias.png">

El primero es VirtualBox.

<img src="/images/m103/c3/3-virualvox.png">

VirtualBox como un hipervisor de código abierto que nos permite ejecutar máquinas virtuales en su máquina host.

El segundo es Vagrant, que es una herramienta de código abierto que nos permite construir y mantener estos entornos de software virtual muy rápidamente.

<img src="/images/m103/c3/3-vagrant.png">

Más específicamente, Vagrant hace que sea realmente fácil copiar archivos y compartir archivos desde su máquina host a su máquina virtual.

Por ejemplo, si tenía un folleto de ejercicios o algún código que escribió en su máquina local, puede tomar esos archivos y copiarlos directamente en la máquina virtual.

<img src="/images/m103/c3/3-esquema-4.png">

<img src="/images/m103/c3/3-esquema-5.png">

Veremos más sobre esto en un minuto.

Como parte de las notas de la conferencia, encontrará las instrucciones para instalar estas dos herramientas de software para Windows.

Así que no pasaré mucho tiempo en este video con respecto a eso.

Pero una vez que los tenga instalados, comenzaremos a configurar el entorno de clase virtual.

Primero, vamos a crear un directorio para este curso llamado m103 y un directorio principal llamado universidad.

```sh
$ mkdir -p ~/university/m103/ 
```

Este -p aquí crea el directorio principal `university` en caso de que no exista.

```sh
$ cp -r m103-vagrant-env/ ~/university/m103/ 

En mi caso
$ cp -r m103/ ~/university/m103/ 
```

Así que aquí tenemos una copia recursiva, que es anotada por `-r`, que se usa para copiar directorios completos en otra ubicación.

En este caso, estamos copiando el directorio `m103-vagrant-env` en la carpeta `m103`.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ cd university/m103/m103-vagrant-env
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ pwd
/Users/adolfodelarosa/university/m103/m103-vagrant-env
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ 
```

Entonces, ahora que estamos dentro de la carpeta `m103-vagrant-env`, podemos comenzar a mirar el folleto real para ver lo que acabamos de descargar.

<img src="/images/m103/c3/3-carpeta103.png">

Como podemos ver, tenemos un archivo `Vagrantfile` y un archivo `provision-mongod`.

El archivo `Vagrantfile` especifica algunos detalles de imagen del sistema operativo de referencia.

Como puede ver, tenemos el sistema operativo que estamos ejecutando, Ubuntu.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ cat Vagrantfile
if Vagrant::VERSION < "2.0.0"
  $stderr.puts "Must redirect to new repository for old Vagrant versions"
  Vagrant::DEFAULT_SERVER_URL.replace('https://vagrantcloud.com')
end

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = false
  config.vm.synced_folder "shared/", "/shared", create: true
  config.vm.synced_folder "dataset/", "/dataset", create: true

  config.vm.define "mongod-m103" do |server|
    server.vm.provider "virtualbox" do |vb|
	     vb.customize ["modifyvm", :id, "--cpus", "2"]
       vb.name = "mongod-m103"
       vb.memory = 2048
    end
    server.vm.hostname = "m103"
    server.vm.network :private_network, ip: "192.168.103.100"
    server.vm.provision :shell, path: "provision-mongod", args: ENV['ARGS']
  end
end
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ 
```

Y solo quiero llamar su atención rápidamente sobre esta carpeta que creamos llamada `shared/`.

Esta carpeta conectará nuestra máquina virtual a nuestra máquina host.

**Entonces, en cualquier momento, si desea copiar un archivo de su máquina host en la máquina virtual, ya sea un folleto para un laboratorio o algún código que escribió en su máquina host, todo lo que debe hacer es copiarlo la carpeta compartida dentro del entorno virtual y la copiaría en la máquina virtual**.

Aquí hay algunos otros detalles, como el nombre del vm, `vb.name = "mongod-m103"` y la cantidad de memoria, que es de poco más de dos gigabytes `vb.memory = 2048`.

También hay una dirección IP externa para el cuadro Vagrant, 192.168.103.100 `ip: "192.168.103.100"`, que en realidad tendrá que usar en algunos laboratorios posteriores.

Así que ahora podemos echar un vistazo al archivo de aprovisionamiento y ver qué hay dentro.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ cat provision-mongod
#!/usr/bin/env bash
#
# Bash script for provisioning the MongoDB instances

set -e
set -x

function ip_config(){
  export CLIENT_IP_ADDR=`ifconfig  | grep 'inet addr:'| grep -v '127.0.0.1' | cut -d: -f2 | awk '{ print $1}' | tail -1`
  export CLIENT_NAME=`hostname | cut -d. -f 1 | tr '[:upper:]' '[:lower:]'`
  echo "Configuring /etc/hosts ..."
  echo "127.0.0.1 localhost localhost.localdomain localhost4 localhost4.localdomain4 " > /etc/hosts
  echo "::1       localhost localhost.localdomain localhost6 localhost6.localdomain6" >> /etc/hosts
  echo "fe00::0 ip6-localnet" >> /etc/hosts
  echo "ff00::0 ip6-mcastprefix" >> /etc/hosts
  echo "ff02::1 ip6-allnodes" >> /etc/hosts
  echo "ff02::2 ip6-allrouters" >> /etc/hosts
  echo "ff02::3 ip6-allhosts" >> /etc/hosts
  echo "$CLIENT_IP_ADDR $CLIENT_NAME" >> /etc/hosts
}

function install_mongod(){
  echo "Install MongoDB Enterprise"
  # install MongoDB using apt-get so it installs with service support and has a
  # default configuration file in /etc/
  # have to force-yes -y to force install of unsigned packages (our 3.4 key does
  # not match, no 3.6 key in the docs) and to agree to install
  sudo apt-get install --force-yes -y mongodb-enterprise
  mkdir -p /var/log/mongodb/
  sudo chown vagrant:vagrant -R /var/log/mongodb
  sudo chown vagrant:vagrant -R /var/lib/mongodb
  sudo echo "
security:
  authorization: enabled"  | sudo tee -a /etc/mongod.conf
  echo "Done installing MongoDB Enterprise"
}

function user_setup(){
  sudo sh -c "killall mongod; true"
  sudo mkdir -p /data
  sudo chmod -R 777 /data
  mkdir -p /data/db
  mkdir -p /home/vagrant/data
  chmod -R 777 /home/vagrant/data
  chown -R vagrant:vagrant /home/vagrant/data
  mkdir -p /var/m103/validation
  echo "Set LC_ALL=C to .profile"
  sudo echo "export LC_ALL=C" >> /home/vagrant/.profile
  sudo echo "PATH=$PATH:/var/m103/validation" >> /home/vagrant/.profile
}

function update_repo(){
  echo "Install MongoDB Enterprise Repository"
  # set to track mongodb development (rc)
  echo "deb [ arch=amd64 ] http://repo.mongodb.com/apt/ubuntu trusty/mongodb-enterprise/3.6 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-enterprise.list
  sudo apt-key adv --keyserver keyserver.ubuntu.com --recv 0C49F3730359A14518585931BC711F9BA15703C6

  echo "Update Repositories"
  sudo apt-get update -y
  echo "Installing MongoDB Enterprise Dependencies"
  sudo apt-get install -y libgssapi-krb5-2 libsasl2-2 libssl1.0.0 libstdc++6 snmp
}

function config(){
  sudo su
  # disable THP
  echo -e "never" > /sys/kernel/mm/transparent_hugepage/enabled
  echo -e "never" > /sys/kernel/mm/transparent_hugepage/defrag
  # disable mongod upstart service
  echo 'manual' | sudo tee /etc/init/mongod.override
}

function download_dataset() {
  echo "Downloading Dataset"
  curl -s https://s3.amazonaws.com/edu-static.mongodb.com/lessons/M103/products.json.tgz -o products.json.tgz
  tar -xzvf products.json.tgz -C /dataset
  rm -rf products.json.tgz

  curl -s https://s3.amazonaws.com/edu-static.mongodb.com/lessons/M103/products.part2.json.tgz -o products.part2.json.tgz
  tar -xzvf products.part2.json.tgz -C /dataset
  rm -rf products.part2.json.tgz
}

function download_validators() {
  echo "Downloading Validation Scripts"
  curl -s https://s3.amazonaws.com/edu-static.mongodb.com/lessons/M103/m103_validation.tgz -o m103_validation.tgz
  tar -xzvf m103_validation.tgz -C /var/m103/validation
  rm -rf m103_validation.tgz
  echo "#!/bin/bash
        curl -s https://s3.amazonaws.com/edu-static.mongodb.com/lessons/M103/m103_validation.tgz -o m103_validation.tgz
        sudo tar -xzvf m103_validation.tgz -C /var/m103/validation
        rm -rf m103_validation.tgz" > /var/m103/validation/download_validators
  echo "#!/bin/bash
        echo -n 'm103 rocks' | openssl sha256 | sed -e s/\(stdin\)=.//" > /var/m103/validation/validate_box
  chmod -R +x /var/m103/validation/
  chown root:root /var/m103/validation
  echo "Done: Downloaded Validation Scripts"
}

function data_path() {
  sudo mkdir -p /data
  sudo chown -R vagrant:vagrant /data
}

function install_pymongo() {
  sudo apt-get -y install python-pip
  sudo pip install pymongo
}

function verify_ip() {
  export EXPECTED_IP=192.168.103.100
  ifconfig | grep $EXPECTED_IP
  ret=$?
  if [ $ret -ne 0 ]
  then
    ERR="The VM does not have the expected IP: $EXPECTED_IP
instead it has: $CLIENT_IP_ADDR
Ensure no other vagrant VM has that same expected IP: $EXPECTED_IP
You should recreate this VM after destroying it with 'vagrant destroy'"
    fatal "$ERR"
  fi
}

function fatal() {
  echo ERROR
  echo "$1"
  exit 1
}

config
ip_config
update_repo
install_mongod
user_setup
data_path
install_pymongo
download_dataset
download_validators
# Starting at this point, it is only validations so removing exit on error
set +e
verify_ip
echo "DONE"
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ 

```

Este archivo es bastante más largo que el archivo Vagrantfile.

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

Lo primero que vamos a aprender es llamar `vagrant up`.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ vagrant up
Bringing machine 'mongod-m103' up with 'virtualbox' provider...
==> mongod-m103: This machine used to live in /Users/adolfodelarosa/m103/m103-vagrant-env but it's now at /Users/adolfodelarosa/university/m103/m103-vagrant-env.
==> mongod-m103: Depending on your current provider you may need to change the name of
==> mongod-m103: the machine to run it as a different machine.
==> mongod-m103: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> mongod-m103: flag to force provisioning. Provisioners marked to run always will still run.
```

Esto realmente abrirá nuestra caja como se especifica en el archivo Vagrant y el archivo de aprovisionamiento.

Esto puede tardar un tiempo en ejecutarse, especialmente si es la primera vez que lo ejecuta.

Así que solo espera unos minutos y debería completarse.

Entonces, una vez que este cuadro se está ejecutando, podemos verificar el estado y el estado del cuadro utilizando el estado de comando `vagrant status`.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ vagrant status
Current machine states:

mongod-m103               running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ 

```

Esto nos dice que nuestra caja `mongod-m103` está funcionando.

Y también nos da algunos comandos que podemos usar para apagarlo con un `vagrant halt`, suspenderlo con un `vagrant suspend` y volver a iniciarlo con `vagrant up`.

Sin más preámbulos, vamos a SSH a la caja.

Entonces aquí podemos conectarnos a la caja usando el SSH con el comando `vagrant ssh`.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ vagrant ssh
Welcome to Ubuntu 14.04.6 LTS (GNU/Linux 3.13.0-170-generic x86_64)

 * Documentation:  https://help.ubuntu.com/

  System information as of Mon Feb 17 16:01:38 UTC 2020

  System load:  0.11              Processes:           83
  Usage of /:   5.1% of 39.34GB   Users logged in:     0
  Memory usage: 8%                IP address for eth0: 10.0.2.15
  Swap usage:   0%                IP address for eth1: 192.168.103.100

  Graph this data and manage this system at:
    https://landscape.canonical.com/

New release '16.04.6 LTS' available.
Run 'do-release-upgrade' to upgrade to it.


Last login: Mon Feb 17 16:01:38 2020 from 10.0.2.2
vagrant@m103:~$ 
```

Esto reconocerá el archivo Vagrant y el archivo de aprovisionamiento y lo usará para conectarse al VirtualBox correcto.

Entonces, una vez que tengamos ssh, puede notar que el indicador de Shell ha cambiado para decir `vagrant@m103:~$ `.

Y esto es algo bueno.

Significa que ya no estamos conectados a nuestra máquina host y en realidad estamos conectados directamente a la máquina virtual.

Y podemos ejecutar comandos para explorar realmente nuestra máquina virtual.

Solo echaremos un vistazo a la versión mongod que está instalada en nuestra caja.

```sh
vagrant@m103:~$ mongod --version
db version v3.6.17
git version: 3d6953c361213c5bfab23e51ab274ce592edafe6
OpenSSL version: OpenSSL 1.0.1f 6 Jan 2014
allocator: tcmalloc
modules: enterprise 
build environment:
    distmod: ubuntu1404
    distarch: x86_64
    target_arch: x86_64
vagrant@m103:~$ 
```

Y podemos ver su versión 3.6, que es la última.

Esto se debe a que el archivo de aprovisionamiento fue y obtuvo la última versión de MongoDB para nosotros.

Así que no tuvimos que descubrir cómo hacerlo.

También se instaló en la ubicación correcta para que pueda saltar y comenzar a ejecutar comandos Mongo.

Si queremos salir de la caja, todo lo que tenemos que hacer es escribir `exit` y estamos conectados de nuevo a nuestra máquina host.

```sh
vagrant@m103:~$ exit
logout
Connection to 127.0.0.1 closed.
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ 
```

Si queremos detener la caja, podemos ejecutar `vagrant halt`.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ vagrant halt
==> mongod-m103: Attempting graceful shutdown of VM...
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ 
```

Y esto apagará con gracia nuestra máquina virtual para que el estado permanezca igual.

Y si corremos `vagrant up` nuevamente, lo traerá de nuevo.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ vagrant up
Bringing machine 'mongod-m103' up with 'virtualbox' provider...
==> mongod-m103: Clearing any previously set forwarded ports...
==> mongod-m103: Clearing any previously set network interfaces...
==> mongod-m103: Preparing network interfaces based on configuration...
    mongod-m103: Adapter 1: nat
    mongod-m103: Adapter 2: hostonly
==> mongod-m103: Forwarding ports...
    mongod-m103: 22 (guest) => 2222 (host) (adapter 1)
==> mongod-m103: Running 'pre-boot' VM customizations...
==> mongod-m103: Booting VM...
==> mongod-m103: Waiting for machine to boot. This may take a few minutes...
    mongod-m103: SSH address: 127.0.0.1:2222
    mongod-m103: SSH username: vagrant
    mongod-m103: SSH auth method: private key
==> mongod-m103: Machine booted and ready!
==> mongod-m103: Checking for guest additions in VM...
    mongod-m103: The guest additions on this VM do not match the installed version of
    mongod-m103: VirtualBox! In most cases this is fine, but in rare cases it can
    mongod-m103: prevent things such as shared folders from working properly. If you see
    mongod-m103: shared folder errors, please make sure the guest additions within the
    mongod-m103: virtual machine match the version of VirtualBox you have installed on
    mongod-m103: your host and reload your VM.
    mongod-m103: 
    mongod-m103: Guest Additions Version: 4.3.40
    mongod-m103: VirtualBox Version: 6.0
==> mongod-m103: Setting hostname...
==> mongod-m103: Configuring and enabling network interfaces...
==> mongod-m103: Mounting shared folders...
    mongod-m103: /shared => /Users/adolfodelarosa/university/m103/m103-vagrant-env/shared
    mongod-m103: /dataset => /Users/adolfodelarosa/university/m103/m103-vagrant-env/dataset
    mongod-m103: /vagrant => /Users/adolfodelarosa/university/m103/m103-vagrant-env
==> mongod-m103: Machine already provisioned. Run `vagrant provision` or use the `--provision`
==> mongod-m103: flag to force provisioning. Provisioners marked to run always will still run.
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ 
```

Podemos verificar esto ejecutando el estado nuevamente `vagrant status`.

```sh
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ vagrant status
Current machine states:

mongod-m103               running (virtualbox)

The VM is running. To stop this VM, you can run `vagrant halt` to
shut it down forcefully, or you can run `vagrant suspend` to simply
suspend the virtual machine. In either case, to restart it again,
simply run `vagrant up`.
mini-de-adolfo:m103-vagrant-env adolfodelarosa$ 
```

Y como podemos ver, la caja se está ejecutando.

Para resumir, esto es lo que se espera de usted.

<img src="/images/m103/c3/3-resumen.png">

* Descargue el zip que contiene el entorno Vagrant.

* Descomprima el zip y colóquelo en su nueva carpeta m103 para este curso.

* Puede activar el entorno virtual con `vagrant up`

* Conectarse a out box usando `vagrant ssh`

* Verificar que el script de aprovisionamiento se ejecutó correctamente

* Verificar la versión de MongoDB que está instalada dentro de la máquina virtual.

* Puede salir de la caja y poner nuestro sistema en reposo

* Y luego volver a activarlo para que pueda usarlo.

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

### Lab: Setting Up the Vagrant Environment

**Problem:**

In order to effectively learn from this class, you should be completing problems in a real MongoDB cluster. To do this, we will use a development environment called Vagrant that makes it easy to provision a virtual machine with Virtualbox.

We first need to download [Vagrant](https://www.vagrantup.com/downloads.html) and [Virtualbox](https://www.virtualbox.org/wiki/Downloads). Vagrant requires that we use the latest version of Virtualbox (in the link), so click the link and download the correct option for your operating system.

Now, download the handout above and create a folder in your home directory called `m103`. Copy the handout to this directory and then navigate to the handout directory `m103-vagrant-env` in your terminal. Bring up your Vagrant environment by running these commands (this will take a few minutes):

```sh
vagrant up --provision
```

This command brings up your virtual machine, if it's not already running. It also builds directories in the VM and downloads all the software and validation scripts necessary to complete this course.

```sh
vagrant ssh
```

This command connects to your Vagrant instance using SSH. If it prompts you for a username/password, use the username **vagrant** and the password **vagrant**.

To validate that your box is running properly, execute the following command from the Vagrant command line:

```sh
vagrant@m103:~$ validate_box
```

Please enter the validation key you receive here:

Enter answer here:

## 6. Tema: Solución de problemas del entorno Vagrant

### Troubleshooting the Vagrant Environment

In this lesson you can find the common pitfalls for setting up your vagrant environment as well as the troubleshooting guide .

#### Error while mounting shared folders

```sh
==> mongodb-mXXX: Mounting shared folders...
    mongodb-mXXX: /home/vagrant/shared => /Users/example/MXX/shared
Vagrant was unable to mount VirtualBox shared folders. This is usually
because the filesystem "vboxsf" is not available. This filesystem is
made available via the VirtualBox Guest Additions and kernel module.
Please verify that these guest additions are properly installed in the
guest. This is not a bug in Vagrant and is usually caused by a faulty
Vagrant box. For context, the command attempted was:

mount -t vboxsf -o uid=1000,gid=1000 home_vagrant_shared /home/vagrant/shared

The error output from the command was:

mount: unknown filesystem type 'vboxsf'
```

Here is a possible solution:

```sh
mXXX-vagrant-env$ vagrant plugin update vagrant-vbguest
mXXX-vagrant-env$ vagrant destroy
mXXX-vagrant-env$ vagrant up
```

**IP mismatch while doing ssh**

```sh
mXXX-vagrant-env$ vagrant ssh
Welcome to Ubuntu 14.04.5 LTS (GNU/Linux 3.13.0-153-generic x86_64)
<–some text–>
Swap usage: 0% IP address for eth1: 192.168.X.X
<–some more text–>
vagrant@mXXX:~$
```

If you are not able to view IP address for eth1 as shown above, then you can check the IP using:

```sh
vagrant@mXXX:~ ping m103
```

If the IP address for eth1(192.168.X.X) or the IP address in the output of above ping command differs for some reason, then destroy your vagrant using:


```sh
vagrant@mXXX:~ exit
mXXX-vagrant-env$ vagrant destroy
```

Then re-provision your vagrant VM by running vagrant up again. If it still doesn't work, then please share the following:

* what was the value in the VM
* what is the value for the host
* output of vagrant global status

**Localhost IP mismatch while configuring replica sets:**

If you encounter below error:

```sh
Failed to connect to 127.0.1.1:27001, in(checking socket for error after poll), reason: Connection refused
```

Check the localhost IP address using:

```sh
vagrant@mXXX:~ ping localhost
```

If the IP address is set to 127.0.1.1, instead of expected 127.0.0.1, then run the following command to resolve this issue:

```sh
vagrant@mXXX:~ exit
mXXX-vagrant-env$ vagrant halt
mXXX-vagrant-env$ vagrant up --provision
```

**Password for vagrant?**

If you are being asked for the password, then credentials are:

```sh
username: vagrant
password: vagrant
```

*Need to Enter Password Again and Again for Vagrant ??*

Solution:

If you are working with PuTTy and every time you enter vagrant, it asks for your password, please try the following solution:

1. Open the **PuTTYgen** utility;
2. Click on the Load button;
3. Open the private_key file under the VM directory structure
4. Change the value in the Number of bits in a generated key: field to 2048 , leave everything else as it is
5. Save the file as private.ppk
6. Use this `private.ppk` file as the "private key file for authentication" in PuTTy session config

All works fine Source: [https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Connect-to-Your-Vagrant-Virtual-Machine-with-PuTTY](https://github.com/Varying-Vagrant-Vagrants/VVV/wiki/Connect-to-Your-Vagrant-Virtual-Machine-with-PuTTY)

**Public Key not available error**

If you encounter below error:

```sh
database: Reading package lists...
 database: W
 database: :
 database: GPG error: http://repo.mongodb.com trusty/mongodb-enterprise/3.2 Release: The following signatures could not be verified because the public key is not available: NO_PUBKEY D68FA50FEA312927
 database: + install_mongod
 database: + echo 'Install MongoDB Enterprise'
 ........
 database: There are problems and -y was used without --force-yes
 The SSH command responded with a non-zero exit status. Vagrant
 assumes that this means the command failed. The output for this command
 should be in the log above. Please read the output to determine what
 went wrong.
```

The possible solution is:

Modify the line **"apt-get install -y mongodb-enterprise"** in **provision-mongod** file under m103-vagrant-env directory to:

```sh
sudo apt-get install -y mongodb-enterprise --force-yes
```

**Vagrant Issues on windows:**

*Must redirect to new repository for old Vagrant versions*

If you encounter this error when provisioning vagrant:

```sh
G:\mXXX>vagrant up --provision
```

Must redirect to new repository for old Vagrant versions It just hangs there for a long time.

Here are some steps that were followed by some students that worked:

For `Windows7 SP1 machine` :

1. Download the latest versions of vagrant and virtual box.
2. Update Powershell from 2 to 3 using the instructions here: [https://docs.microsoft.com/en-us/skypeforbusiness/set-up-your-computer-for-windows-powershell/download-and-install-windows-powershell-3-0](https://docs.microsoft.com/en-us/skypeforbusiness/set-up-your-computer-for-windows-powershell/download-and-install-windows-powershell-3-0)
3. Enable the virtualization in the BIOS (on some machines, it was under device configuration), Steps to do it can be found out here: Enable-Hardware- [https://www.wikihow.tech/Enable-Hardware-Virtualization](https://www.wikihow.tech/Enable-Hardware-Virtualization)

Launch `vagrant --repair` or the command mentioned in error message, and again ran `vagrant up`, then try again.

**Stderr: VBoxManage.exe: error: VT-x is not available**

If you encounter below error:

```sh
Command: ["startvm", "d3da0d72-3297-4e35-b301-23c8cfb4db96", ""–type",
"headless "]
Stderr: VBoxManage.exe: error: VT-x is not available (VERR_VMX_NO_VMX)
VBoxManage.exe: error: Details: code E_FAIL (0x80004005), component
ConsoleWrap, interface IConsole
```

Here is the possible solution:

1. Enable virtualization in the BIOS.
2. Make sure you have Hyper-V turned off in Windows 10 For Windows 10: Press Windows key. Type "Turn Windows features on or off" Deselect the checkbox next to Hyper-V. Select OK. Select Restart now.

**Vagrant issues on Ubuntu:**

`VBoxManage: error: Failed to create the host-only adapter`

Error:


```sh
==> mongod-mXXX: Clearing any previously set network interfaces…
There was an error while executing VBoxManage, a CLI used by Vagrant
for controlling VirtualBox. The command and stderr is shown below.
Command: ["hostonlyif", "create"]
Stderr: 0%…
Progress state: NS_ERROR_FAILURE
VBoxManage: error: Failed to create the host-only adapter
VBoxManage: error: VBoxNetAdpCtl: Error while adding new interface: failed to open /dev/vboxnetctl: No such file or directory
VBoxManage: error: Details: code NS_ERROR_FAILURE (0x80004005), component HostNetworkInterfaceWrap, interface IHostNetworkInterface
VBoxManage: error: Context: "RTEXITCODE handleCreate(HandlerArg*)" at line 94 of file VBoxManageHostonly.cpp
1355238bc7db15948d1084776f2d62c9d060a2dd.png
```

Possible Solution:

* Disable the Secure Boot on your Ubuntu machine.

Steps to disable Secure boot:

* Click simultaneously the shortcut Restart + Shift key.
* Click Troubleshoot → Advanced options → Start-up Settings → Restart.
* Click repeatedly the F10 key (BIOS setup), before the "Startup Menu" opens.
* Go to Boot Manager and disable the option Secure Boot. ...
* Save the changes and reboot.

**SSL error**

```sh
Error: SSL certificate problem: self signed certificate in certificate chain
Bringing machine 'mongod-mXXX' up with 'virtualbox' provider…
==> mongod-mXXX: Box 'ubuntu/trusty64' could not be found. Attempting to find and install…
mongod-mXXX: Box Provider: virtualbox
mongod-mXXX: Box Version: >= 0
The box 'ubuntu/trusty64' could not be found or
could not be accessed in the remote catalog. If this is a private
box on HashiCorp's Vagrant Cloud, please verify you're logged in via
vagrant login. Also, please double-check the name. The expanded
URL and error message are shown below:
URL: ["https://vagrantcloud.com/ubuntu/trusty64 1"]
Error: SSL certificate problem: self signed certificate in certificate chain.
```

This is most likely a consequence of your anti-virus blocking the execution of vagrant or its access the system certificates.

Possible Solution:

*Disable Antivirus and restart you vagrant machine*

**Restarting/Destroying Vagrant from Virtual Box GUI**

*Problem: If you are unsure on how to destroy or restart or shutdown vagrant.*

Solution:

* Open VirtualBox
* From Left side, right-click on vagrant environment
* You can see options like : "Start", "Remove" etc
* You can also modify memory allocation for vagrant environment here.

