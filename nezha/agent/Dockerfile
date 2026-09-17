FROM --platform=$BUILDPLATFORM debian:trixie AS build
ARG VERSION
ARG TARGETARCH
WORKDIR /root
RUN apt update && \
    apt install -y curl jq unzip libcap2-bin
RUN TAG=$(curl -s https://api.github.com/repos/nezhahq/agent/releases | jq -r '.[0].tag_name') && \
    curl -L -O https://github.com/nezhahq/agent/releases/download/${VERSION}/nezha-agent_linux_${TARGETARCH}.zip && \
    unzip -p nezha-agent_linux_${TARGETARCH}.zip nezha-agent > /root/nezha-agent-${TARGETARCH} && \
    chmod +x nezha-agent-${TARGETARCH} && \
    setcap cap_net_raw+ep nezha-agent-${TARGETARCH};

FROM gcr.io/distroless/static-debian13:nonroot AS nezha-agent
#FROM alpine:edge AS nezha-agent
ARG TARGETARCH
COPY --from=build /root/nezha-agent-${TARGETARCH} /usr/local/bin/nezha-agent
ENTRYPOINT ["/usr/local/bin/nezha-agent"]
#ENTRYPOINT ["sh", "-c", "tail -f /dev/null"]
