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

Cuando una aplicación crea objetos, estos están almacenados en memoria, con la volatilidad que esto implica. Dichos objetos se denominan transitorios o **transient**. Con Hibernate, los objetos que tenemos que persistir se "rastrean" en lo que se conoce como una sesión **(Session)**, creada a partir de una **SessionFactory**, de acuerdo con al configuración proporcionada. También se proporciona la gestión de conexiones y transacciones.

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

### 3.3. Componentes (@Embedded)
Un componente se da cuando en Java tenemos una clase como tal, pero dicha clase solo tiene existencia o sentido dentro de otra clase, sin tener sentido en ninguna otra clase. Un ejemplo podría ser la puntuación en IMDB de una película, que solo tiene sentido que se guarde para dicha película.

Así pues, mediante anotaciones crearemos una clase y la marcaremos como **@Embeddable** y, dentro de nuestra clase **Peli**, marcamos dicho campo componente como **@Embedded**:

- **Clase IMDB**
```java
Embeddable
public class IMDB implements Serializable {
    @Column
    private String url;

    @Column
    private double nota;

    @Column
    private long votos;
}
```

- **Dentro de la clase Peli**
```java
@Embedded
private IMDB imdb;

public Peli_Anotada(String titulo, int anyo, String elDirector, IMDB imdb) {
    this.titulo = titulo;
    this.anyo = anyo;
    this.elDirector = elDirector;
    this.imdb = imdb;
}
```

## 4. Mapeo de relaciones en Hibernate
El concepto de direccionalidad de las relaciones pueden ser:
- **Unidireccional**: Una relación es unidireccional cuando accederemos al objeto relacionado a partir de otro objeto.
- **Bidireccional**: Cuando los elementos relacionados suelen tener la misma "ponderación" o entidad.

### 4.1. Relaciones uno a uno (@OneToOne)
#### Relación 1:1 Unidireccional
En la tabla de Grupo existe un campo "tutor", del que parte una FK a Profesor. La anotación **referencedColumnName** es opcional.
- **Tabla Grupo**
```java
@Entity
@Table(name="Grupo")
public class Grupo {
    @Id
    @GeneratedValue(statregy = GenerationType.IDENTITY)
    @Column(name="idGrupo")
    private Long idGrupo;
    ...
    @OneToOne(cascade=CascadeType.ALL)
    @JoinColumn(name = "tutor", referencedColumnName = "idProfe")
    private Profesor elTutor;
    ...
}
```
- **Tabla Profesor**
```java
@Entity
@Table(name="Profesor")
public class Profesor {
    @Id
    @GeneratedValue(statregy = GenerationType.IDENTITY)
    @Column(name="idProfe")
    private Long idProfe;
    ...
    @OneToOne(mappedBy = "elProfe")
    private Grupo elGrupo;
    ...
}
```
Hemos indicado con este nuevo campo que el propietario de la relación es Grupo.

Algunas opciones de Cascade:
- **CascadeType.ALL**: se aplican todos los tipos de cascada.
- **CascadeType.PERSIST**: las operaciones de guardado de las entidades propietarias se propagrán a las entidades relacionadas. Solo se aplica si las entidades se guardan con el método **persist()** en vez de **save()**. Para utilizar **save()** con seguridad, remplaza **CascadeType.PERSIST** por **CascadeType.SAVE_UPDATE**.
- El resto de opciones, **CascadeType.MERGE**, **CascadeType.REMOVE**, **CascadeType.REFRESH** y **CascadeType.DETACH** realizan lo que su nombre indica, unir, eliminar, refrescar y sacar la unidad de persistencia.

### 4.2. Relaciones uno a muchos (@OneToMany/@ManyToOne)
#### Relación unidireccional
La unidireccionalidad debe aparecer **solo** en la tabla propietaria. Así pues, en la clase Libro indicaremos que contiene un Autor. En la tabla Autor no se indicará nada.

En el campo tipo Autor se mapea con la anotación **@ManyToOne** a la columna **idAutor**. Se indica que cualquier guardado en Libro provoca que se persista también el Autor (**CascadeType=PERSIST**). También se anota que la **FK** tendrá como nombre **FK_LIB_AUT**.

#### Relación uno a muchos bidireccional
El mapeado de la relación bidireccional nos va a permitir acceder a la información relacionada en ambas direcciones. Para ello añadiremos en Autor un conjunto (**Set**) de todos los libros que ha escrito.
- **Tabla Libro**
```java
@Entity
@Table(name="Libro")
public class Libro implements Serializable {
    @Id
    @GeneratedValue(statregy = GenerationType.IDENTITY)
    @Column(name="idLibro")
    private Long idLibro;
    ...
    @ManyToOne(cascade=CascadeType.PERSIST)
    @JoinColumn(name = "idAutor", foreignKey = @ForeignKey(name = "FK_LIB_AUT"))
    private Autor elAutor;
}
```
- **Tabla Autor**
```java
@Entity
@Table(name="Autor")
public class Autor {
    @OneToMany(mappedBy = "elAutor", cascade=CascadeType.PERSIST, fetch = FetchType.LAZY)
    private Set<Libro> losLibros;
}
```

Aparece un nuevo marcador **fetch**, cuyo comportamiento determinará el momento en que se realiza la carga de los datos de las colecciones, y cuyos valores pueden ser:
- **FetchType.EAGER**: Tenemos todos los datos al momento.
- **FetchType.LAZY**: Los datos se cargan cuando hacen falta.

### 4.3. Relaciones muchos a muchos (@ManyToMany)
Dentro de las relaciones binarias, podemos encontrar dos posibilidades: relaciones que simplemente indican la relación o relaciones que, aparte de indicarla, añaden nuevos atributos a esta.

Las clases **Modulo** y **Profesor** quedan como a continuación:
- **Tabla Profesor**
```java
@ManyToMany(cascade=CascadeType.PERSIST, fetch=FetchType.LAZY)
@JoinTable(name="Docencia",
            joinColumns = {@JoinColumn(name = "idProfesor",
                foreignKey = @ForeignKey(name = "FK_DOC_PROF"))},
            inverseJoinColumns = {@JoinColumn(name = "idModulo",
                foreignKey = @ForeignKey(name = "FK_DOC_MOD"))});
private Set<Modulo> losModulos = new HashSet<>();

public void addModulo(Modulo m) {
    if (!this.losModulos.contains(m)) {
        losModulos.add(m);
    }
    m.addProfesor(this);
}
```
- **Tabla Módulo**
```java
@ManyToMany(cascade=CascadeType.PERSIST, fetch=FetchType.LAZY, mappedBy = "losModulos")
private Set<Profesor> losProfesores = new HashSet<>();

public void addProfesor(Profesor p) {
    if (!this.losProfesores.contains(p)) {
        losProfesores.add(m);
    }
    p.addModulo(this);
}
```

Fíjate en lo siguiente:
- En ambas clases, el mapeo es **@ManyToMany**, conteniendo un **Set** de objetos de la otra clase, idicando las operaciones en cascada (**cascade**) y la carga de los objetos relacionados (**fetch**).
- En la clase propietaria (**Profesor**) se inicia la relación, enlazando con una tabla **Docencia** siguiendo la FK donde se va a enlazar (**joincolumns**) con el campo **idProfe** (**@JoinColumn**).
- Se mapea desde la tabla **Docencia** hasta la entidad de origen **Modulo** de manera inversa, esto se consigue con **inverseJoinColumns**, enlazando desde el campo **idModulo** (**@JoinColumn**).
- En la clase relacionada (**Modulo**), que no es la propietaria, simplemente le indicamos que la propietaria es **Profesor**, mediante **mappedBy="losModulos"**.

## 5. Consultas con HQL
El lenguaje **HQL (Hibernate query languaje)** nació con la finalida de salvar de neuvo el modelo relacional, ya que es un superconjunto de SQL. La primera consideración es que su funcionalidad es la de recuperar objetos de la base de datos, no tablas, como hacíamos en el lenguaje SQL mediante los **ResultSet**.

Las consultas con HQL se realizarán a partir de una interfaz **Query**, que será el modo donde especificaremos qué queremos recuperar. Opcionalmente podemos añadir a la consulta los parámetros necesarios para su ejecución, para evitar consultas **hard-coded**.

## 5.1. Recuperación de objetos simples
Estas consultas son las que permiten recueperar un objeto o colección de objetos desde las bases de datos.
```java
Query q = laSesion.createQuery("select a from Alumno a"); // Todos

Query q = laSesion.createQuery("select a from Alumno a where a.idAlumno=1"); // Un alumno

List<Alumno> alumnos = q.list();
for (Alumno a : alumnos) {
    System.out.println(a);
}
```
Este segundo ejemplo permite obtener solo uno, con lo que se indica en la cláusula **where**.
```java
Alumno a = (Alumno)q.uniqueResult();
Query<Alumno> q = laSesion.createQuery("select a from Alumno a where a.idAlumno=1", Alumno.class);
Alumno a = q.uniqueResult();
```

## 5.2. Consultas mixtas
Entendemos por consultas mixtas aquellas que no devuelven objetos enteros como tales. Estas consultas devolverán, o bien parte de objetos, o bien un objeto más algo más. La novedad es que el resultado será un array de objetos (**Object[]**), y, por lo tanto, deberemos ser muy cuidadosos con el tipo de cada celda, así como con el tamaño de dicho array, ya que estará fuertemente ligado a la propia consulta.

```java
Query q = laSesion.createQuery("select a.nombre, a.edad from Alumno a");
List<Object[]> losAlumnos = q.list();
for (Object[] alu : losAlumnos) {
    System.out.println("El alumno " + alu[0] + " tiene " + alu[1] + " años.");
}
```

## 5.3. Los múltiples Select en cascada
Cuando realicemos una consulta a una clase que contiene objetos enlazados, habitualmente por relaciones, está consulta generará un consulta anidada por cada objeto enlazado, para cargar su contenido. Este es el comportamiento con una consulta ansiosa (**Eager**). Habrá que valorar el cargarlas en diferido o **Lazy**.

## 5.4. Consultas sobre colecciones
Vamos a consultar el nombre de los alumnos y cuántos exámenes ha realizado cada uno. Dicha inforamción está en el set de exámenes, por lo que necesitaremos manipular dicha colección:
```java
Query q = laSesion.createQuery("select a.nombre, size(a.losExamenes) from Alumno a");
List<Object[]> losAlumnos = q.list();
for (Object[] alu : losAlumnos) {
    System.out.println("El alumno " + alu[0] + " ha hecho " + alu[1] + " examenes.");
}
```

Como puede apreciarse, hemos aplicado la función **size()** a la colección para ver su tamaño. Podemos aplicar, por tanto:
- **Size(colección)**: recuperar el tamaño.
- **Colección is empty | colección is not empty**: para determinar si está vacía.
- Pueden combinarse los operadores **in**, **all** mediante el operador **elements(colección)**.

## 5.5. Consultas con parámetros. Consultas nominales
La gestión de parámetros se realiza del mismo mod que las sentencias (**Statements**) y pueden realizarse mediante parámetros posicionales o nominales.

#### Parámetros posicionales
Para poder asignarle el valor al parámetro, Hibernate nos ofrece una amplia batería de métodos sobrecargados, **setParameter(posicion, valor)**, donde valor puede tomar desde los tipos básicos hasta Objetos.

#### Parámetros nominales
Los parámetros indican con **nombreParametro** y se asignarán con **setParameter("nombreParametro", valor)**, indicando el nombre del parámetro.

#### Consultas nominales
Fuera de la definición de la clase se creará una colección **@NamedQueries**, que contendrá un array de elementos **@NamedQuery(nombre="", definicion="")**.

Para invocarlos, en vez de crear un objeto **Query**, lo crearemos mediante un **NamedQuery**, indicando el nombre de este, y asignándole parámetros, si los tuviese.

## 5.6. Inserciones, actualizaciones y borrados
Para la inserción, no se permite asignar los valores directamente, sino que solo los podemos obtener de una subconsulta:
```hql
insert into entidad (propiedades) select_hql
```
La sintaxis de update o delete es similar a la de SQL:
```hql
update from entidad set [atrib=valor] where condicion
delete from entidad where condicion
```
Además de todo lo visto:
- Estas instrucciones pueden contener parámetros.
- El **where** es opcional, pero lo borrará o actualizará todo.
- Estas consultas se ejecutan todas mediante **executeUpdate()**, que retorna un entero con el número de filas afectadas.

## 6. Enlaces Web
- [Equivalencia entre los tipos de datos básicos dentro de los SGBD y dentro de los programas Java.](https://docs.jboss.org/hibernate/orm/4.1/manual/en-US/html/ch05.html#mapping-types-basictypes)
- [Novedades, software y manuales de Hibernate.](https://hibernate.org/orm/)
- [Contiene los enlaces a las librerías que podemos añadir en nuestros proyectos con los gestores de dependencias más conocidos (Maven, Gradle, etc.).](https://mvnrepository.com/)
- [En esta gigantesca base de datos, nos encontramos con muchas tablas y datos con los que poder trabajar y probar nuestras aplicaciones.](https://dev.mysql.com/doc/employee/en/)

## 7. Respuestas cuestionario
- [Cuestionario](./CUESTIONARIO.md)