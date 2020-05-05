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
RUN chmod +x /root/scripts/startserver.sh

ENV PATH="$PATH:/root/appengine-java-sdk-1.9.66/bin/"

ENV PATH="$PATH:/root/scripts/"

ENV JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"

ENV PATH="$PATH:$JAVA_HOME/bin"

RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" |  tee -a /etc/apt/sources.list.d/google-cloud-sdk.list

RUN  apt-get install apt-transport-https ca-certificates gnupg -y
RUN  apt-get install curl -y

RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg |  apt-key --keyring /usr/share/keyrings/cloud.google.gpg add -

RUN apt-get update &&  apt-get install google-cloud-sdk -y

RUN  apt-get update &&  apt-get --only-upgrade install google-cloud-sdk-skaffold kubectl google-cloud-sdk-anthos-auth google-cloud-sdk-minikube google-cloud-sdk google-cloud-sdk-app-engine-grpc google-cloud-sdk-kind google-cloud-sdk-pubsub-emulator google-cloud-sdk-app-engine-go google-cloud-sdk-firestore-emulator google-cloud-sdk-cloud-build-local google-cloud-sdk-datastore-emulator google-cloud-sdk-kpt google-cloud-sdk-app-engine-python google-cloud-sdk-spanner-emulator google-cloud-sdk-cbt google-cloud-sdk-bigtable-emulator google-cloud-sdk-app-engine-python-extras google-cloud-sdk-datalab google-cloud-sdk-app-engine-java -y
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
