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