FROM mcr.microsoft.com/dotnet/core/sdk:3.1.302-buster as build-env
LABEL AUTHOR="Paulo Eduardo Correia <https://github.com/correia97/docker-images>"


# Atualiza as ferramentas linux e instala o JAVA
RUN apt-get update && \
    apt-get install -y \
    ca-certificates  default-jdk  \
    apt-transport-https && \
    rm -rf /var/lib/apt/lists/*

# JAVA download, descompacta, move e deleta arquivo gz
RUN curl -O https://download.java.net/java/GA/jdk13/5b8a42f3905b406298b72d750b6919f6/33/GPL/openjdk-13_linux-x64_bin.tar.gz \
&& tar xvf openjdk-13_linux-x64_bin.tar.gz \
&& mv jdk-13 /usr/lib/jvm/ \
&& rm openjdk-13_linux-x64_bin.tar.gz

# Configura o JAVA
RUN update-alternatives --config java
RUN export JAVA_HOME=/usr/lib/jvm/jdk-13
ENV JAVA_HOME /usr/lib/jvm/jdk-13
ENV PATH=${JAVA_HOME}/bin:$PATH 

# Instala o sonnar
RUN dotnet tool install --global dotnet-sonarscanner  --version 4.10.0
# Variavel de path das ferramentas
ENV PATH="${PATH}:/root/.dotnet/tools"

RUN java -version