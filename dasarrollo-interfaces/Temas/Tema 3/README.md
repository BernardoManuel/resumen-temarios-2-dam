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