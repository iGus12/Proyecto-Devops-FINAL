# 🚀 Proyecto Innovatech Chile — Plataforma Orquestada de Ventas y Despachos

[cite_start]Este repositorio contiene la solución DevOps completa para la automatización, empaquetado, integración continua y despliegue en la nube de la plataforma **Innovatech Chile**[cite: 1662, 1666, 2881, 2886]. [cite_start]La solución abarca desde el entorno de desarrollo local contenerizado hasta la infraestructura elástica, de alta disponibilidad y autoescalable en AWS[cite: 1670, 1779, 1780, 2966, 3136].

## 👥 Integrantes
* [cite_start]**Agustín Varas** [cite: 1663, 1854]
* [cite_start]**Carla Vásquez** [cite: 1663, 1855]
* [cite_start]**Docente:** Brian Daniel Guzmán Martínez [cite: 1882]
* [cite_start]**Institución:** Duoc UC [cite: 1849]

---

## 📐 Arquitectura de la Solución

[cite_start]El sistema se compone de una arquitectura desacoplada de tres capas y microservicios[cite: 2994]:

1. [cite_start]**Frontend:** Interfaz SPA desarrollada en React, servida y optimizada mediante un servidor web Nginx[cite: 2912].
2. [cite_start]**Backend Ventas:** Microservicio desarrollado en Java (Spring Boot) encargado de la lógica comercial[cite: 2915, 2997].
3. [cite_start]**Backend Despachos:** Microservicio desarrollado en Java (Spring Boot) encargado del control logístico[cite: 2915, 2998].
4. [cite_start]**Base de Datos:** Motor relacional MySQL local [cite: 2920] [cite_start]y Amazon RDS MySQL en producción[cite: 3316].

```text
[ Cliente Web ] 
       │
       ▼ (Puerto 80)
[cite_start][ Application Load Balancer (ALB) ] 
       │
       [cite_start]├─► /               ──► [ svc-frontend ] ──► (ECS Fargate) [cite: 3092, 3312]
       ├─► /api/v1/ventas  ──► [ svc-back-ventas ] ──► [ RDS MySQL ] [cite: 3096, 3308, 3316]
       └─► /api/v1/despachos ──► [ svc-back-despachos ] ──► [ RDS MySQL ] [cite: 3096, 3315, 3316]