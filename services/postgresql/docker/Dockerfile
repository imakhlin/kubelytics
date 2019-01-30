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

RUN echo "Running as user: $(id -u):$(id -g)"

USER root

# Create iguazio user/group
RUN set -ex && \
    groupadd -g $POSTGRES_UID $POSTGRES_UNAME && \
    useradd --uid $POSTGRES_UID --gid $POSTGRES_UNAME --home /home/$POSTGRES_UNAME --shell /bin/bash --create-home $POSTGRES_UNAME

RUN chown -R $POSTGRES_UID:$POSTGRES_GID /home/$POSTGRES_UNAME && \
    chown -R $POSTGRES_UID:$POSTGRES_GID /opt/bitnami && \
    chown -R $POSTGRES_UID:$POSTGRES_GID /bitnami/postgresql && \
    chown -R $POSTGRES_UID:$POSTGRES_GID /.nami && \
    mkdir $PGDATA/data && \
    chown -R $POSTGRES_UID:$POSTGRES_GID $PGDATA

USER $POSTGRES_UID

ENV HOME /home/$POSTGRES_UNAME

RUN set -x && \
    echo "alias ll='ls -lhaG'" >> $HOME/.bash_aliases && \
    . $HOME/.bash_aliases
