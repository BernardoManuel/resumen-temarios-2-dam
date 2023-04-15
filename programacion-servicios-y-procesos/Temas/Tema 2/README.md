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