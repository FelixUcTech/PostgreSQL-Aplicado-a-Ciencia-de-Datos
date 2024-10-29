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
