FROM alpine:latest AS builder

RUN apk add --no-cache ca-certificates wget

ARG VER=0.37.6

WORKDIR /tmp

RUN wget https://github.com/AdguardTeam/dnsproxy/releases/download/v$VER/dnsproxy-linux-arm64-v$VER.tar.gz \
    && mkdir dnsproxy && tar -xzf dnsproxy-linux-arm64-v$VER.tar.gz -C dnsproxy && chmod 777 dnsproxy/linux-arm64/dnsproxy

FROM alpine:latest

RUN apk add --no-cache bash

WORKDIR /app

COPY --from=builder /tmp/dnsproxy/linux-arm64/dnsproxy .

CMD ./dnsproxy -l 0.0.0.0 -p 10053 -u tls://doh.doh.my.id -b 9.9.9.9:53 -b 8.8.8.8:53 --cache --all-servers --ipv6-disabled

EXPOSE 10053
