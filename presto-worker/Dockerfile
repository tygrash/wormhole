FROM prestosql/presto:316

USER root
RUN useradd -m datamaster
RUN chown -R datamaster:datamaster /usr/lib/
RUN chown -R datamaster:datamaster /home/presto/

USER datamaster
RUN mkdir -p /home/presto/data

WORKDIR /usr/lib/presto/

ADD configs/config.properties.template etc/config.properties
ADD configs/jvm.config.template etc/jvm.config
ADD configs/log.properties.template etc/log.properties
ADD configs/node.properties.template etc/node.properties

RUN mkdir catalog
ADD configs/hive.properties.template etc/catalog/hive.properties

ADD ../utils/jars/alluxio-2.0.0-client.jar plugin/hive-hadoop2

CMD ["./bin/launcher", "run"]