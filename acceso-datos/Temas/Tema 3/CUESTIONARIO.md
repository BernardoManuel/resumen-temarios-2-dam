# Cuestionario - Tema 3

#### Pregunta 1 - La finalidad de los ORM es...

- [X] **a. Relacionar de manera eficiente la equivalencia entre clases y tablas para posibilitar la conversión entre estas.**
- [ ] b. Una ampliación del modelo relacional para poder representar objetos en el modelo relacional.
- [ ] c. Documentar los cambios producidos en los datos en tiempo de ejecución.
- [ ] d. Ayudar a los programadores a gestionar de manera eficiente la memoria de los objetos persistentes, una vez cargados de la base de datos y transformados.

#### Pregunta 2 - Hibernate es...

- [ ] a. Un framework independiente del lenguaje de programación.
- [ ] b. Un leguaje de modelado.
- [X] **c. Un framework para el lenguaje de programación Java.**
- [ ] d. Índices.

#### Pregunta 3 - Las opciones de configuración de Hibernate se especifican...

- [ ] a. En los ficheros de tipo NombreClase.hbm.xml.
- [ ] b. De manera embebida dentro de las clases.
- [ ] c. En el fichero hibernate.reveng.xml.
- [X] **d. En el fichero hibernate.cfg.xml.**

#### Pregunta 4 - Un proyecto en Hibernate deberá contener, como mínimo...

- [ ] a. Fichero de configuración y ficheros de mapeado.
- [X] **b. Fichero de configuración, clases beans y fichero de mapeado.**
- [ ] c. Fichero de configuración, clases POJO y anotaciones.
- [ ] d. Fichero de configuración, fichero de ingeniería inversa y beans.

#### Pregunta 5 - La anotación @Transient en un atributo de una clase...

- [ ] a. Permite evitar que se cree una relación y se guarden internamente los valores.
- [X] **b. Permite evitar que se almacene en la base de datos.**
- [ ] c. Permite generar el valor a partir de la base de datos.
- [ ] d. Solo tiene sentido en valores numéricos.

#### Pregunta 6 - Las anotaciones CascadeType...

- [X] **a. Permiten copiar el comportamiento de la clase propietaria en la relacionada.**
- [ ] b. Solo se aplican a operaciones de guardado.
- [ ] c. Permiten copiar el comportamiento de la clase relacionada en la propietaria.
- [ ] d. Permiten evitar borrados de la clase propietaria al borrar elementos de la relacionada.

#### Pregunta 7 - En una clase tenemos un atributo mapeado con @Id, con generator=identity. ¿Cuándo se asignará el valor a dicho atributo?

- [ ] a. Al crear la sesión.
- [ ] b. Al ejecutar el constructor del objeto.
- [ ] c. Al crear el objeto.
- [X] **d. Al guardar el objeto.**

#### Pregunta 8 - Cuando creamos una relación bidireccional...

- [ ] a. Hay que evitar a toda costa relaciones bidireccionales.
- [X] **b. Se guarda una referencia del objeto propietario en el objeto referenciado y viceversa, solo cuando la relación es uno a uno.**
- [ ] c. Se guarda una referencia del objeto propietario en el objeto referenciado y viceversa, solo cuando la relación es uno a M.
- [ ] d. Guardamos colecciones de una clase dentro de la otra, pero no al revés, para evitar problemas de referencias recursivas.

#### Pregunta 9 - La opción de carga Fetch.Lazy...

- [X] **a. Nos permitirá evitar cargas innecesarias mientras no accedamos a los objetos relacionados.**
- [ ] b. Solo tiene sentido en relaciones uno a uno.
- [ ] c. Carga los objetos relacionados al cargar el objeto propietario.
- [ ] d. Es recomendable cuando el objeto relacionado esta anotado también como @Embedded.

#### Pregunta 10 - Mediante consultas HQL...

- [X] **a. No podemos ejecutar funciones de agrupación.**
- [ ] b. Podemos devolver resultados de distintos tipos, pero como un array de objetos.
- [ ] c. Solo podemos devolver todas las filas de la tabla a la vez; eso sí, convertidas en objetos.
- [ ] d. Solo podemos devolver objetos.