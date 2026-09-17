FROM --platform=$BUILDPLATFORM alpine:latest AS build
ARG TARGETARCH
ARG VERSION
WORKDIR /root
RUN apk update && \
    apk add --no-cache curl jq
RUN TAG=$(curl -s https://api.github.com/repos/nxtrace/NTrace-core/releases | jq -r '.[0].tag_name') && \
    curl -L -O https://github.com/nxtrace/NTrace-core/releases/download/${VERSION}/nexttrace_linux_${TARGETARCH} && \
    curl -L -O https://github.com/nxtrace/NTrace-core/releases/download/${VERSION}/nexttrace-tiny_linux_${TARGETARCH} && \
    curl -L -O https://github.com/nxtrace/NTrace-core/releases/download/${VERSION}/ntr_linux_${TARGETARCH} && \
    chmod +x *_linux_*
#ENTRYPOINT ["sh", "-c", "tail -f /dev/null"]

FROM scratch AS nexttrace
ARG TARGETARCH
COPY --from=build /root/nexttrace_linux_${TARGETARCH} /usr/local/bin/nexttrace
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
ENTRYPOINT ["nexttrace"]

FROM scratch AS tiny
ARG TARGETARCH
COPY --from=build /root/nexttrace-tiny_linux_${TARGETARCH} /usr/local/bin/nexttrace-tiny
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
ENTRYPOINT ["nexttrace-tiny"]

FROM scratch AS ntr
ARG TARGETARCH
COPY --from=build /root/ntr_linux_${TARGETARCH} /usr/local/bin/ntr
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
ENTRYPOINT ["ntr"]
