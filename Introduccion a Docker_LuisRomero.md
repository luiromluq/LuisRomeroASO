# Introducción a Docker

## UD.1 Introducción a los contenedores y a Docker

### 1. Introducción

En esta unidad realizaremos una introducción al concepto de contenedores. Nos centraremos en
contenedores Linux y en concreto en la tecnología de Docker.

### 2. Conceptos previos

#### 2.1. Virtualización

La virtualización combina hardware y software para crear la ilusión de manejar recursos virtuales como si fueran reales.
Se usa en despliegue de sistemas, desarrollo, análisis de malware o escalado horizontal, ya que es fácil de implementar y reduce costes de energía y mantenimiento.

#### 2.2. ¿Qué es una máquina virtual?

Una máquina virtual (VM) permite ejecutar un sistema operativo dentro de otro, simulando una máquina real.
Se usa para probar sistemas o software sin necesitar un equipo físico.
Existen varios tipos: máquinas virtuales de proceso, emuladores, hipervisores y contenedores (donde se ubica Docker).

#### 2.3. ¿Qué es una máquina virtual de proceso?

Permite ejecutar programas diseñados para una arquitectura diferente, como un proceso más en el sistema actual.
Ejemplos:

- JVM (Java Virtual Machine): ejecuta bytecodes Java en cualquier sistema.
- .NET: similar, en el entorno Microsoft.

#### 2.4. ¿Qué es un emulador?

Un emulador reproduce un hardware o API específicos, como los emuladores de consolas antiguas o Wine, que permite ejecutar programas de Windows en otros sistemas.

#### 2.5. ¿Qué es un hipervisor?

Un hipervisor simula el hardware completo de una máquina, permitiendo instalar distintos sistemas operativos (como Windows sobre Linux).
Ejemplos: VirtualBox, VMWare, emuladores de consolas.

### 3. Contenedores

#### 3.1. ¿Qué son los contenedores?

Son una forma de virtualización a nivel de sistema operativo (OS-level virtualization).
A diferencia del hipervisor, no emulan hardware, sino que comparten el sistema del anfitrión en entornos privados aislados (procesos, memoria, red, etc.).
Esto implica que solo funcionan nativamente en el mismo tipo de sistema operativo.

#### 3.2. Analogía con contenedores de transporte marítimo

Al igual que los contenedores de transporte siguen un estándar para poder moverse entre medios, los contenedores informáticos cumplen un estándar que les permite ejecutarse en cualquier máquina compatible, independientemente del contenido del software.

#### 3.3. Contenedores para desarrollo y despliegue de aplicaciones

Simplifican el desarrollo, distribución y despliegue de software:

- Evitan problemas de compatibilidad.
- Permiten tener entornos de compilación y prueba listos.
- Facilitan el uso de CI/CD (Integración y entrega continuas).

#### 3.4. Contenedores para despliegue de servicios

Usados para desplegar servidores (web, correo, BD, DNS...).
Permiten replicar configuraciones locales en la nube sin errores del tipo “en mi máquina funcionaba”.
Además, facilitan el escalado horizontal con orquestadores.

#### 3.5. Ventajas e inconvenientes del uso de contenedores

Ventajas:

- Ocupan menos espacio (usan el SO anfitrión).
- Son más rápidos que las máquinas virtuales.
- Cuentan con amplio soporte (Microsoft, Nginx, MySQL, etc.).

Inconvenientes:

- Menor rendimiento que el hardware real.
- La gestión de datos persistentes es más compleja.
- Generalmente se manejan por línea de comandos.

#### 3.6. En resumen, ¿Cuándo es adecuado usar contenedores?

Casos recomendados:

- Usuarios: probar servicios localmente (p. ej. desde Docker Hub).
- Desarrolladores: crear y distribuir apps sin problemas de configuración.
- Pruebas y CI/CD: probar distintas configuraciones y recursos.
- Escalado horizontal: ejecutar múltiples instancias en clústeres.

### 4. Contenedores en sistemas Linux

#### 4.1. ¿Es nuevo el concepto de entornos privados en sistemas Unix?

Los contenedores no son nuevos: existen desde chroot (1982) y Jail (FreeBSD, 1999), precursores del aislamiento moderno.

#### 4.2. Sistemas privados modernos en Linux: contenedores

En 2008 surgieron los Linux Containers (LXC), base de los actuales sistemas como LXD y LXCFS.
Se puede obtener más información en  https://linuxcontainers.org/

#### 4.3. ¿Cómo funcionan los contenedores modernos en Linux?

Los contenedores Linux se apoyan en:

- Namespaces: aíslan procesos y recursos (un proceso puede ser “root” dentro del contenedor sin serlo fuera).
- Cgroups: controlan y limitan recursos como memoria o CPU.

#### 4.4. ¿Puedo poner en marcha un contenedor Linux “A mano”?

Es posible crear contenedores sin Docker, usando scripts de Julia Evans (disponibles en GitHub). https://gist.github.com/jvns/ea2e4d572b4e2285148b8e87f70eed73
Sus materiales se pueden consultar en https://wizardzines.com/

#### 4.5. Los contenedores Linux ¿Pueden funcionar en sistemas como Windows o MacOS?

Pueden ejecutarse en otros sistemas, aunque con menor rendimiento.
En Windows, Docker usa WSL2 o Hyper-V; en MacOS, se usa Docker Desktop.
Estas opciones son útiles para aprendizaje o desarrollo cruzado, aunque pierden velocidad.

### 5. Contenedores Docker

#### 5.1. ¿Qué es Docker?

Docker es un sistema de contenedores Linux de código abierto.
Versiones:
- Docker CE (Community Edition): gratuita.
- Docker EE (Enterprise Edition): con soporte y certificaciones.
Compatible con servicios en la nube: AWS, Azure, Google Cloud, Digital Ocean, OVH, etc.

#### 5.2. La arquitectura de Docker

Consta de tres componentes:
- Cliente: se comunica con el servidor.
- Servidor (Host): gestiona imágenes y contenedores.
- Registro (Registry): almacena imágenes (el principal es Docker Hub).

#### 5.3. Docker en sistemas Windows y MacOS

- En Linux, Docker se ejecuta casi a velocidad nativa.
- En Windows, usa WSL2 o Hyper-V para los contenedores Linux, y el núcleo de Windows para los “Windows Server Core”.
- En MacOS, se ejecuta mediante Docker Desktop o Hyperkit.

#### 5.4. Docker corriendo contenedores Windows Server Core y contenedores MacOS

Docker también permite lanzar:
- Contenedores Windows Server Core (solo sobre Windows).
- Contenedores MacOS, usando KVM en Linux con el proyecto Docker-OSX.

### 6. Conclusión

Se han repasado los conceptos básicos sobre virtualización. Tras ello, hemos procedido a introducir el concepto de contenedor y sus características.
Hemos introducido la solución Docker, la cual instalaremos y utilizaremos en futuras unidades.

### 7. Bibliografía

- WizardZines “How containers work” https://wizardzines.com/zines/containers/
- Docker Docs https://docs.docker.com/
- Linux containers https://linuxcontainers.org/
- OS Level virtualization https://en.wikipedia.org/wiki/OS-level_virtualization

## UD 02. Instalación de Docker

### 1. Introducción

En esta unidad se explican los distintos métodos para instalar Docker en diferentes sistemas operativos.
Se dan recomendaciones, pasos de post-instalación y verificaciones para asegurar su correcto funcionamiento.
Aunque Docker puede usarse en Windows y MacOS, se recomienda Linux, ya que su implementación es más estable y con menos problemas.

### 2. Instalación de Docker en sistemas Linux (Ubuntu)

Esta sección trata la instalación de Docker Engine CE (Community Edition) en sistemas basados en Ubuntu (Ubuntu, Kubuntu, Linux Mint, etc.).
Para otras distribuciones (CentOS, Debian, Fedora) el proceso es similar y puede consultarse en la documentación oficial de Docker.

#### 2.1. Instalación desde el repositorio oficial de Ubuntu (No recomendado)

Es posible instalar Docker desde los repositorios de Ubuntu, pero no se recomienda, ya que suelen tener versiones antiguas.
En este curso se instala desde el repositorio oficial de Docker CE.

#### 2.2. Instalación desde el repositorio de Docker-CE (Recomendado)

Se recomienda instalar Docker CE desde su fuente oficial.
Versiones soportadas de Ubuntu (64 bits):
- Ubuntu Kinetic 22.10
- Ubuntu Jammy 22.04 (LTS)
- Ubuntu Focal 20.04 (LTS)
- Ubuntu Bionic 18.04 (LTS)

##### 2.2.1. Paso 1: Eliminando versiones antiguas de Docker Engine

Antes de instalar, se eliminan posibles versiones previas con:

    sudo apt-get remove docker docker-engine docker.io containerd runc

##### 2.2.2. Paso 2: Incluyendo el repositorio de Docker CE

Para poner en marcha el repositorio, lo primero que haremos será actualizar nuestro índice de
paquetes y tras ello, instalar los paquetes necesarios

    sudo apt-get update
    sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

Una vez realizado este paso, descargamos la clave GPG del repositorio de Docker CE y la
incluiremos. Podemos hacer todo con las siguientes líneas

    sudo mkdir -m 0755 -p /etc/apt/keyrings
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o
    /etc/apt/keyrings/docker.gpg

Ahora, solo nos queda añadir el repositorio de Docker CE como fuente para instalación de
paquetes.

    lsb_release -cs

Este comando nos dirá qué distribución tenemos. Por ejemplo, si tenemos “Ubuntu Kinetic 22.10
(LTS)”, este comando imprimirá por pantalla “Kinetic”

En algunas versiones derivadas de Ubuntu, como Linux Mint, aunque la distribución esté basada en
Ubuntu Kinetic, no devolverá el texto “Kinetic”, sino otro diferente. Si estáis en este caso, deberéis
introducir a mano la versión de Ubuntu en que se basa vuestra distribución (sustituyendo el
comando de “lsb_release -cs” de la siguiente línea)

    echo \
    "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]
    https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

O en el caso que tengáis una distribución basada en Ubuntu con el problema comentado
anteriormente, sustituir “$(lsb_release -cs)” a mano por el nombre, de una forma similar a:

    echo \"deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg]
    https://download.docker.com/linux/ubuntu \kinetic stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

##### 2.2.3. Paso 3: Instalando Docker Engine CE

Actualizar el índice de paquetes e instalar:

    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Comprobar la instalación:

    sudo docker version

    
#### 2.3. Post instalación

La documentación oficial sugiere varios pasos de configuración posterior.
Aquí se explican dos: permitir usar Docker sin privilegios y controlar el arranque automático.

##### 2.3.1.

Docker usa sockets Unix, que requieren permisos de root.
Para permitir que usuarios normales lo usen:

    sudo groupadd docker
    sudo usermod -aG docker $USER

Tras esto, el usuario debe cerrar sesión y volver a entrar.
Si aparece un error de permisos en ~/.docker/config.json, puede resolverse con:

    sudo rm -rf ~/.docker/
    sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
    sudo chmod g+rwx "$HOME/.docker" -R

##### 2.3.2. Activar/desactivar arranque al inicio

Activar al inicio:

    sudo systemctl enable docker.service
    sudo systemctl enable containerd.service

Desactivar:

    sudo systemctl disable docker.service
    sudo systemctl disable containerd.service

Iniciar, detener o reiniciar manualmente:

    sudo systemctl start/stop/restart docker.service
    sudo systemctl start/stop/restart containerd.service

#### 2.4. Desinstalando Docker en Ubuntu

Desinstalar el motor de Docker:

    sudo apt-get purge docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

Para eliminar imágenes y contenedores:

    sudo docker system prune -a

O eliminar manualmente:

    sudo rm -rf /var/lib/docker


### 3. Instalación de Docker en sistemas Windows

Existen guías diferentes para:
- Windows 10 Pro / Server: requiere Hyper-V.
- Windows 10 Home: requiere WSL2.

#### 3.1. Pasos previos Windows 10 Pro y Windows Server: activando Hyper-v

Debe habilitarse Hyper-V mediante los siguientes enlaces de Microsoft, donde se explica como se hace:

Guía Windows 10 Pro: https://docs.microsoft.com/es-es/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v

Guía Windows Server: https://learn.microsoft.com/es-es/windows-server/virtualization/hyper-v/get-started/Install-Hyper-V?tabs=powershell&pivots=windows-server

#### 3.2. Pasos previos Windows 10 Home: Instalando WSL2

Pasos resumidos:

1.Habilitar WSL:

    dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

2.Actualizar Windows (mínimo versión 1903, build 18362).
3.Activar “Virtual Machine Platform”:

    dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

4.Instalar el kernel de Linux para WSL2:
5.Establecer WSL2 como versión predeterminada:

    wsl --set-default-version 2

6.Instalar una distribución Linux (ej. Ubuntu) desde Microsoft Store y configurarla al iniciarla por primera vez.

#### 3.3. Instalación de Docker Desktop

Descargar desde: https://docs.docker.com/desktop/setup/install/windows-install/
Seguir los pasos del instalador.
Verificar instalación:

    docker version

#### 3.4. Resolviendo problemas en la instalación de Docker Desktop

Docker Desktop puede presentar errores o fallos al actualizar.
Para solucionarlo:

- Desinstalar Docker Desktop.
- Eliminar la carpeta C:\Users\<usuario>\.docker
- Borrar variables de entorno relacionadas con Docker (DOCKER_TLS_VERIFY, DOCKER_CERT_PATH, DOCKER_HOST).
- Reiniciar el sistema.
- Reinstalar Docker Desktop.

### 4. Instalación de Docker en sistemas MacOS

Para instalar Docker Desktop en MacOS:
- Descargar el paquete .dmg desde Docker Hub: https://docs.docker.com/desktop/setup/install/mac-install/

### 5. Playgrounds de docker

Existen entornos online que permiten practicar Docker sin instalarlo, el más conocido es Play with Docker: https://labs.play-with-docker.com/
Permite hacer pruebas o enseñar Docker de forma segura desde el navegador.

### 6. Conclusión

Se han descrito los procedimientos para instalar Docker en Linux, Windows y MacOS, así como los pasos de post-instalación y verificación.
Se recomienda, siempre que sea posible, usar Docker en Linux para evitar errores y obtener mejor rendimiento.

### 7. Bibliografía

- Docker Docs https://docs.docker.com/

## UD 03. Principales acciones con Docker

### 1. Introducción

Esta unidad explica las acciones básicas de Docker, como crear, gestionar e inspeccionar contenedores.
Al finalizar, se espera tener un manejo práctico y fluido de Docker desde la línea de comandos.

### 2. ¿Gestionaremos Docker mediante interfaz gráfica?

Aunque existen herramientas gráficas para gestionar Docker, en este curso se trabajará exclusivamente por línea de comandos.
Esto se debe a que el uso de GUI puede ocultar conceptos fundamentales del funcionamiento interno de Docker.

### 3. Imágenes y contenedores

#### 3.1. ¿Qué es una imagen y un contenedor?

Imagen:
- Es una plantilla de solo lectura usada para crear contenedores.
- Contiene un sistema de archivos predefinido y parámetros configurables (comandos, variables, etc.).
- Las imágenes pueden basarse en otras imágenes, creando capas sucesivas.
- Crear una nueva imagen equivale a añadir una capa sobre otra existente.

Contenedor:
- Es una instancia ejecutable de una imagen.
- Puede arrancarse, detenerse y ejecutarse múltiples veces.
- Tiene un ID único de 64 caracteres (normalmente se usan los 12 primeros).
- Los contenedores pueden identificarse por su ID o nombre, siempre que sea único.

Símil: La imagen sería como un DVD de instalación de Linux, y el contenedor sería el sistema operativo instalado a partir de él.

#### 3.2. ¿Dónde se almacenan imágenes, contenedores y datos?

El almacenamiento varía según el sistema operativo y el driver usado.
Con el comando:

    docker info

podemos ver el driver de almacenamiento y el directorio de Docker.
Por ejemplo, con el driver overlay2:
- Las imágenes se guardan en /var/lib/docker/overlay2.
- La configuración de contenedores está en /var/lib/docker/containers.
- Los volúmenes se usan para datos persistentes (se verán más adelante).

### 4. Registro: Docker Hub

Docker Hub es la plataforma de registro oficial de imágenes Docker.
Permite almacenar imágenes públicas o privadas y buscar imágenes listas para usar: https://hub.docker.com/search?q=&type=image

Docker lo usa como registro predeterminado, aunque se pueden usar otros o incluso montar un registro privado. Más información: https://www.digitalocean.com/community/tutorials/how-to-set-up-a-private-docker-registry-on-ubuntu-18-04-es

### 5. Creando y arrancando contenedores con "Docker run" 

#### 5.1. ¿Qué hace el comando “docker run”?

Este comando crea un contenedor a partir de una imagen y lo arranca.
Un error común es pensar que solo inicia contenedores; en realidad, cada ejecución crea uno nuevo.
Documentación: https://docs.docker.com/reference/cli/docker/container/run/

#### 5.2. Creando contenedores sin arrancarlos

Si se desea crear un contenedor sin iniciarlo, se usa:

    docker create

#### 5.3. Repasando caso práctico “Hello World”

En anteriores unidades propusimos un sencillo caso práctico para comprobar que funcionaba
Docker usando el siguiente comando:

    docker run hello-world

##### 5.3.1. Repaso parte 1: obteniendo la imagen

Si la imagen no está localmente, Docker la descarga de Docker Hub.
El comando por defecto busca la versión latest si no se especifica una.
Por ejemplo: hello-world:latest descarga la última versión disponible.

##### 5.3.2. Repaso parte 2: el contenedor se crea y ejecuta un comando

Una vez descargada, Docker crea el contenedor, ejecuta el comando interno (hello) y muestra el mensaje de prueba.
El código del programa se puede consultar en: https://github.com/docker-library/hello-world/blob/master/hello.c

### 6. Listar contenedores disponibles en el sistema con "Docker ps"

Permite ver contenedores en ejecución:

    docker ps

Para listar todos los contenedores (incluso los detenidos):

    docker ps -a

Muestra información como: ID, imagen usada, comando de arranque, estado, puertos y nombre asignado.

### 7. Parando y arrancando contenedores existentes con "docker start/stop/restart"

Arrancar:

    docker start <id/nombre>

Detener:

    docker stop <id/nombre>

Reiniciar:

    docker restart <id/nombre>

La descripción completa de estos comandos la podéis encontrar en:
- https://docs.docker.com/engine/reference/commandline/start/
- https://docs.docker.com/engine/reference/commandline/stop/
- https://docs.docker.com/engine/reference/commandline/restart/

### 8. Inspeccionando contenedores con "docker inspect"

Muestra información detallada de un contenedor (ID completo, red, almacenamiento, etc.):

    docker inspect <id/nombre>

### 9. Ejecutar comandos dentro de un contenedor (docker exec)

Permite ejecutar comandos dentro de un contenedor en ejecución:

    docker exec [opciones] <id/nombre> <comando>

Ejemplos: 

    docker exec -d contenedor touch /tmp/prueba
    docker exec -it contenedor bash
    docker exec -it -e VAR1=1 contenedor bash

El parámetro -it enlaza la entrada y salida de la terminal.

### 10. Copiando ficheros entre anfitrión y contenedores con  "docker cp"

Copia archivos entre el anfitrión y los contenedores (no entre contenedores):

    docker cp idcontainer:/tmp/prueba ./
    docker cp ./miFichero idcontainer:/tmp

### 11. Accediendo a un proceso en ejecución con "docker attach"

Enlaza la terminal del usuario al proceso que corre en un contenedor:

    docker attach <id/nombre>

Ejemplo:

    docker run -d --name=muchotexto busybox sh -c "while true; do $(echo date); sleep 1; done"
    docker attach muchotexto

### 12. Obteniendo información de los logs con "docker log"

Muestra la salida generada por un contenedor:

    docker logs [opciones] <id/nombre>

Ejemplo:

    docker logs -f --until=2s muchotexto

### 13. Renombrando contenedores con "docker rename"

Cambia el nombre de un contenedor:

    docker rename contenedor1 contenedor2

### 14. Principales parámetros del comando "docker run"

Formato general:

    docker run [parámetros] imagen [comando] [argumentos]

#### 14.1. Ejemplo 1: lanzando Ubuntu y accediendo a una terminal

    docker run -it --name=nuestroUbuntu1 ubuntu /bin/bash

- -i: modo interactivo.
- -t: asigna pseudoterminal.
- --name: asigna nombre.
Al salir con exit, el contenedor se detiene.

#### 14.2. Ejemplo 1 EXTRA: accediendo a terminal desde el contenedor parado

    docker start -ai <id/nombre>

- -a: enlaza la salida estándar.
- -i: modo interactivo.

#### 14.3. Ejemplo 2: ejecutando una versión de una imagen y autoeliminando el contenedor

    docker run -it --rm ubuntu:14.04 /bin/bash

- --rm: elimina el contenedor al detenerse.

#### 14.4. Ejemplo 3: lanzando un servidor web en background y asociando sus puertos

    docker run -d -p 1200:80 nginx

- -d: ejecuta en segundo plano.
- -p 1200:80: mapea puerto 1200 del host al 80 del contenedor.
Los puertos solo se definen al crear el contenedor.

#### 14.5. Ejemplo 3 EXTRA: cambiando el “index.html” y consultando logs

Modificar index.html en /usr/share/nginx/html.
Copiar o editar desde host con docker cp o docker exec.
Ver logs:

docker logs -n 10 NOMBRE_CONTENEDOR

#### 14.6 Ejemplo 4: estableciendo variables de entorno

    docker run -it -e MENSAJE=HOLA ubuntu bash
    echo $MENSAJE

-e: define variables de entorno.


### 15. Bibliografia

- Docker Docs: https://docs.docker.com/

## UD 04. Gestión de imágenes en Docker

### 1. Introducción

Se explica cómo gestionar imágenes de contenedores: listarlas, eliminarlas, consultar su historial y crearlas manualmente o mediante docker build con Dockerfile.

### 2. Listando imágenes locales y para su descarga

#### 2.1. Listando imágenes locales

- docker images: lista las imágenes almacenadas.
- docker images [REPOSITORIO:TAG]: filtra imágenes.
- -f permite filtros avanzados (ej. docker images -f=reference="u*:*04").

#### 2.2. Listando imágenes para su descarga

    docker search [nombre] 

muestra imágenes disponibles en Docker Hub.

### 3. Descargando y eliminando imágenes (y contenedores) locales

#### 3.1. Descargando imágenes con “docker pull”

- "docker pull nombre:tag" descarga una imagen desde el registro.

#### 3.2. Observar el historial de una imagen descargada

- "docker history imagen" muestra capas y versiones de una imagen.

#### 3.3. Eliminando imágenes con “docker rmi”

- "docker rmi imagen:tag" elimina imágenes locales.
- "docker rmi $(docker images -q)" elimina todas las que no estén en uso.

#### 3.4. Eliminando contenedores con “docker rm”

- "docker rm ID/NOMBRE" elimina contenedores detenidos.
- "docker stop $(docker ps -a -q)" seguido de "docker rm $(docker ps -a -q)" los elimina todos.

#### 3.5. Eliminando todas las imágenes y contenedores con “docker system prune -a”

- "docker system prune -a" elimina imágenes y contenedores parados de una sola vez.

### 4. Creando nuestras propias imágenes a partir de un contenedor existente

- docker commit -a "autor" -m "comentario" ID_CONTENEDOR usuario/imagen:version convierte un contenedor en imagen.
- docker tag permite añadir etiquetas.
- docker rmi elimina etiquetas no necesarias.

### 5. Exportando/importando imágenes locales a/desde ficheros

- docker save -o archivo.tar imagen guarda una imagen en un fichero.
- docker load -i archivo.tar la restaura en otro sistema.

### 6. Subiendo nuestras propias imágenes a un repositorio (Docker Hub)

#### 6.1. Paso 1: creando repositorio para almacenar la imagen en Docker Hub

En primer lugar, debéis crearos una cuenta en https://hub.docker.com e iniciar sesión. Una vez
iniciada sesión, debéis acceder a “Repositories” y ahí a “Create repository”

Tras ello, podréis quedar un repositorio con vuestra cuenta y elegir si dicho repositorio es público o privado

Una vez creado, si tu usuario es “luis” y la imagen se llama “prueba”, podremos referenciarla en distintos contextos como “luis/prueba”

#### 6.2. Paso 2: almacenando imagen local en repositorio Docker Hub

En primer lugar, deberemos iniciar sesión mediante consola al repositorio mediante el comando

    docker login

Una vez iniciada sesión, debemos hacer un “commit” local de la imagen, siguiendo la estructura
vista en puntos anteriores. Un ejemplo podría ser:

    docker commit -a "Luis" -m "Ubuntu modificado" IDCONTENEDOR luis/prueba

Hecho este “commit” local, debemos subirlo usando “docker push”

    docker push luis/prueba

Una vez hecho eso, si la imagen es pública (o privada con permisos), cualquiera podrá descargarla
y crear contenedores usando “docker pull” o “docker run”.

### 7. Generar automáticamente nuestras propias imágenes mediante Dockerfile

Docker nos permite generar de forma automática nuestras propias imágenes usando “docker build” y los llamados “Dockerfile”

#### 7.1. Editor Visual Studio Code y plugins asociados a Docker

Los ficheros “Dockerfile” pueden crearse con cualquier editor de texto, una recomendación es el editor multiplataforma “Visual Studio Code”

#### 7.2. Creando nuestro primer Dockerfile

Ejemplo básico:

    FROM ubuntu:latest
    RUN apt update && apt install -y nano
    CMD /bin/bash

#### 7.3. Otros comandos importantes de Dockerfile

- EXPOSE: define puertos expuestos.
- ADD / COPY: copia archivos (ADD permite descomprimir).
- ENTRYPOINT: define el comando base del contenedor.
- USER: establece el usuario por defecto.
- WORKDIR: define el directorio de trabajo.
- ENV: variables de entorno.
- ARG, VOLUME, LABEL, HEALTHCHECK: parámetros adicionales útiles.

##### 7.3.1. Comando EXPOSE

La opción EXPOSE nos permite indicar los puertos por defecto expuestos que tendrá un contenedor basado en esta imagen.

##### 7.3.2. Comando ADD/COPY

ADD y COPY son comandos para copiar ficheros de la máquina anfitriona al nuevo contenedor. Se recomienda usar COPY, excepto que queramos descomprimir un “zip”, que ADD permite su descompresión.

##### 7.3.3. Comando ENTRYPOINT

Por defecto, los contenedores Docker están configurados para que ejecuten los comandos que se lancen mediante “/bin/sh -c”. Dicho de otra forma, los comandos que lanzábamos, eran parámetros para “/bin/sh -c”. Podemos cambiar qué comando se usa para esto con ENTRYPOINT.

##### 7.3.4. Comando USER

Por defecto, todos los comandos lanzados en la creación de la imagen se ejecutan con el usuario root (usuario con UID=0). Para poder cambiar esto, podemos usar el comando USER, indicando el nombre de usuario o UID con el que queremos que se ejecute el comando.

##### 7.3.5. Comando WORKDIR

Cada vez que expresamos el comando WORKDIR, estamos cambiando el directorio de la imagen donde ejecutamos los comandos. Si este directorio no existe, se crea.

##### 7.3.6. Comando ENV

El comando ENV nos permite definir variables de entorno por defecto en la imagen.

##### 7.3.7. Otros comandos útiles: ARG, VOLUME, LABEL, HEALTHCHECK

- ARG: permite enviar parámetros al propio “Dockerfile” con la opción “--build-arg” del comando “docker build”.
- VOLUME: permite establecer volúmenes por defecto en la imágen. Hablaremos de los volúmenes más adelante en el curso.
- LABEL: permite establecer metadatos dentro de la imagen mediante etiquetas.
- HEALTHCHECK: permite definir cómo se comprobará si ese contenedor está funcionando correctamente o no.

### 8. Trucos para hacer nuestras imágenes más ligeras

- Usar imágenes base ligeras (ej. Alpine).
- Evitar instalaciones innecesarias.
- Unificar comandos en un solo RUN.
- Limpiar cache (rm -rf /var/lib/apt/lists/*).
- Usar --no-install-recommends en instalaciones.
- Analizar Dockerfile con FromLatest.io.

### 9. Bibliografía

- Docker Docs https://docs.docker.com/
- Visual Studio Code Learn https://code.visualstudio.com/learn
- FROM:latest https://www.fromlatest.io/#/

## UD 05. Cheatsheet Docker

### 1. Gestión de redes

Creamos la red “redtest”

    docker network create redtest

Nos permite ver el listado de redes existentes

    docker network ls

Borramos la red “redtest”

    docker network rm redtest

Conectamos el contenedor que creamos a la red “redtest”.

    docker run -it --network redtest ubuntu /bin/bash

Conectamos un contenedor a una red.

    docker network connect IDRED IDCONTENEDOR

Desconectamos un contenedor de una red

    docker network disconnect IDRED IDCONTENEDOR

### 2. Volúmenes

Creamos un contenedor y asignamos un volumen con “binding mount”

    docker run -d -it --name appcontainer -v /home/sergi/target:/app nginx:latest

Creamos un contenedor y asignamos un volumen Docker llamado “micontenedor”.

    docker run -d -it --name appcontainer -v micontenedor:/app nginx:latest

Permite crear, listar o eliminar volúmenes Docker.

    docker volume create/ls/rm mivolumen

Permite crear un contenedor y asociar un volumen “tmpfs”.

    docker run -d -it --tmpfs /app nginx

Permite realizar una copia de seguridad de un volumen asociado a “contenedor1” y que se monta en “/datos”.
Dicha copia finalmente acabará en “/home/sergi/backup” de la máquina anfitrión.

    docker run --rm --volumes-from contenedor1 -v /home/sergi/backup:/backup ubuntu bash -c "cd/datos && tar cvf /backup/copiaseguridad.tar ."

Permite eliminar todos los lúmenes de tu máquina.

    docker volume rm $(docker volume ls -q)
