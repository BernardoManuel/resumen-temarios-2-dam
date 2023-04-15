# Cuestionario - Tema 1

#### Pregunta 1 - ¿En qué estado se encuentra un proceso que acaba de iniciarse mediante la llamada al sistema fork?
- [ ] a. En ejecución.
- [ ] b. En espera de evento. (S)
- [ ] c. Finalizado. (T)
- [ ] d. En espera E/S. (D)
- [X] e. Preparado (en la cola de procesos del procesador).

#### Pregunta 2 - ¿Qué orden podemos utilizar desde bash para obtener información sobre los procesos ligeros del sistema?
- [ ] a. ps -threads
- [X] b. ps -eLf
- [ ] c. top
- [ ] d. systemctl
- [ ] e. ps aux

#### Pregunta 3 - ¿Qué hace exactamente el comando cat << FIN > fichero.txt?
- [X] a. Toma como entrada del comando cat lo que escribimos por teclado hasta el símbolo FIN, y guarda lo introducido en fichero.txt.
- [ ] b. Muestra por pantalla el contenido del fichero.txt hasta que encuentra el texto FIN. 
- [ ] c. Muestra por pantalla el contenido del fichero.txt
- [ ] d. Da un error de ejecución, ya que no podemos usar varios tipos de redirección en una misma orden.
- [ ] e. Escribe FIN en fichero.txt

#### Pregunta 4 - ¿Qué tipo de mecanismo de comunicación son las tuberías con nombre y los sockets?
- [ ] a. Mecanismos de comunicación mediante variables de la Shell.
- [ ] b. Mecanismos de comunicación mediante E/S.
- [X] c. Mecanismos IPC.
- [ ] d. Mecanismos de comunicación mediante variables de entorno.
- [ ] e. Mecanismos de comunicación mediante señales.

#### Pregunta 5 - Si definimos el objeto pb como ProcessBuilder pb = new ProcessBuilder("firefox"), ¿podremos utilizarlo para lanzar otros comandos?
- [ ] a. No es posible, una vez inicializado el ProcessBuilder no se puede modificar.
- [X] b. Sí, modificando el atributo command de pb tantas veces como deseemos.
- [ ] c. No es posible, aunque modifiquemos el command, este siempre lanzará Firefox.
- [ ] d. No es posible al menos hasta que lancemos el primer start() sobre Firefox.
- [ ] e. Sí, modificando el atributo command de pb solamente una vez.

#### Pregunta 6 - ¿Cuál o cuáles de los siguientes métodos de Runtime se utilizan para conocer la cantidad de memoria disponible para la JVM?
- [ ] a. freeMemory()
- [ ] b. totalMemory()
- [ ] c. Gc()
- [ ] d. availableProcessors()
- [X] e. maxMemory()

#### Pregunta 7 - ¿Qué stream obtenemos con getInputStream()?
- [ ] a. El stream de salida conectado a la salida estándar del proceso.
- [X] b. El stream de entrada conectado a la salida estándar del proceso.
- [ ] c. El stream de entrada conectado a la entrada estándar del proceso.
- [ ] d. El stream de entrada conectado a la salida de error del proceso.
- [ ] e. El stream de salida conectado a la entrada estándar del proceso.

#### Pregunta 8 - ¿Qué valor utilizaremos si deseamos redirigir, directamente desde ProcessBuilder, la salida de un proceso para mostrarla por pantalla?
- [ ] a. ProcessBuilder.Redirect.DISCARD
- [ ] b. ProcessBuilder.Redirect.to()
- [ ] c. ProcessBuilder.Redirect.appendTo()
- [X] d. ProcessBuilder.Redirect.INHERIT
- [ ] e. ProcessBuilder.Redirect.PIPE

#### Pregunta 9 - ¿A qué nos referimos cuando hablamos de eficiencia en la distribución de tareas?
- [ ] a. A que los fragmentos deben generar volúmenes de datos que equilibren la carga entre las diferentes tareas.
- [ ] b. A que el tamaño y la cantidad de fragmentos deben ser flexibles..
- [ ] c. A que el código debe ser legible y fácil de entender y depurar. 
- [ ] d. A que el diseño debe aportar flexibilidad en el número y el tamaño de las tareas generadas.
- [X] e. A que las tareas deben tener suficiente trabajo como para compensar el coste de creación y gestión de estas.

#### Pregunta 10 - ¿Qué estructura de soporte implica que las unidades de ejecución ejecuten el mismo programa en paralelo pero sobre diferentes datos?
- [ ] a. Loop Parallelism.
- [ ] b. Pipelining.
- [X] c. SPMD.
- [ ] d. Fork/Join.
- [ ] d. Master/Worker.