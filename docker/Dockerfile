FROM python:3.10-slim-bullseye as builder

RUN apt-get update \
    && apt-get install -y wget ca-certificates git gosu \
    && git clone https://github.com/jiangbaoming/Movie_Data_Capture.git \
    && mv Movie_Data_Capture /mdc \
    && cd /mdc \
    && pip install --upgrade \
        pip \
        pyinstaller \
    && ( pip install --no-cache-dir -r requirements.txt || true ) \
    && pip install --no-cache-dir requests lxml Beautifulsoup4 pillow \
    && pyinstaller \
        -D -n Movie_Data_Capture Movie_Data_Capture.py \
    && cp /mdc/config.ini /mdc/dist/Movie_Data_Capture/config.template

FROM debian:11-slim

ENV TZ="Asia/Shanghai"
ENV UID=99
ENV GID=100
ENV UMASK=002

ADD docker-entrypoint.sh docker-entrypoint.sh
COPY --from=builder /mdc/dist/Movie_Data_Capture /app

RUN \
    apt-get -y update && apt-get -y upgrade \
    && apt install -y -q \
        gosu \
    && apt-get autoremove --purge -y \
    && apt-get clean -y \
    && chmod +x docker-entrypoint.sh \
    && mkdir -p /data /config \
    && useradd -d /config -s /bin/sh mdc \
    && chown -R mdc /data /config

VOLUME [ "/data", "/config" ]

ENTRYPOINT ["/docker-entrypoint.sh"]
