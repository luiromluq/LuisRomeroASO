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

    cat /proc/interrupts

<img width="694" height="635" alt="image" src="https://github.com/user-attachments/assets/1715bf8c-383b-4b50-8e9f-b7cc649112ca" />

## c: Inicie un comando en segundo plano (e.g., sleep 600 &) para crear un "trabajo". Luego, use el comando jobs para listar el trabajo y el comando ps -L para identificar los hilos (LWP) de un proceso multiproceso del sistema, diferenciándolos del PID principal.

    sleep 600 &
    
<img width="293" height="40" alt="image" src="https://github.com/user-attachments/assets/52923a45-4461-4521-833a-8e86449d464c" />

    jobs
    
<img width="383" height="41" alt="image" src="https://github.com/user-attachments/assets/b199e92a-cd53-4d1f-96d7-f619558b02a7" />

    ps -L

<img width="367" height="94" alt="image" src="https://github.com/user-attachments/assets/3dfb8d03-4a9e-40f5-80e2-4ef9a86ed004" />

## d: Cree un proceso (e.g., un loop infinito en Bash o sleep 500 &). Manipule su prioridad utilizando el comando renice para reducirla. Finalmente, termine dicho proceso de forma forzosa usando el comando kill con la señal adecuada (por ejemplo, SIGKILL o -9).

    sleep 500 &

<img width="296" height="40" alt="image" src="https://github.com/user-attachments/assets/146e9210-a622-4357-abac-8e8c9dbf61b9" />

    renice +5 57163

<img width="531" height="39" alt="image" src="https://github.com/user-attachments/assets/6e0908af-5628-47d8-963c-b7af1767de34" />
    
    kill -9 57163

<img width="312" height="25" alt="image" src="https://github.com/user-attachments/assets/03359918-eb35-4b35-927a-844874695278" />

## e: Determine el PID de su shell actual ($$). Navegue al directorio correspondiente en el sistema de archivos /proc (cd /proc/<PID>) y localice los links simbólicos que apuntan a los archivos abiertos por su shell (/proc/<PID>/fd) y el ejecutable original.

    echo $$

<img width="366" height="57" alt="image" src="https://github.com/user-attachments/assets/ec2275dd-5dd6-4fb9-8161-f05b7656e2c8" />

    cd /proc/$$

<img width="295" height="44" alt="image" src="https://github.com/user-attachments/assets/0629da23-da24-456f-a15f-f5ec725b2fe8" />
    
    ls -l fd

<img width="721" height="133" alt="image" src="https://github.com/user-attachments/assets/206045f9-80aa-4505-b179-ced2d32d0a79" />

    ls -l exe

<img width="654" height="77" alt="image" src="https://github.com/user-attachments/assets/a164c0af-60b4-404d-a788-f17e117722da" />

## f: Utilice el comando top o htop (si está instalado) para controlar y realizar un seguimiento continuo de los procesos. Identifique el proceso que está consumiendo la mayor cantidad de recursos de CPU y genere un informe filtrado de dicho proceso usando el comando ps.

    top

<img width="954" height="926" alt="image" src="https://github.com/user-attachments/assets/4e84bf54-02bb-480c-a314-696620746233" />

Identifica el proceso que más CPU gasta.

    ps -fp 4392
    
<img width="629" height="58" alt="image" src="https://github.com/user-attachments/assets/16459398-c792-49dc-bad1-ebfce532cb53" />

## g: Compruebe la jerarquía de los procesos del sistema utilizando el comando pstree. Identifique específicamente el proceso con PID 1 (el proceso raíz del sistema, fundamental en la secuencia de arranque) y verifique cuáles son sus procesos hijos directos.

    pstree -p

<img width="937" height="848" alt="image" src="https://github.com/user-attachments/assets/83ae166b-269a-4896-9eec-e30561ab5ce9" />

    pstree -p 1

<img width="937" height="848" alt="image" src="https://github.com/user-attachments/assets/28956a94-82c3-457b-ae85-ec692db9a46d" />

## h: Simule la identificación de un proceso no reconocido (por ejemplo, un proceso que corre bajo un usuario sin privilegios que consume mucha CPU). Utilice el comando lsof -i para verificar si dicho proceso sospechoso está estableciendo conexiones de red y tome medidas de seguridad terminándolo si es necesario.

## i: Documente la función, el PID y el usuario propietario de dos procesos habituales del sistema (por ejemplo, cron o sshd). Para la documentación, utilice comandos de inspección como systemctl status (si su sistema usa systemd) y ps -fp <PID> para ver el árbol de procesos padre-hijo.
