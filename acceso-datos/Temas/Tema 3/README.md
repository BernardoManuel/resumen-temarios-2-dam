# Tema 3 - Herramientas de mapeo Objeto-Relacional. JPA
## 1. Herramientas de mapeo. Características
Las técnicas de los **ORM** se encargan de realizar una correspondencia entre los datos primitivos de ambos modelos y sus estructuras: entre tablas y objetos, campos y atributos, sus identificadores y sus claves principales.

Del mismo modo que la definición de los datos, necesitaremos un mecanismo de persistencia de los objetos, de manera que los objetos puedan ser "rastreados" en memoria, y que los cambios en estos se vean reflejados directametne en la base de datos.

Este tipo de herramientas aportan, entre otras, las siguientes características:
- Disminuyen el tiempo de desarrollo.
- Permiten realizar una abstracción del SGBD subyacente, en parte gracias a JDBC.
- Manipularemos solo objetos en nuestro programa, olviándonos del concepto de tabla, fila y columna.

Hay que distinguir tres componentes:
- **Técnicas de mapeo**: Consistentes en un sistema para expresar la correspondencia entre las clases y el esquema de la base de datos.
- **Un lenguaje de consulta orientado a objetos**: Realmente accederá a las tablas, permitiendo salvar el desfase objeto-relacional.
- **Técnicas de sincronización**: Supone el núclero funcional para posibilitar la sincronización de los objetos persistentes de la aplicación con la base de datos.

#### Técnicas de mapeo
Destacamos dos técnicas de mapeo objeto-relacional:
- Aquellas que incrustan las definiciones dentro del código de las clases y están vinculadas como las macros de C++ o las anotaciones de PHP y Java.
- Aquellas que guardan las definiciones en ficheros independientes del código, generalmente en XML o JSON

#### Lenguaje de consulta
En las herramientas de mapeo, existe lo que se denomina **OQL (object query language)**. Este lenguaje tiene muchas similitudes con SQL, pero una gran diferencia, que se basa en objetos y no en tablas.

#### Técnicas de sincronización
La sincronización es un ode los aspectos más delicados de las herramientas ORM. Consiste en procesos complejos que implican técnicas para las siguientes funcionalidades:
- Descubrir los cambios que sufren los objetos durante su ciclo de vida para poder almacenarlos.
- Crear e iniciar nuevas instancias de objetos a partir de los datos guardados en la base de datos.
- A partir de los objetos, extraer su información para reflejar en las tablas e la base de datos.

## 1.2. Hibernate
Hibernate es un ***framework* ORM** para Java, que facilita el mapeo de atributos entre una base de datos relacional y el modelo de objetos de nuestra aplicación mediante **ficheros XML** o anotaciones en los **beans** de las entidades.

Cuando una aplicación crea objetos, estos están almacenados en memora, con la volatilidad que esto implica. Dichos objetos se denominan transitorios o **transient**. Con Hibernate, los objetos que tenemos que persistir se "rastrean" en lo que se conoce como una sesión **(Session)**, creada a partir de una **SessionFactory**, de acuerdo con al configuración proporcionada. También se proporciona la gestión de conexiones y transacciones.

Como es lógico, los objetos volátiles podrán persistirse, pasando de unos estados a otros, como veremos más adelante, con métodos como **save()** o **persist()**. También se proporciona una interfaz **query**, para lanzar consultas en HQL (**Hibernate query language**, su versión del **OQL**).

## 2. Configuración e instalación de Hibernate
### 2.1. Proyecto con Hibernate y MySQL
Para trabajar con MySQL e Hibernate, añadiremos al fichero correspondiente:

- **Maven (en POM.xml)** 
```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>8.0.27</version>
</dependency>
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>5.6.3.Final</version>
</dependency>
```
- **Gradle (en build.gradle)**
```gradle
dependencies {
    implementation group: 'mysql', name: 'mysql-connector-java', version: '8.0.27'
    implementation group: 'org.hibernate', name: 'hibernate-core', version: '5.6.3.Final'    
}
```
### 2.2. Estructura de un proyecto de Hibernate
#### Bean (Clases)
Estas clases se denominaban **POJO (plain old Java object)**. Los POJO son objetos comunes, que no pueden heredar ni implementar clases ni interfaces preestablecidas de Java ni tener anotaciones.

Como una extensión de los POJO aparecen los **beans**, cápsula o granos, que son más restrictivos, y tienen las siguietes características:
- Hacer sus atributos privados.
- Implementar las interfaz **serializable**.
- Acceder a los campos mediante **getters** y **setters** públicos.
- Implementar un constuctor por defecto.

#### Fichero de mapeado
Una vez definidas las entidades, necesitaremos el fichero de mapeado para cada **bean**. En dicho mapeado se indica a qué tabla de la base de datos se guardará dicho **bean**, así como con qué columna y tipo de datos debe coincidir cada atributa de este.

Deberá existir pues un fichero de mapeado por cada **bean**. Si el **bean** se llama, por ejemplo, **Empleado.java**, el **bean** asociado se llamará **Empleado.hbm.xml** (**hbm** por **Hibernate mapping**).

### 2.3. Configuración del proyecto
Básicamente, tenemos que realizar dos operaciones, configurar el proyecto y cargar dicha configuración al ejecutarlo, configuración que pasamos a desarrollar.

#### Fichero Hibernate.cfg.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
 <!DOCTYPE hibernate-configuration PUBLIC "-//Hibernate/Hibernate Configuration DTD 3.0//EN" "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
 <hibernate-configuration>
    <session-factory>
        
    <!-- Propiedades de la conexión -->

	<!-- Driver JDBC -->
    <property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>

    <!-- Conexión. Añadir ?createDatabaseIfNotExist=true para crear la BBDD  -->
    <property name="connection.url">jdbc:mysql://localhost:3308/nombreBBDD</property>

    <!-- Usuario y password de la BBDD -->
    <property name="connection.username">root</property>
    <property name="connection.password">root</property>

    <!--  dialecto dentro del conector. Importante para claves ajenas-->
    <property name="dialect">org.hibernate.dialect.MySQL5InnoDBDialect</property>
    
    <!-- Configuraciones adicionales -->

    <!-- JDBC connection pool Conexiones concurrentes -->
    <property name="connection.pool_size">5</property>

    <!-- Una sesion de trabajo por Thread-->
    <property name="current_session_context_class">thread</property>


    <!-- Informa de las operaciones "reales" SQL. Interesante en desarrollo -->
    <property name="show_sql">true</property>
    
    <!-- Mantenimiento de la BBDD -->

    <property name="hbm2ddl.auto">update</property>
    
    <!-- opciones de hbm2dll:
    create : Borra y crea SIEMPRE la base de datos
    update : Mantiene los datos, actulizando la estructura de la BBDD. Util en     producción.
    create-drop : Crea todo, y al finalizar el programa lo borra
    validate: comprueba que las especificaciones del mapeo son validas con el diseño relacional de la BBDD
    -->
    
    <!-- Ficheros de mapeo. Pueden combinarse-->

    <!-- Mapeo DENTRO DE LA CLASE -->
    <mapping class="paquete.clase1" />
    <mapping class="paquete.clase2" />

    <!-- Mapeo EN FICHERO EXTERNO -->
    <mapping resource="clase1.hbm.xml" />
    <mapping resource="clase2.hbm.xml" />
    </session-factory>
</hibernate-configuration>
```
Hay que tener en cuenta las partes del fichero:
- Configuración de la base de datos.
- Para mostrar las consultas SQL se recomienda tenerla a **true** para ver como se produce en realidad el mapeo de los objetos a consultas SQL.
- La opción **hbm2ddl** es muy potente, ya que, si partimos solo del modelo orientado a objetos, Hibernate creará la base de datos por nosotros.
- Los ficheros de mapeo XML deben estar junto a las clases Java, en el mismo paquete.
- Los mapeos dentro de las clases hacen referencia a los propios beans.

#### Carga de la sesión y la configuración. Clase HibernateUtil.java
Para cargar la configuración y obtener un objeto de tipo **Session**, deberemos crear una clase que realice esta tarea. Esta clase habitualmente se llama **HibernateUtil.java**, y el código es el siguiente:

```java
public class HibernateUtil {

    private static final SessionFactory sessionFactory;

    // Código estático. Sólo se ejecuta una vez, como un Singleton
    static {
        try {
            // Creamos es SessionFactory desde el fichero hibernate.cfg.xml 
            sessionFactory = new Configuration()
                .configure(new File("hibernate.cfg.xml")).buildSessionFactory();    
        } catch (Throwable ex) {
            System.err.println("Error en la inicialización.  " + ex);
            throw new ExceptionInInitializerError(ex);
        }
    }

    public static SessionFactory getSessionFactory() {
        return sessionFactory;
    }
}
```

En dicha clase se crea una instancia única **(Singleton)** de la factoria de sesiones, a partir del fichero de configuración indicado. A partir del **SessionFactory** ya podemos empezar a crear sesiones, y transacciones en ellas.

## 3. Mapeo de objetos. JPA
Ahora tenemso que empezar a mapear entidades, y necesitamos tener el modelo relacional que vamos a mapear. Partiremos del siguiente ejemplo con la entidad *"Pelis"* en una base de datos *"Cine"*.
```sql
CREATE TABLE 'Peli' {
    'idPeli' int(11) NOT NULL AUTO_INCREMENT,
    'titulo' varchar(45) NOT NULL,
    'anyo' varchar(45) NOT NULL,
    'director' varchar(45) NOT NULL,
    PRIMARY KEY ('idPeli')
} ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

A continuación, vamos a crear el bean que contendrá las Pelis. Lo crearemos en un nuevo paquete al que llamaremos **Model**, ya que contendrá los beans que conforman el modelo de datos. Dicha clase quedará:
```java
package Model;
import java.io.Serializable;

public class Peli implements Serializable {
    private Long idPeli;
    private String titulo;
    private int anyo;
    private String elDirector;

    public Peli() {
    }

    public Peli(String titulo, int anyo, String elDirector) {
        this.titulo = titulo;
        this.anyo = anyo;
        this.elDirector = elDirector;
    }
}
```
### 3.1. Mapeo de entidades. Archivo de mapeo
Para poder persistir este tipo de objeto, debemos crear un archivo externo a la clase, de extensión **.hbm.xml** y con el mismop nombre de la clase. La localización del archivo ni importa a priori, aunque es buena idea tener las clases del modelo por un lado y los archivos de mapeo por otro. Así pues, crearemos un paquete **ORM** y dentro de él crearemos el archivo **Peli.hbm.xml**.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN" "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="Model.Peli" table="Peli" >
        <id column="idPeli" name="idPeli" type="long">
            <generator class="native"></generator>
         </id>
        <property name="titulo" type="string"/>
        <property name="anyo" />
        <property column="director" name="elDirector" />
    </class>
</hibernate-mapping>
```

### 3.2. Mapeo de entidades. Anotaciones
Podemos realizar dentro de la clase Java las anotaciones pertinentens, mediante el estándar **JPA (Java persistence API)**. La ventaja es clara: solo manipularemos un fichero, aunque, por el contrario si deseamos dejar de persistir una clase, nos quedará "emborronada" con las anotaciones.
```java
@Entity
@Table(name="Peli")
public class Peli_Anotada implements Serializable{
    
    @Id
    @GeneratedValue(strategy=GenerationType.IDENTITY)
    private Long idPeli;
    
    @Column
    private String titulo;
    
    @Column
    private int anyo;
    
    @Column(name="director")
    private String elDirector;
    
. . . . //  Obviamos el resto de la clase
```