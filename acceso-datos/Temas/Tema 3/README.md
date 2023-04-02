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