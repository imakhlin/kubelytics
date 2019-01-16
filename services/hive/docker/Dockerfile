ARG DOCKER_USER
ARG HADOOP_VERSION
ARG HIVE_VERSION
ARG HIVE_DB_TYPE
ARG PSQL_JDBC_VERSION
ARG MYSQL_JDBC_VERSION
ARG IGUAZIO_RELEASE_VERSION
ARG BASE_DOCKER_USER

FROM bde2020/hive as hive-base
FROM $DOCKER_USER/hadoop:${HADOOP_VERSION}

ENV HIVE_VERSION=${HIVE_VERSION:-2.3.3}
ENV HIVE_BASE_HOME /opt/hive
ENV HIVE_HOME /hive
ENV HIVE_CONF_DIR ${HIVE_HOME}/etc
ENV PATH ${HIVE_HOME}/bin:$PATH

# Set hive metastore database type
ENV HIVE_DB_TYPE=${HIVE_DB_TYPE:-postgres}

# Support PostgreSQL
ENV PSQL_JDBC_VERSION=${PSQL_JDBC_VERSION:-42.2.5}
ENV PSQL_JDBC_DRIVER=https://jdbc.postgresql.org/download/postgresql-${PSQL_JDBC_VERSION}.jar

# Support MySQL (MariaDB)
ENV MYSQL_JDBC_VERSION=${MYSQL_JDBC_VERSION:-2.3.0}
ENV MYSQL_JDBC_DRIVER=https://downloads.mariadb.com/Connectors/java/connector-java-${MYSQL_JDBC_VERSION}/mariadb-java-client-${MYSQL_JDBC_VERSION}.jar

USER root

COPY --chown=1000:1000 --from=hive-base $HIVE_BASE_HOME $HIVE_HOME
COPY --chown=1000:1000 --from=hive-base /usr/local/bin/startup.sh /usr/local/bin/startup.sh
COPY --chown=1000:1000 --from=hive-base /usr/local/bin/entrypoint.sh /usr/local/bin/entrypoint.sh

RUN set -ex && \
  wget --quiet ${PSQL_JDBC_DRIVER} -P ${HIVE_HOME}/lib/ && \
  wget --quiet ${MYSQL_JDBC_DRIVER} -P ${HIVE_HOME}/lib/ &&\
  chown -R ${CONTAINER_USER}:${CONTAINER_USER} ${HIVE_HOME} &&\
	chmod 0755 /usr/local/bin/startup.sh && \
	chmod 0755 /usr/local/bin/entrypoint.sh

EXPOSE 10000
EXPOSE 10002

ENTRYPOINT ["entrypoint.sh"]

CMD ["startup.sh"]

USER ${CONTAINER_USER}
