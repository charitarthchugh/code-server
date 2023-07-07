ARG NVIDIA_IMAGE=nvcr.io/nvidia/cuda:12.2.0-devel-ubuntu20.04
FROM $NVIDIA_IMAGE

LABEL org.opencontainers.image.source https://github.com/charitarthchugh/code-server
RUN apt update && apt upgrade -y
RUN apt install curl -y
RUN curl -fsSL https://code-server.dev/install.sh | sh

WORKDIR /root
EXPOSE 8081

ENTRYPOINT [ "code-server" ]
CMD [ "--bind-addr", "0.0.0.0:8081" ]