FROM openjdk:8-jre

RUN groupadd neo4j
RUN useradd -s /bin/false -g neo4j -d /opt/neo4j-community-2.2.9 neo4j

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

CMD /opt/tomcat/bin/startup.sh && /opt/neo4j-community-2.2.9/bin/neo4j start && tail -f /dev/null

# build with: docker build . -t qstore4s
# run with: docker run -d -p 8081:8080 qstore4s
