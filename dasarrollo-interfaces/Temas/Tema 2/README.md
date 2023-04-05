# Tema 2 - Desarrollo de interfaces
## 1. Introducción
### 1.1. Python
Es un lenguaje de programación multiparadigma, interpretado, multilataforma y libre.

Es un lenguaje de propósito general, es decir, no está tipificado para un fin en concreto.

Caracterísiticas:
- **De alto nivel**
- **Interpretado**
- **Multiplataforma**
- **Libre**
- **Limpio y legible**
- **Tipado fuerte y dinámico**
- **Amplia comunidad**

### 1.2. Qt
Qt es un framework de desarrollo de aplicaciones multiplataformas para escritorio, sistemas empotrados y sistemas móviles.

No es un lenguaje de programación, sino un conjunto de herramientas para el desarrollo de interfaces gráficas de usuario multiplataforma mediante C++.

### 1.3. PySide6
**PySide es la unión de Python y Qt**. Se trata del binding oficial, es decir, la implementación par apoder hacer uso desde Python del framework Qt, escrito para C++. Python posee otras librería sobre las que poder desarrollar GUIs, como son Tkinter, Kivy o wxPython.

## 2. Componentes de uso común
### 2.1. Componentes software
El desarrollo de la interfaz de una aplicación se basa en la construcción de una aplicación a partir de componentes software ya existentes, limitando al mínimo necesario el desarrollo de código nuevo. Cualquier aplicación puede estar compuesta por múltiples componentes, y los componentes principales tienen componentes secundarios anidados dentro de ellos.

### 2.2. Componentes PySide6
Los widgets son los elementos básicos y esenciales para crear interfaces de usuario en Qt. Pueden mostrar información, recibir información del usuario y contener otros widgets de forma agurpada. Un widget que no está incrustado en un widget padre se muestra en forma de ventana independiente.

Cada aplicación tendrá al menos una ventana, pero podrá tener más. Normalmente una aplicación terminará al cerrar la última de las ventanas.

Algunas cosas a tener en cuenta del código:
- Para crear una clase derivada de otra, se pasa la clase base entre paréntesis. Heredamos de la clase QWidget para crear nuestra popia ventana. Un QWidget es la clase base para todos los widgets. Cualquier clase que herede de ésta, se puede mostrar como una ventana independiente, es decir, que no tiene un padre, como ventana principal. super() se refiere a la clase de la que se hereda.
- El constructor de la clase es el método especial __init__.
- El parámetro self hace referencia a la misma clase que estamos implementando
- Los componentes están ocultos por defecto. Si les pasamos el *parent* en su creación, se mostrarán al mostrar su *parent*. En caso contrario, los podemos mostrar con el método show, pero se mostrarán como una ventana independiente.
- Accedemos o cambios las propiedas de los Widgets a través de métodos públicos. Los métodos de asignación de valores a propiedades, setters, empiezan por set seguido del nombre de la propiedad que queremos asignar, mientras que los de lectura de valores, getters, suelen empezar por el nombre de la propiedad a leer.

### 2.3. Eventos
Cada interacción del usuario con la interfaz generá un **evento**. Este evento será añadido a la cola de eventos (**event queue**) para ser gestionado.

El bucle de eventos,  que es un bucle infinito, comprobará en cada interación si hay eventos pendientes de ser gestionados. En caso de ser así, el evento será gestionado por el gestor de eventos que ejecutará su manejador. Cuando este termina, el control vuelve al bucle de eventos para esperar más eventos.

#### Señales y ranuras
Al producirse un evento sobre la etiquete, la aplicación no ejecutaba ninguna funcionalidad asociada a ese evento. Necesitamos conectar lo eventos a alguna funcionalidad concreta.

Un **señal** en Qt se emite cuando el usuario produce un evento. Las ranuras (**slots**) son escuchadores de señales que se ejecutarán al lanzarse la emisión de la señal a la que están conectados.

### 2.4. Principales componentes en formularios
Vamos a hacer un listado con los componentes más habituales de PySide6 usados en formularios, junto con algunas de sus señales y funciones más útiles.

| Widget | Señales | Funciones |
| --- | --- | --- |
| QCheckBox | stateChanged() | isChecked() setCheckState() |
| QLabel |  | setText() |