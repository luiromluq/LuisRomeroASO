# Documentación final

## 1.1 Acceso Remoto y Servicios Orientados a Sesión

El acceso remoto permite controlar un ordenador desde otro dispositivo, accediendo a sus archivos y programas como si estuvieras allí físicamente.

Tipos de métodos:

### Basados en CLI (línea de comandos):

- SSH: Protocolo seguro para gestionar servidores. Usa cifrado, autenticación y permite transferir archivos (SFTP/SCP).

- Telnet: Permite control remoto, pero sin cifrado. Es inseguro y hoy solo se usa en redes internas.

### Basados en interfaz gráfica:

- VNC: Permite ver y controlar gráficamente un equipo remoto. Envía la pantalla en pequeños rectángulos.

- X11 Forwarding: Redirige la interfaz gráfica de apps remotas hacia el cliente mediante un túnel SSH.

### Basados en cliente local o web:

- Webmin: Panel web para administrar sistemas Linux (servicios, red, firewall, procesos, etc.).

## 1.2 Diferencias entre servicios orientados a sesión vs. no orientados a sesión

### Orientados a sesión:
Requieren una conexión establecida entre los dos equipos, con autenticación inicial y mantenimiento de la sesión.
Ejemplos: SSH, Telnet, VNC.

### No orientados a sesión:
No necesitan establecer conexión previa. Priorizan la velocidad sobre la fiabilidad.
Ejemplos: UDP, ICMP, IP.

## 1.3 Comparativa: Importancia de la seguridad (Telnet vs SSH)

### Seguridad:
- SSH: Muy seguro.
- Telnet: Inseguro.

### Cifrado:
- SSH: Sí, asimétrico y simétrico.
- Telnet: No tiene cifrado.

### Autenticación:
- SSH: Claves, certificados y contraseñas cifradas.
- Telnet: Usuario y contraseña en texto plano.

### Uso actual:
- SSH: Se sigue usando.
- Telnet: Obsoleto.

## 2.1: Instalar y configurar el servidor OpenSSH

Instalación ssh con la orden

    sudo apt install openssh-server

Comprobamos el estado del servicio ssh con la orden

    systemctl status ssh

## 2.2: Configurar sshd_config para seguridad

Entramos al archivo de configruacion de ssh en este caso con sudo nano /etc/ssh/sshd_config, en las versiones recientes de ssh el protocolo es 2 por defecto y no esta presente en el archivo aun así se ha añadido la linea.

## 2.3: Demostrar el uso de SSH con compresión (-C)

Conectarse al usuario mediante ssh con la opción -C

## 3.1 – Creación de cuentas de usuario para acceso remoto

Abrir una terminal local en tu equipo.
Crear un usuario nuevo para acceso remoto:

    sudo adduser usuario_remoto

- Asignar contraseña cuando el sistema lo solicite.
- Le damos un nombre
- Le damos siguiente a todo ya que no queremos introducir esos datos y para finalizar le damos a que la información es correcta.

<img width="2576" height="1731" alt="Image" src="https://github.com/user-attachments/assets/97838ca2-9901-4804-931e-04bfc4a1d0f1" />

Dar permisos básicos de sudo al usuario si es necesario:

    sudo usermod -aG sudo usuario_remoto

Editar la configuración de SSH para permitir el acceso:

    sudo nano /etc/ssh/sshd_config

Añadir o verificar la línea:

    AllowUsers usuario_remoto

Reiniciar el servicio SSH:

    sudo systemctl restart ssh

## 3.2 – Transferencia segura de archivos con SCP o SFTP

Probar la conexión SSH:

    ssh usuario_remoto@192.168.1.172

Transferir un archivo desde tu equipo al servidor usando SCP:

    scp prueba1.txt grupo1@192.168.1.42:/home/grupo1/

Descargar un archivo del servidor:

    scp grupo1@192.168.1.42:/etc/ssh/sshd_config  ./

Alternativa con SFTP:

    sftp grupo1@192.168.1.42
    put archivo.txt
    get /etc/ssh/sshd_config

## 3.3 – Gestión de servicios mediante SSH

Conectarse al servidor mediante SSH.

Iniciar el servicio cups ya que cupsd ya no se usa:

    ssh grupo1@192.168.1.42 "sudo systemctl start cups"

Detener el servicio:

    ssh grupo1@192.168.1.42 "sudo systemctl stop cups"

Consultar el estado del servicio:

    ssh grupo1@192.168.1.42 "systemctl status cups"

## 4.1: Instalar y configurar un servicio de administración remota de GUI

Primero instalamos tigthserver en el servidor con el siguiente comando:

    sudo apt install tightvncserver

Lo ejecutamos, y tendremos que crear una contraseña. Esto nos servirá para darle seguridad

    tightvncserver

En password de solo visualizaciónle damos que no. Y se creará.

Además, por defecto nos ha puesto ":2", que corresponde al puerto TCP 5900 + 2 = 5902.

Entramos y editamos el siguiente archivo en ".vnc/xstartup" y agregamos lo siguiente:

    sudo nano .vnc/xstartup

Si no tenemos instalado xfce4, lo hacemos con el siguiente comando:

    sudo apt install xfce4

## 4.2: Demostrar cómo se tuneliza el tráfico VNC o de otra aplicación insegura a través de un Túnel SSH para garantizar la encriptación.

En el cliente estableceremos una conexión segura con el servidor, haremos un túnel SSH y, luego, le indicaremos al cliente VNC que se conecte a través de ese túnel en lugar de crear una conexión directa.

En la terminal del cliente pondremos lo siguiente:

    ssh -L 5901:localhost:5901 usuario-servidor@IP-Servidor

L: redirección local (forward local -> remoto a través del servidor SSH).
5902: puerto en el cliente donde escuchará el túnel.
host: localhost (el propio servidor).
5902: puerto en el host remoto.
user-server@ip-server: a donde nos conectamos por SSH

## 4.3: Realizar pruebas de acceso desde un sistema heterogéneo (simulado o real) al servidor Debian 13 utilizando el túnel seguro.

Ahora en el cliente con el remmina ( si lo tenemos instalado, y si no, ponemos sudo apt install remmina) probamos desde el visor VNC a conectarnos de manera remota.

Ponemos lo siguiente:

    127.0.0.1:5902

Cuando le demos a ENTER nos pedirá, de nuevo la contraseña de vnc que pusimos en el servidor.

Y cuando le demos a aceptar el cliente se conectará remotamente a servidor.

## Tarea 5.1: Instalar y configurar un servicio de administración remota basado en web (ej. Webmin) y accederlo vía HTTPS.

### 5.1.1. Se ha actualizado el sistema y se ha añadido la clave del repositorio de Webmin:

    sudo apt update
    sudo apt install wget apt-transport-https

    wget -qO - https://download.webmin.com/jcameron-key.asc

    sudo sh -c 'echo "deb https://download.webmin.com/download/repository sarge contrib" >> /etc/apt/sources.list'

    sudo apt install webmin

### 5.1.2. Verificación del servicio.

    sudo systemctl status webmin

### 5.1.3. Acceso remoto vía HTTPS

    https://192.168.1.60:10000

## Tarea 5.2: Utilizar la interfaz web o comandos para gestionar servicios y procesos de forma remota, documentando el proceso.

### 5.2.1 Gestión de servicios

Acciones realizadas:

Comprobación del estado de servicios.

    sudo systemctl status webmin
 
Inicio y parada de servicios del sistema.

    sudo systemctl start servicio
    sudo systemctl stop servicio

Verificación del arranque automático.

    sudo systemctl restart servicio
    sudo systemctl enable servicio

## Procesos de administracion local y herramientas locales 


## 1. apache2
**Comando:**  
```
sudo apt install -y apache2
```  
**Función:** Servidor web para alojar páginas y aplicaciones HTTP/HTTPS.

## 2. ssh / openssh-server
**Comando:**  
```
sudo apt install -y ssh
```  
**Función:** Conexión remota segura mediante SSH.

## 3. ufw
**Comando:**  
```
sudo apt install ufw -y
```  
**Función:** Firewall sencillo para permitir o bloquear puertos.

## 4. software-properties-common
**Comando:**  
```
sudo apt install software-properties-common -y
```  
**Función:** Añadir nuevos repositorios y gestionar claves APT.

## 5. apt-transport-https
**Comando:**  
```
sudo apt install apt-transport-https -y
```  
**Función:** Permite usar repositorios HTTPS en APT.

## 6. webmin
**Comandos:**  
```
sudo apt install webmin -y
sudo apt install ./webmin_2.105_all.deb -y
```  
**Función:** Panel web de administración (usuarios, firewall, servicios, discos, etc.).

## 7. wget
**Comando:**  
```
sudo apt install wget -y
```  
**Función:** Descarga de archivos desde consola.

## 8. curl
**Comando:**  
```
sudo apt install curl -y
```  
**Función:** Descarga de datos desde URLs y pruebas de APIs.

## 9. gnupg2
**Comando:**  
```
sudo apt install gnupg2 -y
```  
**Función:** Gestionar claves GPG para repos y verificaciones.

## 10. perl + módulos necesarios
**Comando:**  
```
sudo apt install perl libnet-ssleay-perl libauthen-pam-perl libpam-runtime libio-pty-perl -y
```  
**Función:** Librerías requeridas para ejecutar Webmin.

## 11. vim y nano
**Comando:**  
```
sudo apt install vim nano -y
```  
**Función:** Editores de texto.

## 12. htop
**Comando:**  
```
sudo apt install htop -y
```  
**Función:** Monitor de procesos.

## 13. net-tools
**Comando:**  
```
sudo apt install net-tools -y
```  
**Función:** Herramientas de red clásicas.

## 14. lsb-release
**Comando:**  
```
sudo apt install lsb-release -y
```  
**Función:** Información del sistema.

## 15. aptitude
**Comando:**  
```
sudo apt install aptitude -y
```  
**Función:** Gestor de paquetes avanzado.

## 16. systemd & systemd-sysv
**Comando:**  
```
sudo apt install systemd systemd-sysv -y
```  
**Función:** Sistema de inicio y servicios.

## 17. passwd, adduser, login
**Comando:**  
```
sudo apt install passwd adduser login -y
```  
**Función:** Gestión de usuarios.

## 18. sysstat
**Comando:**  
```
sudo apt install sysstat -y
```  
**Función:** Monitorización del rendimiento.

## 19. iotop
**Comando:**  
```
sudo apt install iotop -y
```  
**Función:** Monitor de disco.

## 20. iftop
**Comando:**  
```
sudo apt install iftop -y
```  
**Función:** Monitor de tráfico de red.

## 21. iproute2
**Comando:**  
```
sudo apt install iproute2 -y
```  
**Función:** Herramientas modernas de red.

## 22. iputils-ping
**Comando:**  
```
sudo apt install iputils-ping -y
```  
**Función:** Comando ping.

## 23. traceroute
**Comando:**  
```
sudo apt install traceroute -y
```  
**Función:** Rutas de paquetes.

## 24. lvm2
**Comando:**  
```
sudo apt install lvm2 -y
```  
**Función:** Gestión de volúmenes lógicos.

## 25. gparted
**Comando:**  
```
sudo apt install gparted -y
```  
**Función:** Editor gráfico de particiones.

## 26. smartmontools
**Comando:**  
```
sudo apt install smartmontools -y
```  
**Función:** Estado físico de discos.

## 27. parted
**Comando:**  
```
sudo apt install parted -y
```  
**Función:** Particionado de discos desde terminal.

## 28. cron
**Comando:**  
```
sudo apt install cron -y
```  
**Función:** Tareas programadas.

## 29. anacron
**Comando:**  
```
sudo apt install anacron -y
```  
**Función:** Ejecuta tareas aunque el equipo esté apagado.

## 30. rsyslog
**Comando:**  
```
sudo apt install rsyslog -y
```  
**Función:** Registro de logs del sistema.

## 31. logrotate
**Comando:**  
```
sudo apt install logrotate -y
```  
**Función:** Rotación automática de logs.

## 32. nmap
**Comando:**  
```
sudo apt install nmap -y
```  
**Función:** Escáner de puertos y análisis de red.


### 5.2.2. Gestión de Procesos

Acciones realizadas:

- Visualización de procesos en ejecución.

- Comprobación de carga del sistema.

## Tarea 5.3: Finalizar la documentación consolidada (RA4.i) de todos los procesos y servicios administrados remotamente, incluyendo los archivos de configuración clave (sshd_config).

### 5.3.1. Administración remota mediante SSH

Se revisó y configuró el archivo de configuración SSH para permitir acceso seguro en la ubicación:

    /etc/ssh/sshd_config

### 5.3.2. Parámetros del archivo sshd_config

Lineas descomentadas:

    Port 22
    PermitRootLogin no
    PasswordAuthentication yes

Y tras las modificaciones, ejecutamos: 

    sudo systemctl restart sshd
    sudo systemctl status sshd

### 5.3.3. Conclusión de la administración remota

El sistema queda completamente administrable por:

- SSH seguro, mediante el servicio OpenSSH.

- Webmin, proporcionando administración web mediante HTTPS.

Ambos servicios funcionan correctamente y permiten la gestión remota del servidor, procesos y servicios del sistema.
