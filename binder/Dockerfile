from ubuntu:bionic

ENV PG_MAJOR 10
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
	build-essential \
	ca-certificates \
	curl \
	git \
	postgresql postgresql-contrib \
	postgresql-plpython-${PG_MAJOR} \
	python3 python3-pip \
	sudo

RUN update-alternatives --install /usr/bin/python python /usr/bin/python3 1
RUN update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 1

RUN pip install --upgrade pip wheel setuptools
RUN pip install --no-cache-dir notebook==5.*
RUN pip install postgres_kernel

ENV NB_USER jovyan
ENV NB_UID 1000
ENV HOME /home/${NB_USER}

RUN adduser --disabled-password \
    --gecos "Default user" \
    --uid ${NB_UID} \
    ${NB_USER}

# Make sure the contents of our repo are in ${HOME}
COPY . ${HOME}
USER root
RUN chown -R ${NB_UID} ${HOME}

RUN ls ${HOME}/*

## configure postgres db
RUN ${HOME}/binder/setup-db

USER ${NB_USER}

## install the postgres kernel
RUN ${HOME}/binder/setup-pgsql-kernel

## COPY ./seed.sql /docker-entrypoint-initdb.d/seed.sql

### COPY ./seed.sql /tmp/seed.sql
### 
### COPY scripts/init_docker_postgres.sh /docker-entrypoint-initdb.d/
