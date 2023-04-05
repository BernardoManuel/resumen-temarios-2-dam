# Cuestionario - Tema 2

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

- [ ] a. Aprendizaje, ayuda, eficacia, errores, satisfacción.
- [ ] b. Facilidad, recuerdo, eficacia, mensajes, satisfacción.
- [ ] c. Aprendizaje, recuerdo, velocidad, errores, experiencia.
- [X] d. Aprendizaje, recuerdo, eficacia, errores, satisfacción.

#### Pregunta 4 - ¿Cuál de las siguientes afirmaciones sobre la evaluación de la usabilidad es falsa?:

- [ ] a. Se pueden evaluar métricas cuantitativas o cualitativas.
- [X] b. Los participantes en la sesión son especialistas en usabilidad.
- [ ] c. El moderador de la sesión debe procurar mantenerse neutral.
- [ ] d. La sesión puede ser presencial o remota.

#### Pregunta 5 - ¿Cuál es el número recomendado de participantes en una sesión de evaluación de la usabilidad?:

- [ ] a. Dos.
- [X] b. Cinco
- [ ] c. Diez
- [ ] d. No hay límite, cuantos más mejor.

#### Pregunta 6 - De los siguientes tipos de documentos relacionados con las pautas de diseño de interfaces, ¿cuáles son desarrollados por empresas del sector como Microsoft o Apple, o por proyectos de software libre como KDE o GNOME?

- [ ] a. Estándares internacionales.
- [ ] b. Guías de producto.
- [ ] c. Estándares corporativos.
- [X] d. Guías de la plataforma.

#### Pregunta 7 - Los principios generales de diseño de la interfaz son:

- [ ] a. Contraste, jerarquía, proximidad y color.
- [X] b. Contraste, jerarquía, proximidad y alineación.
- [ ] c. Controles, jerarquía, proximidad y color.
- [ ] d. Contraste, jerarquía, texto y color.

#### Pregunta 8 - En un campo de un formulario el usuario debe escoger un color (y solo uno) de diez colores posibles. ¿Qué tipo de control sería el más adecuado para este campo?

- [X] a. Lista desplegable.
- [ ] b. Botones de radio.
- [ ] c. Casillas de verificación.
- [ ] d. Entrada de texto.

#### Pregunta 9 - Indica cuál de las siguientes afirmaciones sobre el uso del color es falsa:

- [X] a. El conjunto de colores que utilizamos en nuestro diseño debe ser amplio y variado.
- [ ] b. Nunca se debe referenciar un elemento de la interfaz por su color.
- [ ] c. El significado de los colores puede variar dependiendo del origen cultural de los usuarios.
- [ ] d. Los colores se deben utilizar para comunicar algo al usuario.

#### Pregunta 10 - Indica cuál de los siguientes mensajes de error al usuario está redactado de forma correcta:

- [ ] a. Error interno en la llamada al método. Código: 0x0012367AF
- [X] b. Se ha producido un error y la acción no ha podido completarse con éxito. Puede volver a intentarlo más tarde.
- [ ] c. Se ha producido un desbordamiento de la memoria interna de la aplicación y la aplicación se cerrará.
- [ ] d. La acción que usted ha iniciado ha provocado un error fatal en la aplicación.