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

#### killall
Con killall podemos enviar una señal concreta a todos los procesos con el mismo nombre, en lugar de su PID.

#### Captura de señales. La orden trap
Mediante la orden **trap** podemos capturar señales y realizar ciertas acciones en respuesta a esta señal. La sintaxis de **trap** es:
```bash
trap 'Órdenes' [lista de señales]
```

### 2.4. Mecanismos IPC (Inter Process Comunication)
Vamos a ver ahora algunos mecanismos pensados específicamente para la comunicación entre procesos, bien en la misma máquina o en máquinas remotas.

#### Tuberías con nombre (colas de mensajes)
Las tuberías con nombre (**named pipes**) actúan como colas FIFO. Se trata de ficheros especiales sobre los que los procesos pueden leer y escribir.

Para crear un fichero que sea una tubería con nombre utilizamos la orden **mkfifo** como sigue:
```bash
mkfifo cola
```
Ahora podemos leer este fichero con un simple **cat**.

Cuando realizamos esta lectura, el proceso no finaliza, sino que espera a la entrada de información en la cola. Desde otra terminal podemos comprobar si se trata de una tubería con nombre (con un test -p) y, en caso afirmativo, escribir en ella:
```bash
[ -p cola ] && echo “hola” > cola
```
Debemos tener en cuenta que se trata de un mecanismo bloqueante: cuando un proceso productor escribe en la tubería queda a la espera de que un proceso consumidor la lea Y cuando un proceso consumidor intenta leer y la cola está vacía, espera a que un proceso productor escriba en ella.

#### Comunicación mediante sockets
Los **sockets** permiten la comunicación en re procesos ituados en diferentes máquinas, y se especifican
mediante una dirección IP y un núme o de puerto (**TCP** o **UDP**).

**Bash** permite establecer conexiones con sockets mediante las pseudo rutas /dev/tcp y /dev/udp. Recordemos que estas pseudo-rutas no son icheros en disco como tal, sino que se trata de una abstracción que utiliza el si tema operativo para que se puedan realizar sobre ellos las mismas operaciones que sobre los ficheros (lecturas y escrituras).

La sintaxis para estab ecer una conexión en un socket mediante bash es:
```bash
exec {descriptor_de_fichero}<>/dev/{protocolo}/{host}/{puerto}
```
Siendo el descriptor de fichero un número positivo diferente de 0, 1 y 2 (puesto que estos son utilizados para stdin, stdout y stderr).

Además, podemos indicar que el socket sea b direccional (<>) o unidireccional (< o >).

#### La herramienta socat
Con la herramienta **socat** podemos establecer una comunicación bidireccional de flujo de bytes entre dos canales, ya sean tuberías, **sockets**, etc.

La sintaxis básica de socat es:
```bash
socat [opciones] [direccion_1] [direccion_2]
```
Con lo que se establece un canal de comunicación entre las direcciones indicadas. Dichas direcciones pueden incluir palabras clave con valor, de la forma CLAVE:valor. Ejemplos:
```bash
socat -u TCP-LISTEN:6666 STDOUT

socat -u STDIN TCP:127.0.0.1:6666
```

## 3. Progrmación multiproceso. Procesos en Java
Mediante las clases **ProcessBuilder** y **Runtime**, Java nos permitirá ejecutar comandos en la terminal, generando procesos que serán gestionados mediante la clase **Process**.

### 3.1. La clase ProcessBuilder
La clase **java.lang.ProcessBuilder** nos permite lanzar cualquier ejecutable desde Java. Para ello, una vez creada la instancia **ProcessBuilder**, establecemos ciertos atributos del proceso y creamos este mediante el método **start**.
```java
ProcessBuilder pb = new ProcessBuilder();
```

### 3.2. La clase Process
La clase **java.lang.Process** es una clase abstracta que encapsula la información entorno a la ejecución de un proceso.

Cuando invocamos al método **start** de un **ProcessBuilder**, este nos devuelve una instancia de **Process**, con la información sobre el proceso lanzado. Esta clase puede utilizarse para realizar operaciones de entrada y salida de los procesos, comprobar si este ha finalizado, su estado de finalización, o bien enviarle una señal para cerrarlo forzosamente.
```java
Process p1 = pb.start();

System.out.println("PID del proceso 1: " + p1.pid());
```

### 3.3. La interfaz ProcessHandle.Info
La interfaz **ProcessHandle.Info** nos permite obtener información adicional sobre un proceso. 

Para obtener dicha información únicamente deberemos acceder al método info() de la clase Process:
```java
ProcessHandle.Info informacion = p.info();
```

### 3.4. La clase java.lang.Runtime
La API de Java para la gestión de procesos, además de las clases **ProcessBuilder** y **Process**, se completa con la clase **Runtime**, que encapsula el entorno de ejecución de un proceso.

Dado que también es una clase abstracta, no puede ser instanciada directamente, por lo que para usarla
deberemos hacer uso del método estático **Runtime.getRuntime()**.

| Método | Descripción |
| --- | --- |
| int availableProcessors()  | Devuelve el número de procesadores disponibles para la JVM. |
| Process exec(comando) | Permite ejecutar el comando indicado. Admite diferentes firmas para indicar comando, argumentos o entorno. |
| 9 | SIGKILL |
| 15 | SIGTERM |
| 18 | SIGCONT |
| 19 | SIGSTOP |

## 5. Programación multiproceso

## 6. Enlaces web

## 7. Respuestas cuestionario