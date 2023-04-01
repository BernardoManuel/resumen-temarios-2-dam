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
### 3.1. Establecimiento de la conexión
Para crear una conexión a la base de datos, una vez cargado el driver, deberemos especificar dos conceptos básicos, junto con algunas opciones más:
- **Host**
- **Puerto**

Toda esta información la recogeremos en lo que se denomina **"cadena de conexión"**.

En Java, la clase necesaria para gestionar el driver es **java.sql.DriverManager**. Los drivers intentan cargar del sistema al leer la propiedad **jdbc.drivers**, pero podemos indicar que está cargado mediante la instrucción:
```java
Class.forName("com.mysql.cj.jdbc.Driver");
```

La clase que centralizará todas la operaciones con la base de datos es **java.sql.Connection**, y la debemos obtener del **DriverManager** con cualquiera de los tres métodos estáticos que tiene:
```java
static Connection getConnection(String url)

static Connection getConnection(String url, Properties info)

static Connection getConnection(String url, String user, String password)

```
### 3.2. Parámetros de la conexión
En la cadena de conexión, de manera obligatoria tenemos que indicar el host y el puerto, como hemos visto.

```java
String connectionUrl = "jdbc:mysql://localhost:3308/BDJuegos";
Connection conn = DriverManager.getConnection(connectionUrl, "root", "toor");
```
Permite conectarse al servidore de la máquina local en el puerto 3308 y marcar como activa la base de datos denominada **BDJuegos**.

### 3.3. Organizar y centralizar la conexión
Nuestra apliación va a conectarse a una base de datos. A dicha base de datos podemos hacerle muchas peticiones, y, si estamos implementando una aplicación multihilo, este número de peticiones puede incrementarse mucho. Por este motivo, debemos tener controlado dónde y cuándo se crean y se cierran las conexiones. Una buena idea es crear una cñase que encapsule todos estos procesos. El esqueleto de dicha clase sería el siguiente:

```java
public class ConexionBD {
    private Connection conn = null;
    
    private void connect() {
        // Realizar la conexión.
    }

    public void disconnect() {
        if (conn != null) {
            conn.close();
        }
    }

    public Connection getConexion() {
        if (conn == null) {
            this.connect();
        }

        return this.conn;
    }
}
```

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
## 5. Consultas a la base de datos
### 5.1. Operaciones sobre la base de datos
Para crear las sentencias, tenemos las interfaces **Statement** y **PreparedStatement**, obtenidas a partid de los objetos **Connection**. Posteriormente, las ejecutaremos con **execute()** o **executeQuery()**.

### 5.2. Tipos de sentencias
##### Sentencias fijas
Son consultas que son "constantes", es decir, que no dependen de ningún argumento. Son las más simples. Se crea el **Statement** y se lanza la consulta
```java
String sql = "select * from Persona;";
Statement stm = conn.createStatement();

ResultSet rst = stm.executeQuery(sql);
while (rst.next()) {
    System.out.println(rst.getString("nombre") + " " + rst.getString("apellidos") + " " + rst.getInt("edad"));
}
```
Otra consulta podría ser una actualización o inserción:

```java
String sql = "insert into Persona(nombre, apellidos, edad) values ('Diego', 'Gómez Moreno', 23);";
Statement stm = conn.createStatement();

int filas = stm.executeUpdate(sql);
if (filas == 1) {
    System.out.println("Inserción realizada con éxito.");
} else {
    System.out.println("Error en la inserción.");
}
```
##### Sentencias variables

## 6. Enlaces Web
- [Información de los métodos y propiedades del objeto ResultSet.](https://docs.oracle.com/javase/7/docs/api/java/sql/ResultSet.html)
- [Consulta sobre el objeto DataBaseMetadata, sobre todo con la información de los ResultSet y los campos que contiene al consultar la información de tablas, campos y claves.](https://docs.oracle.com/javase/7/docs/api/java/sql/DatabaseMetaData.html)
- [Ejemplos prácticos para patrones de diseño Java.](https://refactoring.guru/es/design-patterns/singleton)

## 7. Respuestas cuestionario
- [Cuestionario](./CUESTIONARIO.md)