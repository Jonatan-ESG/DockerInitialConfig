# Proyecto ASP.NET Server con Docker

Este proyecto configura y ejecuta un servidor ASP.NET utilizando Docker. Se incluye un `Dockerfile` para construir la imagen `asp_server` y un archivo `docker-compose.yml` para gestionar los contenedores de la aplicación y la base de datos.

## Requisitos Previos

Asegúrate de tener instalados los siguientes programas:

-   [Docker](https://www.docker.com/get-started)
-   [Docker Compose](https://docs.docker.com/compose/install/)

## Instrucciones

### 1. Construir la Imagen Docker

1. Navega al directorio del proyecto donde se encuentra el `Dockerfile`.
2. Ejecuta el siguiente comando para construir la imagen Docker:

    ```sh
    docker build -t asp_server .
    ```

    Este comando construirá la imagen `asp_server` utilizando el `Dockerfile` en el directorio actual.

### 2. Configuración del Docker Compose

Asegúrate de tener un archivo `docker-compose.yml` en el mismo directorio con la siguiente configuración:

```yaml
version: '3.7'
services:
    database:
        container_name: mssql-db
        hostname: mssql-db
        image: mcr.microsoft.com/mssql/server:2022-latest
        ports:
            - '1400:1433'
        environment:
            ACCEPT_EULA: 'Y'
            MSSQL_SA_PASSWORD: 'S2V@Cs2JOWgQ'
        volumes:
            - db:/var/opt/mssql
        networks:
            - application_network
    web:
        container_name: aspnet_webserver
        image: asp_server
        build: .
        ports:
            - '8000:80'
        networks:
            - application_network
volumes:
    db:
networks:
    application_network:
        driver: bridge
```

### 3. Ejecutar los Contenedores con Docker Compose

1. Ejecuta el siguiente comando para iniciar los servicios definidos en `docker-compose.yml`:

    ```sh
    docker-compose up -d
    ```

    Este comando iniciará los contenedores en segundo plano.

### 4. Verificar la Ejecución

1. Abre un navegador web y navega a `http://localhost:8000/weatherforecast` para verificar que el servidor ASP.NET está funcionando correctamente.

### Comandos Útiles

-   **Verificar contenedores en ejecución:**

    ```sh
    docker ps
    ```

-   **Detener y eliminar los contenedores:**

    ```sh
    docker-compose down
    ```

## Estructura del Proyecto

-   `Dockerfile`: Define la imagen Docker para el servidor ASP.NET.
-   `docker-compose.yml`: Define y configura los servicios para Docker Compose.

## Recursos Adicionales

-   [Documentación Oficial de Docker](https://docs.docker.com/)
-   [Documentación de Docker Compose](https://docs.docker.com/compose/)

---

Si tienes alguna pregunta o encuentras algún problema, por favor abre un issue en el repositorio.
