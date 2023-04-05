# Cuestionario - Tema 3

#### Pregunta 1 - Dado el fragmento de código siguiente, determina la afirmación correcta:
```python
from PySide6.QtWidgets import (
    QApplication,
    QWidget,
    QLabel
)
 
class VentanaPrincipal(QWidget):
    def __init__(self):
        super().__init__()
        QLabel("Hola mundo!", self)
       
 
app = QApplication([])
ventana = VentanaPrincipal()
app.exec()
```
- [ ] a. No se mostrará nada porque no se ejecuta el bucle de eventos de la aplicación.
- [ ] b. Se mostrará una ventana con el texto «Hola Mundo!».
- [X] c. No se mostrará nada porque no se ejecuta el show() del componente, pero se ejecutará la aplicación.
- [ ] d. Se mostrará una ventana que no responde a los eventos y se tendrá que matar el proceso.

#### Pregunta 2 - Dado el fragmento de código siguiente, determina las afirmaciones que son correctas:
```python
from PySide6.QtWidgets import (
    QApplication,
    QWidget,
    QLabel
)
 
class VentanaPrincipal(QWidget):
 
    def __init__(self):
        super().__init__()
        self.etiqueta = QLabel("Hola mundo!")
        self.etiqueta.show()
 
app = QApplication([]) 
ventana = VentanaPrincipal()
ventana.show()
 
app.exec()
```

- [ ] a. Se mostrará una ventana con el texto «Hola mundo!».
- [ ] b. No se ejecutará la aplicación.
- [ ] c. No se mostrará ninguna ventana.
- [X] d. Se mostrarán dos ventanas, una de ella con el texto «Hola mundo!».

#### Pregunta 3 - Observa la imagen siguiente y determina las afirmaciones correctas:
![DIN2-Pregunta3.png](https://i.postimg.cc/KYxD0279/DIN2-Pregunta3.png)

- [ ] a. El layout principal es un QHBoxLayout que contiene un QFormLayout y un QPushButton.
- [ ] b. Tiene un layout en forma de formulario.
- [X] c. El layout principal es un QVBoxLayout que contiene un QFormLayout y un QPushButton.
- [ ] d. El layout principal es un QFormBoxLayout que contiene un QVormLayout y un QPushButton.

#### Pregunta 4 - Observa la imagen siguiente y determina las afirmaciones correctas sobre la ventana:
![DIN2-Pregunta4.png](https://i.postimg.cc/P5RXJhXC/DIN2-Pregunta4.png)

Seleccione una o más de una:
- [X] a. Tiene una QSlider.
- [ ] b. Tiene un QSpinBox.
- [ ] c. Tiene un QProgressBar.
- [X] d. Tiene un QDial.

#### Pregunta 5 - Dado el código siguiente, ¿qué fragmento de código es correcto para que la etiqueta refleje el valor del slider?
```python
class VentanaPrincipal(QMainWindow):
 
    def __init__(self):
        super().__init__() 
        self.setWindowTitle("Evaluación final - pregunta 5")
 
        layout_principal = QVBoxLayout()
        widget_principal = QWidget()
       
        widget_principal.setLayout(layout_principal)
 
        self.setCentralWidget(widget_principal)
 
        slider = QSlider()
        slider.setRange(0, 10)
 
        self.etiqueta = QLabel("0:")
 
        layout_principal.addWidget(slider)
        layout_principal.addWidget(self.etiqueta)
 
        slider.valueChanged.connect(self.valor_cambiado)
 
app = QApplication([]) 
ventana = VentanaPrincipal()
ventana.show() 
app.exec()
```

- [ ] a. 
```python
def valor_cambiado(self, valor):
    self.etiqueta.setText(valor)
```
- [ ] b.
```python
def valor_cambiado(self):
    self.etiqueta.setText(slider.value)
```
- [ ] c. 
```python
def valor_cambiado(self):
    self.etiqueta.setText(str(valor))
```
- [X] d. 
```python
def valor_cambiado(self, valor):
    self.etiqueta.setText(str(valor))
```

#### Pregunta 6 - En una aplicación deseamos que una misma funcionalidad se ejecute desde una opción en la barra de tareas y una opción en la barra de menús. ¿Cómo procederemos a su implementación?

- [ ] a. Definimos un atajo de teclado para las dos interfaces.
- [ ] b. No se puede implementar con una sola función, necesitamos una por interfaz.
- [X] c. Definimos una acción que añadimos al menú y a la barra de tareas. Conectamos la señal triggered a la función deseada.
- [ ] d. Definimos una función a la que conectamos los eventos de clic del botón en el menú y los de clic del botón en la barra de tareas.

#### Pregunta 7 - Dado el fragmento de código siguiente, ¿cuál es la línea de código que hace que la acción se ejecute desde el menú y desde la barra de herramientas?:
```python
    barra_menus = self.menuBar()
    menu = barra_menus.addMenu("&Menu")
    
    accion = QAction("Acción", self)
                    
    barra_herramientas = QToolBar("Barra de herramientas")

    menu.addAction(accion)
    barra_herramientas.addAction(accion)
    self.addToolBar(barra_herramientas)

def funcion(self):
    ...
```

- [ ] a. barra_herramientas.triggered.connect(self.funcion)
- [X] b. accion.triggered.connect(self.funcion)
- [ ] c. Barra_menus.triggered.connect(self.funcion)
- [ ] d. menu.triggered.connect(self.funcion)

#### Pregunta 8 - ¿Para qué sirve el código siguiente?
```python
def cargar_traductor(app):
    translator = QTranslator(app)
    translations = QLibraryInfo.location(QLibraryInfo.TranslationsPath)
    translator.load("qt_es", translations)
    app.installTranslator(translator)
```
- [ ] a. Traducirá toda la interfaz al español.
- [ ] b. Traducirá los diálogos predefinidos a la lengua definida en el sistema.
- [X] c. Traducirá los diálogos predefinidos al español.
- [ ] d. Instala un traductor en el sistema.

#### Pregunta 9 - Indica las afirmaciones correctas para crear un diálogo como el siguiente:
[![DIN2-Pregunta9.png](https://i.postimg.cc/DZy3PMy9/DIN2-Pregunta9.png)](https://postimg.cc/8F3Y1wv4)

- [ ] a. Creamos un QWidget al que añadimos los componentes requeridos.
- [X] b. Creamos un QMessageBox.critical al que añadimos los botones.
- [ ] c. Creamos un QMessageBox.error al que añadimos los botones.
- [ ] d. Creamos un QMainWindow.

#### Pregunta 10 - ¿Qué tipo de diálogo y de función utilizamos para pedir al usuario el nombre de archivo donde guardar unos cambios?

- [ ] a. QInputDialog.getFileName(...)
- [ ] b. QFileDialog.getOpenFileName(...)
- [ ] c. QDialog.getSaveFileName(...)
- [X] d. QFileDialog.getSaveFileName(...)