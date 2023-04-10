# Cuestionario - Tema 6

#### Pregunta 1 - ¿Qué tipo de base de datos NoSQL es MongoDB?

- [ ] a. Base de datos XML.
- [ ] b. Base de datos en grafo.
- [X] c. Base de datos documental.
- [ ] d. Base de datos clave-valor.
- [ ] e. Base de datos orientada a objetos

#### Pregunta 2 - ¿A qué elemento del modelo relacional se podría equiparar un documento JSON en MongoDB?
- [ ] a. Atributos.
- [X] b. Registro.
- [ ] c. Base de datos.
- [ ] d. Tabla.

#### Pregunta 3 - Si incorporamos a una colección un documento sin identificador, ¿qué tipo de dato contendrá _id?

- [ ] a. NiumberLong
- [ ] b. NumberInt
- [ ] c. Object
- [ ] d. No necesita identificador, es un JSON.
- [X] e. ObjectId

#### Pregunta 4 - ¿Qué modificador utilizamos para eliminar un ítem arbitrario en un vector?

- [ ] a. $push
- [ ] b. $inc
- [X] c. $pull
- [ ] d. $pop
- [ ] e. $unset

#### Pregunta 5 - ¿Qué significa el documento de consulta {valor:{$lt:5}}?

- [X] a. Que el valor de la clave es menor que 5.
- [ ] b. Que el valor de la clave es menor o igual que 5.
- [ ] c. Que el valor de la clave es mayor que 5.
- [ ] d. Que el valor de la clave es mayor o igual que 5.
- [ ] e. El documento no está bien construido.

#### Pregunta 6 - ¿Para qué se utiliza el operador $size?

- [ ] a. Para obtener el número de documentos de una consulta.
- [ ] b. Para obtener el tamaño de un vector.
- [X] c. Para filtrar documentos por el tamaño de un vector.
- [ ] d. Para conocer el valor máximo de un vector de números.
- [ ] e. Para obtener el número de caracteres de una clave de tipo string.

#### Pregunta 7 - ¿Cómo accedemos a documentos incrustados o componentes de un vector dentro de una operación de filtrado? }

- [ ] a. Mediante corchetes [ ]
- [ ] b. Mediante corchetes [ ] y entrecomillado.
- [ ] c. Mediante el operador punto «.»
- [X] d. Mediante el operador punto «.» y entrecomillado.
- [ ] e. Mediante llaves {

#### Pregunta 8 - ¿Qué método de la interfaz MongoDatabase de driver de MongoDB nos devuelve una lista de strings con los nombres las diferentes colecciones de la base de datos?

- [ ] a. listCollections()
- [ ] b. getCollection()
- [ ] c. createCollection()
- [X] d. listCollectionNames()
- [ ] e. drop()

#### Pregunta 9 - ¿Cómo definimos en una clase anotada con @Document qué campo es el identificador?

- [X] a. Mediante la anotación @Id.
- [ ] b. Mediante la anotación @Document
- [ ] c. Mediante la anotación @_id
- [ ] d. Mediante la anotación @PrimaryKey
- [ ] e. Mediante la anotación @AutoWired

#### Pregunta 10 - ¿Desde qué componente de la arquitectura MVC de Spring realizamos las consultas a la base de datos?

- [ ] a. Controller.
- [X] b. Repository.
- [ ] c. Service.
- [ ] d. Serviceimpl.
- [ ] d. Model.