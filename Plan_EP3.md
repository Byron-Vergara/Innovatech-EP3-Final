# Plan de Desarrollo EP3: Orquestación con AWS EKS (Kubernetes)

Este plan está diseñado para cumplir con el 100% de la rúbrica utilizando la tecnología recomendada por el profesor: **AWS EKS**.

---

## Fase 1: Infraestructura de Red y Seguridad (¡LISTO! ✅)
*Ya completamos esta fase creando la VPC y los Security Groups en la consola.*
- [x] VPC (`Innovatech-EP3-vpc`) con subredes públicas y privadas, IGW y NAT Gateway.
- [x] Security Groups (`SG-ALB`, `SG-Frontend`, `SG-Backend`, `SG-BaseDatos`).

---

## Fase 2: Repositorios ECR y Preparación de Roles (Lo que sigue)
*Preparar los permisos y dónde guardaremos las imágenes.*
- [ ] **Crear Repositorios Amazon ECR:** Crear repositorios privados para `frontend`, `backend` y `basedatos`.
- [ ] **Crear IAM Role para el Clúster:** Para que AWS permita crear el control plane de EKS.
- [ ] **Crear IAM Role para los Nodos:** Para que los nodos EC2 de Kubernetes puedan descargar imágenes de ECR.

---

## Fase 3: Creación del Clúster EKS y Node Group
*Encender el cerebro de Kubernetes y sus "músculos" (Nodos).*
- [ ] **Crear Clúster EKS:** Usando la VPC creada. (Toma ~15 min).
- [ ] **Crear Managed Node Group:** Kubernetes levantará instancias EC2 automáticas (Workers) para correr los contenedores.
- [ ] **Configurar `kubectl`:** Conectar el CloudShell a tu nuevo clúster para mandarle comandos.

---

## Fase 4: Manifiestos de Kubernetes (La receta mágica)
*Escribir los archivos YAML que le dirán a Kubernetes cómo levantar tu aplicación.*
- [ ] **Despliegue de Base de Datos:** Crear `db-deployment.yaml` y `db-service.yaml` (IP interna).
- [ ] **Despliegue de Backend:** Crear `backend-deployment.yaml` y `backend-service.yaml` (IP interna, conectado a la BD).
- [ ] **Despliegue de Frontend:** Crear `frontend-deployment.yaml` y `frontend-service.yaml` (De tipo `LoadBalancer` para que AWS cree automáticamente la URL pública).
- [ ] **Aplicar manifiestos:** Ejecutar `kubectl apply -f` para desplegar todo.

---

## Fase 5: Autoescalado (HPA - Horizontal Pod Autoscaler)
*Hacer que la arquitectura crezca sola si hay mucho tráfico.*
- [ ] **Instalar Metrics Server:** El plugin de Kubernetes que lee el consumo de CPU.
- [ ] **Configurar HPA:** Crear una regla que diga: *Si el CPU de un pod supera el 70%, crear más pods automáticamente*.

---

## Fase 6: Pipeline CI/CD con GitHub Actions
*Automatizar el despliegue para que ocurra solo al hacer "git push".*
- [ ] **Actualizar Secrets:** Subir las nuevas credenciales de AWS al repositorio de GitHub.
- [ ] **Modificar YAMLs de Actions:** Enseñar a GitHub a conectarse a tu clúster EKS y correr `kubectl set image` para actualizar las versiones sin que la página se caiga (Rolling Update).

---

## Fase 7: Validación y Pruebas
*Preparación para la defensa.*
- [ ] **Prueba de Web Pública:** Acceder a la URL entregada por el servicio de Frontend.
- [ ] **Monitoreo:** Mostrar en vivo el comando `kubectl get pods -w` para ver los contenedores.
- [ ] **Prueba de Autoescalado:** Hacer peticiones masivas y ver cómo nacen nuevos contenedores.
