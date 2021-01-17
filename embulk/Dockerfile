FROM openjdk:8-jre-alpine

RUN apk add unzip curl
RUN curl --create-dirs -o /usr/local/bin/embulk -L "https://dl.embulk.org/embulk-0.10.23.jar" && chmod +x /usr/local/bin/embulk
RUN apk add --no-cache tzdata && cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && apk del tzdata
RUN apk add --no-cache -f libc6-compat
ENV GLIBC_VER=2.32-r0

# install glibc compatibility for alpine
RUN apk --no-cache add \
        binutils \
    && curl -sL https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -o /etc/apk/keys/sgerrand.rsa.pub \
    && curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-${GLIBC_VER}.apk \
    && curl -sLO https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VER}/glibc-bin-${GLIBC_VER}.apk \
    && apk add --no-cache -f \
        glibc-${GLIBC_VER}.apk \
        glibc-bin-${GLIBC_VER}.apk \
    && apk --no-cache del \
        binutils \
    && rm glibc-${GLIBC_VER}.apk \
    && rm glibc-bin-${GLIBC_VER}.apk \
    && rm -rf /var/cache/apk/*

RUN curl "https://d1vvhvl2y92vvt.cloudfront.net/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip" \
  && unzip "awscliv2.zip" \
  && ./aws/install --bin-dir /usr/local/bin

WORKDIR /embulk

RUN curl --create-dirs -o .embulk/jruby/jruby-complete-9.2.14.0.jar -L https://repo1.maven.org/maven2/org/jruby/jruby-complete/9.2.14.0/jruby-complete-9.2.14.0.jar
RUN echo "jruby=file:///embulk/.embulk/jruby/jruby-complete-9.2.14.0.jar" > .embulk/embulk.properties
RUN embulk gem install msgpack bundler liquid
