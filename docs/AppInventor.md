#AppInventor
## ¿ Que vamos a hacer?
   Este manual vamos a realizar una DOCKERIZACIÓN de sistemas Linux en las distribuciones de Ubuntu 19.10. Para ello se ha utilizado Docker como servidor de contenedores. 
   Hemos creado un archivo DockerFile y hemos instalado compose para su depliegue de contenedores.
   
   La idea es dockerizar un sistema, para desplegar AppInventor. 
    
   Hemos creado dos lineas de creación del contenedor:
    
-------------------------
   - 1) Contendor con volumen con AppInventor compartido por la máquina HOST para poder añadir componenetes.    
        Hemos creado un contenedor con la distribución ubuntu 18.10, instalandole las librerias necesarias para que se puede ejecutar AppInventor en el sistema y hemos creado un volumen compartido entre el HOST y el contenedor para poder agregar las librerías de AppInventor y nuevos componetes que vayamos desarrollando.
        Para ello vamos a crearnos una imagen desde otra imagen y añadiremos las dependencias necesarias así como el volumen compartido.   
        En este documentos se explica paso a paso como realizar desde cero el DockerFile, pero si se prefiere se puede pasar a la instalación directamente ya que se ha retocado la imagen.
        Apartado [Creando tu imagen](#creando-tu-imagen)

-------------------------

   - 2) Contenedor que se crea con todo lo necesario para ejecutar appInventor.
        Si solo queréis desplegar el contenedor para AppInventor, ir al apartado [Contenedor](#contenedor)

-------------------------

## Creando tu imagen

Creamos una carpeta llamada appinventor-docker
Ahí descargamos el código que tenemos en este repositorio
git clone  `https://github.com/MarcosJerez/appinventor-docker.git`


Vamos a  `cd appinventor-sources/docker`

    * `git submodule update --init` - Y ejecutamos

* `git clone https://github.com/mit-cml/appinventor-sources.git` - No bajamos AppInventor


Como podemos observar en el DockerFile Partimos de una imagen de ubuntu que cambiamos a la 18.04

    FROM ubuntu:18.04

    RUN apt-get update && apt-get install -y software-properties-common && add-apt-repository ppa:openjdk-r/ppa && apt-get update && apt-get install -y \
        wget \
	unzip \
	openjdk-8-jdk \
	ant \
	libc6 \
	zlib1g \
	libstdc++6 \
	lib32ncurses5 \
	lib32stdc++6 \
	git

    RUN wget -O /tmp/appengine.zip https://storage.googleapis.com/appengine-sdks/featured/appengine-java-sdk-1.9.66.zip

    RUN unzip /tmp/appengine.zip -d /root && rm /tmp/appengine.zip

    COPY startserver.sh /root/scripts/

    COPY ant.sh /root/scripts/

    RUN chmod +x /root/scripts/startserver.sh

    RUN chmod +x /root/scripts/ant.sh

    ENV PATH="$PATH:/root/appengine-java-sdk-1.9.66/bin/"

    ENV PATH="$PATH:/root/scripts/"

    ENV JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"

    ENV PATH="$PATH:$JAVA_HOME/bin"

    RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" |  tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

    RUN apt-get install apt-transport-https ca-certificates gnupg -y
   
    RUN apt-get install curl -y

    RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg |  apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

    RUN apt-get update &&  apt-get install google-cloud-sdk -y

    RUN apt-get update &&  apt-get --only-upgrade install google-cloud-sdk-skaffold kubectl google-cloud-sdk-anthos-auth google-cloud-sdk-minikube google-cloud-sdk google-cloud-sdk-app-engine-grpc google-cloud-sdk-kind google-cloud-sdk-pubsub-emulator google-cloud-sdk-app-engine-go google-cloud-sdk-firestore-emulator google-cloud-sdk-cloud-build-local google-cloud-sdk-datastore-emulator google-cloud-sdk-kpt google-cloud-sdk-app-engine-python google-cloud-sdk-spanner-emulator google-cloud-sdk-cbt google-cloud-sdk-bigtable-emulator google-cloud-sdk-app-engine-python-extras google-cloud-sdk-datalab google-cloud-sdk-app-engine-java -y

    WORKDIR /root/
    
    EXPOSE 8888



En el DockerFile como podemos ver instalamos las dependecias necesarias dentro de nuestro contenedor
            
            wget \
            unzip \
            openjdk-8-jdk \
            ant \
            
            libc6 \
            zlib1g \
                        
            lib32ncurses5 \
            lib32bz2-1.0 \
            lib32stdc++6 \
            lib32z1 \
            git

* `docker build -t appinventorserver .` -- creamos la imagen

Hemos preparado un docker-compose.yml
   
    version: '3'
    services:
      appinventorserver:
        image: appinventorserver3
        ports:
         - "8888:8888"
        volumes:
         - /home/marcos/Documentos/INDESS/Proyectos/DockerSpace/docker/compose/appinventor-docker/appinventor-sources:/root/appinventor-sources
        stdin_open: true
        tty: true

Podemos ejecutar con docker 
* `sudo docker run -it -v "/home/marcos/Documentos/INDESS/Proyectos/DockerSpace/docker/appinventor-docker/appinventor-sources:/root/appinventor-sources/" -p 8888:8888 appinventorserver`

ó con docker-compose

* `sudo docker-compose up`

En el contenedor:
    

* `cd appinventor-sources/appinventor/`

* `ant MakeAuthKey`

* `ant`

Salimos del contenedor y ejecutamos appInventor desde docker:

* `docker-compose exec startserver.sh`

ó

* `docker-compose exec dev_appserver.sh --port=8888 --address=0.0.0.0 appengine/build/war` 

    
## Contenedor

Creamos una carpeta llamada appinventor-docker
Ahí descargamos el código que tenemos en este repositorio
git clone  `https://github.com/MarcosJerez/appinventor-docker.git`

* `cd appinventor-docker/docker/`

Cambiamos estos dos ficheros:

DockerFile:

    FROM ubuntu:18.04

    RUN apt-get update && apt-get install -y software-properties-common && add-apt-r
    epository ppa:openjdk-r/ppa && apt-get update && apt-get install -y \
            wget \
            unzip \
            openjdk-8-jdk \
            ant \
            libc6 \
            zlib1g \
            libstdc++6 \
            lib32ncurses5 \
            lib32stdc++6 \
            git


    RUN wget -O /tmp/appengine.zip https://storage.googleapis.com/appengine-sdks/fea
    tured/appengine-java-sdk-1.9.66.zip

    RUN unzip /tmp/appengine.zip -d /root && rm /tmp/appengine.zip
    #bajar y copiar el ant.sh
    COPY startserver.sh /root/scripts/
    RUN chmod +x /root/scripts/startserver.sh

    ENV PATH="$PATH:/root/appengine-java-sdk-1.9.66/bin/"

    ENV PATH="$PATH:/root/scripts/"

    ENV JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"

    ENV PATH="$PATH:$JAVA_HOME/bin"

    RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" |  tee -a /etc/apt/sources.list.d/google-cloud
    -sdk.list

    RUN  apt-get install apt-transport-https ca-certificates gnupg -y
    RUN  apt-get install curl -y

    RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg |  apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

    RUN apt-get update &&  apt-get install google-cloud-sdk -y

    RUN  apt-get update &&  apt-get --only-upgrade install google-cloud-sdk-skaffold kubectl google-cloud-sdk-anthos-auth google-cloud-sdk-minikube google-cloud-sdk go
    ogle-cloud-sdk-app-engine-grpc google-cloud-sdk-kind google-cloud-sdk-pubsub-emulator google-cloud-sdk-app-engine-go google-cloud-sdk-firestore-emulator google-clo
    ud-sdk-cloud-build-local google-cloud-sdk-datastore-emulator google-cloud-sdk-kpt google-cloud-sdk-app-engine-python google-cloud-sdk-spanner-emulator google-cloud
    -sdk-cbt google-cloud-sdk-bigtable-emulator google-cloud-sdk-app-engine-python-extras google-cloud-sdk-datalab google-cloud-sdk-app-engine-java -y
    WORKDIR /root/
    RUN git clone https://github.com/mit-cml/appinventor-sources.git
    RUN cd /root/appinventor-sources/ && git submodule update --init

    WORKDIR /root/appinventor-sources/appinventor
    RUN ls
    RUN  ant MakeAuthKey
    RUN  ant 
    WORKDIR /root/

    EXPOSE 8888
    CMD ["startserver.sh", "run"]


docker-compose:

    version: '3'
    services:
      appinventorserver:
        build: .       
        ports:
         - "8888:8888"    
        stdin_open: true
        tty: true


* `sudo docker-compose up -d`

## Estructura UML
