# Run Keybase messenger in a container
#
# docker run -it \
#	-v /tmp/.X11-unix:/tmp/.X11-unix \ # mount the X11 socket
#       -v /etc/localtime:/etc/localtime:ro \
#       -v $HOME/.config/keybase:/home/kb/.config/keybase \ # for persistence
#       -v $HOME/.config/Keybase:/home/kb/.config/Keybase \ # for persistence
#       -v $HOME/.local/share/keybase:/home/kb/.local/share/keybase \ # for persistence
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
RUN groupadd -r kb && useradd -r -g kb -G audio,video kb \
    && mkdir /home/kb && chown -R kb:kb /home/kb

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
    && rm -rf /var/lib/apt/lists/* \
    && rm keybase_amd64.deb

# Run as non privileged user
USER kb

ENTRYPOINT run_keybase -g && /opt/keybase/Keybase 
