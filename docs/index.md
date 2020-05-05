# Docker
   La idea detrás de [Docker](https://docs.docker.com) es crear contenedores ligeros y portables para las aplicaciones software que puedan ejecutarse en cualquier máquina con Docker instalado, independientemente del sistema operativo que la máquina tenga por debajo, facilitando así también los despliegues.

#Compose
Docker Compose es una herramienta que permite simplificar el uso de Docker, generando scripts que facilitan el diseño y la construcción de servicios o de aplicaciones con múltiples componentes. 
Con Compose puedes crear diferentes contenedores y al mismo tiempo, en cada contenedor, diferentes servicios, unirlos a un volúmen común, iniciarlos y apagarlos, etc. Es un componente fundamental para poder construir aplicaciones y microservicios.

Esta herramienta llamada docker-compose nos permite crear un fichero de configuración con todos los contenedores que queremos levantar, establecer las dependencias entre ellos y configurarlos individualmente.

## Instalación
------------------------------------------------------------------------------------------
###Centos 7
 Aquí veremos como se instala en Centos 7

 Es recomentdable desinstalar viejas versiones de Docker:

    sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-engine

######Docker 
   Necesitamos tener un sistema Dockerizado.  Para ello necesitamos instalar:

* `sudo yum update` 
* `sudo yum install -y yum-utils` 
* ` sudo yum-config-manager  --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo` - Añadimos a nuestro repositorio 
* `sudo yum install docker-ce docker-ce-cli containerd.io` - Instalamos la ultima versión de docker

* `sudo systemctl enable docker` - Para que arranque por defecto   
* `sudo systemctl start docker` - Arrancamos el sistema
* ` sudo docker run hello-world` - Para verificar que se ha arrancado correctamente docker

Si se quiere instalar otra version de docker podemos ver las versiones con este comando:

* `sudo yum install docker-ce docker-ce-cli containerd.io`

######Compose
  * `sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

Si se quiere instalar otra version de compose podemos ver las versiones con este comando:
[Compose repository release page on GitHub](https://github.com/docker/compose/releases)

  * `sudo chmod +x /usr/local/bin/docker-compose` - Aplicar permisos

------------------------------------------------------------------------------------------

###Ubuntu 
 Aquí veremos como se instala en Ubuntu 19.10


######Docker 
   Necesitamos tener un sistema Dockerizado.  Para ello necesitamos instalar:

* `sudo apt-get remove docker docker-engine docker.io containerd runc` -Para desinstalar viejas versiones


Vamos a hacerlo con Docker Engine

* `sudo apt-get update` - Actualizamos 
* `sudo apt-get install docker-ce docker-ce-cli containerd.io` 

Si no esta en nuestro repositorios lo añadimos

* `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add` 
* `sudo bash -c 'echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu disco stable" > /etc/apt/sources.list.d/docker-ce.list'`

* `sudo apt-get update` - Actualizamos 
* `apt-cache policy docker-ce` - Vemos que esta en el repositorio
* `sudo apt-get install -y docker-ce` - Instalamos
    
si quieres instalar una especifica versión

* `apt-cache madison docker-ce`
* `sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io`


Arranque del servicio

* `sudo systemctl enable docker` - Para que arranque por defecto   
* `sudo systemctl start docker` - Arrancamos el sistema
* `sudo docker run hello-world` - Para verificar que se ha arrancado correctamente docker

######Compose
  * `sudo curl -L "https://github.com/docker/compose/releases/download/1.25.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose`

Si se quiere instalar otra version de compose podemos ver las versiones con este comando:
[Compose repository release page on GitHub](https://github.com/docker/compose/releases)

  * `sudo chmod +x /usr/local/bin/docker-compose` - Aplicar permisos

------------------------------------------------------------------------------------------

## Commandos Básicos
####Docker
Seguramente tengas que executar los siguientes comandos con sudo pero si no quieres hacerlo pincah [aquí](https://medium.com/@Grigorkh/how-to-install-docker-on-ubuntu-19-10-60feae8fd382) y al final de la página te dice como

* `docker run -i -t ubuntu /bin/echo` - Crear y ejecutar un contenedor. Lo busca en local y si no lo encuentra se lo descarga. El echo mostrará ese mensaje cuando arranque el contenedor.  
* `docker start -a <pon-la-ID-del-container>` - Para arrancar un contenedor
* `docker stop <pon-la-ID-del-container>` - Para parar


* `docker pull ubuntu:XX.XX` - Descargar imágenes en Docker cloud


* `docker images` - Todas las imágenes disponibles
* `docker image rm <nombre-de-imagen>` - Para borrar imagenes

* `docker container exec -it proxy /bin/bash` - Entrar en un contenedor en ejecución
* `docker ps -a` - Muestra todos los contenedores.

* `docker ps -s` - Muestra el disco usado

* `docker ps --filter` - Este comando es interesante. status={created, restarting, running, removing, paused, exited and dead}
* `docker ps --filter status=running` - Muestra los contenedores corriendo.

* `docker kill $(docker ps -q)` - Para todos los contenedores
* `docker rm $(docker ps -a -q)` - Elimina todos los contenedores
* `docker rmi -f $(docker images -q)` - Elimina todas las imagenes descargadas

        Compartir el directorio de trabajo:   
        $ docker run -ti -v "$PWD":/shared:ro ubuntu:14.04 /bin/bash
        ro = lectura. Por defecto es de lectura/escritura


Desinstalar Docker
* `sudo apt-get purge docker-engine` - Desintala
        
####Compose
* `docker-compose --version` - La version
* `docker-compose up -d` -	Crear el contenedor y lo ejecuta en background
* `docker-compose start` - Levanta el contenedor
* `docker-compose down` - Apaga los servicios que se levanto con docker-compose	y borra los datos.
* `docker-compose stop` - Detiene los servicios y deja los datos como estaban.
* `docker-compose ps` - Muestra los contenedores funcionado
* `docker-compose exec` - Manda un comando y lo ejecuta a uno de los servicios levantados en el contenedor por Docker-compose. Servicios de docker-compose puede ser MYSQL o en el ejemplo: “jekyll”







