# CONFIGURACIÓN DE USUARIOS Y ACCESOS PAM / LDAP Linux / Active Directory Windows

# BLOQUE 1 — PAM (PLUGGABLE AUTHENTICATION MODULES)

# 1. Introducción a PAM

PAM (Pluggable Authentication Modules) es el sistema de autenticación modular de Linux. Permite configurar de forma centralizada cómo el sistema gestiona la autenticación, autorización y cambio de contraseñas sin modificar las aplicaciones individualmente.

En esta práctica se configuran los siguientes módulos:

  • pam_pwquality — Política de complejidad de contraseñas
  
  • pam_faillock   — Bloqueo de cuentas tras intentos fallidos
  
  • pam_access     — Restricción de acceso por usuario/grupo
  
  • pam_limits     — Límites de recursos del sistema

  # 2. Inspección de la estructura PAM

  Antes de modificar nada se examina la estructura de ficheros de configuración PAM y el contenido de los ficheros principales.

Comandos ejecutados:

    ls /etc/pam.d/

    cat /etc/pam.d/common-auth

    cat /etc/pam.d/common-password

El directorio /etc/pam.d/ contiene un fichero por cada servicio. En common-auth se observan pam_unix.so (autenticación local) y pam_sss.so (preparado para SSSD/LDAP). En common-password aparece pam_pwquality.so con retry=3.

<img width="858" height="1013" alt="captura 17" src="https://github.com/user-attachments/assets/ac50d6c3-8b39-4db9-8c1b-1574c7264021" />

# 3. Creación de usuarios y grupos

Se crean dos grupos departamentales y tres usuarios de prueba con distintos niveles de acceso.
Creación de grupos:

    sudo groupadd it

    sudo groupadd rrhh

Creación de usuarios:

    sudo useradd -m -s /bin/bash -G it alumno1

    sudo useradd -m -s /bin/bash -G rrhh alumno2

    sudo useradd -m -s /bin/bash alumno3

Verificación:

    cat /etc/passwd | grep alumno

    id alumno1 && id alumno2 && id alumno3

<img width="858" height="1013" alt="captura 18" src="https://github.com/user-attachments/assets/31ac4b9d-65cb-4ace-a5a6-68f5d7eb08fd" />

<img width="858" height="1013" alt="captura 19" src="https://github.com/user-attachments/assets/151b434a-551e-4416-8199-55b89bfb31cf" />

# 4. Política de contraseñas (pam_pwquality)

Se instala y configura pam_pwquality para exigir contraseñas robustas con múltiples tipos de caracteres.
Instalación:

    sudo apt install libpam-pwquality -y

Copias de seguridad:

    sudo cp /etc/pam.d/common-password /etc/pam.d/common-password.bak

    sudo cp /etc/security/pwquality.conf /etc/security/pwquality.conf.bak

Configuración de /etc/security/pwquality.conf:

    sudo nano /etc/security/pwquality.conf

Parámetros aplicados:

minlen = 12      # Longitud mínima 12 caracteres

dcredit = -1     # Al menos 1 dígito

ucredit = -1     # Al menos 1 mayúscula

lcredit = -1     # Al menos 1 minúscula

ocredit = -1     # Al menos 1 carácter especial

maxrepeat = 3    # Máximo 3 caracteres consecutivos iguales

usercheck = 1    # No puede contener el nombre de usuario

retry = 3        # 3 intentos antes de error

<img width="858" height="1013" alt="captura 20" src="https://github.com/user-attachments/assets/27b6f73b-7e72-4841-9d57-6d33da81774c" />

<img width="858" height="1013" alt="captura 21" src="https://github.com/user-attachments/assets/fa9e50f6-1178-46cb-804d-a1ee8afaa0ca" />

Prueba de la política — se introduce primero una contraseña sin mayúsculas y el sistema la rechaza. Luego se acepta Mindverse2025!:

<img width="858" height="1013" alt="captura 22" src="https://github.com/user-attachments/assets/88e91a1c-db25-4320-b131-d5426d712dcf" />

# 5. Bloqueo por intentos fallidos (faillock)

Se configura faillock para bloquear automáticamente una cuenta tras 3 intentos fallidos de autenticación.

Copia de seguridad:

    sudo cp /etc/pam.d/common-auth /etc/pam.d/common-auth.bak

Configuración de /etc/security/faillock.conf:

    sudo nano /etc/security/faillock.conf

Parámetros configurados:

deny = 3            # Bloqueo tras 3 intentos fallidos

unlock_time = 300   # Desbloqueo automático a los 5 minutos

fail_interval = 900 # Ventana de 15 minutos para contar fallos

silent              # No informa al usuario del bloqueo

audit               # Registra en el log del sistema

<img width="858" height="1013" alt="captura 23" src="https://github.com/user-attachments/assets/1d0a1edf-cb3c-488d-9833-6be19aeee6f1" />

Prueba del bloqueo:

su - alumno1              # Contraseña incorrecta x3

sudo faillock --user alumno1

sudo faillock --user alumno1 --reset

<img width="858" height="1013" alt="captura 24" src="https://github.com/user-attachments/assets/bdc0ff84-628e-4d4b-aaa2-bffc260d8399" />

# 6. Restricción de acceso por grupos (pam_access)

Se configura pam_access para controlar qué usuarios pueden iniciar sesión, permitiendo el grupo 'it' y alumno1, y denegando a alumno3.

Copia de seguridad y edición:

    sudo cp /etc/security/access.conf /etc/security/access.conf.bak

    sudo nano /etc/security/access.conf

Reglas añadidas al final del fichero:

+ : root : LOCAL     # root solo desde consola local
+ 
+ : (it) : ALL       # grupo it acceso total
+ 
+ : alumno1 : LOCAL  # alumno1 solo local
+ 
- : alumno3 : ALL    # alumno3 denegado
- 
- : ALL : ALL        # resto denegado

<img width="858" height="1013" alt="captura 25" src="https://github.com/user-attachments/assets/3ab3957e-0dfa-4daa-9449-da6b6d3b7a7b" />

Activación del módulo en common-account y su:

    sudo nano /etc/pam.d/common-account  # Añadir: account required pam_access.so

    sudo nano /etc/pam.d/su              # Añadir: account required pam_access.so

<img width="858" height="1013" alt="captura 26" src="https://github.com/user-attachments/assets/d807c5d7-5efa-4f0a-825d-d26d4776dedf" />

Verificación:

    grep 'pam_access' /etc/pam.d/common-account

    grep 'pam_access' /etc/pam.d/su

    tail -6 /etc/security/access.conf

<img width="858" height="1013" alt="captura 27" src="https://github.com/user-attachments/assets/9a1665f6-f92c-44b1-9bd4-73faeb6cd09a" />

<img width="858" height="1013" alt="captura 28" src="https://github.com/user-attachments/assets/0b4a4154-961c-49a1-ae2b-1557e730b961" />

# 7. Límites de recursos (pam_limits)

Se configura pam_limits para establecer restricciones de recursos por usuario y grupo.

Edición de /etc/security/limits.conf:

    sudo nano /etc/security/limits.conf

Líneas añadidas al final:

alumno1   soft  nproc     50    # Máx 50 procesos (aviso)

alumno1   hard  nproc    100    # Límite duro 100 procesos

@it       soft  nofile  1024    # Grupo it: 1024 ficheros abiertos

@it       hard  nofile  2048    # Grupo it: límite duro 2048

@rrhh     hard  nproc     30    # Grupo rrhh: máx 30 procesos

*         hard  maxlogins  3    # Todos: máx 3 sesiones simultáneas

<img width="858" height="1013" alt="captura 29" src="https://github.com/user-attachments/assets/46ed3cd6-3db0-4e6c-bfb6-dcf050fbbbe7" />

Verificación:

    tail -10 /etc/security/limits.conf

    ulimit -a

    su - alumno1 -c "ulimit -a"

<img width="858" height="1013" alt="captura 30" src="https://github.com/user-attachments/assets/29093393-218d-4fa6-a94a-84c8bb9feece" />

# 8. Verificación final y logs
   
Se revisan los logs de autenticación, el historial de sesiones y el estado de los módulos PAM activos.

Log de autenticación:

    sudo tail -20 /var/log/auth.log

    last

El auth.log muestra toda la actividad: sesiones abiertas/cerradas de alumno1-3, intentos fallidos y operaciones sudo realizadas durante la práctica.

<img width="858" height="1013" alt="captura 31" src="https://github.com/user-attachments/assets/d641eb9b-8d2f-4ca0-b582-187058b3510e" />

Módulos PAM activos:

    sudo pam-auth-update --list

Módulos activos confirmados: Pwquality, Unix authentication, SSS authentication, Register user sessions in systemd, GNOME Keyring Daemon, Inheritable Capabilities Management.

<img width="858" height="1013" alt="captura 32" src="https://github.com/user-attachments/assets/de819652-a290-4648-b441-35d968718504" />

# BLOQUE 2 — OPENLDAP EN UBUNTU LINUX

# 9. Instalación de OpenLDAP (slapd)

Se instala el servidor OpenLDAP (slapd) junto con las utilidades de cliente ldap-utils. Durante la instalación se establece la contraseña del administrador LDAP.

    sudo apt update

    sudo apt install slapd ldap-utils -y

Durante la instalación: contraseña de administrador LDAP → Mindverse2025!

Verificación del servicio:

    sudo systemctl status slapd

<img width="858" height="1013" alt="captura 33" src="https://github.com/user-attachments/assets/fa6726dd-c2c1-49fc-99f0-656f2a92c856" />

<img width="858" height="1013" alt="captura 34" src="https://github.com/user-attachments/assets/6fc91edb-624e-495e-9a3f-368ce2304a98" />

# 10. Configuración del dominio LDAP

Se reconfigura slapd para establecer el dominio mindverse.local y el nombre de la organización MindVerse Solutions.

    sudo dpkg-reconfigure slapd

Respuestas al asistente de configuración:

  • ¿Omitir configuración? → No
  
  • Nombre DNS del dominio → mindverse.local
  
  • Nombre de la organización → MindVerse Solutions
  
  • Contraseña administrador → Mindverse2025!
  
  • ¿Eliminar base de datos al purgar? → No
  
  • ¿Mover base de datos antigua? → Sí
  
Verificación de la base de datos LDAP:

    sudo slapcat | head -20

La salida confirma: dn: dc=mindverse,dc=local, o: MindVerse Solutions, administrador cn=admin,dc=mindverse,dc=local.

<img width="858" height="1013" alt="captura 35" src="https://github.com/user-attachments/assets/a3029a43-5ccb-458e-a830-f5cac12835e2" />

# 11. Creación de unidades organizativas (OUs)
    
Se crea la estructura jerárquica del directorio con dos unidades organizativas: una para usuarios y otra para grupos.

Fichero estructura.ldif:

    nano ~/estructura.ldif

Contenido del fichero:

dn: ou=usuarios,dc=mindverse,dc=local

objectClass: organizationalUnit

ou: usuarios

dn: ou=grupos,dc=mindverse,dc=local

objectClass: organizationalUnit

ou: grupos

Aplicar y verificar:

    ldapadd -x -D "cn=admin,dc=mindverse,dc=local" -W -f ~/estructura.ldif

    ldapsearch -x -LLL -b "dc=mindverse,dc=local" "(objectClass=organizationalUnit)"

<img width="858" height="1013" alt="captura 37" src="https://github.com/user-attachments/assets/de606866-8f71-4a91-b10a-a8b55432f075" />

# 12. Creación de usuarios LDAP
    
Se generan los usuarios del directorio LDAP con sus atributos POSIX para que puedan autenticarse en el sistema Linux.
Generar hash de contraseña:

    HASH=$(slappasswd -s 'Mindverse2025!')

    echo $HASH

Crear fichero usuarios.ldif con el hash:

    cat > ~/usuarios.ldif << EOF

dn: uid=alumno1,ou=usuarios,dc=mindverse,dc=local

objectClass: inetOrgPerson / posixAccount / shadowAccount

uid: alumno1  |  uidNumber: 2001  |  gidNumber: 2001

homeDirectory: /home/alumno1  |  loginShell: /bin/bash

userPassword: {SSHA}...

(ídem para alumno2 con uidNumber: 2002)

EOF

<img width="858" height="1013" alt="captura 38" src="https://github.com/user-attachments/assets/9ff09d6b-3b4d-4286-9c77-272e7d84c326" />

<img width="858" height="1013" alt="captura 39" src="https://github.com/user-attachments/assets/78fda2ca-61a9-42d6-9416-53313cb78d40" />

<img width="858" height="1013" alt="captura 40" src="https://github.com/user-attachments/assets/823a1586-2d00-4412-856b-7682472718db" />

Aplicar y verificar:

    ldapadd -x -D "cn=admin,dc=mindverse,dc=local" -W -f ~/usuarios.ldif

    ldapsearch -x -LLL -b "ou=usuarios,dc=mindverse,dc=local"

<img width="858" height="1013" alt="captura 41" src="https://github.com/user-attachments/assets/b72a5395-7bda-4be5-9e2d-6a8141bc2a66" />

# 13. Instalación y configuración de SSSD

SSSD (System Security Services Daemon) actúa como intermediario entre el sistema Linux y el servidor LDAP, permitiendo que los usuarios del directorio se autentiquen como usuarios locales.

Instalación:

    sudo apt install sssd sssd-ldap ldap-utils -y

Configuración de /etc/sssd/sssd.conf:

    sudo nano /etc/sssd/sssd.conf

Contenido del fichero:

[sssd]

services = nss, pam

config_file_version = 2

domains = mindverse.local

[domain/mindverse.local]

id_provider = ldap

auth_provider = ldap

ldap_uri = ldap://localhost

ldap_search_base = dc=mindverse,dc=local

ldap_default_bind_dn = cn=admin,dc=mindverse,dc=local

ldap_default_authtok = Mindverse2025!

ldap_tls_reqcert = never

enumerate = true

cache_credentials = true

Permisos y reinicio:

    sudo chmod 600 /etc/sssd/sssd.conf

    sudo systemctl restart sssd

    sudo systemctl status sssd

<img width="858" height="1013" alt="captura 43" src="https://github.com/user-attachments/assets/d482c47e-4be2-442e-a6f9-203a22c10f2a" />

<img width="858" height="246" alt="captura 44" src="https://github.com/user-attachments/assets/8160d7d8-aa5f-429e-9a9d-6877c90256bc" />

<img width="858" height="448" alt="captura 45" src="https://github.com/user-attachments/assets/0a35a20b-25c7-4bf6-8b65-48f9c2fcb57d" />

# 14. Verificación de autenticación LDAP
    
Se verifica que el sistema resuelve los usuarios LDAP como si fueran usuarios locales y que la autenticación funciona correctamente.

Resolución de usuarios via SSSD:

getent passwd alumno1

getent passwd alumno2

Prueba de autenticación:

    su - alumno1   # Contraseña: Mindverse2025!

    whoami

    id

    exit

El sistema resuelve correctamente ambos usuarios desde el directorio LDAP. La sesión de alumno1 se abre con éxito, whoami devuelve 'alumno1' y el comando id muestra uid=1001 con sus grupos correctos.

<img width="858" height="266" alt="captura 46" src="https://github.com/user-attachments/assets/0f6052d8-1d54-4f94-aae6-f38a1056639b" />

# BLOQUE 3 — ACTIVE DIRECTORY EN WINDOWS SERVER

# 15. Nota sobre el entorno — Por qué no se ejecutó

Este bloque NO ha podido ejecutarse en la práctica porque el grupo trabaja sobre
un servidor físico real con Ubuntu Linux, sin hipervisor (VirtualBox, VMware, etc.).

A continuación se documenta el procedimiento completo que se seguiría con
acceso a una máquina Windows Server, paso a paso.

# 16. Instalación de Windows Server y AD DS

En un entorno con Windows Server disponible, los pasos serían los siguientes:

Requisitos previos:

  • ISO de Windows Server 2019 o 2022
  
  • Mínimo 4 GB de RAM y 2 vCPUs para la VM
  
  • IP estática configurada (ej: 192.168.1.10/24)
  
  • Nombre del servidor: MINDVERSE-DC
  
Instalación del rol AD DS desde PowerShell:

Install-WindowsFeature -Name AD-Domain-Services -IncludeManagementTools

O desde el Asistente (Server Manager):

  1. Abrir Server Manager → Manage → Add Roles and Features
     
  3. Seleccionar 'Active Directory Domain Services'
     
  5. Instalar y reiniciar si es necesario

# 17. Configuración del dominio AD
    
Una vez instalado el rol AD DS se promueve el servidor a controlador de dominio.

Desde PowerShell:

Import-Module ADDSDeployment

Install-ADDSForest \

  -DomainName 'mindverse.local' \
  
  -DomainNetbiosName 'MINDVERSE' \
  
  -SafeModeAdministratorPassword (ConvertTo-SecureString 'Mindverse2025!' -AsPlainText -Force) \
  
  -InstallDns \
  
  -Force
  
El servidor se reinicia automáticamente. Tras el reinicio el dominio mindverse.local está operativo.

Verificación desde PowerShell:

Get-ADDomain

Get-ADDomainController

# 19. Creación de usuarios y grupos en AD

Se crean las unidades organizativas, grupos y usuarios equivalentes a los del directorio LDAP.

Crear OUs desde PowerShell:

New-ADOrganizationalUnit -Name 'Usuarios' -Path 'DC=mindverse,DC=local'

New-ADOrganizationalUnit -Name 'Grupos' -Path 'DC=mindverse,DC=local'

Crear grupos:

New-ADGroup -Name 'it' -GroupScope Global -Path 'OU=Grupos,DC=mindverse,DC=local'

New-ADGroup -Name 'rrhh' -GroupScope Global -Path 'OU=Grupos,DC=mindverse,DC=local'

Crear usuarios:

New-ADUser -Name 'alumno1' -SamAccountName 'alumno1' \

  -UserPrincipalName 'alumno1@mindverse.local' \
  
  -Path 'OU=Usuarios,DC=mindverse,DC=local' \
  
  -AccountPassword (ConvertTo-SecureString 'Mindverse2025!' -AsPlainText -Force) \
  
  -Enabled $true
  
Add-ADGroupMember -Identity 'it' -Members 'alumno1'

New-ADUser -Name 'alumno2' ... (ídem para alumno2 en grupo rrhh)

Verificación:

Get-ADUser -Filter * -SearchBase 'OU=Usuarios,DC=mindverse,DC=local'

Get-ADGroupMember -Identity 'it'

# 21. Unión de un cliente Linux al dominio AD

Desde una máquina Ubuntu cliente se instalan las herramientas necesarias y se une al dominio mindverse.local.

Instalar dependencias en el cliente Ubuntu:

sudo apt install realmd sssd sssd-tools adcli samba-common-bin -y

Descubrir el dominio:

realm discover mindverse.local

Unirse al dominio:

sudo realm join mindverse.local -U Administrator

Introducir la contraseña del Administrador de AD cuando se solicite.

Verificar la unión:

realm list

id alumno1@mindverse.local

# 22. Verificación de autenticación AD desde Linux

Se comprueba que los usuarios del dominio AD pueden autenticarse en el cliente Ubuntu.

Resolución de usuarios AD:

    getent passwd alumno1@mindverse.local

Autenticación:

    su - alumno1@mindverse.local   # Contraseña: Mindverse2025!
    whoami

    id

    exit

Logs de autenticación:

    sudo tail -20 /var/log/auth.log

En un entorno real con Windows Server, estos comandos devolverían el usuario resuelto desde el controlador de dominio AD y la sesión se abriría correctamente con los grupos del dominio asignados.
