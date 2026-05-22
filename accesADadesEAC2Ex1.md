# Apuntes de Estudio: Persistencia Avanzada con JDBC y PostgreSQL

Este documento resume los conceptos clave, la sintaxis y los procesos necesarios para trabajar con tipos de datos avanzados de PostgreSQL (Tipos Compuestos y Arrays) utilizando la API de JDBC en Java.

---

## 1. Conceptos Clave

### A. Tipos Compuestos (Composite Types / UDT)
En PostgreSQL, un tipo compuesto es un objeto estructurado que contiene varios campos internamente (similar a una estructura o clase). 
* **En la base de datos:** Se define mediante `CREATE TYPE nombre_tipo AS (...)`.
* **En Java:** No tiene un tipo primitivo directo. Se mapea enviando sus componentes por separado o desempaquetándolos en la consulta SQL.

### B. Mapeo de Arrays (`VARCHAR[]`)
PostgreSQL permite almacenar colecciones de elementos directamente en una columna utilizando arrays.
* Permite evitar tablas intermedias en relaciones simples.
* En Java se maneja mediante colecciones (`List<String>`) que deben convertirse a `java.sql.Array` antes de interactuar con la base de datos.

### C. Abstracción de Excepciones
Encapsular las excepciones de base de datos (`SQLException`) dentro de una excepción propia (`GestorExcepcio`) evita que las capas superiores de la aplicación dependan directamente de la tecnología de persistencia utilizada.

---

## 2. Procesos y Sintaxis JDBC Paso a Paso

### Paso 1: Inserción con Tipos Compuestos y Arrays
Para insertar datos en un tipo compuesto, se utiliza la sintaxis de conversión de tipos (Casting `::`) de PostgreSQL en el `PreparedStatement`.

#### Ejemplo de SQL:
```sql
INSERT INTO pista (id, especificacions, remuntadors_acces) 
VALUES (?, (?, ?, ?)::dades_tecniques, ?)