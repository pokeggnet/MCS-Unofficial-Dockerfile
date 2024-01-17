#MADE BY CIDER789
FROM node:14-slim

ARG INSTALL_PATH=/opt/mcsm
ARG JDK_VERSION_8=8
ARG JDK_VERSION_11=11
ARG JDK_VERSION_17=17

# Install git and openjdk 11
RUN apt-get update && \
    apt-get install -y git && \
    apt-get install -y openjdk-${JDK_VERSION_11}-jre-headless && \
    rm -rf /var/lib/apt/lists/*

# Install openjdk 8 from AdoptOpenJDK
RUN apt-get update && \
    apt-get install -y wget gnupg && \
    wget -qO - https://adoptopenjdk.jfrog.io/adoptopenjdk/api/gpg/key/public | gpg --dearmor > /usr/share/keyrings/adoptopenjdk-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/adoptopenjdk-archive-keyring.gpg] https://adoptopenjdk.jfrog.io/adoptopenjdk/deb/ buster main" | tee /etc/apt/sources.list.d/adoptopenjdk.list > /dev/null && \
    apt-get update && \
    apt-get install -y adoptopenjdk-${JDK_VERSION_8}-hotspot-jre && \
    rm -rf /var/lib/apt/lists/*

# Install openjdk 17 from Debian Bullseye repositories
RUN echo "deb http://deb.debian.org/debian bullseye main" >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y openjdk-17-jdk && \
    rm -rf /var/lib/apt/lists/*

# Install mcsm
RUN git clone https://github.com/MCSManager/MCSManager-Daemon-Production $INSTALL_PATH/daemon && \
    cd $INSTALL_PATH/daemon && \
    npm i --production

# Set environment variables
ENV TZ=Asia/Shanghai
ENV PUID=0
ENV PGID=0
ENV UMASK=022

# Define volumes, expose ports, and set working directory
VOLUME [ "$INSTALL_PATH/daemon/data", "$INSTALL_PATH/daemon/logs" ]
EXPOSE 24444 25565-25575
WORKDIR $INSTALL_PATH/daemon

# Set the default command to start the application
CMD [ "node", "app.js" ]