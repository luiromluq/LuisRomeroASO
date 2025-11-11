# ASO_02: Administra procesos del sistema describiéndolos y aplicando criterios de seguridad y eficiencia

## a: Utilice el comando ps junto con opciones de formato (ps -eo state,pid,cmd) y el comando grep para identificar y contar cuántos procesos del sistema se encuentran en los estados 'dormido' (S) o 'zombie' (Z).

## b: Examine el sistema de archivos /proc y documente la tabla de interrupciones leyendo el archivo /proc/interrupts. Describa qué tipo de dispositivo está asociado con la interrupción ID 1 (típicamente el teclado/mouse).

## c: Inicie un comando en segundo plano (e.g., sleep 600 &) para crear un "trabajo". Luego, use el comando jobs para listar el trabajo y el comando ps -L para identificar los hilos (LWP) de un proceso multiproceso del sistema, diferenciándolos del PID principal.

## d: Cree un proceso (e.g., un loop infinito en Bash o sleep 500 &). Manipule su prioridad utilizando el comando renice para reducirla. Finalmente, termine dicho proceso de forma forzosa usando el comando kill con la señal adecuada (por ejemplo, SIGKILL o -9).

## e: Determine el PID de su shell actual ($$). Navegue al directorio correspondiente en el sistema de archivos /proc (cd /proc/<PID>) y localice los links simbólicos que apuntan a los archivos abiertos por su shell (/proc/<PID>/fd) y el ejecutable original.

## f: Utilice el comando top o htop (si está instalado) para controlar y realizar un seguimiento continuo de los procesos. Identifique el proceso que está consumiendo la mayor cantidad de recursos de CPU y genere un informe filtrado de dicho proceso usando el comando ps.

## g: Compruebe la jerarquía de los procesos del sistema utilizando el comando pstree. Identifique específicamente el proceso con PID 1 (el proceso raíz del sistema, fundamental en la secuencia de arranque) y verifique cuáles son sus procesos hijos directos.

## h: Simule la identificación de un proceso no reconocido (por ejemplo, un proceso que corre bajo un usuario sin privilegios que consume mucha CPU). Utilice el comando lsof -i para verificar si dicho proceso sospechoso está estableciendo conexiones de red y tome medidas de seguridad terminándolo si es necesario.

## i: Documente la función, el PID y el usuario propietario de dos procesos habituales del sistema (por ejemplo, cron o sshd). Para la documentación, utilice comandos de inspección como systemctl status (si su sistema usa systemd) y ps -fp <PID> para ver el árbol de procesos padre-hijo.
