# Proyecto guiado: Administración de Active Directory Domain Services

## Introducción

Este módulo trata los pasos principales para implementar, configurar y mantener un controlador de dominio.

## Preparación

Se trata de un proyecto guiado, donde se completa una serie de tareas.

Necesitaremos:
- Ordenador Windows/Linux/MAC
- Dos máquinas virtuales Windows Server.

### Configuración

La sección Configuración consta de tres tareas principales:
- Instalar VirtualBox
- Crear una máquina virtual de controlador de dominio de Windows Server
- Crear un servidor miembro del dominio de Windows Server

#### Instalar VirtualBox

1- Descargar VirtualBox

Ve a la página oficial de VirtualBox:
Abre tu navegador y ve a https://www.virtualbox.org/
.

Descargar la versión para Windows:
En la página de inicio, haz clic en el botón de "Downloads" (Descargas) en el menú lateral o directamente en la página principal. Luego, selecciona la opción para descargar VirtualBox for Windows hosts.

Descargar el archivo:
El archivo de instalación para Windows será un archivo .exe. Haz clic en él para iniciar la descarga.

2- Ejecutar el archivo de instalación

Abrir el archivo descargado:
Una vez que la descarga esté completa, abre el archivo .exe que descargaste. Es posible que Windows te pida permiso para permitir que el programa realice cambios en tu dispositivo. Haz clic en "Sí".

Iniciar el asistente de instalación:
Se abrirá el asistente de instalación de VirtualBox. Haz clic en "Next" (Siguiente).

3- Elegir las opciones de instalación

Seleccionar la carpeta de instalación:
El asistente te preguntará en qué carpeta quieres instalar VirtualBox. Si no tienes preferencias, puedes dejar la opción predeterminada. Haz clic en "Next".

Seleccionar los componentes adicionales:
Aquí puedes elegir si deseas crear accesos directos en el escritorio, o habilitar otras opciones como la instalación de herramientas de red. Si no sabes qué elegir, deja las opciones por defecto y haz clic en "Next".

Confirmar la instalación de controladores de red (si se te pregunta):
Si aparece un mensaje sobre el control de acceso a tu red o la instalación de adaptadores de red virtuales, haz clic en "Sí" para continuar.

4- Iniciar la instalación

Instalar VirtualBox:
Haz clic en "Install" (Instalar) para comenzar el proceso. El asistente comenzará a copiar los archivos y a configurar VirtualBox en tu sistema.

5- Finalizar la instalación

Completar la instalación:
Una vez que la instalación haya terminado, se te mostrará una pantalla que dice "Completar". Si deseas iniciar VirtualBox de inmediato, marca la opción "Start Oracle VM VirtualBox" y haz clic en "Finish".

6- Abrir VirtualBox

Ejecutar VirtualBox:
Si no lo hiciste en el paso anterior, puedes abrir VirtualBox desde el menú de inicio o desde el acceso directo en tu escritorio.

7- Listo para usar

Ahora ya puedes empezar a usar VirtualBox para crear y administrar máquinas virtuales en tu PC con Windows. Para crear una nueva máquina virtual, simplemente haz clic en el botón "Nuevo" y sigue el asistente para configurarla.

#### Crear una máquina virtual de controlador de dominio de Windows Server

En esta tarea, implementará y configurará un controlador de dominio de Windows Server para el laboratorio donde realizará tareas relacionadas con la credencial de habilidad aplicada. Para ello, asegúrese de haber descargado el archivo ISO de Windows Server 20XX Evaluation Edition.

1- Crear una nueva máquina virtual

- Abre VirtualBox:
  Haz doble clic sobre el acceso directo de VirtualBox o busca "VirtualBox" en el menú de inicio.

- Iniciar el asistente para crear una nueva máquina virtual:
  Haz clic en "Nuevo" (en la parte superior izquierda de la ventana de VirtualBox).

- Configurar nombre y tipo de sistema operativo:

- Nombre: Escribe un nombre para tu máquina virtual (TAILWIND-DC1)

- Tipo: Asegúrate de que esté seleccionado "Microsoft Windows".

- Versión: Selecciona la versión correspondiente de Windows Server (por ejemplo, Windows 2016 (64-bit) si estás instalando Windows Server 2016).

- Haz clic en "Next" (Siguiente).

2- Asignar memoria RAM

- Asignar memoria (RAM):
  VirtualBox te pedirá que asignes cuánta memoria RAM tendrá tu máquina virtual. Te recomiendo asignar entre 2 GB y 8 GB dependiendo de la cantidad de RAM que tenga tu computadora y el rendimiento que esperes de   la máquina virtual.

- Para una instalación básica, 2 GB es suficiente, pero para un rendimiento más fluido, puedes optar por 4 GB o más.

- Luego, haz clic en "Next" (Siguiente).

3- Crear un disco duro virtual

- Crear un disco duro virtual:
  Selecciona la opción "Crear un disco duro virtual ahora" y haz clic en "Create" (Crear).

- Elegir el tipo de archivo de disco duro:
  Selecciona VDI (VirtualBox Disk Image) y haz clic en "Next".

- Elegir el almacenamiento en disco (dinámico o fijo):

- Dinámicamente asignado: El archivo del disco virtual crecerá según lo necesite, hasta el tamaño máximo que le asignes.

- Tamaño fijo: El archivo del disco tendrá el tamaño fijo desde el principio.

- Te recomiendo elegir "Dinamicamente asignado" para ahorrar espacio en disco. Haz clic en "Next".

- Asignar tamaño del disco duro virtual:
  Elige el tamaño del disco duro. Para una instalación básica de Windows Server, puedes asignar 20 GB o más, dependiendo de lo que necesites. Luego, haz clic en "Create" (Crear).

4- Configurar la máquina virtual

- Seleccionar la máquina virtual recién creada:
  La máquina virtual que acabas de crear debería aparecer en la lista de la izquierda. Haz clic en ella para seleccionarla.

- Configurar los detalles del sistema:

  Haz clic en "Configuración" (ícono de engranaje en la parte superior).

  En el panel de la izquierda, selecciona la opción "Sistema".

  En la pestaña de "Placa base", asegúrate de que la opción de "Floppy" esté desmarcada (solo deben estar marcadas "CD/DVD" y "Disco duro").

5- Agregar la ISO de Windows Server

- Configurar la unidad de CD/DVD:

- En la ventana de configuración, selecciona "Almacenamiento" en el panel de la izquierda.

- En la sección de "Controlador: IDE" o "Controlador: SATA" (dependiendo de la versión de VirtualBox), verás un icono de un disco (vacío) bajo la opción "Vacío".

- Haz clic en ese disco vacío y, en el panel derecho, haz clic en el ícono de disco al lado de "CD/DVD".

- Selecciona "Seleccionar archivo de disco virtual..." y navega hasta la ubicación del archivo ISO de Windows Server que descargaste previamente.

- Confirmar la selección de la ISO:
  Haz clic en "Aceptar" para cerrar la ventana de configuración.
6- Instalar Windows Server

- Seleccione Iniciar . Cuando aparezca el mensaje "Presione cualquier tecla para arrancar desde el CD o DVD", utilice el ratón para seleccionar dentro de la ventana de la máquina virtual y pulse la barra espaciadora. Esto configura la máquina virtual para que arranque desde el archivo ISO adjunto.

- En la página de configuración del sistema operativo del servidor de Microsoft, acepte los valores predeterminados y haga clic en Siguiente.

- En la página Instalar ahora, seleccione Instalar ahora .

- En la página de configuración del sistema operativo de Microsoft Server, seleccione Windows Server 20XX Standard Evaluation (Experiencia de Escritorio) y haga clic en Siguiente.

<img width="1029" height="852" alt="image" src="https://github.com/user-attachments/assets/e1ce9c25-5f15-4ed5-8a05-c2eeaa5fc69d" />

- En la página Avisos aplicables y términos de licencia, revise la licencia y marque la casilla "Acepto".  Haga clic en Siguiente.

- En la página ¿Qué tipo de instalación desea?, seleccione Personalizada.

- En la página "¿Dónde desea instalar el sistema operativo?", seleccione la Unidad 0 y haga clic en Siguiente. El sistema operativo se instalará. Esto tarda varios minutos, dependiendo de la velocidad del equipo. La máquina virtual se reiniciará.

- En la página "Personalizar configuración", se le solicitará una contraseña para la cuenta de administrador integrada. Ingresa la contraseña que desees. Esta contraseña es de demostración y no debe usarse en sistemas de producción. Después de ingresar la contraseña de administrador dos veces, haga clic en " Finalizar".

- En la pantalla de bloqueo de la máquina virtual, ingresa la contraseña para iniciar sesión.

- Después de iniciar sesión, haga clic derecho en el ícono de red, representado por un globo terráqueo en la barra de tareas, y seleccione Abrir configuración de red e Internet.

- En la página Estado de la red, seleccione Cambiar opciones del adaptador.

- En la página Conexiones de red, haga clic con el botón derecho en Ethernet y seleccione Propiedades.

- En la página Propiedades de Ethernet, seleccione el elemento Protocolo de Internet versión 4 (TCP/IPv4) y haga clic en Propiedades.

- En la pestaña General de la página Propiedades del Protocolo de Internet versión 4 (TCP/IPv4), establezca la configuración de la dirección IP de la siguiente manera y haga clic en Aceptar :

Utilice la siguiente dirección IP:

Dirección IP: 10.10.10.10
Máscara de subred: 255.255.255.0
Puerta de enlace predeterminada: 10.10.10.1

Utilice las siguientes direcciones de servidor DNS:

Servidor DNS preferido: 1.1.1.1
Servidor DNS alternativo: 8.8.8.8

- Haga clic en Cerrar . Cuando se le pregunte si desea permitir que el equipo sea detectable, seleccione Sí.

- En el menú Inicio, abra el Administrador del servidor, seleccione Servidor local y, a continuación, Nombre del equipo. Esto abrirá el cuadro de diálogo Propiedades del sistema. En la página Nombre del equipo del cuadro de diálogo Propiedades del sistema, seleccione Cambiar .

- En el cuadro de diálogo Cambios de nombre de computadora/dominio, configure el nombre de la computadora en TAILWIND-DC1 y luego haga clic en Aceptar.

<img width="326" height="232" alt="image" src="https://github.com/user-attachments/assets/66cdb970-93ec-4b29-a579-66fa8d2bc709" />

- En el cuadro de diálogo que le informa que necesita reiniciar el equipo, haga clic en Aceptar.

- En el cuadro de diálogo Propiedades del sistema, seleccione Cerrar .

- En el cuadro de diálogo " Debe reiniciar el equipo para aplicar estos cambios" , haga clic en "Reiniciar ahora" . El equipo se reiniciará.

- Cuando la computadora se haya reiniciado, inicie sesión como Administrador con la contraseña que configuró durante la instalación.

- En el Administrador del servidor, seleccione el menú Administrar y luego seleccione Agregar roles y características.

- En la página Antes de comenzar del asistente Agregar roles y características, seleccione Siguiente.

- En la página Seleccionar tipo de instalación, seleccione Instalación basada en roles o en características y haga clic en Siguiente.

- En la página Seleccionar servidor de destino, haga clic en Seleccionar un servidor del grupo de servidores , asegúrese de que TAILWIND-DC1 esté seleccionado y haga clic en Siguiente.
  
- En la página Seleccionar roles de servidor, marque la casilla Servicios de dominio de Active Directory . Esto abrirá la página Agregar funciones. Seleccione Agregar funciones.

- En la página Seleccionar roles de servidor, haga clic en Siguiente.

- En la página Seleccionar características, haga clic en Siguiente.

- En la página Servicios de dominio de Active Directory, haga clic en Siguiente.

- En la página Confirmar selecciones de instalación, seleccione Instalar.

- Al finalizar la instalación, haga clic en Cerrar.

- En el menú del Administrador del servidor, seleccione el ícono de notificación junto a la bandera en la esquina superior derecha.

- En el menú que se abre al seleccionar el icono de notificación, seleccione Promocionar este servidor a controlador de dominio. Esto iniciará el Asistente de configuración de Servicios de dominio de Active Directory.

- En la página Configuración de implementación, seleccione Agregar un nuevo bosque y configure el nombre de dominio raíz como tailwindtraders.internal. Haga clic en Siguiente.

<img width="957" height="1076" alt="image" src="https://github.com/user-attachments/assets/7f6877e6-9410-41b6-83aa-293eb15ebea5" />

- En la página de opciones del controlador de dominio, acepte la configuración predeterminada y proporcione la contraseña del modo de restauración de servicios de directorio (DSRM). Para ello, introduzca la contraseña dos veces. Haga clic en Siguiente.
  
- En la página Opciones de DNS, haga clic en Siguiente.

- En la página Opciones adicionales, haga clic en Siguiente.

- En la página Rutas, haga clic en Siguiente.

- En la página Opciones de revisión, haga clic en Siguiente.

- En la página de verificación de prerrequisitos, haga clic en Instalar. La máquina virtual se reiniciará.
  
- Cuando la máquina virtual se reinicie, inicie sesión como tailwindtraders\administrator con la contraseña que configuró para la cuenta de administrador predeterminada.

<img width="958" height="1079" alt="image" src="https://github.com/user-attachments/assets/941d6730-f6d8-4735-8a39-20df117034f2" />

#### Crear un servidor miembro del dominio de Windows Server

En esta tarea, se implementa y configura un servidor miembro del dominio de Windows Server 20XX para el laboratorio donde se realizan tareas relacionadas con la credencial de habilidad aplicada. Esta tarea también utiliza el archivo ISO de la edición de evaluación.

- En VirtualBox, selecciona Nuevo y luego seleccione Máquina virtual.
  
- En la página Antes de comenzar del Asistente para nueva máquina virtual, haga clic en Siguiente.
  
- En la página Especificar nombre y ubicación del Asistente para nueva máquina virtual, ingrese el nombre que desee y haga clic en Siguiente.

- En la página Asignar memoria, configure la memoria de inicio en 4096 MB y deje seleccionada la opción " Usar memoria dinámica para esta máquina virtual". Haga clic en Siguiente.

- En la página Configurar red, configure la Conexión en el menú desplegable en NAT y haga clic en Siguiente.
  
- En la página Conectar disco duro virtual, acepte los valores predeterminados y haga clic en Siguiente.
  
- En la página Opciones de instalación, seleccione la opción Instalar un sistema operativo desde un archivo de imagen de arranque y, a continuación, haga clic en Explorar para seleccionar el archivo ISO de Windows Server 20XX Evaluation Edition. Haga clic en Siguiente.

- En la página Resumen, haga clic en Finalizar.

- Haz clic con el botón derecho en el servidor miembro y seleccione Configuración.

- Haga doble clic en el servidor miembro. Se abrirá la ventana Conexión de máquina virtual. Haga clic en Inicio . Cuando aparezca el mensaje "Presione cualquier tecla para arrancar desde el CD o DVD", utilice el ratón para seleccionar dentro de la ventana de la máquina virtual y pulse la barra espaciadora. Esto configura la máquina virtual para que arranque desde el archivo ISO adjunto.

- En la página de configuración del sistema operativo del servidor de Microsoft, acepte los valores predeterminados y haga clic en Siguiente.

- En la página Instalar ahora, haga clic en Instalar ahora.

- En la página de configuración del sistema operativo de Microsoft Server, seleccione Windows Server 20XX Standard Evaluation (Experiencia de Escritorio) y haga clic en Siguiente.

- En la página Avisos aplicables y términos de licencia, revise la licencia y marque la casilla "Acepto". Haga clic en Siguiente.

- En la página ¿Qué tipo de instalación desea?, seleccione Personalizada.

- En la página "¿Dónde desea instalar el sistema operativo?", seleccione la Unidad 0 y haga clic en Siguiente. La máquina virtual se reiniciará.

- En la página "Personalizar configuración", se le solicitará una contraseña para la cuenta de administrador integrada. Ingresa la contraseñaque desees dos veces. Esta contraseña es de prueba y no debe usarse en sistemas de producción. También puede elegir su propia contraseña aquí. Después de ingresar la contraseña de administrador dos veces, seleccione " Finalizar ". No se conectará a la máquina virtual en ejecución.

- En la pantalla de bloqueo de la máquina virtual, ingrese la contraseña de administrador para iniciar sesión.

- Después de iniciar sesión, haga clic derecho en el ícono de red, representado por un globo terráqueo en la barra de tareas, y seleccione Abrir configuración de red e Internet .

- En la página Estado de la red, seleccione Cambiar opciones del adaptador.

- En la página Conexiones de red, haga clic con el botón derecho en Ethernet y seleccione Propiedades.

- En la página Propiedades de Ethernet, seleccione el elemento Protocolo de Internet versión 4 (TCP/IPv4) y haga clic en Propiedades .

- En la pestaña General de la página Propiedades del Protocolo de Internet versión 4 (TCP/IPv4), establezca la configuración de la dirección IP de la siguiente manera y haga clic en Aceptar :

Utilice la siguiente dirección IP:

Dirección IP: 10.10.10.20
Máscara de subred: 255.255.255.0
Puerta de enlace predeterminada: 10.10.10.1

Utilice las siguientes direcciones de servidor DNS:

Servidor DNS preferido: 10.10.10.10
Servidor DNS alternativo: 8.8.8.8

<img width="421" height="436" alt="image" src="https://github.com/user-attachments/assets/f87e3652-3f9d-43fe-842d-d1632f96cafe" />

- Haga clic en Cerrar . Cuando se le pregunte si desea permitir que el equipo sea detectable, seleccione Sí .
- En el menú Inicio, abra el Administrador del servidor, seleccione Servidor local y, a continuación, Nombre del equipo. Esto abrirá el cuadro de diálogo Propiedades del sistema. En la página Nombre del equipo del cuadro de diálogo Propiedades del sistema, seleccione Cambiar .
- En el cuadro de diálogo Cambios de nombre de computadora/dominio, configure el nombre de la computadora en TAILWIND-MBR1 y luego haga clic en Aceptar .
- En el cuadro de diálogo que le informa que necesita reiniciar el equipo, haga clic en Aceptar .
- En el cuadro de diálogo Propiedades del sistema, haga clic en Cerrar .
- En el cuadro de diálogo "Debe reiniciar el equipo para aplicar estos cambios", haga clic en " Reiniciar ahora" . El equipo se reiniciará.
- Cuando la computadora se haya reiniciado, inicie sesión como Administrador con la contraseña que configuró durante la instalación.
- En la consola del Administrador del servidor, seleccione la sección Servidor local. En esta sección, seleccione TAILWIND-MBR1 junto a Nombre del equipo. Esto abrirá el cuadro de diálogo Propiedades del sistema.
- En el cuadro de diálogo Propiedades del sistema, haga clic en Cambiar.

- En el cuadro de diálogo Cambios de nombre de computadora/dominio, seleccione Dominio en Miembro de , ingrese el nombre de dominio TAILWINDTRADERS y haga clic en Aceptar.

- En el cuadro de diálogo Cambios de nombre de equipo/dominio, ingrese el siguiente nombre de usuario y contraseña y haga clic en Aceptar:

Nombre de usuario: TAILWINDTRADERS\Administrador
Contraseña: La que hayas puesto

En unos momentos, aparecerá el cuadro de diálogo "Bienvenido al dominio Tailwintraders". Haga clic en "Aceptar".

- En el cuadro de diálogo Propiedades del sistema, haga clic en Cerrar.

- En el cuadro de diálogo que le solicita que reinicie la computadora, haga clic en Reiniciar ahora.

## Ejercicio: Configurar operaciones del controlador de dominio
## Ejercicio: Configurar operaciones de administración de usuarios
## Ejercicio: Administrar directivas de contraseña
## Ejercicio: Configurar opciones de seguridad
## Evaluación del módulo
## Resumen
