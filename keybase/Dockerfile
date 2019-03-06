# Run Keybase messenger in a container
#
# docker run -it \
#	-v /tmp/.X11-unix:/tmp/.X11-unix \ # mount the X11 socket
#       -v /etc/localtime:/etc/localtime:ro \
#       -v $HOME/.config/keybase:/home/keybase/.config/keybase \ # for persistence
#       -v $HOME/.config/Keybase:/home/keybase/.config/Keybase \ # for persistence
#       -v $HOME/.local/share/keybase:/home/keybase/.local/share/keybase \ # for persistence
#	-e DISPLAY=unix$DISPLAY \
#	--device /dev/snd \ # so we have sound
#	--name keybase \
#	egdio/keybase

# Base docker image
FROM debian:sid-slim
LABEL maintainer "Ethan Gregory Dodge <ethan@egd.io>"

# Install libraries for GUI
RUN apt update && apt install -y \
    libnss3 \
    libgtk-3-0 \
    libx11-xcb1 \
    libxtst6 \
    libxss1 \
    libasound2 \
    --no-install-recommends

# Add keybase user
RUN groupadd -r keybase && useradd -r -g keybase -G audio,video keybase \
    && mkdir /home/keybase && chown -R keybase:keybase /home/keybase

# Install keybase and dependencies
RUN apt update && apt install -y \
    curl \
    && apt install -y \
    libappindicator1 \
    fuse \
    libgconf-2-4 \
    psmisc \
    lsof \
    procps \
    --no-install-recommends \
    && curl --remote-name https://prerelease.keybase.io/keybase_amd64.deb \
    && dpkg -i keybase_amd64.deb \ 
    && rm -rf /var/lib/apt/lists/*

# Run as non privileged user
USER keybase

ENTRYPOINT run_keybase -g && /opt/keybase/Keybase 
