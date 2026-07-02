# Innovatech - Arquitectura de Microservicios en AWS EKS

Este repositorio contiene la implementación final del proyecto semestral **Innovatech (EP3)**, enfocado en el despliegue automatizado de una arquitectura de microservicios hacia un entorno Kubernetes de nivel de producción, utilizando Amazon Web Services (AWS) y metodologías DevSecOps.

## Arquitectura del Sistema

El sistema cuenta con una arquitectura desacoplada compuesta por los siguientes contenedores:
*   **Frontend Dashboard:** Aplicación SPA en React, empaquetada con Nginx. Actúa como el punto de entrada principal expuesto mediante un LoadBalancer de AWS.
*   **API Ventas (Backend):** Servicio construido en Java Spring Boot, encargado de procesar la lógica de negocio de ventas y transacciones.
*   **API Despachos (Backend):** Servicio construido en Java Spring Boot, dedicado a la gestión logística y despachos.
*   **Base de Datos:** MySQL 8.0, aislada en la red interna para máxima seguridad.

## Infraestructura Cloud (AWS)

La arquitectura subyacente ha sido diseñada para Alta Disponibilidad (HA):
*   **Amazon EKS (Elastic Kubernetes Service):** Clúster principal orquestando los pods en múltiples nodos EC2 (`t3.medium`).
*   **Amazon ECR (Elastic Container Registry):** Bóvedas privadas donde se alojan las imágenes de los microservicios compilados, protegiendo la propiedad intelectual del código.
*   **VPC & Security Groups:** Red aislada en las zonas `us-east-1a` y `us-east-1b`, asegurando que solo el tráfico de internet HTTP ingrese por el balanceador, y bloqueando el acceso público a las APIs y Base de Datos.

## Dockerización y Contenedores

El proyecto incluye la contenedorización completa desde cero de los microservicios:
*   Se crearon los respectivos `Dockerfile` implementando el patrón **Multi-stage build**, lo que permite compilar el código fuente en un contenedor temporal y trasladar únicamente los binarios limpios al contenedor de producción (reduciendo drásticamente el tamaño y mejorando la seguridad).
*   **Backends (Ventas y Despachos):** Se utilizó `eclipse-temurin:17-jdk-alpine` como imagen base para producción, asegurando compatibilidad moderna con Java 17 y resolviendo los fallos globales de obsolescencia de las imágenes estándar de `openjdk`.
*   **Frontend Dashboard:** Se utilizó un entorno Node para la fase de construcción (build) y `nginx:alpine` para servir la aplicación en producción de manera ultra ligera.

## Integración y Entrega Continua (CI/CD)

El ciclo de desarrollo está 100% automatizado mediante flujos de trabajo en **GitHub Actions**. Ante cualquier actualización en el código base, el pipeline ejecuta:
1.  Construcción de binarios (Maven para Java y NPM para Node).
2.  Empaquetado de las imágenes en contenedores Docker (utilizando bases optimizadas como `eclipse-temurin:17-jdk-alpine`).
3.  Autenticación e ingesta de imágenes en Amazon ECR, protegiendo las llaves AWS mediante GitHub Secrets.
4.  Despliegue automatizado hacia los Nodos de Kubernetes.

## Stack Tecnológico
*   **DevOps / Orquestación:** Docker, Kubernetes (K8s), AWS EKS.
*   **Desarrollo:** Java 17, Spring Boot, React, Node.js, Nginx.
*   **Automatización:** GitHub Actions, Maven.
