# Wordnet doesn't install on Ubuntu 20, so we have to use 16
FROM ubuntu:16.04

ARG TOMCAT_USER
ARG TOMCAT_PASSWORD

# install Wordnet
RUN apt update
RUN apt-get -y install tcl8.5
RUN apt-get -y install tcl8.5-dev
RUN apt-get -y install tk8.5-dev
RUN apt-get -y install wget

RUN wget http://wordnetcode.princeton.edu/3.0/WordNet-3.0.tar.gz && tar -xf WordNet-3.0.tar.gz
WORKDIR WordNet-3.0
RUN ./configure --with-tcl=/usr/lib/tcl8.5 --with-tk=/usr/lib/tk8.5 && make && make install

# install Java 17
RUN apt-get -y install software-properties-common
RUN apt update
RUN add-apt-repository ppa:linuxuprising/java
RUN apt update
RUN echo oracle-java17-installer shared/accepted-oracle-license-v1-3 select true | /usr/bin/debconf-set-selections
RUN apt install -y oracle-java17-installer --install-recommends

# install Tomcat
RUN groupadd tomcat
RUN useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat

WORKDIR ../tomcat
RUN wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.18/bin/apache-tomcat-10.1.18.tar.gz
RUN mkdir /opt/tomcat
RUN tar xzvf apache-tomcat-10.*.gz -C /opt/tomcat --strip-components=1

WORKDIR /opt/tomcat
RUN chgrp -R tomcat /opt/tomcat
RUN chmod -R g+r conf && chmod g+x conf
RUN chown -R tomcat webapps/ work/ temp/ logs/

# allow Jenkins to deploy to tomcat
COPY files/context.xml /opt/tomcat/webapps/manager/META-INF/

# set up Tomcat users
COPY config/cp.env .
COPY files/tomcat-users.xml /opt/tomcat/conf/
RUN source cp.env && sed -i -e 's/\[username\]/'$TOMCAT_USER'/' /opt/tomcat/conf/tomcat-users.xml
RUN source cp.env && sed -i -e 's/\[password\]/'$TOMCAT_PASSWORD'/' /opt/tomcat/conf/tomcat-users.xml

EXPOSE 8080

CMD /opt/tomcat/bin/startup.sh && tail -f /dev/null

# build with: docker build . -t tomcat-wordnet
# run with: docker run -d -p 8081:8080 tomcat-wordnet
