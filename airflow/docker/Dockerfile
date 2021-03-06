#  Licensed to the Apache Software Foundation (ASF) under one   *
#  or more contributor license agreements.  See the NOTICE file *
#  distributed with this work for additional information        *
#  regarding copyright ownership.  The ASF licenses this file   *
#  to you under the Apache License, Version 2.0 (the            *
#  "License"); you may not use this file except in compliance   *
#  with the License.  You may obtain a copy of the License at   *
#                                                               *
#    http://www.apache.org/licenses/LICENSE-2.0                 *
#                                                               *
#  Unless required by applicable law or agreed to in writing,   *
#  software distributed under the License is distributed on an  *
#  "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY       *
#  KIND, either express or implied.  See the License for the    *
#  specific language governing permissions and limitations      *
#  under the License.                                           *

FROM ubuntu:16.04

ENV SLUGIFY_USES_TEXT_UNIDECODE=yes

# install deps
RUN apt-get update -y && apt-get install -y \
        wget \
        libczmq-dev \
        libcurlpp-dev \
        software-properties-common \
        curl \
        libssl-dev \
        git \
        inetutils-telnet \
        bind9utils \
        zip \
        unzip \
        locales


RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    locale-gen

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

RUN add-apt-repository ppa:jonathonf/python-3.6
RUN apt-get update
RUN apt-get install -y build-essential python3.6 python3.6-dev python3-pip python3.6-venv
RUN apt-get clean

RUN python3.6 -m pip install pip --upgrade
RUN python3.6 -m pip install wheel


# Since we install vanilla Airflow, we also want to have support for Postgres and Kubernetes
RUN pip install -U setuptools && \
    pip install kubernetes && \
    pip install cryptography && \
    pip install 'redis>=2.10.5,<3' && \
    pip install boto3 && \
    pip install pytz && \
    pip install pyOpenSSL && \
    pip install ndg-httpsclient && \
    pip install pyasn1 && \
    pip install psycopg2-binary==2.7.4  # I had issues with older versions of psycopg2, just a warning

# install airflow
COPY airflow.tar.gz /tmp/airflow.tar.gz
RUN pip install --no-use-pep517 /tmp/airflow.tar.gz

COPY airflow-test-env-init.sh /tmp/airflow-test-env-init.sh

COPY bootstrap.sh /bootstrap.sh
RUN chmod +x /bootstrap.sh

ENTRYPOINT ["/bootstrap.sh"]
