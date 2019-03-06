# Run Signal messenger in a container
#
# docker run -it \
#	-v /tmp/.X11-unix:/tmp/.X11-unix \ # mount the X11 socket
#       -v /etc/localtime:/etc/localtime:ro \
#       -v $HOME/.config/Signal:/home/signal/.config/Signal \
#	-e DISPLAY=unix$DISPLAY \
#	--device /dev/snd \ # so we have sound
#	--name signal \
#	egdio/signal-desktop

# Base docker image
FROM debian:sid-slim
LABEL maintainer "Ethan Gregory Dodge <ethan@egd.io>"

# Install depedencies, add Signal key and repo, install 
RUN apt update && apt install -y \
    curl \
    gnupg \
    libgtk-3-0 \
    libx11-xcb1 \
    && curl -s https://updates.signal.org/desktop/apt/keys.asc | apt-key add - \
    && echo "deb [arch=amd64] https://updates.signal.org/desktop/apt xenial main" | tee -a /etc/apt/sources.list.d/signal-xenial.list \
    && apt update \
    && apt install -y signal-desktop \
    --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Add signal user
RUN groupadd -r signal && useradd -r -g signal -G audio,video signal \
    && mkdir /home/signal && chown -R signal:signal /home/signal

# Run as non privileged user
USER signal

ENTRYPOINT [ "/usr/local/bin/signal-desktop" ]
