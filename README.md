# STxxxx <Topicos Especiales en Telematica>
## Estudiantes:
- **Nombre(s):** Juan Jose Villada Calle
                </br>David Elias Franco Velez
                </br>Daniel Velez Duque
                </br>Humberto Antonio Carbonó
- **Email(s):** jjvilladac@eafit.edu.co
                </br>defrancov@eafit.edu.co
                </br>dvelezd2@eafit.edu.co
                </br>hacarbonop@eafit.edu.co
## Profesor:
- **Nombre:** Alvaro Enrique Ospina
- **Email:** aeospinas@eafit.edu.co

---

# Reto de Despliegue de WordPress en Kubernetes con Alta Disponibilidad en AWS

### 1. Descripción breve de la actividad
En esta actividad se desarrolló un entorno de despliegue para un sitio WordPress en un clúster de alta disponibilidad en Kubernetes sobre AWS. Se incluyó un balanceador de carga, almacenamiento persistente compartido para archivos, y una base de datos MySQL. El objetivo fue garantizar la persistencia de los datos y la escalabilidad del sitio en un entorno de producción. El sitio quedó accesible públicamente y funcional, aunque no se logró implementar HTTPS y un dominio personalizado.

#### 1.1 Aspectos cumplidos de la actividad propuesta
- **Configuración de clúster EKS en AWS:** Se desplegó un clúster de Kubernetes en AWS (EKS) con nodos autoescalables.
- **Despliegue de WordPress:** WordPress fue desplegado en el clúster, permitiendo acceder a la aplicación.
- **Base de datos MySQL:** WordPress se configuró para utilizar una base de datos MySQL desplegada en el clúster, con almacenamiento persistente para mantener los datos.
- **Almacenamiento compartido mediante NFS:** Se implementó un servidor NFS para almacenar archivos multimedia, con capacidad de ser accedido por múltiples réplicas de WordPress.
- **Balanceador de carga:** Se configuró un servicio LoadBalancer para exponer WordPress públicamente.

#### 1.2 Aspectos no cumplidos de la actividad propuesta
- **Configuración de DNS personalizado:** No se logró asignar un dominio personalizado a la URL del balanceador de carga.
- **Implementación de SSL con HTTPS:** No se completó la configuración de HTTPS utilizando cert-manager.

---

### 2. Información general de diseño de alto nivel y arquitectura
El diseño de la aplicación sigue una arquitectura de alta disponibilidad en Kubernetes, con los siguientes componentes clave:
- **Aplicación:** WordPress se ejecuta en múltiples réplicas, escalables según la demanda.
- **Base de Datos:** MySQL proporciona almacenamiento para los datos estructurados de WordPress.
- **Almacenamiento compartido:** Un servidor NFS permite que múltiples réplicas de WordPress accedan a archivos multimedia subidos.
- **Balanceador de carga:** AWS asigna un balanceador de carga al servicio de WordPress, distribuyendo el tráfico entre las réplicas.
- **Certificado SSL (No completado):** Se intentó configurar cert-manager para HTTPS, pero no se logró implementarlo con éxito.

---

### 3. Descripción del ambiente de desarrollo y técnico
- **Lenguaje y Herramientas:** Kubernetes, AWS EKS, Helm, cert-manager para certificados SSL, MySQL 5.7, WordPress (latest).
- **Librerías y Paquetes:**
  - `eksctl`: Herramienta de línea de comandos para gestionar el clúster de EKS.
  - `kubectl`: Herramienta de línea de comandos para gestionar el clúster de Kubernetes.
  - `cert-manager`: Herramienta para gestionar certificados SSL (no implementado completamente).

#### Instrucciones para compilación y ejecución
1. **Despliegue de WordPress**
   - **Configuración del clúster en EKS:**
     ```bash
     eksctl create cluster --name wordpress-cluster --region us-east-1 --nodegroup-name standard-workers --node-type t3.medium --nodes 3 --nodes-min 2 --nodes-max 4 --managed
     ```
   - **Despliegue de MySQL y NFS:**
     - Archivos YAML de configuración para despliegue de MySQL y servidor NFS en Kubernetes.
     - Configuración de servicios, volúmenes persistentes y claims.
   - **Despliegue de WordPress:**
     - Archivos YAML de configuración para despliegue de WordPress en Kubernetes, incluyendo configuración de base de datos y almacenamiento NFS.
   - **Balanceador de carga:** El balanceador se asigna automáticamente al crear el servicio LoadBalancer de WordPress.

#### Detalles Técnicos
- **Parámetros importantes del proyecto:**
  - `WORDPRESS_DB_HOST`: Nombre del servicio de MySQL en el clúster.
  - `WORDPRESS_DB_USER`, `WORDPRESS_DB_PASSWORD`, `WORDPRESS_DB_NAME`: Variables de entorno configuradas mediante un secreto de Kubernetes.
  - **Volúmenes y Almacenamiento:**
    - `wordpress-pvc`: PersistentVolumeClaim configurado para montar el almacenamiento NFS en los pods de WordPress.

---

### 4. Descripción del ambiente de ejecución
- **IP o Nombre de Dominio:**
  - URL del balanceador de carga para acceder a WordPress: `http://a7be0ab17d12e4c48a52974e74db45ef-d3e6df34b722b4fe.elb.us-east-1.amazonaws.com`
- **Configuración de parámetros de ejecución:**
  - `WORDPRESS_DB_HOST`: Dirección del servicio MySQL.
  - `wordpress-pvc`: Volumen compartido con NFS para persistencia de archivos.
- **Instrucciones de Ejecución:**
  - Para acceder a WordPress, se usa el navegador y navega a la URL del balanceador de carga.

#### Mini guía para el usuario
1. Accede al sitio de WordPress en la URL pública del balanceador de carga.
2. Inicia sesión en el panel administrativo de WordPress (`/wp-admin`) para gestionar contenido.
3. Puedes subir archivos e imágenes; estos serán almacenados en el sistema de archivos NFS para persistencia.

---

### 5. Información adicional relevante para esta actividad
Esta implementación usa una configuración de alta disponibilidad en AWS EKS, pero no se incluyó HTTPS ni un dominio personalizado. Estos aspectos podrían completarse con acceso a un dominio y configuraciones adicionales en cert-manager.

---
