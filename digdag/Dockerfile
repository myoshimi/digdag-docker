FROM openjdk:11.0.3-jdk-stretch
MAINTAINER Masato YOSHIMI <yoshimi.masato@tis.co.jp>

# docker run -it --rm -v /var/run/docker.sock:/var/run/docker.sock digdag /bin/bash

ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64

RUN set -xe && \
    apt-get update && \
    apt-get install -y \
        curl \
        apt-transport-https \
        ca-certificates \
        gnupg2 \
        gettext-base \
        postgresql-client \
        software-properties-common \
        jq && \
    curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | apt-key add - && \
    add-apt-repository    "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
        $(lsb_release -cs) \
        stable" && \
    apt-get update && apt-get install -y docker-ce && \
    curl -L https://github.com/docker/compose/releases/download/$(curl -sS https://api.github.com/repos/docker/compose/tags | jq ".[].name" | grep -v rc | grep -E '^"[0-9.]+"' | grep -oP '[0-9\.]+' | head -1)/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose && \
    chmod 755 /usr/local/bin/docker-compose && \
    apt-get clean all

# VOLUME [ /var/run/docker.sock ]

RUN curl -o /usr/local/bin/digdag --create-dirs -L "https://dl.digdag.io/digdag-latest" && \
    chmod +x /usr/local/bin/digdag

EXPOSE 65432 65433

COPY ./entrypoint.sh /usr/local/bin/
COPY ./server.properties /etc/digdag/

ENTRYPOINT ["/usr/local/bin/entrypoint.sh", "/usr/local/bin/digdag", "server", "--bind", "0.0.0.0", "--config", "/etc/digdag/server.properties"]
