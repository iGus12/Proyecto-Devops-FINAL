# Proyecto Innovatech Chile — Plataforma Orquestada de Ventas y Despachos

Este repositorio contiene la solución DevOps completa para la automatización, empaquetado, integración continua y despliegue en la nube de la plataforma **Innovatech Chile**. La solución abarca desde el entorno de desarrollo local contenerizado hasta la infraestructura elástica, de alta disponibilidad y autoescalable en AWS.

## Integrantes
* **Christian Cabrera**
* **Agustín Varas**
* **Carla Vásquez**
* **Docente:** Brian Daniel Guzmán Martínez
* **Institución:** Duoc UC

---

## Arquitectura de la Solución

El sistema se compone de una arquitectura desacoplada de tres capas y microservicios:

1. **Frontend:** Interfaz SPA desarrollada en React, servida y optimizada mediante un servidor web Nginx.
2. **Backend Ventas:** Microservicio desarrollado en Java (Spring Boot) encargado de la lógica comercial.
3. **Backend Despachos:** Microservicio desarrollado en Java (Spring Boot) encargado del control logístico.
4. **Base de Datos:** Motor relacional MySQL local y Amazon RDS MySQL en producción.

```text
[ Cliente Web ] 
       │
       ▼ (Puerto 80)
[ Application Load Balancer (ALB) ]
       │
       ├─► /               ──► [ svc-frontend ] ──► (ECS Fargate)
       ├─► /api/v1/ventas  ──► [ svc-back-ventas ] ──► [ RDS MySQL ]
       └─► /api/v1/despachos ──► [ svc-back-despachos ] ──► [ RDS MySQL ]
