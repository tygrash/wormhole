FROM prestosql/presto:316

USER root
RUN useradd -m datamaster
RUN chown -R datamaster:datamaster /usr/lib/
RUN chown -R datamaster:datamaster /home/presto/

USER datamaster
RUN mkdir -p /home/presto/data

WORKDIR /usr/lib/presto/

ADD presto-worker/configs/config.properties.template etc/config.properties
ADD presto-worker/configs/jvm.config.template etc/jvm.config
ADD presto-worker/configs/log.properties.template etc/log.properties
ADD presto-worker/configs/node.properties.template etc/node.properties

RUN mkdir catalog
ADD presto-worker/configs/hive.properties.template etc/catalog/hive.properties

ADD utils/jars/alluxio-2.0.0-client.jar plugin/hive-hadoop2
ADD utils/jars/json-serde-1.3.8-jar-with-dependencies.jar plugin/hive-hadoop2
ADD utils/jars/json-udf-1.3.8-jar-with-dependencies.jar plugin/hive-hadoop2s

CMD ["./bin/launcher", "run"]