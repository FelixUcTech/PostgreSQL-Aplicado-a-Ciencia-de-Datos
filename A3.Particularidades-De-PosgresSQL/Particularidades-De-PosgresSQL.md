# Particularidades-De-PosgresSQL
## Diferencias entre otros manejadores y PostgreSQL
PostgreSQL es un manejador de base de datos es cumpletamente abierto. Es una base de datos sumamente adaptable.
Es una base de datos que se se puede manejar de manera avanzada, ejemplo maneja documentos json (Objetos clave valor), aplicaciones orientadas a estadísticas, etc.
También tiene su propio lenguaje para consultas avanzadas que es el PLPGSQL, es un leguaje que te permité hacer un poco de programación
Implementa fácilmente las particiones.
Common table expressions, Crea una contexto donde sea de consulta más eficiente
Window fuctions: Nos ayudan a hacer una cunsulta más especializada

## Capacidades de PLPGSQL
PLPGSQL es un leguaje procedural que complementa el leguaje standard sql, permite funciones y triggers para encapsular y reusar sentencias de manera simple, permite realizar programación estructurada como otros leguajes de progtranación, realiza cálculos comlejos y lo hace de maner nativa con los datos que contiene, permite al usuario definir listas, diccionarios e incluso usar tipos como JSON.

Procedimientos Almacenados:
PL/pgSQL permite definir procedimientos almacenados, que son bloques de código que se pueden reutilizar para realizar tareas específicas en el lado del servidor de la base de datos.

Control de Flujo: Ofrece estructuras de control de flujo familiares, como bucles, condicionales y manejo de excepciones, lo que facilita la escritura de lógica compleja en la base de datos.

Variables y Tipos de Datos: Soporta variables y tipos de datos, permitiendo almacenar y manipular datos dentro de los procedimientos almacenados.

Triggers: PL/pgSQL se utiliza comúnmente para escribir triggers (disparadores) que responden a eventos en las tablas, ejecutando lógica personalizada antes o después de ciertas operaciones.

Funciones: Permite la creación de funciones que pueden aceptar parámetros y devolver resultados, facilitando la modularidad y reutilización del código.

Manipulación de Datos: Ofrece capacidades completas para manipular datos en la base de datos, incluyendo inserción, actualización y eliminación de registros.

Conexión con SQL: PL/pgSQL está integrado con SQL, lo que permite combinar fácilmente el código procedural con consultas SQL para realizar operaciones complejas y personalizadas.

Optimización de Rendimiento: Al ejecutarse en el servidor de la base de datos, los procedimientos almacenados escritos en PL/pgSQL pueden ser altamente eficientes y contribuir a la optimización del rendimiento.

Seguridad: Puede ayudar a mejorar la seguridad al permitir el encapsulamiento de la lógica de negocio en el servidor de la base de datos, reduciendo la exposición de la lógica a través de la capa de aplicación.

Integración con Transacciones: PL/pgSQL es compatible con transacciones, lo que significa que puedes realizar operaciones atómicas y consistentes a nivel de base de datos.

## Guardar procedimientos con PLPGSQL
Stored procedures no regresan valor, y se han incluido en el estandar sql, no confundirse con las funciones, o function que son mas versalites y pueden regresar valor y se crean completamente con PLPGSQL únicamente.

### Procedimiento con SQL
```sql
CREATE OR REPLACE PROCEDURE test_dropcreate_procedure()
LANGUAGE SQL
AS $$
    DROP TABLE IF EXISTS aaa;
	CREATE TABLE aaa (bbb char(5) CONSTRAINT firstkey PRIMARY KEY);
$$;
```
```sql
CALL test_dropcreate_procedure();
```
Cualquier cosa que queramos automatizar y encapsular podemos agregarlo a un procedimiento.

### Procedimiento con PLPGSQL
Para las funciones de posgres necesitamos dos palabras reservadas, BEGING & END, de ahí en fuera es muy parecido a la función anterior.
``` sql
CREATE OR REPLACE FUNCTION test_dropcreate_function()
RETURNS VOID
LANGUAGE plpgsql
AS $$
BEGIN
	DROP TABLE IF EXISTS aaa;
	CREATE TABLE aaa (bbb char(5) CONSTRAINT firstkey PRIMARY KEY, ccc char(5));
	DROP TABLE IF EXISTS aaab;
	CREATE TABLE aaab (bbba char(5) CONSTRAINT secondkey PRIMARY KEY, ccca char(5));
END
$$;
```
``` sql
SELECT test_dropcreate_function();
```

## PLPGSQL: Conteo, registro y triggers
En este primer ejemplo de funciones se puede observar un retorno de valor entero y que para llamarlo a diferencia de un procedimiento estamos usando la sentencia select, importante señalar que se tiene que usar *begin & end* en las funciones para señalar el inicio y el termino de la misma.

```sql
create or replace function count_total_movies()
returns int
language plpgsql
as $$
begin
	return count(*) from peliculas;
end
$$;
```
``` sql
select count_total_movies();
```
Ejemplo 2, una función orientada a ser un trigger, donde se replicaran automáticamente los datos insertados de una tabla en otra. La importancia de los trigger es poder ejecutar situaciones de agregación muy complejas cada vez que se modifica una base de datos, lo cual nos permite mantener al día situaciones de estadísticas entre otras cosas.
``` sql
create or replace function duplicate_records() --Creación de la función de duplicado de registro
	returns trigger --Retornará un trigger un disparo
	language plpgsql --indicando que lenguaje usaremos
as $$
begin
	insert into aaab(bbba,ccca)
	values(new.bbb, new.ccc);
	return new;
end
$$;
```
``` sql
create trigger aaa_changes
	before insert
	on aaa
	for each row
	execute procedure duplicate_records();
```
``` sql
insert into aaa(bbb,ccc)
values('felix','here');
```
Al insertar los valores se duplican en la tabla aaab, sin embargo se hace el insert normal en la tabla aaa.

## PLPGSQL: Aplicado a ciencia de datos
Ejemplo aplicado
``` sql
CREATE OR REPLACE FUNCTION movies_stats()
RETURNS VOID
LANGUAGE plpgsql
AS $$
DECLARE
	total_rated_r REAL := 0.0;
	total_larger_than_100 REAL := 0.0;
	total_published_2006 REAL := 0.0;
	average_duracion REAL := 0.0;
	average_rental_price REAL := 0.0;
BEGIN
	total_rated_r := COUNT(*) FROM peliculas WHERE clasificacion = 'R';
	total_larger_than_100 := COUNT (*) FROM peliculas WHERE duracion > 100;
	total_published_2006 := COUNT(*) FROM peliculas WHERE anio_publicacion = 2006;
	average_duracion := AVG(duracion) FROM peliculas;
	average_rental_price := AVG(precio_renta) FROM peliculas;

	TRUNCATE TABLE peliculas_estadisticas; --TRUNCATE se utiliza para borrar todos los registros de una tabla de manera rápida y eficiente, sin eliminar la estructura de la tabla. A diferencia de DELETE, TRUNCATE no registra cada fila eliminada, lo que lo hace más rápido, pero tampoco activa triggers ni permite condiciones (WHERE).

	INSERT INTO peliculas_estadisticas (tipo_estadistica, total)
	VALUES
		('Películas con clasificacion R', total_rated_r),
		('Películas de más de 100 minutos', total_larger_than_100),
		('Películas publicadas en 2006', total_published_2006),
		('Promedio de duración en minutos', average_duracion),
		('Precio promedio de renta', average_rental_price);
END
$$;
```
``` sql
SELECT movies_stats();
```
## Integración con otros lenguajes
Como la mayoría de las bases de datos, PostgreSQL cuenta con conectores para diferentes lenguajes de programación, de tal forma que si trabajas con Python, PHP, Java, JavaScript y todos sus frameworks, exista una forma de extraer datos de PostgreSQL y posteriormente utilizar las propiedades de los lenguajes procedurales para transformar y utilizar los datos.

El lenguaje estándar utilizado en bases de datos relacionales es SQL (Structured Query Language), un lenguaje que tiene una estructura sumamente útil para hacer solicitudes de datos, en especial tomando como abstracción un diseño tabular de datos. Sin embargo, carece de estructuras de control y otras abstracciones que hacen poderosos a los lenguajes procedurales de programación.

### Limitaciones de SQL frente a los Lenguajes Procedurales

SQL, por diseño, es un lenguaje de consulta de datos especializado en realizar solicitudes, inserciones y modificaciones en tablas. Sin embargo, carece de varias **estructuras de control** y otras abstracciones que sí poseen los lenguajes de programación procedurales, como Python o Java. Aquí algunos ejemplos:

1. **Bucles**: En SQL estándar, no es posible iterar directamente sobre un conjunto de datos o repetir acciones, como se puede hacer en lenguajes de programación con bucles (`for`, `while`).

   - *Ejemplo en Python*:
     ```python
     for i in range(5):
         print(i)
     ```

   - *En SQL*: No existe una forma directa de hacer esto, aunque algunas bases de datos (como PostgreSQL con PL/pgSQL) permiten incluir bucles como una extensión.

2. **Condicionales complejas**: SQL permite condiciones (`WHERE`, `CASE WHEN`), pero estas son limitadas en comparación con estructuras de control como `if`, `else` o `switch` en otros lenguajes.

   - *Ejemplo en Java*:
     ```java
     if (age > 18) {
         System.out.println("Es adulto");
     } else {
         System.out.println("Es menor de edad");
     }
     ```

   - *En SQL estándar*: Solo se pueden usar condiciones simples con `CASE WHEN`, sin la flexibilidad de anidamiento o combinaciones complejas que los lenguajes procedurales permiten.

3. **Funciones y estructuras de datos avanzadas**: SQL permite crear funciones, pero suelen estar limitadas en comparación con funciones complejas en otros lenguajes, que pueden aceptar parámetros de distintos tipos, manejar excepciones avanzadas y utilizar estructuras como listas o diccionarios.

4. **Variables y manipulación de datos**: SQL no es adecuado para manipulación compleja de datos en tiempo real o procesamiento intensivo (como transformar datos complejos o hacer cálculos recursivos), lo cual se facilita en lenguajes procedurales.

En resumen, SQL es muy eficiente para trabajar con datos en una estructura tabular, pero para procesos más complejos o lógicos, normalmente es necesario complementarlo con lenguajes procedurales que permiten una manipulación avanzada de los datos.

### PL/pgSQL
Como respuesta a los puntos débiles de SQL como estándar, PostgreSQL respondió originalmente creando un lenguaje propio llamado PL/pgSQL (Procedural Language/PostgreSQL Structured Query Language) que es literalmente un superset de SQL que incluye propiedades de un lenguaje estructurado que, por un lado, nos permite crear funciones complejas y triggers; y, por el otro lado, agrega estructuras de control, cursores, manejo de errores, etc.

### Otros lenguajes
Sin embargo, en muchos sentidos, aunque PL/pgSQL ayuda en los casos más genéricos para generar estructuras y funcionalidades más complejas, no se compara con lenguajes completamente independientes y no ligados directamente a una base de datos.

La respuesta sin embargo tampoco es los conectores normales que, si bien resuelven la parte de un lenguaje más complejo, añaden por otro lado una separación de la base de datos, ya que debe correr en un servidor separado y hacer llamadas entre ellos con la latencia como un colateral.

Para mitigar estos problemas tomando lo mejor de ambos mundos, los desarrolladores de PostgreSQL se dedicaron a hacer implementaciones de diversos lenguajes a manera de plugin.

### C
La biblioteca que permite al lenguaje C ejecutarse en PostgreSQL es llamada libpq y es una interfaz de programación que permite extender y hacer de interfaz para permitir a otros lenguajes ejecutarse en esta base de datos.

Puedes encontrar más información de esta interfaz en el siguiente link:

https://www.postgresql.org/docs/11/libpq.html.

### PL/Tcl
Tcl (Tool Command Language) es un lenguaje diseñado con la simpleza en mente y su paradigma consiste en que todo en él es un comando, incluyendo la estructura del lenguaje que, sin embargo, son suficientemente flexibles para poderse sobreescribir, haciéndolo un lenguaje sumamente extensible.

Todo lo anterior es ideal para la integración con el manejador de PostgreSQL ya que permite elaborar comandos para ejecutar las sentencias SQL y extenderlas facilmente.

Si quieres leer más del tema, puedes hacerlo en el siguiente link:

https://www.postgresql.org/docs/11/pltcl.html.

### PL/Perl
Perl es un lenguaje de programación que implementa una estructura de bloques de código y que toma inspiración de programas como C, sh, AWK, entre otros. Y es especialmente bueno para el tratamiento de cadenas de texto. Sin embargo, no se encuentra limitado como un lenguaje de script.

Dada la propiedad de englobar funcionalidad en forma de bloque y de la rapidez y facilidad con la que trabaja con datos tipo cadena, este lenguaje es ideal para el tratamiento de información de una base de datos relacional.

Para conocer más de la implementación de este lenguaje con PostgreSQL puedes leer el siguiente link:

https://www.postgresql.org/docs/11/plperl.html.

### PL/Python
Python, al ser de los lenguajes de programación más extendidos entre programadores de servicios Backend, es una implementación particularmente interesante para PostgreSQL.

Python es un lenguaje de programación fuerte en tratamiento de estructura de datos y tiene un paradigma múltiple con fuertes componentes orientados a objetos, estructurados y una fuerte influencia del paradigma funcional.

Parte de sus fortalezas son sus implementaciones de funciones map, reduce y filter en conjunto con list comprehensions, sets, diccionarios y generadores.

Dadas las propiedades nativas para manejar estructuras de datos complejas, es un lenguaje ideal para manejar la salida de un query SQL.

La implementación de Python para PostgreSQL te permite crear funciones complejas en un lenguaje completo y popular sin tener que utilizar PL/pgSQL. Puedes ver un ejemplo a continuación de la misma función en PL/pgSQL y PL/Python.

https://www.postgresql.org/docs/11/plpython.html.

## Tipo de dato personalizados
Cuando usas tipos de datos personalizados en PostgreSQL, lo haces para definir estructuras de datos que reflejen mejor tu dominio de aplicación. Esto es útil en situaciones donde necesitas restricciones específicas, como en el caso del tipo de dato enum que permite limitar las opciones a un conjunto predefinido, mejorando la integridad de los datos y facilitando consultas más específicas. Por ejemplo, puedes usarlo para campos como clasificaciones o estados, asegurando que solo se ingresen valores válidos.
```sql
CREATE TYPE humor AS ENUM ('triste', 'normal', 'feliz');

CREATE TABLE persona_prueba (
    nombre text,
    humor_actual humor
);

INSERT INTO persona_prueba VALUES ('Pablo', 'molesto'); -- En esta sentencia como no está definido en mi lista de valores no se permite hacer la insersión en mi tabla
INSERT INTO persona_prueba VALUES ('Pablo', 'feliz'); -- Para este caso si se permite dado que feliz si existe

SELECT * FROM persona_prueba;
```
