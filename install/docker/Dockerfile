FROM alpine:3.6

LABEL description="Airsonic is a free, web-based media streamer, providing ubiquitious access to your music." \
      url="https://github.com/airsonic/airsonic"

ENV AIRSONIC_PORT=4040 AIRSONIC_DIR=/airsonic CONTEXT_PATH=/ AIRSONIC_WAR_URL="https://github.com/airsonic/airsonic/releases/download/v10.1.1/airsonic.war"

WORKDIR $AIRSONIC_DIR

RUN apk --no-cache add \
    ffmpeg \
    lame \
    bash \
    libressl \
    fontconfig \
    ttf-dejavu \
    ca-certificates \
    tini \
    openjdk8-jre \
    shadow

COPY run.sh /usr/local/bin/run.sh
ADD $AIRSONIC_WAR_URL airsonic.war
RUN groupadd -g 1337 data && \
    useradd -m -u 2002 -Ng data airsonic -s /bin/bash && \
    chown -R airsonic:data $AIRSONIC_DIR /usr/local/bin/run.sh && \
    chmod +x /usr/local/bin/run.sh && \
    chmod +r airsonic.war

EXPOSE $AIRSONIC_PORT

USER airsonic
ENTRYPOINT ["tini", "--", "run.sh"]
