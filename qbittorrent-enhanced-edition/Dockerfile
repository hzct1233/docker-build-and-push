FROM alpine:latest AS build
ARG TARGETARCH
ARG VERSION
WORKDIR /root
RUN apk update && \
    apk add --no-cache curl jq 7zip
RUN case ${TARGETARCH} in \
    "amd64") TARGETARCH2="x86_64" ;; \
    "arm64") TARGETARCH2="aarch64" ;; \
    *) echo "不支持的架构: ${TARGETARCH}" && exit 1 ;; \
    esac && \
    TAG=$(curl -s https://api.github.com/repos/c0re100/qBittorrent-Enhanced-Edition/releases/latest | jq -r '.tag_name') && \
    curl -fL -O https://github.com/c0re100/qBittorrent-Enhanced-Edition/releases/download/${VERSION}/qbittorrent-enhanced-nox_${TARGETARCH2}-linux-musl_static.zip && \
    7z x qbittorrent-enhanced-nox_${TARGETARCH2}-linux-musl_static.zip qbittorrent-nox -so > qbittorrent-nox-${TARGETARCH}
RUN chmod +x qbittorrent-nox-${TARGETARCH}
# ENTRYPOINT ["sh", "-c", "tail -f /dev/null"]

FROM alpine:latest
ARG TARGETARCH
COPY --from=build /root/qbittorrent-nox-${TARGETARCH} /usr/local/bin/qbittorrent-nox
EXPOSE 8080/tcp 12345/tcp 12345/udp
VOLUME /etc/qBittorrent
ENV QBT_WEBUI_PORT=8080 QBT_TORRENTING_PORT=12345
ENTRYPOINT ["qbittorrent-nox"]
CMD ["--profile=/etc", "--confirm-legal-notice"]

# docker buildx build --load --platform linux/amd64,linux/arm64 C:\Users\Administrator\OneDrive\123\备份\容器\镜像\qbee
