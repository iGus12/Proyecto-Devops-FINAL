# Proyecto DevOps - Innovatech Chile

## Descripción general

Este repositorio contiene el trabajo realizado para la Evaluación Parcial N°2 de la asignatura Introducción a Herramientas DevOps.

El proyecto está compuesto por un frontend desarrollado con React y dos microservicios backend en Spring Boot: Ventas y Despachos. Para esta entrega se preparó la aplicación para ejecutarse con Docker, levantar los servicios mediante Docker Compose, mantener datos persistentes con volúmenes y automatizar el despliegue en AWS EC2 usando GitHub Actions.

## Estructura del repositorio

- `front_despacho/`: contiene el frontend de la aplicación.
- `back-Ventas_SpringBoot/Springboot-API-REST/`: contiene el backend del servicio de ventas.
- `back-Despachos_SpringBoot/Springboot-API-REST-DESPACHO/`: contiene el backend del servicio de despachos.
- `.github/workflows/`: contiene los workflows de GitHub Actions utilizados para el despliegue.
- `docker-compose.yml`: archivo utilizado para levantar los servicios del proyecto.

## Tecnologías utilizadas

- Docker
- Docker Compose
- GitHub Actions
- Docker Hub
- AWS EC2
- React + Vite
- Nginx
- Spring Boot
- Maven
- MySQL

## Contenedorización

Cada componente del proyecto cuenta con su propio `Dockerfile`.

En el caso del frontend, se utiliza una construcción multi-stage. Primero se compila la aplicación con Node.js y luego se sirve el contenido estático mediante Nginx.

En los backends, también se utiliza una construcción multi-stage. Maven se encarga de compilar el proyecto y luego se ejecuta el archivo `.jar` en una imagen más liviana de Java. Además, los contenedores se ejecutan con un usuario no root, aplicando el principio de mínimo privilegio.

## Ejecución local con Docker Compose

Para levantar el proyecto completo, se debe ejecutar el siguiente comando desde la raíz del repositorio:

```bash
docker compose up -d
```

Para revisar los contenedores activos:

```bash
docker ps
```

Para detener los servicios:

```bash
docker compose down
```

## Persistencia de datos

La base de datos MySQL utiliza un volumen llamado `db-data`, definido en el archivo `docker-compose.yml`.

Este volumen permite que la información almacenada no se pierda cuando el contenedor de MySQL se detiene o se vuelve a crear. Se utilizó un named volume porque es administrado directamente por Docker y facilita mantener la continuidad de los datos dentro del entorno de despliegue.

## Puertos utilizados

- Frontend: puerto 80
- Backend Ventas: puerto 8080
- Backend Despachos: puerto 8081
- MySQL: puerto 3306

## Pipeline CI/CD

El repositorio incluye workflows de GitHub Actions para automatizar el despliegue de los componentes principales del proyecto:

- `deploy-Frontend.yml`
- `deploy-ventas.yml`
- `deploy-despachos.yml`

Cada pipeline se activa al realizar un push sobre la rama `deploy`.

El flujo general del pipeline es el siguiente:

1. Se descarga el código del repositorio.
2. Se inicia sesión en Docker Hub usando GitHub Secrets.
3. Se construye la imagen Docker correspondiente.
4. Se publica la imagen en Docker Hub.
5. Se realiza una conexión SSH hacia la instancia EC2.
6. Se descarga la imagen actualizada en el servidor.
7. Se detiene y elimina el contenedor anterior.
8. Se ejecuta el nuevo contenedor con la versión actualizada.

## Secrets utilizados

Los pipelines utilizan GitHub Secrets para evitar que las credenciales queden escritas directamente en el código.

Algunos de los secrets utilizados son:

- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`
- `EC2_HOST`
- `EC2_HOST_FRONT`
- `EC2_USERNAME`
- `EC2_SSH_KEY`
- `DB_ENDPOINT`

## Despliegue en AWS EC2

El frontend se despliega en una instancia EC2 accesible desde Internet mediante el puerto 80.

Los backends se ejecutan como contenedores Docker y se conectan con la base de datos MySQL. La comunicación entre los servicios se controla mediante puertos, variables de entorno y reglas de seguridad configuradas en AWS.

## Pruebas recomendadas

Para verificar que los contenedores estén activos:

```bash
docker ps
```

Para revisar los logs de los servicios:

```bash
docker logs front-despacho-container
docker logs back-ventas-container
docker logs back-despachos-container
docker logs mysql-db-container
```

Para verificar los volúmenes creados:

```bash
docker volume ls
```

Para comprobar la persistencia de datos:

1. Ingresar o consultar datos desde la aplicación.
2. Reiniciar los contenedores.
3. Verificar que los datos sigan disponibles.

## Justificación DevOps

En este proyecto se aplicaron prácticas DevOps mediante el uso de Docker, Docker Compose, GitHub Actions, control de versiones con GitHub, despliegue en AWS EC2 y persistencia con volúmenes.

Estas herramientas permiten automatizar parte del proceso de despliegue, reducir errores manuales y facilitar futuras actualizaciones del sistema.
