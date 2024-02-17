FROM python:3.10-slim-bullseye

RUN apt-get update \
    && apt-get install -y -q \
        bash \
        wget \
        binutils \
        upx \
        ca-certificates \
        git \
        gosu \
    && git clone https://github.com/jiangbaoming/Movie_Data_Capture.git \
    && mv Movie_Data_Capture /mdc \
    && cd /mdc \
    && pip install --upgrade \
        pip \
        pyinstaller \
    && ( pip install --no-cache-dir -r requirements.txt || true ) \
    && pip install --no-cache-dir requests lxml Beautifulsoup4 pillow \
    && apt-get purge -y wget

ENV TZ="Asia/Shanghai"
ENV UID=1026
ENV GID=100
ENV UMASK=002

WORKDIR  /mdc 

ADD docker-entrypoint.sh docker-entrypoint.sh

RUN chmod +x docker-entrypoint.sh \
    && mkdir -p /data /config \
    && useradd -d /config -s /bin/sh mdc \
    && chown -R mdc /data /config

VOLUME [ "/data", "/config" ]

ENTRYPOINT ["/mdc/docker-entrypoint.sh"]
