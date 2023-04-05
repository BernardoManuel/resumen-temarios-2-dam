# Tema 3 - Generación de interfaces a partir de lenguajes de marcas
## 1. Lenguajes de marcas para la generación de interfaces de usuario
Casi todo los lenguajes están basado en XML y utilizan la sintaxis de elementos y atributos para definir la estructura y los componentes de la interfaz.
### 1.1. Ventajas de los lenguajes de marcas para la generación de 
La utilización de este tipo de lenguajes ofrece las ventajas siguientes sobre la creación utilizando un leguaje de programación:
- Mejora la separación de responsabilidades en la aplicación, diferencia claramente la intrefaz de usuario del resto de capas.
- Son lenguajes fácilmente entendibles.
- Su estructura jerárquica en forma de árbol e similar a la estructura visual que se establece entre los componentes de la interfaz.
- Permiten reutilizar el mismo diseño de intrefaz en diferentes plataformas o con distintos lenguajes de programación.

### 1.2. Lenguajes de marcas más utilizados para la generación de interfaces
Alguno de los lenguajes de marcas más populares para la creació nde interfaces gráficas de usuario son:
- Qt UI
- FXML
- XAML
- Gtk UI
- Android XML
- Storyboards

## 2. Herramientas de diseño de interfaces basadas en lenguaje de marcas
A la hora de utilizar un lenguaje de marcas para la generación de la interfaz de usuario de una aplicación, normalmente se presentan dos opciones:
- Utilizar directamente el lenguaje de marcas para definir distintos elementos que formarán la interfaz y sus propiedades con la ayuda de algún editor de texto o código.
- Hacer uso de una herramienta de diseño de tipo WYSIWYG, que nos permitirá definir la interfaz de usuario en un entorno visual.

### 2.1. Distribución de los componentes (layout)
Qt Designer permite definir de forma cómoda y rápida la disposición de los componentes, pudiendo aplicar las diferentes opciones de layout de Qt.

#### Layout principal
Es importante que definamos un layout principal para nuestro formulario, el cual establezca la fomra que se organizan los componentes de la interfaz en el nivel más alto.

Una de las grandes ventajas de una herramienta visual como Qt Designer es que podremos ver en la propia herramienta el resultado de aplicar el layout, lo que incluso nos permitirá redimensionar la ventana para comprobar si el comportamiento de la interfaz se ajusta a lo deseado.

#### Layouts anidados
Es muy probable que el diseño de nuestra interfaz precise anidar en su interior otros layouts para conseguir el posicionamiento adecuado de los componentes.

#### Otras opciones para la distribución de componentes
Aunque los diferentes layouts disponibles en Qt y la posibilidad de combinarlos nos aportan gran flexibilidad, normalmente no es suficiente para conseguir diseños de interfaz complejos.

Una de estas posibilidades son los espaciadores, que permiten introducir un espacio entre controles dentro de un layout.

### 2.2. Conexión de señales a ranuras
Qt Designer también permite asociar una señal de un componente a alguna de las ranuras de otro componente.

### 2.3. Previsualizar el resultado
Durante el proceso de creación de nuestro formulario con Qt Designer podemos obtener una vista previa del resultado, mucho más cercana a cómo se verá realmente nuestro diseño cuando se ejecuta la aplicación.

## 3. Uso de la interfaz en una aplicación
Gran parte de las tecnologías de interfaz de usuario actuales disponen de un lenguaje de marcas para definir el diseño de las ventanas sin hacer uso de un lenguaje de programación. Y también suelen contar con una herramienta visual de diseño que facilita la tarea de definición de la interfaz y que genear de forma automática un documento en el lenguaje de marcas correspondiente.

### 3.1. Formato Qt UI
La herramienta Qt Designer almacena el diseño realiza en un formato conocido como Qt UI, asignado a los ficheros de diseño la extensión *.ui. Este formato está baso en XML y, aunque no está concebido para ser utilizado por los desarrolladores fuera de la herramienta de diseño, es posible manipularlo directamente en algún editor de texto.

### 3.2. Incluir la interfaz en una aplicación
A la hora de utilizar nuestro fichero de diseño de la interfaz desde una aplicación, se presentan dos opciones diferentes:
- Utilizar una herramienta de generación de código que, a partir del fichero de descripción de la interfaz, genere el código correspondiente en el lenguaje de programación adecuado.
- Cargar el fichero de descripción de la interfaz directamente desde el código de nuestra aplicación, generándose en tiempo de ejecución el código correspondiente.

#### Generar código Python a partir del fichero UI
Se utiliza la erramienta *User Interface Compiler (uic)*, incluida en la instalación de Qt. El comando siguiente generará el fichero de código Python ventana.py a partir del fichero de diseño de interfaz ventana.ui.
```shell
uic -g python ventana.ui -o ventana.py
```
Para poder utilizar esta clase en nuestro programa principal, lo primero que debemos hacer es la importación correspondiente.
```python
from ventana import Ui_MainWindow
```
Deberemos crear una nueva clase que herede tanto de la clase generada como de la clase del objeto principal de nuestro diseño. En el constructor de esta nueva clase, se llamará al métido setupUi para que se generen todos los objetos de la interfaz.
```python
class VentanaPrincipal(QMainWindow, Ui_MainWindow):
    def __init__(self):
        super(VentanaPrincipal, self).__init__()
        self.setupUi(self)
```

#### Cargar el fichero UI desde Python
Tenemos otra alternativa para utilizar el diseño realizado en Qt Designer, que consiste en cargar directamente el fichero UI desde la aplicación Python. Para ello, utilizaremos la clase QUiLoader incluida en el módulo QtUiTools.
```python
from PySide6.QtUiTools import QUiLoader
```
Esta clase ofrece el método load, que recibirá como parámetro el fichero UI y generará la ventana.
```python
loader = QUiLoader()
window = loader.load("ventana.ui", None)
window.show()
```

### 3.3. Manipulación de los objetos generados
Podemos acceder a cualquiera de los objetos generados para modificar alguna de sus propiedades o asociar una ranura a una de sus señales. Para ello, simplemente utilizaremos el nombre que hayamos dado al objeto en Qt Designer y la misma sintaxis que vimos en la unidad anterior.
```python
window.cerrar_boton.setText("Close")
```