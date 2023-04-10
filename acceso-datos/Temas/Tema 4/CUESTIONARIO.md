# Cuestionario - Tema 4

#### Pregunta 1 - La finalidad de los ORM es...

- [ ] a. Son la evolución de las bases de datos objeto-relacionales.
- [ ] b. Son la evolución natural de las bases de datos relacionales.
- [X] c. Parten de un diseño totalmente nuevo, una reimplementación del modelado de datos en el que se basan.
- [ ] d. Aplican sus herramientas para solventar el desfase objeto-relacional.

#### Pregunta 2 - Indica qué cosas SÍ permiten las bases de datos objeto-relacionales que NO permiten las bases de datos relacionales.
Seleccione una o más de una:
- [X] a. Herencia.
- [X] b. Valores multivaluados tipo conjunto.
- [ ] c. Claves ajenas. 
- [ ] d. Procedimientos almacenados.
- [X] e. Tipos estructurados. 
- [X] f. Valores multivaluados tipo lista.

#### Pregunta 3 - Indica cuáles de las siguientes afirmaciones son correctas: «Con respecto a MySQL, Postgres tiene la gran ventaja de que…»
Seleccione una o más de una:

- [X] a. Permite herencia.
- [X] b. Permite declarar tipos nuevos. 
- [ ] c. Permite restricciones de integridad referencial.
- [ ] d. Permite búsquedas indexadas por campos no clave. 

#### Pregunta 4 - La creación de nuevos tipos de datos en PostgreSQL permite...

- [ ] a. Evitar las comprobaciones con las cláusulas check.
- [ ] b. Definir valores multivaluados.
- [ ] c. PostgreSQL no permite definir tipos de datos.
- [X] d. Evitar la creación de tablas, ya que realmente se comportan como clases embebidas.

#### Pregunta 5 - Cuando insertamos un valor en una tabla que hereda de otra tabla...

- [ ] a. Deberemos proveer los valores de la tabla heredera.
- [ ] b. Deberemos proveer solo los datos de la tabla padre.
- [ ] c. Deberemos realizar dos operaciones de inserción.
- [X] d. Deberemos proveer valores de ambas tablas.

#### Pregunta 6 - Indica la sentencia que es falsa:

- [ ] a. ObjectDB contiene un ORM embebido que mapea los objetos en tablas.
- [X] b. ObjectDB permite guardar directamente los objetos en la base de datos.
- [ ] c. ObjectDB permite la creación de bases de datos relacionales.
- [ ] d. ObjectDB permite la conexión desde lenguajes como C++ o Java.

#### Pregunta 7 - Si queremos evitar el almacenaje de un atributo en una entidad anotada como @Entity, deberemos marcarlo como...

- [X] a. @Transient
- [ ] b. @Id
- [ ] c. No hace falta anotarlo en dicho campo.
- [ ] d. @Volatile

#### Pregunta 8 - En las anotaciones para gestionar las relaciones entre clases, debemos...

- [ ] a. Indicar mediante @JoinColumn la columna de la otra clase con la que se relaciona dicho atributo.
- [X] b. Indicar la cardinalidad y, opcionalmente, la gestión de las operaciones en cascada y de carga.
- [ ] c. Indicar simplemente la cardinalidad de la relación, ya que no existen columnas.
- [ ] d. Indicar mediante @transient la posibilidad de que un objeto no participe en la relación.

#### Pregunta 9 - Indica cuáles de las siguientes sentencias son verdaderas:

- [ ] a. Respecto del Query, el TypedQuery facilita la documentación de código y la lectura por parte del programador. 
- [X] b. TypedQuery evita las ligaduras dinámicas en los resultados de la ejecución de la consulta.
- [X] c. Query es relativamente cómodo para los borrados y actualizaciones. 
- [X] d. Podemos realizar las mismas consultas con TypedQuery que con Query.

#### Pregunta 10 - Indica cuáles de las siguientes son ventajas de ObjectDB con respecto a Hibernate:

- [X] a. Programas mucho más ligeros de ejecutar y distribuir.
- [X] b. Velocidad de ejecución.
- [X] c. Capacidad de almacenar datos, y gestor de manera embebida. 
- [ ] d. Mejor compatibilidad con otros lenguajes de programación de los datos almacenados. 