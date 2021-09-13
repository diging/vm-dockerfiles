FROM openjdk:8-jre

RUN groupadd neo4j
RUN useradd -s /bin/false -g neo4j -d /opt/neo4j-community-2.2.9 neo4j

EXPOSE 7474

CMD /opt/neo4j-community-2.2.9/bin/neo4j start && tail -f /dev/null

# build with: docker build . -t neo4j-2.2.9
# run with: docker run -d -p 7475:7474 neo4j-2.2.9
