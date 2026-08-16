FROM --platform=$BUILDPLATFORM debian:trixie AS build
WORKDIR /root
RUN apt update && \
    apt install -y curl jq libcap2-bin
RUN TAG=$(curl -s https://api.github.com/repos/komari-monitor/komari-agent/releases | jq -r '.[0].tag_name') && \
    for arch in amd64 arm64; do \
      curl -L -O https://github.com/komari-monitor/komari-agent/releases/download/${TAG}/komari-agent-linux-${arch} && \
      chmod +x komari-agent-linux-* && \
      setcap cap_net_raw+ep komari-agent-linux-${arch}; \
    done

FROM gcr.io/distroless/static-debian13:nonroot AS komari-agent
ARG TARGETARCH
WORKDIR /home/nonroot/komari-agent
COPY --from=build /root/komari-agent-linux-${TARGETARCH} /usr/local/bin/komari-agent
ENTRYPOINT ["/usr/local/bin/komari-agent"]
CMD ["--help"]
