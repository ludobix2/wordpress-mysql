# 📦 Despliegue de WordPress + MySQL en Kubernetes con Namespaces

Este repositorio contiene los archivos YAML necesarios para desplegar una aplicación WordPress con base de datos MySQL en un clúster de Kubernetes, organizados en namespaces separados (`wordpress` y `mysql`), usando volúmenes persistentes locales (`hostPath`) para almacenamiento de datos.

---
## 📁 Estructura del repositorio
```
.
├── manual-pv.yaml                # PersistentVolumes para MySQL y WordPress
├── mysql-deployment.yaml        # Service y Deployment de MySQL
├── mysql-pvc.yaml               # PersistentVolumeClaim de MySQL
├── wordpress-deployment.yaml    # Service (NodePort) y Deployment de WordPress
├── wordpress-pvc.yaml           # PersistentVolumeClaim de WordPress
├── namespaces.yaml              # Namespaces "mysql" y "wordpress"
```
---

## 🚀 Pasos para desplegar

> Requisitos:
> - Clúster Kubernetes operativo (local o en la nube)
> - `kubectl` configurado
> - Acceso de administrador para crear namespaces y volúmenes

### 1. Clonar el repositorio
```
git clone https://github.com/ludobix2/wordpress-mysql.git
cd wordpress-mysql
```
### 2. Crear los namespaces
```
kubectl apply -f namespaces.yaml
```
### 3. Crear los volúmenes persistentes (hostPath)
```
kubectl apply -f manual-pv.yaml
```
### 4. Desplegar la base de datos MySQL
```
kubectl apply -f mysql-pvc.yaml
kubectl apply -f mysql-deployment.yaml
```
Verifica que esté funcionando:

```
kubectl get pods -n mysql
kubectl get svc -n mysql
```
### 5. Desplegar WordPress

```
kubectl apply -f wordpress-pvc.yaml
kubectl apply -f wordpress-deployment.yaml
```

Verifica que esté funcionando:
```
kubectl get pods -n wordpress
kubectl get svc -n wordpress
```

## 🌐 Acceder a la aplicación WordPress

Usa la IP del nodo + el puerto `30080` para acceder desde tu navegador:

```
http://<NODE-IP>:30080
```
## 🧠 Detalles técnicos

- **MySQL y WordPress están separados por namespaces** para mejorar organización y gestión.
- Se usan **volúmenes persistentes locales (`hostPath`)** para mantener los datos incluso si los pods se reinician.
- La comunicación entre WordPress y MySQL se hace mediante DNS interno de Kubernetes:  
  `mysql.mysql.svc.cluster.local`.

## 📌 Autor

**Luis Nahuelan**  
Estudiante de Ingeniería en Infraestructura y Plataformas Tecnologicas - Duoc UC  
Contacto: lu.nahuelan@duocuc.cl

## ✅ Estado del despliegue

✔️ Comprobado: WordPress y MySQL se ejecutan correctamente en namespaces separados y se comunican entre sí.
