# Prioridad y monitorización de procesos

El objetivo de esta actividad es entender el funcionamiento del sistema de prioridades en Linux y utilizar las órdenes asociadas a su gestión. También habrá que conocer las herramientas de monitorización ps y top .

Para realizar esta tarea se piden las siguientes actividades:

## Arrancar el editor de texto gedit en el entorno gráfico.

     gedit

## Localizar el PID del proceso y visualizar su prioridad general y la prioridad de usuario nice .

    ps aux

Al usar esta orden, se nos da un listado largo, aunque nos ayuda a localizar el proceso, el cual tiene un PID=10 

    ps l -p 10
    F   UID     PID    PPID PRI  NI    VSZ   RSS WCHAN  STAT TTY        TIME COMMAND
    1     0      10       2   0 -20      0     0 -      I<   ?          0:00 [mm_per

## Bajar al mínimo la prioridad nice de este proceso. Comprobar el resultado y ver cómo se comporta la prioridad general.

Vemos que la prioridad general (PRI) es 0 y la prioridad de usuario nice (NI) está a -20. Bajamos ahora al mínimo la prioridad nice :

    sudo renice 30 -p 10
    10 (process ID) old priority -20, new priority 19

Aunque ponemos un número muy alto, en este caso 30, la prioridad nice queda a 19 (prioridad mínima). Comprobamos ahora la prioridad general:

    ps l -p 10
    F   UID     PID    PPID PRI  NI    VSZ   RSS WCHAN  STAT TTY        TIME COMMAND
    1     0      10       2  39  19      0     0 -      IN   ?          0:00 [mm_per

## Usar la utilidad top para asignar ahora la máxima prioridad al proceso. Conviene que pruebe cómo se pueden ordenar los procesos por diferentes campos.

    top
    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND  
    2117 luis      20   0 4015824 338312 131632 S   9,7   9,5   0:17.21 gnome-s

Vamos ahora a ordenar los procesos por el campo de prioridad nice . Primero accedemos a la pantalla de ordenación pulsando la letra O y seleccionamos el campo nice como campo de ordenación (letra I ):

Ahora ya vemos nuestro proceso con prioridad mínima (número nice más positivo). La prioridad se cambia con la letra r ( renice ), indicando primero el PID del proceso:

    PID to renice [default pid = 10]

Y después el nuevo valor nice :

    Renice PID 10 to value -20

Ahora el proceso ha quedado con la máxima prioridad (valor más negativo de nice ). Reordenamos los procesos por nice , pero de forma inversa (letra N ), y vemos el resultado:
