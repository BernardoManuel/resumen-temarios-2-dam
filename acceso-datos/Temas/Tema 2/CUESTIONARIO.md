# Cuestionario - Tema 2

#### Pregunta 1 - La carga del driver mediante Class.forName(), independientemente del SGBD al que se conecte...

- [ ] a. Debe hacerse una vez antes de cada operación en la base de datos.
- [X] **b. Solo con una vez basta, pero en versiones actuales de Java ya no es necesaria.**
- [ ] c. Está depreciada.
- [ ] d. No es necesaria, por compatibilidad de versiones antiguas de Java.

#### Pregunta 2 - Las referencias que utilizan los objetos se implementan en las Bases de datos relacionales como...

- [ ] a. Claves primarias.
- [X] **b. Claves ajenas.**
- [ ] c. Claves únicas.
- [ ] d. Índices.

#### Pregunta 3 - Mediante un driver JDBC podemos...

- [ ] a. Acceder solo a bases de datos relacionales.
- [ ] b. Solucionar el desfase objeto-relacional.
- [ ] c. Acceder a múltiples SGBD, aunque sea embebido.
- [X] **d. Acceder un único SGBD.**

#### Pregunta 4 - Para guardar los datos de la conexión desde nuestros programas, la mejor opción es...

- [ ] a. Programarlos directamente dentro de nuestras variables.
- [ ] b. Los datos de conexión nunca deben estar almacenados.
- [X] **c. Almacenarlos en un fichero de configuración, editable mediante un editor de texto.**
- [ ] d. Preguntárselos cada vez al usuario.

#### Pregunta 5 - Indica la sentencia que es falsa:

- [ ] a. El ResultSet es, a efectos prácticos, una tabla con datos.
- [ ] b. El ResultSet está fuertemente sobrecargado, con funciones de acceso a todos los tipos básicos.
- [ ] c. El ResultSet dispone de una función de recuento de columnas.
- [X] **d. El ResultSet, cuando se abre, dispone de un cursor que apunta a la primera fila con información.**

#### Pregunta 6 - Uno de los mayores problemas al acceder a los datos es...

- [ ] a. La última columna es ResultSet.getColumnCount()-1.
- [ ] b. La última fila es ResultSet.getRowCount()-1.
- [ ] c. La primera fila es la 1.
- [X] **d. La primera columna es la 1.**

#### Pregunta 7 - Para realizar una sentencia SQL de inserción, lo más adecuado es...

- [ ] a. Un executeQuery() de un Statement o PreparedStatement().
- [ ] b. Un execute() de un Statement.
- [ ] c. Un execute() de un PreparedStatement().
- [X] **d. Un executeUpdate() de un Statement o PreparedStatement().**

#### Pregunta 8 - Un ResultSet puede actualizarse...

- [X] **a. Cuando se abre con la opción UPDATABLE.**
- [ ] b. Siempre.
- [ ] c. Nunca.
- [ ] d. Solo cuando es UPDATABLE y SCROLLABE.

#### Pregunta 9 - Un ResultSet puede actualizarse...

- [ ] a. Siempre.
- [X] **b. Menos en caso de que el tipo de consulta que ha generado dicho ResultSet tenga una agrupación de tablas.**
- [ ] c. Nunca.
- [ ] d. Menos en caso de que el ResultSet tenga más de una fila.

#### Pregunta 10 - Cuando realizamos una consulta mediante un ResultSet, si queremos saber los tipos de datos que hemos seleccionado...

- [ ] a. No podemos acceder a los metadatos de la conexión.
- [X] **b. Debemos acceder a los metadatos del propio ResultSet, después de ejecutar la consulta.**
- [ ] c. Debemos acceder a los metadatos del propio ResultSet, antes de ejecutar la consulta.
- [ ] d. Podemos acceder a los metadatos de la conexión a partir de la base de datos.