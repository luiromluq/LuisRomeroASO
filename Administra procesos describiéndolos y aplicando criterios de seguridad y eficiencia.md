# ASO_02: Administra procesos del sistema describiéndolos y aplicando criterios de seguridad y eficiencia

## a: Utilice el comando ps junto con opciones de formato (ps -eo state,pid,cmd) y el comando grep para identificar y contar cuántos procesos del sistema se encuentran en los estados 'dormido' (S) o 'zombie' (Z).

Para identificar estos procesos:

    ps -eo state,pid,cmd | grep -E "^[SZ]"

<img width="951" height="925" alt="image" src="https://github.com/user-attachments/assets/632cf266-a6c0-4560-8c48-2839b7a0d083" />

Para contarlos:

    ps -eo state | grep -E "^[SZ]" | wc -l

<img width="542" height="41" alt="image" src="https://github.com/user-attachments/assets/91551514-be2d-4f6e-b402-41ca1836c0c9" />

(Da como resultado 167)

## b: Examine el sistema de archivos /proc y documente la tabla de interrupciones leyendo el archivo /proc/interrupts. Describa qué tipo de dispositivo está asociado con la interrupción ID 1 (típicamente el teclado/mouse).

Ver interrupciones y encontrar la interrupción 1

    cat /proc/interrupts

<img width="694" height="635" alt="image" src="https://github.com/user-attachments/assets/1715bf8c-383b-4b50-8e9f-b7cc649112ca" />

En la linea que empieza con 1: vemos que pone i8042, que se refiere a teclado/ratón.

## c: Inicie un comando en segundo plano (e.g., sleep 600 &) para crear un "trabajo". Luego, use el comando jobs para listar el trabajo y el comando ps -L para identificar los hilos (LWP) de un proceso multiproceso del sistema, diferenciándolos del PID principal.

Crear un trabajo:

    sleep 600 &
    
<img width="293" height="40" alt="image" src="https://github.com/user-attachments/assets/52923a45-4461-4521-833a-8e86449d464c" />

Listar trabajos:

    jobs
    
<img width="383" height="41" alt="image" src="https://github.com/user-attachments/assets/b199e92a-cd53-4d1f-96d7-f619558b02a7" />

Ver hilos (LWP) de procesos:

    ps -L

<img width="367" height="94" alt="image" src="https://github.com/user-attachments/assets/3dfb8d03-4a9e-40f5-80e2-4ef9a86ed004" />

## d: Cree un proceso (e.g., un loop infinito en Bash o sleep 500 &). Manipule su prioridad utilizando el comando renice para reducirla. Finalmente, termine dicho proceso de forma forzosa usando el comando kill con la señal adecuada (por ejemplo, SIGKILL o -9).

Crear proceso:

    sleep 500 &

<img width="296" height="40" alt="image" src="https://github.com/user-attachments/assets/146e9210-a622-4357-abac-8e8c9dbf61b9" />

Cambiar prioridad:

    renice +5 57163

<img width="531" height="39" alt="image" src="https://github.com/user-attachments/assets/6e0908af-5628-47d8-963c-b7af1767de34" />

Terminar proceso:

    kill -9 57163

<img width="312" height="25" alt="image" src="https://github.com/user-attachments/assets/03359918-eb35-4b35-927a-844874695278" />

## e: Determine el PID de su shell actual ($$). Navegue al directorio correspondiente en el sistema de archivos /proc (cd /proc/<PID>) y localice los links simbólicos que apuntan a los archivos abiertos por su shell (/proc/<PID>/fd) y el ejecutable original.

Ver PID de tu shell:

    echo $$

<img width="366" height="57" alt="image" src="https://github.com/user-attachments/assets/ec2275dd-5dd6-4fb9-8161-f05b7656e2c8" />

Entrar al directorio del proceso:

    cd /proc/$$

<img width="295" height="44" alt="image" src="https://github.com/user-attachments/assets/0629da23-da24-456f-a15f-f5ec725b2fe8" />

Ver archivos abiertos:

    ls -l fd

<img width="721" height="133" alt="image" src="https://github.com/user-attachments/assets/206045f9-80aa-4505-b179-ced2d32d0a79" />

Ver ejecutable:

    ls -l exe

<img width="654" height="77" alt="image" src="https://github.com/user-attachments/assets/a164c0af-60b4-404d-a788-f17e117722da" />

## f: Utilice el comando top o htop (si está instalado) para controlar y realizar un seguimiento continuo de los procesos. Identifique el proceso que está consumiendo la mayor cantidad de recursos de CPU y genere un informe filtrado de dicho proceso usando el comando ps.

Listar con top:

    top

<img width="954" height="926" alt="image" src="https://github.com/user-attachments/assets/4e84bf54-02bb-480c-a314-696620746233" />

Identifica el proceso que más CPU gasta.
Para filtrar sus datos:

    ps -fp 4392
    
<img width="629" height="58" alt="image" src="https://github.com/user-attachments/assets/16459398-c792-49dc-bad1-ebfce532cb53" />

## g: Compruebe la jerarquía de los procesos del sistema utilizando el comando pstree. Identifique específicamente el proceso con PID 1 (el proceso raíz del sistema, fundamental en la secuencia de arranque) y verifique cuáles son sus procesos hijos directos.

Ver la jerarquía y localizar PID 1

    pstree -p

<img width="954" height="927" alt="image" src="https://github.com/user-attachments/assets/5c8e41c9-2c0f-4544-91d8-6665cf5552fb" />

Para ver los hijos de PID 1:

    pstree -p 1

<img width="957" height="909" alt="image" src="https://github.com/user-attachments/assets/5427cbc3-cc33-4c5c-b906-0ba1e3f1ed9c" />

## h: Simule la identificación de un proceso no reconocido (por ejemplo, un proceso que corre bajo un usuario sin privilegios que consume mucha CPU). Utilice el comando lsof -i para verificar si dicho proceso sospechoso está estableciendo conexiones de red y tome medidas de seguridad terminándolo si es necesario.

Ver conexiones y procesos:

<img width="955" height="890" alt="image" src="https://github.com/user-attachments/assets/04717379-569d-4a02-895f-4bbd555eccc5" />

Filtrar por un PID:

<img width="952" height="240" alt="image" src="https://github.com/user-attachments/assets/9db0016f-1276-4631-a233-1821e9540014" />

Para terminar el proceso:

<img width="316" height="26" alt="image" src="https://github.com/user-attachments/assets/3705542f-e5b2-4635-8eae-f4d7389a0489" />

## i: Documente la función, el PID y el usuario propietario de dos procesos habituales del sistema (por ejemplo, cron o sshd). Para la documentación, utilice comandos de inspección como systemctl status (si su sistema usa systemd) y ps -fp <PID> para ver el árbol de procesos padre-hijo.

Para ver el estado:

    systemctl status cron

<img width="797" height="219" alt="image" src="https://github.com/user-attachments/assets/dd0fc4fc-4c4b-4904-b10e-dd48cf7a9981" />

Para ver el PID:

    pidof cron
    
<img width="284" height="36" alt="image" src="https://github.com/user-attachments/assets/0d7c99b6-cc5c-4416-b485-50a13ceed32b" />

Para verlo:

    ps -fp $(pidof cron)

o

    ps -fp <PID>
    
<img width="631" height="56" alt="image" src="https://github.com/user-attachments/assets/51c86179-eb66-4bad-b72b-dcdf329e97a5" />

Si queremos ver el arbol:

    pstree -p | grep cron
    
<img width="391" height="39" alt="image" src="https://github.com/user-attachments/assets/2655fe87-33ca-485d-a4ea-f5f8a593edf5" />


