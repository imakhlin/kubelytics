ARG MARIADB_VERSION=10.4

FROM mariadb:${MARIADB_VERSION}

# Note, base image assumes that "mariadb" user have UID/GID=1001
ARG MARIADB_UNAME=mysql
ARG MARIADB_UID=1001
ARG MARIADB_GID=1001

USER root

# Create iguazio user/group
RUN set -ex \
    && mkdir -p /home/$MARIADB_UNAME \
    && usermod \
      -u $MARIADB_UID \
      --shell /bin/bash $MARIADB_UNAME \
    && groupmod -g $MARIADB_GID $MARIADB_UNAME \
    && apt-get update -q \
    && apt-get install -yq --no-install-recommends curl \
    && chown -R $MARIADB_UID:$MARIADB_GID /home/$MARIADB_UNAME \
    && mkdir -p /var/lib/mysql/mysql \
    && chown -R $MARIADB_UID:$MARIADB_GID /var/lib/mysql \
    && chown -R $MARIADB_UID:$MARIADB_GID /usr/lib/mysql/plugin/auth_pam_tool_dir

USER $MARIADB_UID

ARG MYSQL_ROOT_PASSWORD=ChangeMe!
ENV MYSQL_ROOT_PASSWORD=${MYSQL_ROOT_PASSWORD}
ENV HOME /home/$MARIADB_UNAME

RUN set -x \
    && echo "alias ll='ls -lhaG'" >> $HOME/.bash_aliases \
    && . $HOME/.bash_aliases
