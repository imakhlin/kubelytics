ARG DOCKER_USER
ARG POSTGRES_VERSION=11.1.0

ARG POSTGRES_UNAME=postgres
ARG POSTGRES_UID
ARG POSTGRES_GID

ARG PG_HOME

FROM bitnami/postgresql:${POSTGRES_VERSION}

USER root

# Note, base image assumes that "postgres" user have UID/GID=1001
ENV POSTGRES_UID=${POSTGRES_UID:-1001} \
    POSTGRES_GID=${POSTGRES_GID:-1001} \
    POSTGRES_UNAME=${POSTGRES_UNAME:-"postgres"} \
    PG_HOME=${PG_HOME:-"/opt/bitnami/postgresql"}

ENV PGDATA=$PG_HOME/data \
    PATH=$PATH:$PG_HOME/bin

# Create iguazio user/group
RUN set -e; \
    groupadd -g $POSTGRES_UID $POSTGRES_UNAME; \
    useradd -m -k /etc/skel/ -u $POSTGRES_UID -g $POSTGRES_UNAME -d /home/$POSTGRES_UNAME -s /bin/bash $POSTGRES_UNAME; \
    echo "alias ll='ls -la'" >> /home/$POSTGRES_UNAME/.bash_aliases; \
    chown -R $POSTGRES_UID:$POSTGRES_GID /home/$POSTGRES_UNAME; \
    chown -R $POSTGRES_UID:$POSTGRES_GID $PG_HOME; \
    unset -v POSTGRES_UID; \
    unset -v POSTGRES_GID; \
    unset -v POSTGRES_UNAME
