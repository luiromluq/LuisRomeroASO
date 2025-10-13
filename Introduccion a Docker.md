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
incluiremos. Podemos hacer todo con las siguientes línea:s

##### 2.2.3. Paso 3: Instalando Docker Engine CE



#### 2.3. Post instalación
