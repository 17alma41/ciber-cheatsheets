## Laboratorio: SQL Injection (Oracle) - Enumeración y Versión

**Objetivo:** Identificar la base de datos como Oracle, determinar el número de columnas y extraer la versión.

### 1. Reconocimiento (Fingerprinting & Columnas)

Interceptar la petición en Burp Suite y modificar el parámetro `category`:

**Payload:**
```sql
'+UNION+SELECT+'abc','def'+FROM+dual--
```

**Explicación**:

*UNION SELECT*: Une nuestros resultados falsos a la consulta original.

'*abc','def'*: Prueba si la consulta original tiene 2 columnas y si ambas aceptan texto.

*FROM dual*: Confirmación de Oracle. Esta tabla es obligatoria en Oracle para hacer SELECT sin tablas reales. Si no da error, estamos ante una DB Oracle.

### 2. Explotación (Extracción de Versión)
Una vez confirmadas las 2 columnas y que es Oracle, extraemos la información:

**Payload:**

```sql
'+UNION+SELECT+BANNER,+NULL+FROM+v$version--
```

**Explicación:**

*BANNER*: Columna del sistema que contiene el texto de la versión (ej. Core 11g...).

*NULL*: Relleno obligatorio. Como la consulta original tiene 2 columnas, necesitamos rellenar la segunda posición para evitar errores de sintaxis. NULL es válido para cualquier tipo de dato.

*FROM v$version*: Tabla del sistema de Oracle donde se almacena la información de la versión.

### 📝 Resumen Técnico

¿Por qué FROM dual? Porque Oracle no permite SELECT sin FROM.

¿Por qué NULL? Para igualar el número de columnas del UNION (2 columnas).

¿Por qué BANNER? Es el nombre estándar de la columna de versión en la vista v$version.  
