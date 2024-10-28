## Docker

### ¿Qué es Docker?

![](./img/docker-logo.svg)

Docker es una plataforma de código abierto que permite empaquetar, distribuir y ejecutar aplicaciones en contenedores. 

### Contenedores e imágenes


![](./img/containerized.webp)

Los contenedores son instancias en ejecución de imágenes de Docker. Las imágenes de Docker son plantillas que contienen el código y las dependencias de una aplicación.

#### Registro de Docker

El registro de Docker es un servicio que permite almacenar y distribuir imágenes de Docker. El registro de Docker público es [Docker Hub](https://hub.docker.com/), que contiene miles de imágenes de Docker creadas por la comunidad.

Tabmién podemos crear nuestro propio registro de Docker utilizando [Docker Registry](https://docs.docker.com/registry/). o utilizar Azure Container Registry por ejemplo.

### Casos de uso

Docker se utiliza en una amplia variedad de casos de uso, incluyendo:

- ** Desarrollo de aplicaciones**: Docker facilita el desarrollo de aplicaciones al permitir a los desarrolladores empaquetar sus aplicaciones y sus dependencias en contenedores.
- ** Replicación de entorno de producción**: Docker permite replicar el entorno de producción en un entorno de desarrollo, lo que facilita la detección y corrección de errores.
- ** Instalación de dependencias**: Docker facilita la instalación de dependencias de una aplicación al empaquetarlas en contenedores.

#### Ejemplo de instalación de software ( rabbitmq )

```  powershell
docker run -d --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:3-management
```

### Contenerizar nuestra aplicación

Para contenerizar una aplicación, es necesario crear un archivo `Dockerfile` que contenga las instrucciones necesarias para construir una imagen de Docker.

Para ello partimos casi siempre de una imagen base, por ejemplo para empaquetar nuestra API ASP.NET Core:

``` Dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyApi/MyApi.csproj", "MyApi/"]
RUN dotnet restore "MyApi/MyApi.csproj"
COPY . .
WORKDIR "/src/MyApi"
RUN dotnet build "MyApi.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyApi.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyApi.dll"]
```

Para crear nuestra imgan de Docker ejecutamos el siguiente comando:

``` powershell
docker build -t myapi .
```

Y para ejecutar nuestra imagen de Docker:

``` powershell
docker run -d -p 8080:80 --name myapi myapi
```

Si queremos subir nuestra imagen a un regitro en Azure 
  
``` powershell
docker tag myapi myregistry.azurecr.io/myapi
docker push myregistry.azurecr.io/myapi
```



### Docker Compose

Docker Compose es una herramienta que permite definir y ejecutar aplicaciones multi-contenedor con Docker.

#### Ejemplo docker-compose wordpress

``` yaml
version: '3.3'

services:
  db:
    image: mysql:5.7
    volumes:
      - db_data:/var/lib/mysql
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: somewordpress
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wordpress
      MYSQL_PASSWORD: wordpress

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8000:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wordpress
      WORDPRESS_DB_PASSWORD: wordpress
volumes:
  db_data: {}
```

Para ejecutar nuestra aplicación con Docker Compose:

``` powershell
docker-compose up -d
```

[Kubernetes](kubernetes.md)
