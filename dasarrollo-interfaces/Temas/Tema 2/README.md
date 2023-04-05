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
| QGroupBox |  |  |
| QComboBox | currentIndexChanged() currentTextChanged() | addItem() setCurrentIndex() |
| QRadioButton | toggled() | isChecked() |
| QPushButton | clicked() | setCheckable() |
| QTabWidget | currentChanged() tabBarClicked() | addTab(widget, label) setCurrentIndex() setCurrentWidget() |
| QTableWidget | cellChanged() currentCellChanged() ... | clear() insertColumn() insertRow() removeColumn() removeRow() |
| QLineEdit | textChanged() | setText() clear() |
| QTextEdit | textChanged() | setText() clear() |
| QProgressBar | valueChanged() | setValue() setOrientation() setMaximum() setMinimum() |
| QDateTimeEdit | dateChanged() dateTimeChanged() timeChanged() | setDate() setDateTime() setTime() |
| QSlider | valueChanged() | setValue() |
| QDial | valueChanged() | setValue() |

## 3. Contenedores de componentes. Diseño
### 3.1. Layouts
```python
from PySide6.QtWidgets import QApplication, QLabel, QWidget
class Ventana(QWidget):
    def __init__(self):
        QWidget.__init__(self)
        self.setWindowTitle("Ventana")

        self.label1 = QLabel("Etiqueta 1", self)
        self.label2 = QLabel("Etiqueta 2", self)

        self.label2.move(0, 30)
if __name__ == "__main__":
    app = QApplication([])
    ventana = Ventana()
    ventana.show()
    app.exec()
```

En este apartado vamos a estudiar una forma más eficiente de gestionar todo esto a trabés de layouts: diseños o disposiciones que podemos aplicar a una interfaz para ordenar sus componentes. Con la combinación de estos layouts es posible definir el diseño de cualquier interfaz gráfica de usuario.

### 3.2. QVBoxLayout
En esta disposición los elementos están en vertical. Se irán añadiendo los componentes al final de una pila de componentes, uno encima de otro.

```python
from PySide6.QtWidgets import { 
    QApplication, QMainWindow, QWidget, QVBoxLayout, QPushButton
}
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Layout vertical")
        
        layout_vertical = QVBoxLayout()

        layout_vertical.addWidget(QPushButton("Uno"))
        layout_vertical.addWidget(QPushButton("Dos"))
        layout_vertical.addWidget(QPushButton("Tres"))
        layout_vertical.addWidget(QPushButton("Cuatro"))

        componente_principal = QWidget()
        componente_principal.setLayout(layout_vertical)
        self.setCentralWidget(componente_principal)
app = QApplication([])
ventana = VentanaPrincipal()
ventana.show()
app.exec()
```

### 3.3. QHBoxLayout
En este apartado nos centramos en la disposición horizontal de los componentes. Usamos un layout horizontal:

```python
from PySide6.QtWidgets import { 
    QApplication, QMainWindow, QWidget, QHBoxLayout, QPushButton
}
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Layout horizontal")
        
        layout_horizontal = QHBoxLayout()
        componente_principal = QWidget()

        componente_principal.setLayout(layout_horizontal)
        self.setCentralWidget(componente_principal)

        layout_horizontal.addWidget(QPushButton("Uno"))
        layout_horizontal.addWidget(QPushButton("Dos"))
        layout_horizontal.addWidget(QPushButton("Tres"))
        layout_horizontal.addWidget(QPushButton("Cuatro"))
app = QApplication([])
ventana = VentanaPrincipal()
ventana.show()
app.exec()
```

### 3.4. QHBoxLayout
Aunque con el uso de layout verticales y horizontales podríamos conseguir casi cualquier disposición, esto puede no resultar cómodo de gestionar en algunas ocasiones.

```python
from PySide6.QtWidgets import { 
    QApplication, QMainWindow, QWidget, QGridLayout, QPushButton
}
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Layout cuadrícula")
        
        layout_cuadricula = QGridLayout()
        componente_principal = QWidget()
        componente_principal.setLayout(layout_cuadricula)
        self.setCentralWidget(componente_principal)

        layout_cuadricula.addWidget(QPushButton("0,0"), 0, 0)
        layout_cuadricula.addWidget(QPushButton("0,1"), 0, 1)
        layout_cuadricula.addWidget(QPushButton("0,2"), 0, 2)
        layout_cuadricula.addWidget(QPushButton("0,3"), 0, 3)
        
        layout_cuadricula.addWidget(QPushButton("1,0-3"), 1, 0, 1, 4)
        
        layout_cuadricula.addWidget(QPushButton("2,0-1"), 2, 0, 1, 2)
        layout_cuadricula.addWidget(QPushButton("2,2-3"), 2, 2, 1, 2)
app = QApplication([])
ventana = VentanaPrincipal()
ventana.show()
app.exec()
```

### 3.4. QHBoxLayout
Aunque con el uso de layout verticales y horizontales podríamos conseguir casi cualquier disposición, esto puede no resultar cómodo de gestionar en algunas ocasiones.

```python
from PySide6.QtWidgets import { 
    QApplication, 
    QMainWindow, 
    QWidget, 
    QFormLayout, 
    QLabel,
    QLineEdit,
    QSpinBox,
    QDoubleSpinBox
}
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Layout formulario")
        
        layout_formulario = QFormLayout()
        componente_principal = QWidget()
        componente_principal.setLayout(layout_cuadricula)
        self.setCentralWidget(componente_principal)

        layout_formulario.addRow(QLabel("Texto: "), QLineEdit())
        layout_formulario.addRow(QLabel("Entero: "), QSpinBox())
        layout_formulario.addRow(QLabel("Decimal: "), QDoubleSpinBox())
        
app = QApplication([])
ventana = VentanaPrincipal()
ventana.show()
app.exec()
```

### 3.4. QStackedLayout
Por último, vamos a ver un layout que permite apilar componentes, pero no verticalmente, de modo que todos son visible, sino en profundidad, de forma que solo uno de los elementos será visible. Para gestionar qué elemento es visible utilizamos setCurrentIndex o setCurrentWidget.

```python
from PySide6.QtWidgets import { 
    QApplication, 
    QMainWindow, 
    QWidget, 
    QPushButton, 
    QStackedLayout,
    QLabel,
    QVBoxLayout,
    QHBoxLayout
}
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Layout apilado")
        
        layout_principal = QHBoxLayout()
        componente_principal = QWidget()
        componente_principal.setLayout(layout_principal)
        self.setCentralWidget(componente_principal)

        self.pila = QStackedLayout()
        self.pila.addWidget(QLabel("Capa 1"))
        self.pila.addWidget(QLabel("Capa 2"))
        self.pila.addWidget(QLabel("Capa 3"))

        layout_botones = QVBoxLayout()
        boton1 = QPushButton("Ver capa 1")
        boton1.clicked.connect(self.activar_capa1)
        boton2 = QPushButton("Ver capa 2")
        boton2.clicked.connect(self.activar_capa2)
        boton3 = QPushButton("Ver capa 3")
        boton3.clicked.connect(self.activar_capa3)

        layout_botones.addWidget(boton1)
        layout_botones.addWidget(boton2)
        layout_botones.addWidget(boton3)

        layout_principal.addLayout(self.pila)
        layout_principal.addLayout(layout_botones)

        def activar_capa1(self):
            self.pila.setCurrentIndex(0)

        def activar_capa2(self):
            self.pila.setCurrentIndex(1)

        def activar_capa3(self):
            self.pila.setCurrentIndex(2)
        
app = QApplication([])
ventana = VentanaPrincipal()
ventana.show()
app.exec()
```

## 4. Menús, barras de herramientas, barra de estado y componentes flotantes
La ventana principal de cualquier aplicaicón, su estructura básica suele seguir un esquema parecido al siguiente:
- Un menú, normalmente en forma de desplegable, aunque puede ser en forma de pestañas.
- Barras de herramientas, con funcionalidad habitual en un solo clic; acceder a ellas mediante menú sería más tedioso.
- Un componente principal, que ocupa la parte central de la aplicación.
- Una barra de estado, que indica el estado o alguna configuración activa de la aplicación.

### 4.1. QActions
En las aplicaciones se puede ejecutar una misma funcionalidad interaccionando con diferentes interfaces de usuario, ya sea a través de menús, botones de la barra de herramientas o atajos de teclado. Aquí es donde entran en juego las QAction de Qt. Además, se les puede asignar un texto de estado, que será usado en la barra de estados.

### 4.2. Barra de menú
Para añadir menús a QMainWindow utilizaremos el método **.addMenu()** de su barra de menús menuBar(). A este nuevo menú le pondemos agregar nuevos submenús con addMenu() y separadores, para organizar de forma más coherente las opciones, con addSeparator().

```python
from PySide6.QtWidgets import QApplication, QMainWindow
from PySide6.QtGui import QAction, QKeySequence
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Ventana principal con menús")
        
        barra_menus = self.menuBar()
        menu = barra_menus.addMenu("&Menu")

        accion = QAction("&Imprimir por consola", self)
        accion.setShortcut(QKeySequence("Ctrl+p"))
        accion.triggered.connect(self.imprimir_por_consola)
        menu.addAction(accion)
    
    def imprimir_por_consola(self):
        print("Acción lanzada a través del menú o del atajo")

if __name__ == "__main__":
    app = QApplication([])
    ventana1 = VentanaPrincipal()
    ventana1.show()
    app.exec()
```

### 4.3. Barra de herramientas
Para añadir la acción una barra de herramientas haremos los siguientes pasos:
1. Creamos una barra de herramientas instanciando la clase **QToolBar**
2. Añadimos la acción a la barra de herramientas con el método **addAction**
3. Añadimos la barra de herramientas a la ventana principal con **addToolBar**

Las opciones disponibles, que están en el módulo Qt de Qt.Core, son las siguientes:
| Flag | Resultado |
| --- | --- |
| Qt.ToolButtonIconOnly | Solo muestra el icono |
| Qt.ToolButtonTextOnly | Solo muestra el texto |
| Qt.ToolButtonTextBesideIcon | Muestra el texto al lado del icono |
| Qt.ToolButtonTextUnderIcon | Muestra el texto debajo del icono |
| Qt.ToolButtonFollowStryle | Sigue la configuración del sistema |

```python
import os
from PySide6.QtGui import QAction, QIcon, QKeySequence
from PySide6.QtWidgets import QApplication, QMainWindow, QToolBar
# Nuestra ventana principal hereda de QMainWindow
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle ("Ventana principal con menu y barra de herramientas")
        barra_menus = self.menuBar()
        menu = barra_menus.addMenu("&Menu")
        ruta_a_icono = os.path.join(os.path.dirname(__file__), "console-log-icon.png")
        # Añadimos un icono a la acción
        accion = QAction(QIcon(ruta_a_icono), "Imprimir por consola", self) 
        accion.setWhatsThis("Al pulsar sobre el botón se imprimirá un texto por consola") accion.setShortcut (QKeySequence ("Ctrl+p"))
        accion.triggered.connect(self.imprimir_por_consola)
        menu.addAction (accion)
        # Creamos la barra de herramientas
        barra_herramientas = QToolBar ("Barra de herramientas 1")
        # Añadimos el QAction a la barra de herramientas
        barra_herramientas.addAction (accion)
        # Añadimos la barra de herramientas a la aplicación
        self.addToolBar (barra_herramientas)

    def imprimir_por_consola (self):
        print ("Acción lanzada a través del menú, del atajo o de la barra de herramientas")

if __name__ == "__main__":
    app = QApplication([])
    ventana1 = VentanaPrincipal()
    ventana1.show()
    app.exec()
```

### 4.4. Barra de estado
El principal uso de la barra de estado es mostrar información al usuario, y los métodos más utilizados, como addWidgt, addPermanentWidget, showMessage y clearMessage, nos servirán para agregar componentes y mostrar/ocultar mensajes.

Cada indicador de estado puede ser de una de estas tres categorías:
1. Temporal
2. Normal
3. Permanente

```python
import os
import platform
from PySide.QtCore import Qt
from PySide6.QtGui import QAction, QIcon, QKeySequence
from PySide6.QtWidgets import (QApplication, QMainWindow, QToolBar, QLabel, QDockWidget, QTextEdit)
# Nuestra ventana principal hereda de QMainWindow
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle ("Ventana principal con menú, barra de herramientas y barra de estado")
        barra_menus = self.menuBar()
        menu = barra_menus.addMenu("&Menu")
        ruta_a_icono = os.path.join(os.path.dirname(__file__), "console-log-icon.png")
        
        accion = QAction(QIcon(ruta_a_icono), "Imprimir por consola", self) 
        accion.setWhatsThis("Al pulsar sobre el botón se imprimirá un texto por consola") accion.setShortcut (QKeySequence ("Ctrl+p"))
        accion.triggered.connect(self.imprimir_por_consola)
        menu.addAction (accion)

        barra_herramientas = QToolBar ("Barra de herramientas 1")
        barra_herramientas.addAction (accion)
        self.addToolBar (barra_herramientas)

        barra_estado = self.statusBar()
        barra_estado.addPermanentWidget(QLabel(platform.system()))
        barra_estado.showMessage("Listo. Esprando acción ...", 3000)

        dock1 = QDockWidget()
        dock1.setWindowTitle("Componente base 1")
        dock1.setWidget(QTextEdit(""))
        dock1.setMinimumWidth(50)

        self.addDockWidget(Qt.RightDockWidgetArea, dock1)
        self.setCentralWidget(QLabel("Componente principal"))

    def imprimir_por_consola (self):
        print ("Acción lanzada a través del menú, del atajo o de la barra de herramientas")

if __name__ == "__main__":
    app = QApplication([])
    ventana1 = VentanaPrincipal()
    ventana1.show()
    app.exec()
```

### 4.5. Componentes flotantes
Los componentes flotantes aportan gran versatilidad a las aplicaciones. Son componentes que pueden cambiar de ubicación, desacoplaser e incluso cerrarse.

```python
import os
import platform
from PySide6.QtGui import QAction, QIcon, QKeySequence
from PySide6.QtWidgets import QApplication, QMainWindow, QToolBar
# Nuestra ventana principal hereda de QMainWindow
class VentanaPrincipal(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle ("Ventana principal con menú, barra de herramientas y barra de estado")
        barra_menus = self.menuBar()
        menu = barra_menus.addMenu("&Menu")
        ruta_a_icono = os.path.join(os.path.dirname(__file__), "console-log-icon.png")
        
        accion = QAction(QIcon(ruta_a_icono), "Imprimir por consola", self) 
        accion.setWhatsThis("Al pulsar sobre el botón se imprimirá un texto por consola") accion.setShortcut (QKeySequence ("Ctrl+p"))
        accion.triggered.connect(self.imprimir_por_consola)
        menu.addAction (accion)

        barra_herramientas = QToolBar ("Barra de herramientas 1")
        barra_herramientas.addAction (accion)
        self.addToolBar (barra_herramientas)

        barra_estado = self.statusBar()
        barra_estado.addPermanentWidget(QLabel(platform.system()))
        barra_estado.showMessage("Listo. Esprando acción ...", 3000)

    def imprimir_por_consola (self):
        print ("Acción lanzada a través del menú, del atajo o de la barra de herramientas")

if __name__ == "__main__":
    app = QApplication([])
    ventana1 = VentanaPrincipal()
    ventana1.show()
    app.exec()
```

## 5. Diálogos y otras ventanas
### 5.1. Diálogos
Hay varios tipos de ventanas para mostrar o pedir información al usuario:
#### QDialog
Los QDialog son ventanas emergentes temporales que nos permiten comunicarnos con el usuario de la aplicación y que aparecen debido a la producción de un evento. Son ventanas modales, es decir, bloquean la interacción con el resto de la aplicación hasta que su ejecución termine, ya sea cerrándolas o introduciendo la información que se pide.

```python
from PySide6.QtWidgets import QApplication, QDialog, QMainWindow, QPushButton 
class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.setWindowTitle("Aplicación con diálogos")
        boton = QPushButton("Haz clic para que el diálogo aparezca") 
        boton.clicked.connect(self.mostrar_dialogo) 
        self.setCentralWidget(boton)

    def mostrar_dialogo (self):
        print("Clic recibido, se mostrará el diálogo.")

        ventana_dialogo = QDialog(self)
        ventana_dialogo.setWindowTitle("Ventana de diálogo")
        # Lanzamos su bucle de eventos
        ventana_dialogo.exec()

app = QApplication([])
ventana_principal = MainWindow()
ventana_principal.show()
app.exec()
```

#### Diálogos personalizados
Podemos utilizar QPushButton, pero en Qt hay una serie de botones predefinidos diseñados según las guías de estilo de las diferentes plataformas.

Los botones predefinidos en Qt se encuentran en el módulo QDialogButtonBox como propiedades.

### 5.2. QMessageBox
Existen cuadros de diálogo ya prediseñados en Qt. Se encuentra disponibles en el módulo QMessageBox y existen cuatro tipos según el nivel de severidad de la información. Realmente, la única diferencia entre ellos es el incono que muestran
 - Question
 - Information
 - Warning
 - Critical

Igual que en los QDialog, existen botones predefinidos que podemos utilizar en nuestros QMessageBox.

### 5.3. Otros díalogos
Existen otros tipos de diálogos que puedes encontrar en el módulo QtWidgets. Son estos:
- QColorDialog
- QFileDialog
- QFontDialog
- QInputDialog
- QProgressDialog

### 5.4. Otras ventanas
En Qt, cualquier Widget sin parent es una ventana. Esto, a efectos prácticos, significa que, para mostrar una ventana nueva, solo tenemos que crear un Widget y llamar su método show(). Fíjate que incluso podríamos crear una aplicación con varios QMainWindows.

## 6. Enlaces Web

- [Formas de contribuir al proyecto Qt](http://qt.io/contribute/)
- [Información sobre las diferentes licencias de Qt](https://doc-snapshots.qt.io/qt6-dev/licensing.html)
- [Documentación de Qt para Python](https://doc.qt.io/qt-6/index.html)
- [Documentación para C++](https://doc.qt.io/qt-6/index.html)
- [Página de descarga de Python](https://www.python.org/downloads/)
- [Documentación de componentes Qt6](https://doc.qt.io/qt-6/qtwidgets-module.html)

## 7. Respuestas cuestionario

- [Cuestionario](./CUESTIONARIO.md)