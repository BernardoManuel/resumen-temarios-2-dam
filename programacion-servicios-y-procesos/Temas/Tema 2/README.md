# Tema 2 - Programación multihilo
## 1. Threads
### 1.1. Threads en Java
Los **Threads**, hilos de ejecucion o procesos ligeros, son las unidades más pequeñas de procesamiento que pueden ser programdas por los sistemas oeperativos, y que permiten a un mismo proceso ejecutar diferentes tareas de forma simultánea. Cada hilo de ejecución ejecuta una tarea concreta y ofrece así al programador la posibilidad de diseñar programas que ejecutan funciones diferentes de forma concurrente.

Esta técnica de programación con hilos se conoce como **multithreading** o **multihilo**, y permite simplificar el diseño de aplicaciones concurrentes y mejorar el rendimiento en la creación de procesos.

### 1.2. Creación de hilos mediante la clase Thread
Una de las formas de crear un hilo es creando una clase que descienda de la clase **Thread**.

- El código correspondiente para ello sería:
```java
public class MiHilo extends Thread {
    public void run() {
        // Lógica
    }
}
```
- Y ahora, en otra clase, inicializamos y ejecutamos el hilo:
```java
public class Principal {
    public static void main (String[] args) {
        MiHilo t = new MiHilo();
        t.start();
    }
}
```

### 1.3. Creación de hilos mediante la interfaz Runnable
La forma recomendable para crear un **Thread** no es definiendo una subclase de **Thread**, sino definiendo una clase que implemente la interfaz **Runnable**.

1. Definir una clase que implemente la interfaz **Runnable** y sobreescribir el método **public void run()**, con la lógica interna del hilo:
```java
public class ClaseRunnable implements Runnable {
    @Override
    public void run() {
        // Lógica
    }
}
```
2. Crear el **Thread**. Para ello, creamos una instancia de la clase **ClaseRunnable** y se la proporcionamos al constructor de la clase **Thread** para crear el nuevo hilo:
```java
ClaseRunnable objetoRunnable = new ClaseRunnable();
Thread hilo = new Thread(objetoRunnable);
```
3. Lanzar el hilo, invocando al método **start()**:
```java
hilo.start();
```
#### Creaciónm de múltiples hilos
En algunas ocasiones necesitaremos controlar varios hilos de ejecución en nuestra aplicación. En este caso deberemos utilizar estructuras de datos como vectores o listas para almacenarlos.

#### Threads con clases anónimas
Las clases anónimas en Java nos permiten crear objetos que implementen cierta interfaz sin necesidad de crear una clase específicamente para que implemente dicha interfaz. Esto nos puede ser útil, por ejemplo, cuando solamente vamos a necesitar un objeto de dicha clase que a ser de uso inmediato.

### 1.4. El método join
Es posible que, en un momento dado, el hilo principal deba esperar a que finalicen los hilos que ha creado para seguir con su operativa, o bien para finalizar. En este caso, deberemos poner al hilo principal en espera, hasta que terminen los hilos que ha creado y se pueda unificar la ejecución de ambos. Esto se consigue mediante el método **join()**.

#### join() con tiempo de espera
En ocasiones es posible que el hilo al que el hilo principal está esperando se haya quedado bloqueado. En este caso es posible que el hilo principal quede bloqueado también en esta espera. Para evitar esta situación, se puede utilizar un par de versiones del método **join** con un temporizador.
```java
public final void join(long millis) throws InterruptedException

public final void join(long millis, int nanos) throws InterruptedException
```
### 1.5. Otros métodos de la clase Thread
La clase **Thread** posee gran cantidad de propiedades y métodos que podemos consultar en su documentación oficial.

### 1.6. El ciclo de vida de un Thread
Desde que se crea hasta que finaliza, un Thread pasa por diferentes estados. Podríamos definir pues el ciclo de vida de un Thread como el conjunto de estados por los que un Thread pasa desde su creación hasta su destrucción.

La clase **Thread** posee una propiedad estática de tipo enumerado que define los posibles estados de esta. En un momento dado, estos pueden ser:

| **Estado** | **Descripción** |
| --- | --- |
| **NEW** | El hilo se ha creado, pero todavía no ha comenzado su ejecución. |
| **RUNNABLE** | El hilo se encuentra en ejecución o listo para su ejecución y a la espera de que se asignen recursos. |
| **BLOCKED** | El hilo se encuentra bloqueado en un monitor. |
| **WAITING** | El hilo se encuentra esperando indefinidamente una acción de otro proceso. |
| **TIMED_WAITING** | El hilo se encuentra esperando un tiempo concreto una acción de otro proceso. |
| **TERMINATED** | El hilo ha finalizado su ejecución. |

## 2. Métodos sincronizados: synchronized
### 2.1. Objetos compartidos y sincronización
El desarrollo de aplicaciones multihilo comporta la ejecución de varios hilos de forma concurrente. La forma más habitual de mantener la comunicación entre estos hilos, así como con el hilo principal, es mediante el uso de variables u objetos compartidos, a los cuales todos ellos tienen acceso.

Formalmente, podríamos definir estos términos como:
- **Variable/Objeto en conflicto**: variable u objeto que son compartidos por varios hilos de ejecución.
- **Sección crítica**: se refiere a aquella parte del código fuente de un hilo que realiza acciones sobre una variable u objeto en conflicto.

### 2.2. Sincronización mediante monitores
Los monitores tienen la capacidad de bloquear un objeto mientras un hilo está accediendo a él, de modo que no permite el acceso por parte de otros hilos. Esto se conoce como exclusión mutua. Cuando el hilo que bloqueaba el objeto termina sus operaciones, desbloquea el objeto para que puedan acceder otros objetos. De este modo podemos sincronizar el acceso a un objeto compartido.

En java, todos los objetos tienen asociado un monitor, por lo que pueden sincronizarse Para acceder a este mon tor se utiliza la palabra reservada **synchronized**, que podemos emplear de dos formas:

- Para declarar métodos como **synchronized** en el objeto en conflicto, de modo que se entra en el monitor cuando se acceda a él, asegurando así una exclusión mutua.
- Para declarar solamente una sección de código como **synchronized**, de modo que entramos en el monitor sobre el objeto compartido dentro de dicha sección, asegurando así el acceso exclusivo a este.

#### Métodos sincronizados
Para sincronizar un método utilizamos la palabra reservada **synchronized** en su declaración:
```java
class Compartido {
    private dato_compartido;

    [modificador] synchronized tipoRetorno NombreMetodo(lista_argumentos){
    // Acceso al dato compartido
    }
}
```
De este modo, cuando el método **NombreMetodo** es invocado, el hilo que lo invocó entra en el monitor del objeto, quedando este bloqueado. En este momento, ningún otro hilo que intente acceder al objeto compartido ya sea a través de este método sincronizado u otros también sincronizados, podrá hacerlo, garantizando así la exclusión mutua.

#### Bloques synchronized
Además de poder especificar un método en el objeto compartido como synchronized, podemos definir únicamente un bloque de código, del siguiente modo:
```java
synchronized(objeto_en conflicto){
 // Acceso al objeto
 // (Sección crítica)
}
```

#### Syncronized y estados
En el apartado anterior vimos el ciclo de vida de un hilo, con los diferentes estados que este puede tomar. En él existía el estado **bloqueado**, al que se accedía mediante la espera/bloqueo de un monitor.

Cuando un hilo intenta acceder a un método o bloque de código sincronizado y el objeto al que va a acceder está bloqueado por otro hilo, el hilo pasa a un estado bloqueado, pasando a formar parte de una cola de hilos bloqueados a la espera de que se desbloquee el monitor Esta cola tendrá una estructura **FIFO (First In First Out)**, para garantizar la ejecución de todos los hilos.

## 3. Coordinación de Threads
### 3.1. El modelo productor-consumidor
El problema del modelo productor-consumidor es un problema clásico de la sincronización y supone un patrón común a diversos tipos de aplicaciones. El planteamiento inicial de este caso es el siguiente: disponemos de dos hilos, donde uno de ellos produce datos y el otro los consume. El problema se produce cuando el consumidor y el productor no realizan estas tareas de forma coordinada. 

Como ejemplo veamos el siguiente programa, en el que dispondremos de un objeto compartido en el que una clase **productora** escribe valores que son leídos por una clase **consumidora**.

#### Objeto compartido
La siguiente clase ObjetoCompartido tendrá un atributo entero, que guardará un valor, y un **booleano**, que indica la disponibilidad de este. También implementa un método **get**, que devuelve el valor que contiene el objeto y establece a falso la disponibilidad de este, puesto que ha sido consumido. El método **set** establece este valor e indica que está disponible.
```java
class ObjetoCompartido {
    int valor;
    boolean disponible = false; // Inicialmente no tenemos valor

    int get() {
        if (this.disponible) {
            this disponible = false
            return this.valor;
        } else {
            return -1;
        }
    }

    void set(int vale) {
        this.disponible = true;
        this.valor = vale;
    }
}
```
#### Clase productora
Por su parte, las clases **Productor** y **Consumidor** serán dos clases que implementen la interfaz **Runnable** y se inicializarán con un objeto compartido (de la clase **ObjetoCompartido**). Ambas clases en su método **run()** accederán a dicho objeto compartido, de modo que el **Productor** escriba (**produzca**) valores en él y el **Consumidor** lea (**consuma**) dichos valores.
```java
class Productor implements Runnable {
    // Referencia a un objeto compartido
    ObjetoCompartido compartido;

    Productor(ObjetoCompartido compartido) {
        this.compartido = compartido;
    }

    @Override
    public void run() {
        for (int y = 0; y < 5; y++) {
            System.out.println("El productor produce: " + y);
            this.compartido.set(y);

            try {
                Thread.currentThread().sleep(500);
            } catch (InterruptedException e) {
                System.out.println(e.getMessage());
            }
        }
    }
}
```
#### Clase consumidor
```java
class Consumidor implements Runnable {
    // Referencia a un objeto compartido
    private ObjetoCompartido compartido;
    Consumidor(ObjetoCompartido compartido) {
        this.compartido = compartido;
    }

    @Override
    public void run() {
        for (int y = 0; y < 5; y++) {
            System.out.println("El consumidor consume: " + this.compartido.get());
            try {
                Thread.currentThread(sleep(100));
            } catch (InterruptedException e) {
                System.out.println(e.getMessage());
            }
        }
    }
}
```
Finalmente disponemos de una clase principal, ModeloProductorConsumidor que creará un hilo a partir de cada una de estas clases y los lanzará:
```java
public lass ModeloProdu torConsumidor {
    public static void main(String[] args) {
        ObjetoCompartido compartido = new ObjetoCompartido();
        Thread p = new Thread(new Productor(compartido));
        Thread c = new Thread(new Consumidor(compartido));
        p.start();
        c start();
    }
}
```
En el caso contrario, en el que el productor produjese a mayor velocidad que el consumidor, y dado que en nuestro objeto compartido solamente cabe un objeto, habría valores que se perderían, puesto que se generaría el siguiente valor antes de que fuese leído.

La solución natural a este problema consistirá en que el productor espere a que el consumidor consuma los datos antes de producir nuevos datos. Del mismo modo, el consumidor deberá esperar a que los datos se hayan producido para poder consumirlos.

### 3.2. Los métodos wait(), notify() y notifyAll()
Para permitir la coordinación entre hilos en el acceso a objetos compartidos disponemos de los métodos **wait()**, **notify()** y **notifyAll()**. Estos métodos solo pueden utilizarse dentro de un bloque de código synchronized.
| **Método** | **Descripción** |
| --- | ---|
| **void wait()** | Suspende al hilo que lo invocó y libera el monitor hasta que otro hilo invoque al método **notify()** o **notifyAll()**. |
| **void notify()** | Cambia el estado de uno de los hilos suspendidos a ejecución. Si hay varios hilos, escoge a uno de ellos. Se dice que despierta a un hilo que estaba suspendido. |
| **void notifyAll()** | Despierta a todos los hilos suspendidos. Si estos vuelven a acceder al objeto compartido, deberemos hacerlo también de forma coordinada. |

Cuando el productor genera un resultado (hace un **set**), si el objeto compartido todavía contiene un valor que no se ha consumido (por lo tanto, **disponible==true**), deberá esperar a que alguien lo consuma, por lo tanto, deberá quedar suspendido, mediante una invocación a **wait()**, hasta que alguien lea el valor y lo despierte.
```java
while (disponible == true) {
    try {
        wait();
    } catch (InterruptedException e) {...}
}
```

Cabe destacar que este bucle no está continuamente ejecutándose, sino que queda suspendido en el **wait()**. Cuando otro hilo realiza un **notify()**, se lanza el evento **InterruptedException**, que capturamos dentro del bucle, de forma que hace que se despierte y vuelva a comprobar si el dato está disponible. Si el dato sigue estando disponible, vuelve a esperar, y si ya no está disponible, puede continuar, estableciendo el valor del objeto compartido, indicando que hay datos disponibles y avisando a todos los posibles consumidores que estuvieran esperando:
```java
this.valor=val;
this.disponible=true;
notifyAll();
```

Cuando el consumidor tenga que consumir un valor (hacer un **get**) del objeto compartido, el método **get** tiene que asegurar que haya algún valor disponible, de modo que el hilo que lo invoque quedará suspendido en caso de que no haya datos disponibles:
```java
while (disponible == false) {
    try {
        wait();
    } catch (InterruptedException e) {...}
}
```

Una vez haya datos disponibles y el consumidor se haya despertado por la interrupción generada (con **notify/notifyAll**), este consumirá la información e indicará al obje o compartido que la información ya no se encuentra disponible, notificando a todos los Threads que estuvieran también a la espera:
```java
this.disponible=false;
notifyAll();
return this.valor;
```

El código completo para la clase **ObjetoCompartido** quedará pues del siguiente modo. Debéis observar que los métodos **get()** y **set()** se han definido como **synchronized**, ya que debemos garantizar que solamente acceda un hilo al contenido del objeto compartido.
```java
class bjetoCompartido {
    int valor;
    boolean disponible = false; // Inicialmente no tenemos valor
    
    public synchr nized int get() {
        // Mientras no tengamos datos disponibles esperamos
        while (disponible == false) {
            try {
                wait();
            } catch (InterruptedException e) {
            }
        }

        // Cuando se despierte, volvemos a establecer la
        // disponibilidad a falso, notificamos a todos
        // los productores de la disponibilidad y
        // devolvemos el valor.
        this.disponible = false;
        notifyAll();
        return this.valor;
    }

    public synchronized void set(int vale) {
        // Mientras quedan datos nos esperamos
        while (disponible == true) {
            try {
                wait();
            } catch (InterruptedException e) {
            }
        }

        // Cuando se despierte, volvemos a establecer la
        // disponibilidad a cierto, establecemos el valor
        // generado por el productor, y notificamos a todos
        // los consumidores de la disponibilidad.
        this.valor = vale;
        this.disponible = true;
        notifyAll();
    }
}
```

#### wait(), notify() y notifyAll y estados
Un hilo que se encontraba en ejecución pasa a estado **WAITING** cuando invoca al método **wait()**, ya sea un productor o un consumidor. Cuando el hilo o hilos que estaban en espera reciben la excepción **InterruptedException**, bien con un **notify()** o con un **notifyAll()**, estos vuelven al estado de ejecución, donde vuelven a comprobar la disponibilidad de los datos.

## 4. La librería java.util.concurrent
La librería **java.util.concurrent** ofrece un conjunto muy amplio de clases e interfaces orientadas a gestionar la programación concurrente de una forma más sencilla y óptima.

### 4.1. La interfaz Callable
La interfaz **Callable** es una versión mejorada de la interfaz **Runnable**, introducida en Java 1.5, que permite retornar un valor al finalizar su ejecución.

Cuando utilizamos la interfaz **Runnable**, disponemos de un método run() que no acepta parámetros ni devuelve ningún valor. Esto no es un problema, siempre y cuando no necesitemos proporcionar ni  btener datos del Thread. En cambio, cuando hemos necesitado proporcionar valores y obtener resultados de la ejecución de un hilo, hemos optado por la comunicación mediante objetos compartidos.

Por su parte, la interfaz genérica Callable proporciona el método **call()** que dev elve un valor de tipo genérico. La sintaxis general de una clase que implemente esta interfaz será la siguiente:
```java
public class ClaseCallable implements Callable<TipoGenerico> {
    // Constructor
    [public ClaseCallable(args){...}]

    // Método call
    public TipoGenerico call() throws InvalidParamaterException {
        ...
        return valor_retorno;
    }
}
```
#### FutureTask
Con la anterior definición podríamos utilizar el método **call** directamente:
```java
ClaseCallable miCallable =n ew ClaseCallable(args);
TipoGenerico valor = miCallable.call();
```
Pero con ello no estaríamos ejecutando el método de forma asíncrona. Una primera aproximación a ello sería utilizar directamente la clase **FutureTask**, introducida en Java 5, y que se puede utilizar para realizar tareas asíncronas, ya que implementa la interfaz **Runnable** y, por tanto, puede lanzarse como un hilo.

La forma de hacer esto sería mediante la creación de un objeto de tipo **FutureTask**, de forma genérica, a partir del objeto creado de la clase **Callable**.
```java
ClaseCallable miCallable = new ClaseCallable(args);
FutureTask<TipoGenerico> miFutureTask = new FutureTask<TipoGenerico>(miCallable)
```

Este tipo genérico será el mismo que hayamos definido para nuestra clase **Callable**. Dado que **FutureTask** implemente **Runnable**, podemos crear ahora un **Thread** a partir de esta clase y lanzarlo:
```java
Thread miThread = new Thread(miFutureTask);
miThread.start();
```

Con esto el programa principal espera a que finalicen los hilos que ha generado. Una vez haya finalizado **FutureTask**, vamos a poder acceder al valor devuelto por el método **call** mediante **get**:
```java
TipoGenerico resultado = miFutureTask.get();
```

### 4.2. La API ExecutorService
**ExecutorService** es una API de Java que nos permite simplificar la ejecucióin de tareas asíncronas. Para ello nos ofrece un conjunto de hilos preparados para asignarles tareas.

#### Future
La interfaz **Future**, definida en el paquete **java.util.concurrent**, representa el resultado de una operación asíncrona. El valor retorno, del tipo genérico indicado, no lo obtendremos de forma inmediata, sino que se obtendrá en el momento en que finalice la ejecución de la tarea asíncrona. En este momento, el objeto **Future** tendrá disponible dicho valor de retorno.

La interfaz Future proporcionará pues los mecanismos para saber si ya dispon del resultado, para esperar a tener resultados y para consultar dichos resultados, así como para cancelar la función asíncrona si todavía no ha terminado.

#### Asignación de tareas al ExecutorService

#### Finalización del servicio
Una vez que dejemos de utilizar el servicio y que hayamos obtenido todos los resultados utilizaremos el método **shutdown** para finalizar el servicio:
```java
servicio.shutdown();
```

### 4.3. Colas concurrentes: BlockingQueue

## 5. Enlaces Web

- [Referencia a la documentación de OpenJDK sobre la clase Thread.](hhttps://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/lang/Thread.html)
- [Referencia a la documentación de OpenJDK sobre la interfaz Runnable.](https://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/lang/Runnable.html)
- [Documentación oficial de Oracle sobre las interrupciones a threads.](https://docs.oracle.com/javase/tutorial/essential/concurrency/interrupt.html)
- [Artículo en la wiki de OpenJDK sobre sincronización.](https://wiki.openjdk.java.net/display/HotSpot/Synchronization)
- [Documentación oficial de OpenJDK sobre el paquete java.util.concurrent.](https://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/util/concurrent/package-summary.html)
- [Documentación oficial de OpenJDK sobre la interfaz Executor.](https://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/util/concurrent/Executor.html)
- [Documentación oficial de OpenJDK sobre la clase Executors.](https://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/util/concurrent/Executors.html)
- [Documentación oficial de OpenJDK sobre la clase Future.](https://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/util/concurrent/Future.html)
- [Documentación oficial de OpenJDK sobre la interfaz BlockingQueue.](https://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/util/concurrent/BlockingQueue.html)
- [Documentación oficial de OpenJDK sobre la clase ArrayBlockingQueue.](https://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/util/concurrent/ArrayBlockingQueue.html)
- [Documentación oficial de OpenJDK sobre la clase LinkdedBlockingDeque.](https://cr.openjdk.java.net/~iris/se/17/latestSpec/api/java.base/java/util/concurrent/LinkedBlockingDeque.html)

## 6. Respuestas cuestionario

- [Cuestionario](./CUESTIONARIO.md)