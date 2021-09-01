FROM ubuntu:16.04

# install Wordnet
RUN apt update
RUN apt-get -y install tcl8.5
RUN apt-get -y install tcl8.5-dev
RUN apt-get -y install tk8.5-dev
RUN apt-get -y install wget

RUN wget http://wordnetcode.princeton.edu/3.0/WordNet-3.0.tar.gz && tar -xf WordNet-3.0.tar.gz
WORKDIR WordNet-3.0
RUN ./configure --with-tcl=/usr/lib/tcl8.5 --with-tk=/usr/lib/tk8.5 && make && make install

# install Tomcat
RUN apt-get -y install default-jdk

RUN groupadd tomcat
RUN useradd -s /bin/false -g tomcat -d /opt/tomcat tomcat

WORKDIR ../tomcat
RUN wget https://dlcdn.apache.org/tomcat/tomcat-8/v8.5.70/bin/apache-tomcat-8.5.70.tar.gz
RUN mkdir /opt/tomcat
RUN tar xzvf apache-tomcat-8*tar.gz -C /opt/tomcat --strip-components=1

WORKDIR /opt/tomcat
RUN chgrp -R tomcat /opt/tomcat
RUN chmod -R g+r conf && chmod g+x conf
RUN chown -R tomcat webapps/ work/ temp/ logs/

EXPOSE 8080

CMD /opt/tomcat/bin/startup.sh && tail -f /dev/null

# build with: docker build . -t tomcat-wordnet
# run with: docker run -d -p 8081:8080 tomcat-wordnet
