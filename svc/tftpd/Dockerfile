FROM debian:bookworm-slim

RUN apt-get update \
    && apt-get install -y --no-install-recommends tftpd-hpa \
    && rm -rf /var/lib/apt/lists/*

CMD ["/usr/sbin/in.tftpd", "--foreground", "--listen", "--secure", "--blocksize", "512", "--address", "0.0.0.0:69", "/var/tftpboot"]