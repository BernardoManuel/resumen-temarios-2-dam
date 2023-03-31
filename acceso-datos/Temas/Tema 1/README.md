# Tema 1 - Acceso a ficheros
## 1. Sistema de ficheros
### 1.1. Acceso al sistema de ficheros
En Java para utilizar ficheros se utiliza la clase File, cuya utilidad es que representa un enlace a un elemento genérico del sistema de archivos.

Los constructorores principales que posee son:
```java
File(File parent, String child) // Crea el acceso a un elemento (child) a partir de un objeto File existente (parent), que representa a un directorio.

File(String pathname) // Crea el acceso a un elemento mediante una ruta absoluta, o bien un elemento dentro de la ruta de ejecución.
```

### 1.2. Rutas
Dependiendo del sistema en el que nos encontremos, deberemos tratar las rutas de forma diferente. Tenemos que tener en cuenta el **separator**, referido al carácter que separa un elemento contenido dentro de otro cuando especificamos las rutas de las carpetas.

```java
File f = new File("/home/dgomezm/main.java") // Linux

File f = new File("C:\\Usuarios\\dgomezm\\Escritorio\\main.java") // Windows
```

Una vez ya se ha obtenido la referencia al elemento del sistema de ficheros, se podrían hacer una serie de comprobaciones utilizando los métodos de la clase File:

```java
boolean exists() // Devuelve true si el objeto al que apuntamos existe.

boolean isFile()
boolean isDirectory() // Devuelven true en el caso que la referencia sea un fichero o un directorio, respectivamente.

boolean createNewFile() // Crea un fichero nuevo, solo en caso de que no exista.

boolean mkdir()
boolean mkdirs() // Crea un directorio nuevo, solo en caso de que no exista. Mediante mkdirs() crearemos también los directorios padre del actual, en caso de que no existan.

boolean renameTo(File) // Permite renombrar elemento actual a otro.

boolean delete() // Elimina el elemento.
```

### 1.3. Propiedades de los elementos

Para consultar ciertas propiedades de los focheros, se utilizan los siguientes métodos de la clase **File**:

```java
boolean canRead()
boolean canWrite()
boolean canExecute() // Informa de los permisos de usuario para cada fichero.

long length() // Devuelve el tamaño en bytes del fichero. Para los directorios, esta información no es representativa.

String[] list() // Devuelve un vector de String con los elementos que contiene el directorio actual.
```

## 2. Ficheros de texto

Las clases que posee Java para la gestión de ficheros de texto son las siguientes:

```java
FileWriter fw
FileReader fr // Nos premite gestionar ficheros de texto, aunque solo mediante lecturas/escrituras carácter a carácter o en bloque de longitud constante.

BufferedWriter bw
BufferedReader br // Creados a partir de los anteriores, permiten el tratamiento de los ficheros de texto más cercano al ser humano, ya que el tratamiento de sus datos puede realizarse linea a linea.
```

### 2.1. Procesado carácter a carácter
Los objetos **FileWriter/FileReader** suelen ser adecuado para tratamientos generales de ficheros.

Estas clases poseen como métodos generales:

```java
int read() // Lee un solo carácter, aunque devuelve un entero, por lo que habría que hacer un casting a char. 

int read(char[], offset, max) // Lee un bloque de caracteres de una longitud máxima determinada.

void write(int c) // Escribe el carácter como argumento.

void write(char[]) // Escribe el conjunto de caracteres pasados dentro del vector.

```

### 2.2. Procesado línea a línea
Los objetos **BufferedWriter/BufferedReader** se utilizan para leer un texto hasta encontrar el separador de la linea (\n).

Para leer archivos línea a línea deberemos crear la referencia a un fichero y crear el objeto File. Leeremos posteriormente con el método **String readLine()**, guardando el contenido de dicha lectura en una variable **String**. Este método devolverá **null** cuando no se ha podido efectuar la lectura porque se ha llegado al fianl del fichero.

Para la escritura por bloques o líneas, utilizaremos el objeto tipo **BufferedWriter**. El método que permite la escritura es **void write(String s)**, que envía al fichero el texto pasado.

```java
String texto = "Esto es una línea.";
fw.write(texto);
fw.newLine();
```

### 2.3. Archivos CSV

Los archivos CSV son ficheros de texto plano que contienen información respecto a una entidad.

## 3. Ficheros binarios. Ficheros de objetos.

### 3.1. Clases Java para los ficheros binarios.
Para tratar los ficheros binarios en Java tenemos las siguiente clases:

```java
FileInputStream fis
FileOutputStream fos
// Son clases generales de acceso a ficheros binarios. Sus métodos genéricos read() y write() permiten leer o escibir bytes adhoc.

DataInputStream dis
DataOutputStream dos
// La gran ventaja de estas clases son los métodos readTIPO() y writeTIPO(). Ejemplos, readInt(), writeInt(), readUTF() o writeUTF().

RandomAccessFile raf
// Esta clase permite tanto leer como escribir los tipos adecuados.

ObjectInputStream ois
ObjectOutputStream oos
// Esta clase permite leer y escribir objetos directamente. Se requiere que los objetos tengan implementado la interfaz Serializable.

```

## 4. Almacenamiento con documentos XML
### 4.1. Analizadores XML
- **Analizadores secuenciales o sintácticos:** Van extrayendo el contenido según se van descubriendo los hashtags de apertura y cierre.
- **Analizadores jerárquicos:** Suelen ser los más utilizados, ya que guardan todos los datos del documento XML en memoria, en forma estructura jerarquizada; son los preferidos para aplicaciones que tengan que leer los datos de forma continua.

### 4.2. El modelo de objetos del documento (DOM)
El **DOM** es la estructura especificada por el W3C donde se almacena la información de los documentos XML. En **Java**, el DOM se implementa haciendo uso de interfaces.

La interfaz principal del DOM en Java es **Document**.

Además de Document, el W3C también define la clase abstracta **DocumentBuilder**, que permite crear el DOM a partir del XML.

La manera de recuperar el DOM de un XML sería:

```java
public Document ObreXML(String nombreArchivoXML) throws IOException, SAXExceptiom, ParserConfigurationException, FileNotFoundException {
    DocumentBuilderFactory dbFactory = DocumentBuilderFactory.newInstance();

    DocumentBuilder dBuilder = dbFactory.newDocumentBuilder();

    Document doc = dBuilder.parse(new File(nombreArchivoXML));

    return doc;
}

```

### 4.3. Clases y métodos del DOM
- **Document**
- **Element**
- **TextElement**
- **Attribute**

Todas estas interfaces derivan de la interfaz **Node**, por lo que heredarán sus atributos y métodos, y además, aportarán atributos y métodos propios.

#### Lectura de documentos XML

```xml
<curso>
    <modulo>
        <nombre>Acceso a Datos</nombre>
        <horas>6</horas>
        <nota>8.45</nota>
    </modulo>
    <modulo>
        <nombre>Programación de servicios y procesos</nombre>
        <horas>3</horas>
        <nota>9.0</nota>
    </modulo>
</curso>
```