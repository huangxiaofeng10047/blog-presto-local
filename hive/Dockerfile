FROM openjdk:8u242-jre

WORKDIR /opt

ENV HADOOP_VERSION=3.2.0
ENV HIVE_VERSION=3.1.2
ENV AWS_SDK_VERSION=1.11.375
ENV HADOOP_HOME=/opt/hadoop-${HADOOP_VERSION}
ENV HIVE_HOME=/opt/apache-hive-${HIVE_VERSION}-bin

RUN curl -sSL https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-${HIVE_VERSION}/apache-hive-${HIVE_VERSION}-bin.tar.gz | tar zxf -
RUN curl -sSL https://archive.apache.org/dist/hadoop/common/hadoop-${HADOOP_VERSION}/hadoop-${HADOOP_VERSION}.tar.gz | tar zxf -
RUN curl -sSL https://dev.mysql.com/get/Downloads/Connector-J/mysql-connector-java-8.0.19.tar.gz | tar -zx --strip-components 1 -C ${HIVE_HOME}/lib -f - mysql-connector-java-8.0.19/mysql-connector-java-8.0.19.jar
RUN ln -s ${HADOOP_HOME}/share/hadoop/tools/lib/aws-java-sdk-bundle-${AWS_SDK_VERSION}.jar ${HIVE_HOME}/lib/
RUN ln -s ${HADOOP_HOME}/share/hadoop/tools/lib/hadoop-aws-${HADOOP_VERSION}.jar ${HIVE_HOME}/lib/
RUN apt-get update -y && apt-get install -y netcat procps

COPY scripts/entrypoint.sh scripts/wait-for /usr/local/bin/
COPY conf/hive-site.xml ${HIVE_HOME}/conf
COPY conf/hdfs-site.xml ${HADOOP_HOME}/conf

ENV HADOOP_CLASSPATH=${HADOOP_HOME}/share/hadoop/tools/lib/aws-java-sdk-bundle-*.jar:${HADOOP_HOME}/share/hadoop/tools/lib/hadoop-aws-${HADOOP_VERSION}.jar
ENV PATH=${HIVE_HOME}/bin:${PATH}

ENTRYPOINT "/usr/local/bin/entrypoint.sh"
