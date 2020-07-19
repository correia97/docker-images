# Imagem dotnet SDK com java e sonnar scarnner


### Exemplo de utilização com sonarcloud

```bash
# Imagem com SDK para fazer o build da aplicação
FROM correia97/netcoresdksonar:2.1-alpine3.12 as build

# Diretório onde os arquivos serão copiados e onde os comandos serão executados
WORKDIR /app

# Todos os arquivos da pasta corrende para dentro do container
COPY . ./

# Argumento informado durante o build
ARG sonarLogin

# Executa o comando de restore dos pacotes
RUN dotnet restore 

# Start do scanner
RUN dotnet sonarscanner begin /k:"___ProjectKey___" \
                              /o:"__OrganizationName___" /d:sonar.host.url="https://sonarcloud.io" \
                              /d:sonar.login=$sonarLogin \
                              /d:sonar.exclusions=**/*.js,**/*.css,**/obj/**,**/*.dll,**/*.html,**/*.cshtml,*-project.properties

# executa o build do projeto
RUN dotnet build  -c Release

# Realiza a Analise
RUN dotnet sonarscanner end /d:sonar.login=$sonarLogin 

# publica o projeto na pasta out
RUN dotnet publish  src/MyProject.csproj -c Release -o out

# Imgem com Runtime para executar o projeto
FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-alpine3.12

# Diretório onde os arquivos serão copiados e onde os comandos serão executados
WORKDIR /app

# Copia os arquivos publicados do container de build para o container final
COPY --from=build /app/out .


# Define qual o executavel do container
ENTRYPOINT [ "dotnet","MyProject.dll"]

````


Execute o build com o comando

```bash
docker build --build-arg sonarLogin=__SonarToken____  .
```


## Docker Hub
[.Net  core  sdk  sonar](https://hub.docker.com/r/correia97/netcoresdksonar)