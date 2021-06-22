FROM alpine:latest AS builder

RUN apk add --no-cache ca-certificates wget

WORKDIR /tmp

RUN VER=$(curl 'https://api.github.com/repos/AdguardTeam/dnsproxy/releases' | grep dnsproxy-linux-arm64 | cut -d '"' -f 4 | sed -n 2p)

RUN wget https://github.com/AdguardTeam/dnsproxy/releases/download/v$VER/dnsproxy-linux-arm64-v$VER.tar.gz \
    && mkdir dnsproxy && tar -xzf dnsproxy-linux-arm64-v$VER.tar.gz -C dnsproxy && chmod 777 dnsproxy/linux-arm64/dnsproxy

FROM alpine:latest

RUN apk add --no-cache bash

COPY --from=builder /tmp/dnsproxy/linux-arm64/dnsproxy .

CMD ./dnsproxy -l 0.0.0.0 -p 10053 -u quic://dns.adguard.com -b 9.9.9.9:53 -b 8.8.8.8:53 --cache --all-servers --ipv6-disabled

EXPOSE 10053
