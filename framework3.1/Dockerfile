FROM mcr.microsoft.com/dotnet/core/sdk:3.1-alpine3.11 as build-env
LABEL AUTHOR="Paulo Eduardo Correia <https://github.com/correia97/docker-images>"

# Atualiza as ferramentas linux e instala openjdk
RUN apk add --update \
    openjdk11  \
    && rm -rf /var/cache/apk/*

# Configura o JAVA
ENV JAVA_HOME=/usr/lib/jvm/java-11-openjdk
ENV PATH=$PATH:$JAVA_HOME/bin 

# Instala o sonnar
RUN dotnet tool install --global dotnet-sonarscanner  --version 4.10.0
# Variavel de path das ferramentas
ENV PATH="${PATH}:/root/.dotnet/tools"