# Proyecto DevOps - Innovatech Chile

## Descripción general

Este proyecto corresponde a la Evaluación Parcial N°2 de la asignatura Introducción a Herramientas DevOps. La solución implementa una aplicación compuesta por un frontend desarrollado con React y dos microservicios backend desarrollados con Spring Boot: Ventas y Despachos.

El objetivo principal fue contenedorizarlos con Docker, levantar el stack mediante Docker Compose, aplicar persistencia de datos con volúmenes Docker y automatizar el despliegue en AWS EC2 usando GitHub Actions.

## Estructura del repositorio

- `front_despacho/`: frontend desarrollado con React y Vite.
- `back-Ventas_SpringBoot/Springboot-API-REST/`: microservicio backend de ventas.
- `back-Despachos_SpringBoot/Springboot-API-REST-DESPACHO/`: microservicio backend de despachos.
- `.github/workflows/`: pipelines CI/CD para frontend, ventas y despachos.
- `docker-compose.yml`: archivo principal para levantar el stack completo.

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

Cada componente cuenta con su propio Dockerfile.

El frontend utiliza una construcción multi-stage: primero se compila la aplicación con Node.js y luego se sirve el contenido estático mediante Nginx.

Los backends utilizan Dockerfile multi-stage con Maven para compilar el proyecto y Java para ejecutar el archivo `.jar`. Además, se aplica el principio de mínimo privilegio mediante la ejecución con un usuario no root.

## Ejecución local con Docker Compose

Para levantar el proyecto completo:

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

La base de datos MySQL utiliza un named volume llamado `db-data`, definido en el archivo `docker-compose.yml`.

Este volumen permite conservar la información almacenada aunque el contenedor de MySQL sea detenido o creado nuevamente. Se eligió un named volume porque Docker lo administra directamente y facilita la continuidad operativa del sistema en un entorno de despliegue.

## Puertos utilizados

- Frontend: puerto 80
- Backend Ventas: puerto 8080
- Backend Despachos: puerto 8081
- MySQL: puerto 3306

## Pipeline CI/CD

El repositorio incluye workflows de GitHub Actions para automatizar el despliegue de cada componente:

- `deploy-Frontend.yml`
- `deploy-ventas.yml`
- `deploy-despachos.yml`

Cada pipeline se activa al realizar un push sobre la rama `deploy`.

El flujo general del pipeline es:

1. Descargar el código del repositorio.
2. Iniciar sesión en Docker Hub usando GitHub Secrets.
3. Construir la imagen Docker.
4. Publicar la imagen en Docker Hub.
5. Conectarse a la instancia EC2 mediante SSH.
6. Descargar la imagen actualizada.
7. Detener y eliminar el contenedor anterior.
8. Ejecutar el nuevo contenedor.

## Secrets utilizados

Los pipelines utilizan GitHub Secrets para no exponer credenciales directamente en el código.

Ejemplos de secrets utilizados:

- `DOCKER_USERNAME`
- `DOCKER_PASSWORD`
- `EC2_HOST`
- `EC2_HOST_FRONT`
- `EC2_USERNAME`
- `EC2_SSH_KEY`
- `DB_ENDPOINT`

## Despliegue en AWS EC2

El frontend se despliega en una instancia EC2 accesible desde Internet mediante el puerto 80.

Los backends se despliegan como contenedores Docker y se comunican con la base de datos MySQL. La comunicación entre los servicios se controla mediante configuración de puertos, variables de entorno y reglas de seguridad en AWS.

## Pruebas recomendadas

Verificar contenedores:

```bash
docker ps
```

Verificar logs:

```bash
docker logs front-despacho-container
docker logs back-ventas-container
docker logs back-despachos-container
docker logs mysql-db-container
```

Verificar volúmenes:

```bash
docker volume ls
```

Verificar persistencia:

1. Ingresar o consultar datos desde la aplicación.
2. Reiniciar los contenedores.
3. Confirmar que los datos siguen disponibles.

## Justificación DevOps

La solución aplica prácticas DevOps mediante el uso de contenedores Docker, automatización CI/CD con GitHub Actions, control de versiones con GitHub, despliegue en AWS EC2 y persistencia mediante volúmenes. Esto permite mejorar la trazabilidad, reducir errores manuales y facilitar futuras actualizaciones del sistema.
