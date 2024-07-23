# base image to build a JRE
FROM amazoncorretto:17-alpine AS corretto-jdk17

# set proxy 
# ENV http_proxy http://testproxy.com:8080
# ENV https_proxy http://testproxy.com:8080

# required for strip-debug to work
RUN apk add --no-cache binutils

# Build small JRE image
RUN $JAVA_HOME/bin/jlink \
         --verbose \
         --add-modules ALL-MODULE-PATH \
         --strip-debug \
         --no-man-pages \
         --no-header-files \
         --compress=2 \
         --output /customjre

# main app image
FROM alpine:latest
ENV JAVA_HOME=/jre
ENV PATH="${JAVA_HOME}/bin:${PATH}"

# copy JRE from the base image
COPY --from=corretto-jdk17 /customjre $JAVA_HOME

# docker build -t javacorr:17-alpine .
# docker buildx build --platform linux/amd64 -t javacorr:17-alpine-amd  .