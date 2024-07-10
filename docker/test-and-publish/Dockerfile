FROM node:18-bullseye

# typescript
RUN npm install -g pnpm 

# python and java
RUN apt-get update && apt-get install -y lsof python3 python3-pip python3-venv maven openjdk-11-jdk golang
RUN pip3 install poetry
# clean up python installations
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# csharp
RUN curl -Lo dotnet-install.sh https://dot.net/v1/dotnet-install.sh && \
    chmod +x ./dotnet-install.sh && \
    ./dotnet-install.sh --channel 7.0 && \
    ./dotnet-install.sh --channel 5.0 && \
    ./dotnet-install.sh --channel 3.1
ENV PATH=$PATH:/root/.dotnet

# use bash as default shell
SHELL ["/bin/bash", "-c"]