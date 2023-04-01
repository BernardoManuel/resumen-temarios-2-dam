# Tema 2 - Conectores a SGBD
## 1. El desfase objeto-relacional
Los modelos conceptuales nos ayudan a modelar una realidad compleja, y se basan en un proceso de abstracción de la realidad.

Cuando modelamos una base de datos, hacemos uso del modelo conceptual entidad-relación y, posteriormente, llevamos a cabo un proceso de paso a tablas y normalización de este modelo, para tener un modelos relacional de datos.

En la **POO**, intentamos representar la realidad mediante objetos y las relaciones entre ellos. Se trata de otro tipo de modelo conceptual, pero que pretende representar la misma realidad que el relacional.

Así pues, podemos representar un problema de dos formas:
- Modelo relacional de la base de datos.
- Modelo orientado a objetos de nuestras aplicaciones.

### 1.1. Representación de la información con el modelo orientado a objetos
Un objeto puede representar cualquier elemento conceptual: **entidades, procesos, acciones...**. Un objeto no representa únicamente las características o propiedades, sino que se centra también en los proceos que estos sufren. En términos del modelo orientado a objetos, decimos que un objeto son datos más operaciones o comportamientos.

### 1.2. Modelo relacional frente a modelo OO
El modelo orientado a objetos es un modelo dinámico, que se centra en los objetos y en los procesos que estos sufren, pero que no tiene en cuenta su persistencia.

Una forma de tener esta persistencia sería haciendo uso de un SGBD relacional. Aunque hay varias complicaciones:
- El modelo entidad-ralación se centra en los datos, mientras que el modelo orientado a objetos se centra en los objetos.
- Las operaciones que se realizan sobre ellos.

Además, cuando obtenemos los resultado de la consulta, nos encontramos con el problema de la conversión de resultados. Un consulta a una base de datos siempre devuelve un resultado en forma de tabla, por lo que habrá que transformar estas en estados de los objetos de la aplicación.

Hay que tener en cuentas algunas características de la **POO** que no son modeladas con el **modelo E/R**:
- Encapsulación
- Herencia y polimorfismo
- Tipos estructurados

## 2. Gestión de conectores
### 2.1. Protocolos de acceso a base de datos. JDBC
Hay dos principales normas de conexión para acceder a bases de datos:
- **ODBC (open data base connectivity)**: se trata de una API que permite añadir diferentes conectores a varias bases de datos relacionales basadas en SQL, de forma sencilla y transparente.
- **JDBC (java data base connectivity)**: define una API multiplataforma, que podemos utilizar en nuestros programas en Java por la conexiñon a los SGBDR

### 2.2. Arquitectura de JDBC
Las aplicaciones, para acceder a la base de datos, tendran que utilizar las interfaces de JDBC, de forma que para la aplicación sea totalmente transparente la implementación que realiza cada SGBD.

La apliación especificará un controlador JDBC mediante una URL al gestor de drivers, y este es quien se encarga de establecer de forma correcta las conexiones con la base de datos a través de los controladores.

Los controladores pueden ser de diferentes tipos:
- **Tipo I o controladores puente**
- **Tipo II o controladores con API parcialmente nativa o controladores nativos**
- **Tipo III o controladores Java via protocolo de red**
- **Tipo IV o Java puros 100%**

### 2.3. Base de datos embebidas
Se conocen como bases de datos en un único fichero, que almacenan tanto la estructura de la información como la propia información.

##### Ejemplos de bases de datos embebidas
- **SQLite**
- **H2**
- **ObjectDB**

## 3. Conexión a la base de datos

## 4. Metainformación de la base de datos
### 4.1. El objeto ResultSet
El SGDB nos proporciona la información en formato tabulado, que se le denomina **Resultset**

Un Resultset es una tabla resultado de una consulta, con tantas columnas como la sección realizada, y tantas filas como registros hayan satisfecho el From.

Para procesar el resultado de dicho Resultset, haremos uso del método next(), que tiene dos funciones:
- Devuelve **true** si quedan resultado por procesar.
- Adelanta el cursor hasta la siguiente fila.

Recuperamos los elementos de las columnas mediante métidos getTIPO(int pos):
- **TIPO** -> el tipo de datos que queremos recuperar.
- **pos** -> el número de la columna, empezando la primera por 1.

La mayoría de errores están contemplados por la excepción de Java **SQLException**.

El proceso de trabajar con **Resultset** será:
```java
Connection conn = DriveManager.getConnection(connectionUrl, config);

ResultSet rst = ... // Recuperamos datos de alguna manera

while (rst.next()) {
    // Procesado de una fila
    // accederemos a cada una de las columnas
}
```
### 4.2. Metadatos de la base de datos
Para consultar información de la base de datos, Java dispone de la interfaz **DatabaseMetaData**. Así accederemos a ella:

```java
Connection conn = DriveManager.getConnection(connectionUrl, config);

DatabaseMetaData dbmd = conn.getMetaData();
```

Algunos de sus métodos son los siguientes:
```java
String getDatabaseProductName() // El nombre del SGBD

String getDriverName() // El nombre del driver

String getURL() // El URL de conexión

String getUserName() // El nombre de usuario

ResultSet getTables(String catalogo, String esquema, String nombreTabla, String[] tipo)

ResultSet getColumns(String catalogo, String esquema, String nombreTabla, String nombreColumna)
```