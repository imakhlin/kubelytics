ARG POSTGRES_VERSION=11.1.0

FROM bitnami/postgresql:${POSTGRES_VERSION}

# Note, base image assumes that "postgres" user have UID/GID=1001
ARG POSTGRES_UNAME=postgres
ARG POSTGRES_UID=1001
ARG POSTGRES_GID=1001

# Allow control over the NAMI logging level. Set to "trace" for debug.
ARG NAMI_LOG_LEVEL=info

ENV NAMI_LOG_LEVEL=${NAMI_LOG_LEVEL}

# Note, PGDATA should refer to the mount point.
ENV PGDATA=/bitnami/postgresql

USER root

# Create iguazio user/group
RUN set -ex && \
    groupadd -g $POSTGRES_UID $POSTGRES_UNAME && \
    useradd -u $POSTGRES_UID -g $POSTGRES_UNAME --create-home -d /home/$POSTGRES_UNAME -s /bin/bash $POSTGRES_UNAME && \
    echo "alias ll='ls -la'" >> /home/$POSTGRES_UNAME/.bash_aliases && \
    chown -R $POSTGRES_UID:$POSTGRES_GID /home/$POSTGRES_UNAME && \
    chown -R $POSTGRES_UID:$POSTGRES_GID /opt/bitnami/postgresql
