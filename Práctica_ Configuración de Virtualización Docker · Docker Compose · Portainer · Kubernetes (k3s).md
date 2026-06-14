# Práctica: Configuración de Virtualización Docker · Docker Compose · Portainer · Kubernetes (k3s)

# Paso 1 – Verificación de Docker instalado

Comprobación de la versión de Docker Engine (v28.1.1), estado del servicio activo y permisos del usuario. Se verifica que Docker está correctamente instalado y en ejecución.

<img width="994" height="1072" alt="captura 1" src="https://github.com/user-attachments/assets/1921f196-b6ad-40cb-b2be-ce57bfbe3296" />

# Paso 2 – Instalación de Docker Compose v2

Instalación del plugin oficial docker-compose-plugin mediante apt. Se confirma la versión Docker Compose v2.35.1 instalada correctamente. Se actualiza el sistema con apt update antes de la instalación.

<img width="994" height="1072" alt="captura 2" src="https://github.com/user-attachments/assets/efe84c40-8e48-419b-b13e-b9403a799ca3" />

# Paso 3 – Creación de la estructura del proyecto

Creación del directorio /mindverse/proyecto con sudo y acceso al mismo. Creación del fichero docker-compose.yml con nano para definir los servicios web (nginx) y db (mariadb).

<img width="994" height="1072" alt="captura 3" src="https://github.com/user-attachments/assets/c5e55c53-4e1f-44ed-897f-b7a40b5b0181" />

# Paso 4 – Primer despliegue con Docker Compose

Ejecución de 'docker compose up -d'. Descarga de imágenes nginx:alpine y mariadb:10.11. Creación de la red proyecto_default, el volumen proyecto_db_data y arranque de los contenedores proyecto-db-1 y proyecto-web-1.

<img width="994" height="1072" alt="captura 3" src="https://github.com/user-attachments/assets/014faa78-bc65-4030-ab3b-fb376c90ae8c" />

# Paso 5 – Verificación de logs de contenedores

Comprobación de los logs en tiempo real con 'docker compose logs -f'. Se observa el correcto arranque de nginx y la inicialización completa de MariaDB con todos sus procesos internos.

<img width="994" height="1072" alt="captura 3" src="https://github.com/user-attachments/assets/297e914e-7085-4c01-9c4f-a0513400bb55" />

<img width="994" height="1072" alt="captura 4" src="https://github.com/user-attachments/assets/f7c91d4c-f343-4dd8-a12f-c895074eaf8c" />

# Paso 6 – Configuración de permisos de usuario Docker

Adición del usuario al grupo docker con 'usermod -aG docker $USER' y 'newgrp docker'. Actualización de repositorios con apt update para asegurar que los paquetes están al día.

<img width="994" height="1072" alt="captura 2" src="https://github.com/user-attachments/assets/edbe83fc-2ffb-4d84-aff6-8679abfdd7df" />

# Paso 7 – Creación del Dockerfile personalizado

Creación del directorio myapp/src y del Dockerfile con imagen base php:8.2-apache, instalación de extensiones PHP y copia del código fuente. Creación de index.php de prueba con el mensaje 'MindVerse Solutions - OK'.

<img width="994" height="1072" alt="captura 5" src="https://github.com/user-attachments/assets/96b4acc8-32bf-41df-bfa8-61021bcc117a" />

# Paso 8 – Construcción de la imagen propia (docker compose build)

Proceso completo de construcción de la imagen personalizada 'proyecto-app' con docker compose build. Se descargan y procesan todas las capas de la imagen php:8.2-apache y se genera la imagen lista para desplegar.

<img width="994" height="1072" alt="captura 6" src="https://github.com/user-attachments/assets/aae350f9-b151-47b4-9812-a5d422e514cf" />

# Paso 9 – Stack completo con 3 servicios activos

Despliegue final del stack con los tres servicios: proyecto-web-1 (nginx:8080), proyecto-db-1 (mariadb:3306) y proyecto-app-1 (php-apache:8081). Todos en estado Running. Se verifica con docker compose ps.

<img width="994" height="1072" alt="captura 6" src="https://github.com/user-attachments/assets/5b3a9030-ebb6-41cf-8802-bdfda200a44a" />

# Paso 10 – Portainer: instalación y contenedores visibles

Creación del volumen portainer_data y despliegue del contenedor Portainer CE en los puertos 9000/9443 con docker run. Verificación con docker ps mostrando los 4 contenedores activos incluyendo Portainer.

<img width="994" height="1072" alt="captura 6" src="https://github.com/user-attachments/assets/1d021103-f456-4037-9ad0-342dad02a28b" />

# Paso 11 – Portainer: interfaz gráfica (GUI)

Acceso a la interfaz web de Portainer en http://localhost:9000. Vista del listado de contenedores mostrando portainer (running), proyecto-app-1, proyecto-db-1 y proyecto-web-1 activos, junto con los contenedores dockerlamp (exited).

<img width="909" height="1040" alt="captura 8" src="https://github.com/user-attachments/assets/73946051-1e5e-45fa-8ecc-1e5182fb2ef8" />

# Paso 12 – Instalación de Kubernetes (k3s)

Instalación de k3s v1.35.5 en Ubuntu mediante el script oficial 'curl -sfL https://get.k3s.io | sh -'. Descarga del binario, configuración del servicio systemd y habilitación automática al inicio del sistema.

<img width="1004" height="1079" alt="captura 9" src="https://github.com/user-attachments/assets/0a4446e0-26ff-4f91-9228-3c3ba60da314" />

# Paso 13 – Verificación del cluster Kubernetes

Comprobación del estado del servicio k3s (active running) con systemctl y verificación del nodo con 'kubectl get nodes'. El nodo grupo3 aparece en estado Ready con rol control-plane y versión v1.35.5+k3s1.

<img width="987" height="592" alt="captura 10" src="https://github.com/user-attachments/assets/a84dc954-2dc3-4994-b216-ace3ffff7322" />

<img width="987" height="88" alt="captura 11" src="https://github.com/user-attachments/assets/99324019-57de-40cc-afd7-c086a80cb010" />

# Paso 14 – Configuración de kubectl para el usuario

Creación del directorio ~/.kube, copia del fichero de configuración k3s.yaml y ajuste de permisos con chown. Configuración de la variable KUBECONFIG y verificación final con 'kubectl get nodes' sin necesidad de sudo.

<img width="1004" height="395" alt="captura 13" src="https://github.com/user-attachments/assets/2a5b2e06-405c-4ea3-a281-7222c1ce1bf1" />

# Paso 15 – Despliegue de aplicación en Kubernetes

Creación del fichero deployment.yaml con un Deployment de 2 réplicas nginx:alpine y un Service de tipo NodePort en el puerto 30080. Aplicación con 'kubectl apply -f' y verificación de pods y servicios creados correctamente.

<img width="1004" height="1079" alt="captura 14" src="https://github.com/user-attachments/assets/5f181a91-4b84-495b-97f8-37becc386256" />

<img width="983" height="251" alt="captura 15" src="https://github.com/user-attachments/assets/a914380d-881d-47fa-95e6-6e733982ea90" />

# Paso 16 – Verificación final: nginx en Kubernetes (puerto 30080)

Acceso exitoso a http://localhost:30080 confirmando que nginx está corriendo correctamente dentro del cluster Kubernetes k3s. La página 'Welcome to nginx!' confirma el despliegue completo y funcional.

<img width="916" height="1079" alt="captura 16" src="https://github.com/user-attachments/assets/d7def332-89e7-4481-bbba-d63c0c86c593" />
