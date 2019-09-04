# Base docker image
FROM debian:buster-slim
LABEL maintainer "Ethan Gregory Dodge <ethan@egd.io>"

# Install depedencies, add Signal key and repo, install 
RUN apt update && apt install -y \
    magic-wormhole \
    --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

ENTRYPOINT [ "wormhole" ]
