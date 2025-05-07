ARG BUILD_FROM=python:3.11-alpine
FROM ${BUILD_FROM}

ENV LANG C.UTF-8
ENV PYTHONUNBUFFERED 1

ARG DAHUA_BRANCH="master"

WORKDIR /app

# Abhängigkeiten installieren
RUN apk update && \
    apk upgrade && \
    apk add --no-cache \
    git \
    build-base \
    python3-dev \
    py3-pip && \
    pip install --upgrade pip

# Code und Konfiguration kopieren
RUN git clone https://gitlab.com/elad.bar/DahuaVTO2MQTT.git --branch ${DAHUA_BRANCH} . && \
    pip install --no-cache-dir \
    "paho-mqtt==1.6.1" \
    requests \
    prometheus_client

# StrEnum-Patch anwenden
RUN sed -i '/from enum import StrEnum/c\from enum import Enum\n\nclass StrEnum(str, Enum):\n    def __str__(self):\n        return self.value\n' /app/common/enums.py

ADD config.json /app/version.json
COPY run.sh /app/run.sh
RUN chmod +x /app/run.sh

CMD ["/app/run.sh"]
