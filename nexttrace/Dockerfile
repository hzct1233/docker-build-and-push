FROM --platform=$BUILDPLATFORM alpine:latest AS build
WORKDIR /root
RUN apk update && \
    apk add --no-cache curl jq
RUN TAG=$(curl -s https://api.github.com/repos/nxtrace/NTrace-core/releases | jq -r '.[0].tag_name') && \
    for arch in amd64 arm64; do \
      curl -L -O https://github.com/nxtrace/NTrace-core/releases/download/${TAG}/nexttrace_linux_${arch} && \
      curl -L -O https://github.com/nxtrace/NTrace-core/releases/download/${TAG}/nexttrace-tiny_linux_${arch} && \
      curl -L -O https://github.com/nxtrace/NTrace-core/releases/download/${TAG}/ntr_linux_${arch}; \
    done
RUN chmod +x *_linux_*
#ENTRYPOINT ["sh", "-c", "tail -f /dev/null"]

FROM scratch AS nexttrace
ARG TARGETARCH
COPY --from=build /root/nexttrace_linux_${TARGETARCH} /usr/local/bin/nexttrace
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
ENTRYPOINT ["nexttrace"]
# docker buildx build --no-cache --target nexttrace --load --platform linux/amd64,linux/arm64 C:\Users\Administrator\OneDrive\123\备份\容器\镜像\nexttrace

FROM scratch AS tiny
ARG TARGETARCH
COPY --from=build /root/nexttrace-tiny_linux_${TARGETARCH} /usr/local/bin/nexttrace-tiny
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
ENTRYPOINT ["nexttrace-tiny"]
# docker buildx build --target tiny --load --platform linux/amd64,linux/arm64 C:\Users\Administrator\OneDrive\123\备份\容器\镜像\nexttrace

FROM scratch AS ntr
ARG TARGETARCH
COPY --from=build /root/ntr_linux_${TARGETARCH} /usr/local/bin/ntr
COPY --from=build /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
ENTRYPOINT ["ntr"]
# docker buildx build --target ntr --load --platform linux/amd64,linux/arm64 C:\Users\Administrator\OneDrive\123\备份\容器\镜像\nexttrace
