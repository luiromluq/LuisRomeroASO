# Administración de procesos del sistema (Actividades)

## Comunicación de procesos: señales de teclado y valores de retorno

El objetivo de esta actividad es conocer dos formas de comunicarse con los procesos: las señales de teclado Ctrl+C y Ctrl+Z y los valores de retorno.
Para realizar esta tarea se pide poner en marcha un proceso (por ejemplo, el mandato yes ) y enviarle señales mediante la combinación de teclas Ctrl+C y Ctrl+Z . Posteriormente es necesario analizar cuáles han sido estas señales viendo el valor de retorno del proceso.

### Primero arrancamos el proceso yes y lo detenemos con la combinación Ctrl+C :

    # yes
    yyyyyyyyyyyyyyyyyyyyy...
    ^C

### Inmediatamente comprobamos el valor que nos devuelve el proceso al recibir la señal. El valor de retorno se guarda en la variable representada por el símbolo de interrogación ( ?)

    # echo $?
    130

### Si restamos 128 a ese valor de retorno obtenemos el código de esta señal. En este caso la señal 2 corresponde a SIGINT. Hagamos lo mismo con Ctrl+Z :

    # yes
    yyyyyyyyyyyyyyyyyyyyy...
    ^Z
    [ 1 ] + Parado yes
 
    # echo $?
    148

En este caso se trata de la señal 20 (148-128), que corresponde a SIGTSP.

## Comunicación de procesos con el orden kill y captura de señales

El objetivo de esta actividad es practicar el envío de señales con el comando kill y ver cómo se puede capturar una señal para efectuar una acción diferente a la inicialmente programada.

### Para realizar esta tarea se considera el siguiente script Bash, que podemos llamar trap.sh:

    #!/bin/bash
    #trap.sh Script para capturar señales
 
    N = 0
    while true
    don
      ( ( N++ ) )
      if [ $N -gt 200000 ] ; then
      exit 1
      fin
    done
    exit 0

Se pide que este script no pueda ser detenido ni con Ctrl+C ni con Ctrl+Z . Para ello será necesario capturar estas señales de teclado.
Por otra parte, si estas señales son rechazadas… ¿cómo podríamos detener este proceso?

### Debe crear el script y darle permisos de ejecución. Como puede ver, es un simple contador que sólo se detiene al terminar de contar hasta 200.000. Puede comprobar cuánto tiempo tarda en hacer este bucle con el comando time y cambiar el bucle para ajustar el tiempo a sus necesidades:

    # time ./trap.sh
    real 0m11.191s
    user 0m7.496s
    sys 0m3.504s

### Puede comprobar que mientras se ejecuta responde a las señales de teclado. Para capturarlos hay que añadir la siguiente línea de trap :

    #!/bin/bash
    #trap.sh Script para capturar señales
 
    trap "echo proceso $$ ignora la señal" SIGTSTP SIGINT
 
    N = 0
    while true
    don
      ( ( N++ ) )
      if [ $N -gt 200000 ] ; then
      exit 1
      fin
    done
    exit 0

### Para detener este proceso ya nada podemos hacer en esta consola. Debemos abrir otra y enviar una señal al proceso mediante kill o killall 

## Gestión del nivel de ejecución

El objetivo es entender cómo funcionan y se configuran los niveles de ejecución en Linux Debian, así como los archivos y directorios que intervienen.
En Debian, los niveles 2, 3 y 4 suelen ser iguales y el nivel por defecto es el 2.
La tarea consiste en:

- Configurar el nivel 3 para que el servicio Apache no se inicie.
- Establecer el nivel 3 como el nivel predeterminado al arrancar.
- Averiguar cómo se define el nivel por defecto en sistemas con upstart (como Ubuntu).

### Aunque habitualmente es así, primero nos aseguramos que el nivel 2 y el 3 de ejecución son iguales:

    # rm /etc/rc3.d/*
    # cp -d /etc/rc2.d/* /etc/rc3.d

(NOTA: La opción -d copia los enlaces simbólicos como tales.)

### Si queremos que el servicio Apache quede deshabilitado, al entrar en el nivel 3 sólo es necesario cambiar la S inicial por una K en el nombre del enlace simbólico que apunta al script de control de Apache:

    # mv / etc / rc3.d / S18apache2 / etc / rc3.d / K18apache2

### Puede comprobarse fácilmente que en el nivel 2 desde el navegador se obtiene respuesta a localhost y que en cambio en el nivel 3 no:

    # init 2

    # init 3

Para que el nivel 3 sea el nivel por defecto al arrancar, es necesario editar /etc/inittab. La línea donde dice:

    # The default runlevel.
    id: 2 :initdefault:

quedará:

    # The default runlevel.
    id: 3 :initdefault:

### En un sistema basado en upstart tenemos dos opciones. Dado que se garantiza la compatibilidad hacia atrás sólo habría que crear un archivo /etc/inittab que sólo tuviera la línea del initdefault. Sin embargo, esta opción se considera obsoleta. Lo correcto es editar el archivo de configuración /etc/init/rc-sysinit.conf y cambiar la variable DEFAULT_RUNLEVEL:

    # cat /etc/init/rc-sysinit.conf
    # rc-sysinit - System V initialisation compatibility
    # Este task runs the old System V-style system initialisation scripts,
    # and enters the default runlevel when finished.
    ...
    # Default runlevel, este puede ser overriden en el kernel command-line
    # or by faking old /etc/inittab entry
 
    env DEFAULT_RUNLEVEL = 3
    
## Directorio /etc/proc

El objetivo es conocer las características y archivos del directorio virtual /proc.
Las tareas son:

- Averiguar el PID del navegador abierto en el entorno gráfico.
- Consultar la información de ese proceso dentro de /proc.

### Con la orden:

    # ps aux
    
localizo rápidamente el proceso del navegador (PID 2855), en este caso Firefox, y puedo obtener más detalles con:

    # ps -lp 2855
    FS UID PID PPID C PRI NI ADDR SZ WCHAN TTY TIME CMD
    0 S 1000 2855 1 4 80 0 - 81786 - ? 0 :02 / bin / firefox

### Con el número de PID puedo buscar el directorio de información de este proceso en /proc/2855:

    # ls -m /proc/2855
    attr, auxv, cgroup, clear_refs, cmdline, coredump_filter,
    cpuset, cwd, environ, exe, fd, fdinfo, yo, limits,
    loginuid, maps, mem, mountinfo, mounts, mountstats, net,
    oom_adj, oom_score, pagemap, personalidad, root, sched,
    sesionid, smaps, stack, stat , statm, status, syscall,
    task, wchan

### Aquí está toda la información del proceso, por ejemplo:

    # cat /proc/2855/status
    Name:firefoxState:S ( sleeping )
    Tgido: 2855
    Pid: 2855
    PPid: 1
    TracerPid: 0
    Uid: 1000100010001000
    Gid: 1000100010001000
    FDSize: 256
    Groups: 4 20 21 24 25 26 29 30 44 46 108 109 110 112 115 1000
    VmPeak: 354792 kB
    VmSize: 328232 kB
    VmLck: 0 kB
    VmHWM: 88960 kB
    VmRSS: 57808 kB
    VmData: 216496 kB
    VmStk: 100 kB
    VmExe: 52 kB
    VmLib: 43940 kB
    VmPTO: 232 kB
    Threads: 18
    SigQ: 0 / 16382
    SigPnd:0000000000000000
    ShdPnd:0000000000000000
    SigBlk:0000000000000000
    SigIgn:0000000000001000
    SigCgt:00000001800044ef
    CapInh:0000000000000000
    CapPrm:0000000000000000
    CapEff:0000000000000000
    CapBnd:ffffffffffffffffff
    Cpus_allowed: 1
    Cpus_allowed_list: 0
    Mems_allowed: 1
    Mems_allowed_list: 0
    voluntary_ctxt_switches: 4196
    nonvoluntary_ctxt_switches: 2034
    
