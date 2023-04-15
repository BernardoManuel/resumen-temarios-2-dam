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

## 4. La librería java.util.concurrent

### 4.1. La interfaz Callable

### 4.2. La API ExecutorService

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