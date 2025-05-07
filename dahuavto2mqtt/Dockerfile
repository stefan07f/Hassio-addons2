ARG BUILD_FROM=python:3.11-alpine
FROM ${BUILD_FROM}

ENV LANG C.UTF-8
ENV PYTHONUNBUFFERED 1

ARG DAHUA_BRANCH="master"

WORKDIR /app

# Abhängigkeiten installieren – Python ist schon drin, also nur Git und Compiler
RUN apk update && \
    apk upgrade && \
    apk add --no-cache \
    git \
    build-base

# Code klonen und Python-Abhängigkeiten installieren
RUN git clone https://gitlab.com/elad.bar/DahuaVTO2MQTT.git --branch ${DAHUA_BRANCH} . && \
    pip install --no-cache-dir \
    "paho-mqtt==1.6.1" \
    requests \
    prometheus_client

# Patch für StrEnum (für Python < 3.11 Kompatibilität, obwohl du 3.11 nutzt – sicher ist sicher)
RUN sed -i '/from enum import StrEnum/c\from enum import Enum\n\nclass StrEnum(str, Enum):\n    def __str__(self):\n        return self.value\n' /app/common/enums.py

# Konfiguration und Startskript
ADD config.json /app/version.json
COPY run.sh /app/run.sh
RUN chmod +x /app/run.sh

CMD ["/app/run.sh"]
