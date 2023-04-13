# Tema 1 - Programación multiproceso
## 1. Los procesos y el sistema operativo
### 1.1. Procesos y estados de un proceso
Un proceso es **una instancia de un programa en ejecución al que se le ha asignado determinados recursos del sistema.**

Un proceso, además posee un estado; dichos estados siguen un modelo sobre el que se definen transiciones, que se representan en forma de grafo.

Hay tres estados principales:

- **Ejecutable (estado R-Runnable)**: El proceso se encuentra en disposición de ejecutarse, bien en la cola de ejecución (preparado), o bien en ejecución. Cuando un proceso se inicia, mediante la llamada al sistema **fork**, este pasa a la cola de porcesos preparados para su ejecución, y es el planificador del sistema operativo quien decide qué procesos de esta cola se llevan a ejecución. Cuando un proceso que está en ejecución recibe una interrupción, vuelve a la cola de preparados.
- **Bloqueado o en espera (Waiting)**: El proceso ha sido bloqueado, bien porque ha realizado al sistema de **Entrada/Salida** y que da a la espera (**Estado D**) de la finalización de esta operación, o bien porque requiere de otro tipo de evento, como por ejemplo un temporizador. Cuando este evento se produce, o termina la operación de E/S a la que se estaba esperando, el proceso vuelve a la cola de preparados.
- **Finalizado**: El proceso ha finalizado su ejecución y el sistema operativo libera la memoria y los recursos asignados. Esta finalización puede producirse bien porque el proceso recibe una señal de finalización, pasando a estado **Finalizado (T)**, o bien porque el proceso termina la ejecución de todas sus instrucciones. Cuando el proceso finaliza, puede devolver el estado de finalización al proceso que lo creó (**proceso padre**), mediante la llamada al sistema **exit()**. En este caso, el proceso pasará a estado **Zombie (Z)** hasta que el proceso padre recoja esta salida mediante la llamada al sistema **wait()**. Cuando el padre recoge esta salida, el proceso pasa a estado **Terminado**.

### 1.2. Utilidades en GNU/Linux para la gestión de procesos
#### Orden top
Muestra en tiempo real la actividad del procesador, con una lista de los procesos ordenados según el consumo de CPU, permitiendo manipular estos. Top puede clasificar las diferentes tareas según el uso de CPU,de memoria o de tiempo de ejecución. Las características que muestra se pueden configurar bien mediante órdenes interactivas o mediante ficheros de configuración.

#### Orden ps (Process Status)
Se trata de una de las herramientas más populares en el mundo GNU/Linux, y muestra una instantánea de los procesos actuales. Se trata de un comando muy completo, que admite un gran abanico de opciones, tanto de tipo Unix (precedidas de un guion -) como BSD (sin ningún guion) o GNU (precedidas de dos guiones --).

Una de las formas más habituales de utilizar este comando es con los argumentos estilo Unix **-aux** (o **aux** con sintaxis BSD), que mostrará información más detallada sobre todos los procesos del sistema.

En este punto también es interesante conocer el comando **pstree**, que nos muestra el árbol de procesos del sistema, o lo que es lo mismo, la jerarquía de procesos de este.

### 1.3. Threads
Los threads, procesos ligeros o hilos de ejecución son procesos que forman parte de un mismo proceso y comparten determinado recursos, como el espacio de direcciones de memoria, variables, ficheros, señales, etc.

Mediante el comando **ps** podemos obtener también información sobre los hilos de ejecución de un proceso, por ejemplo, utilizando la sintaxis tipo UNIX **ps -eLf** o la sintaxis BSD **ps axms**. Estos parámetros nos ofreceán nuevos datos como LWP (**LightWeight Pr cess**), con el identificador de thread, o NLWP (**Number of LighWeight Pro esses**), con el número de hilos del proceso.

### 1.4. Demonios y servicios. Systemd
Los demonios (**daemon**) son procesos que se ejecutan en segundo plano, sin terminal asociada ni interfaz gráfica, esperando que se produzca algún evento en el sistema o la recepción de una petición de servicio.

En la mayoría de los sistemas GNU/Linux actuales, los servicios son administrados por el demonio **Systemd**. Cuando se lanza un servicio en el sistema, **Systemd** se encarga de su puesta en marcha y gestión, convirtiéndose en el proceso padre del nuevo servicio.

**Systemd** proporciona el comando **systemctl** para interactuar con los servicios.

## 2. Comunicación entre procesos
### 2.1. Comunicación mediante Entrada/Salida
Todo proceso tiene asociados tres flujos de información o streams:
- **stdin** (con descriptor de fichero 0), que representa la entrada estándar del proceso, por donde este recibe los datos. Esta entrada generalmente está asociada a la entrada de teclado de la terminal.
- **stdout** (con descriptor de fichero 1), que representa la salida estándar del proceso, por donde este muestra su salida. Generalmente esta salida está asociada a la pantalla de la terminal.
- **stderr** (con descriptor de fichero 2), que representa la salida de error del proceso, por donde este muestra los mensajes de error. Generalmente se trata de la pantalla de la terminal.

Vamos a ver diferentes mecanismos que aprovechan esta entrada y salida para realizar la comunicación entre
procesos.

#### Redirecciones
Un mecanismo relativamente simple de comunicación de procesos es mediante redirecciones Este mecanismo consiste en redirigir estos descriptores de ficheros desde o hacia otras fuentes diferentes a la estándar.
- Redirección de la entrada estándar (< y <<)
- Redirección de la salida estándar (> y >>)
- Redirección de la salida de error estándar (2> y 2>>)

#### Tuberías (|)
Las tuberías (pipes) permiten conectar la salida estándar de un proceso con la entrada estándar de otro, estableciendo así una relación de productor-consumidor.
El uso de tuberías sigue la siguiente sintaxis:
```bash
comando_productor | comando_consumidor
```

### 2.2. Comunicación mediante variables de entorno y de la shell
En **bash** podemos utilizar variables locales a la shell y variables de entorno, que definiremos mediante export. Estas variables de entorno pueden ser también utilizadas como mecanismo de comunicación entre procesos.

### 2.3. Comunicación mediante señales
Las señales son mensajes que el sistema operativo envía a un proceso en tiempo de ejecución, para informarle del suceso de un evento. Estas interrupciones son enviadas por el kernel al proceso, aunque puede ser otro proceso quien las genere. Algunas de estas señales podrán ser también interpretadas como órdenes que recibe el proceso.

#### Orden kill
Para enviar señales a un proceso utilizamos la orden **kill**, que responde a la siguiente sintaxis:
```bash
kill -señal [Lista de PIDs]
```
Los números de señal más comunes son:
| Número | Señal | Descripción |
| --- | --- | --- |
| 1 | SIGHUP | Permite parar y reiniciar un servicio. |
| 2 | SIGINT | Interrupción (equivalente a Control+C). |
| 9 | SIGKILL | Terminación abrupta del proceso. |
| 15 | SIGTERM | Terminación controlada del proceso. |
| 18 | SIGCONT | Reanuda un proceso suspendido. |
| 19 | SIGSTOP | Suspensión temporal del proceso |



## 5. Programación multiproceso

## 6. Enlaces web

## 7. Respuestas cuestionario