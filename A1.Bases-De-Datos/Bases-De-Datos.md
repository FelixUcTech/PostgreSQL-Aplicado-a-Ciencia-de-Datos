# Bases de datos

## Importanción de bases de datos en pgAdmin (PostgreSQL)

Siempre cuando se inicia un curso tema o desarrollo relacionado a Técnología es importante saber en que ambiente nos estamos desenvoviendo, para estos temas vamos a trabajar en PostgreSQL.

Como nota muchas veces es engorroso cargas una base de datos nueva, pero siempre es bueno ser paciente aún cuando de caracter en nuestra vida cotidiana no somos tan pacientes porque este paso es elemental, es como poner lo simientos a nuestra casa, y debemos importar bien nuestras bases de datos.

## Breve historia de las bases de datos

Originalmente las bases de datos se almacenaban en documentos .txt, el problema de este tipo de almacenamiento es que era muy díficil de manipular, en este contexto nacen las bases de datos relaciones que una forma de entenderlas en información organizada en matrces o tablas.

### 1960: Modelo de Red y Jerárquico:
En los años 60, surgieron los primeros sistemas de bases de datos con modelos de datos jerárquicos y en red. El modelo jerárquico organizaba los datos en una estructura de árbol, mientras que el modelo en red permitía relaciones más complejas.

### 1970: Modelo Relacional y SQL:
En 1970, Edgar F. Codd introdujo el modelo relacional, que se basaba en la teoría matemática de conjuntos. Este modelo permitía la representación de datos en tablas relacionadas. Además, se desarrolló el lenguaje SQL (Structured Query Language) para interactuar con bases de datos relacionales.

### 1974: Fundación de Oracle Corporation:
Larry Ellison, Bob Miner y Ed Oates fundaron Oracle Corporation en 1977. Oracle Database, lanzada en 1979, se convirtió en una de las primeras bases de datos relacionales comerciales exitosas.

### 1980: Auge de las Bases de Datos Relacionales:
Durante la década de 1980, las bases de datos relacionales ganaron popularidad rápidamente. IBM desarrolló DB2, Microsoft lanzó SQL Server y Sybase también contribuyó al crecimiento de esta tecnología.

### 1983: PostgreSQL:
En 1983, el proyecto PostgreSQL (Postgres) se inició en la Universidad de California en Berkeley. PostgreSQL se convirtió en una base de datos de código abierto extremadamente potente y flexible.

### 1990: MySQL y el Auge de las Bases de Datos de Código Abierto:
En 1995, se lanzó MySQL, otra base de datos relacional de código abierto. MySQL ganó popularidad especialmente en aplicaciones web. El auge de las bases de datos de código abierto continuó con proyectos como SQLite.

### 2000: Bases de Datos NoSQL:
A principios del siglo XXI, surgieron las bases de datos NoSQL para abordar limitaciones de escalabilidad y flexibilidad en los sistemas tradicionales. MongoDB, Cassandra y CouchDB son ejemplos de bases de datos NoSQL que se destacaron.
2010 en Adelante: Big Data y Bases de Datos Distribuidas:
Con el crecimiento de grandes volúmenes de datos, surgieron bases de datos diseñadas para manejar entornos distribuidos y grandes conjuntos de datos. Ejemplos incluyen Hadoop, Apache Cassandra y Amazon DynamoDB.

### Tendencias Actuales
Actualmente, las bases de datos en la nube, la inteligencia artificial, el aprendizaje automático y la integración de datos en tiempo real son áreas clave de desarrollo. Bases de datos como Google Bigtable, Amazon Aurora y Microsoft Azure Cosmos DB están liderando estas tendencias.

## Puntos fuertes de las BD relacionales
Se podría llegar a pensar que las bases de datos relaciones son cosas del pasado, que si bien hoy las bases de dato no relaciones funguen un amplio campo para ciertas actividades, aquí presento algunos puntos fuertes por los cuales es importante manejar las bases de datos relaciones.
- Multiproposito
    - Nos permite almacenas, extraer y relalizar estados de agregación, al momento de interactuar con multiples bases de datos de manera sencilla.
    - La base de dato relacional es una navaja suiza.
- Ampliamente utilizadas
    - Dónde te encuentres dentro de Data Science & AI, siempre te vas a encontrar una base de datos relacional.
- Información consistente
    - Quiere decir que existe una congruencia dentro de las bases relacionales, ejemplo de esto es que los motores de bases de datos relaciones deben cumplir con el estandar ACID para que sean considerados de calidad.
    - Esto reduce el mantenimiento de la base de datos.
- Flexible
    - Puede ser ocupada en 1 paso de tu proceso de desarrollo o en todos lo pasos.
- Retrocompatible
    - Evolucionan algunas características pero ciertos estandares se mantienen en el manejo de las bases de datos.
- Completamente programable
    - Tepermite usar lenguajes procedurales, esto permite generar consultas mucho más complejas.

## Conceptos Sí o Sí para operar una DB relacional

### Entidades / Tablas
Cosas del mundo real, cualquier objeto representable en tu base de datos. Las entidades tienen ciertos atributo (este sería el segundo concepto.)

### Atributos
Estos atributos son cualitativos o cuantitativos, estos son o no representados en las columnas y se relacionan de manera directa con las entiedades.

Un alumno es una identidad y tiene ciertos atributos, estos pueden ser matricula, nombre, edad, etc, pero son del alumno.

### Relaciones
Son la manera en la que se unen una identidad y otra entiedad cómo lo sería las bases tablas de relación.

De este tipo de tablas transitivas, es decir, si tienes dos tablas únicas perros y clinicas, que están llenas de atributos respectivamente, se realiza una relación pueden surgir ciertas propiedades nuevas que permiten el analisis de los datos. Iniciando desde cuantas clinicas veterinarias hay para n cantidad de perros en una determinada área.

### Stop procedure
Funciones que ejecuta el manejador de base de datos por nosotros

### Trigger
Acciones que se deben de hacer, antes, durantes o después de que se ejecute una acción específica.



