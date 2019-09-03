FROM mysql/mysql-server:5.7

USER root

RUN yum install wget -y
RUN yum install tar -y

WORKDIR /opt

RUN wget https://archive.apache.org/dist/hadoop/core/hadoop-2.7.3/hadoop-2.7.3.tar.gz
RUN tar -zxf hadoop-2.7.3.tar.gz
RUN rm hadoop-2.7.3.tar.gz
ADD configs/core-site.xml.template /opt/hadoop-2.7.3/etc/hadoop/core-site.xml
ADD ../utils/jars/alluxio-2.0.0-client.jar /opt/hadoop-2.7.3/lib/

RUN wget http://archive.apache.org/dist/hive/hive-2.1.0/apache-hive-2.1.0-bin.tar.gz
RUN tar -zxf apache-hive-2.1.0-bin.tar.gz
RUN rm apache-hive-2.1.0-bin.tar.gz
ADD configs/hive-site.xml.template /opt/apache-hive-2.1.0-bin/conf/hive-site.xml

RUN yum install java-1.8.0-openjdk -y

ENV HADOOP_VERSION="2.7.3"
ENV HADOOP_HOME=/opt/hadoop-2.7.3
ENV HADOOP_CONF_DIR=${HADOOP_HOME}/etc/hadoop
ENV HADOOP_CLASSPATH=${HADOOP_HOME}/lib/alluxio-2.0.0-client.jar:${HADOOP_CLASSPATH}
ENV JAVA_HOME=/usr
ENV HIVE_HOME=/opt/apache-hive-2.1.0-bin
ENV HIVE_CONF_DIR=${HIVE_HOME}/conf
ENV PATH=$PATH:${HIVE_HOME}/bin:${HADOOP_HOME}/bin:.:

RUN yum install mysql-connector-java -y
RUN ln -s /usr/share/java/mysql-connector-java.jar $HIVE_HOME/lib/mysql-connector-java.jar

RUN adduser -m datamaster
RUN usermod -aG wheel datamaster

WORKDIR /opt/hadoop-2.7.3/bin
