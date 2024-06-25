FROM node:18-bullseye

# Required for typescript
RUN npm install -g pnpm 

# Installations for python and java
RUN apt-get update && apt-get install -y lsof python3 python3-pip python3-venv maven openjdk-11-jdk
RUN pip3 install poetry
# Clean up python installations
RUN apt-get clean && rm -rf /var/lib/apt/lists/*
RUN npm install -g pnpm

# Use bash as default shell
SHELL ["/bin/bash", "-c"]