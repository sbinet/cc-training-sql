from postgres:11

RUN apt-get update \
 && apt-get install -y --no-install-recommends \
build-essential \
ca-certificates \
curl \
git \
postgresql postgresql-contrib postgresql-plpython \
python-dev python-pip \
sudo

RUN pip install --no-cache-dir notebook==5.*

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

## configure postgres db
RUN ./binder/setup-db

USER ${NB_USER}

## install the postgres kernel
RUN ./binder/setup-pgsql-kernel

## COPY ./seed.sql /docker-entrypoint-initdb.d/seed.sql

### COPY ./seed.sql /tmp/seed.sql
### 
### COPY scripts/init_docker_postgres.sh /docker-entrypoint-initdb.d/
