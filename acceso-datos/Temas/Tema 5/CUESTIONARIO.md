# Cuestionario - Tema 5

#### Pregunta 1 - La finalidad de la programación por componentes es...

- [ ] a. Simplificar la complejidad de los problemas.
- [X] b. Reutilizar el código, ya que cada componente tiene un uso bien definido.
- [ ] c. Ocultar la información de los objetos, ya que sigue el principio de ocultación de la información.
- [ ] d. Facilitar la documentación del software.

#### Pregunta 2 - Completa la sentencia: «Las aplicaciones web están compuestas por un (1), que es la parte con la que interactúa el usuario y un (2) que es la parte donde se gestiona la lógica de negocio. El desarrollo de ambas partes se conoce como (3)».

- [X] a. (1) frontend, (2) backend, (3) fullstack
- [ ] b. (1) backend, (2) frontend, (3) fullstack
- [ ] c. (1) fullstack, (2) frontend, (3) backend
- [ ] d. (1) frontend, (2) fullstack, (3) backend

#### Pregunta 3 - La parte que acelera SpringBoot ante Spring es...

- [X] a. El arranque del desarrollo de la aplicación.
- [ ] b. La portabilidad de la aplicación.
- [ ] c. El despliegue de la aplicación.
- [ ] d. El mantenimiento del código de la aplicación.

#### Pregunta 4 - Los comportamientos que se detallan durante la creación de un proyecto con Spring Initializr se plasman en el fichero...

- [ ] a. db.config.
- [X] b. Pom.xml.
- [ ] c. User.cfg.
- [ ] d. Application.properties.

#### Pregunta 5 - ¿Cuál de las siguientes es una opción incorrecta para indicar que nuestro servidor necesita cubrir una petición POST a /entradas, y devolvernos un objeto JSON con la respuesta?

- [ ] a. Mapear un método en una clase @Controller, con una petición PostMapping.
- [ ] b. Mapear un método en una clase @Controller, con una petición @ResponseBody y una petición PostMapping.
- [X] c. Mapear un método como @RestController, con una petición RequestMapping de tipo POST.
- [ ] d. Mapear un método en una clase @Restcontroller, con una petición PostMapping.

#### Pregunta 6 - En un método que mapea una petición, queremos recoger datos que viene codificados como parámetros en el cuerpo de la petición; marcaremos dichos argumentos con la anotación...

- [X] a. @RequestBody.
- [ ] b. @PostParam.
- [ ] c. @PathVariable.
- [ ] d. @RequestParam.

#### Pregunta 7 - La clase que se encarga de recuperar la información de la base de datos es...

- [ ] a. El modelo.
- [ ] b. El servicio.
- [ ] c. El controlador.
- [X] d. El repositorio.

#### Pregunta 8 - Para transferir datos desde un controlador a la vista que presentará la información...

- [ ] a. No podemos transferir datos a la vista.
- [X] b. Utilizaremos un objeto Model, en el que añadiremos los datos con pares atributo-valor.
- [ ] c. Indicaremos a la vista el método del repositorio que debe invocar para recuperar la información.
- [ ] d. Haremos una llamada a los métodos de la vista, pasándole en un JSON los valores que haya que representar.

#### Pregunta 9 - Para crear páginas dinámicas, necesitaremos recurrir a...

- [ ] a. Hojas de estilos CSS junto con HTML.
- [ ] b. Un motor de plantillas como Thymeleaf.
- [X] c. Un lenguaje de marcas como XML o HTML junto con un motor de plantillas como Thymeleaf.
- [ ] d. Un lenguaje de programación como HTML.

#### Pregunta 10 - Indica si es V o F. Acerca del envío de colecciones a las plantillas de las vistas...

- [ ] a. Es problemático, puesto que se transfiere gran cantidad de información.
- [ ] b. Debemos procesar en el programa cada uno de los elementos individuales, antes de enviarlo a la vista.
- [ ] c. Puede realizarse, pero enviando dicha colección serializada como un string.
- [X] d. Recibidos los datos en la plantilla, se puede recorrer mediante bucles y generar tablas o estructuras similares.