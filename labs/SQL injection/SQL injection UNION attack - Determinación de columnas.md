## Laboratorio: SQL injection UNION attack - Determinación de columnas

**Objetivo**: Identificar el número exacto de columnas que devuelve la consulta original de la aplicación mediante el uso de valores NULL.

### 1. Reconocimiento (Prueba de error)
Interceptar la petición del filtro de categorías en Burp Suite y forzar un error de estructura:

**Payload**:

```SQL
' UNION SELECT NULL--
``` 

**Explicación**:

*UNION SELECT*: Intenta combinar los resultados de la consulta legítima con nuestra fila inyectada.

*NULL*: Se utiliza porque es compatible con casi cualquier tipo de datos (texto, números, fechas), lo que evita errores de "tipo de datos" y nos permite centrarnos solo en la cantidad de columnas.

*Error*: Si el servidor devuelve un error (como un 500 Internal Server Error), confirma que el número de columnas inyectadas (1) no coincide con el de la consulta original.

### 2. Explotación (Ajuste de columnas)
Añadir valores NULL de forma incremental hasta que la respuesta del servidor sea exitosa (HTTP 200 OK):

**Payload** (Ejemplo con 3 columnas):

```SQL
' UNION SELECT NULL,NULL,NULL--
```

**Explicación**:

Incremento: Se van añadiendo comas y más valores NULL uno a uno en cada intento.

Éxito: Cuando la página carga correctamente y no devuelve error, habremos determinado el número exacto de columnas que maneja la base de datos para esa consulta.


### 📝 Resumen Técnico
¿Por qué usar NULL? Porque el operador UNION requiere que los tipos de datos coincidan. Al no saber qué hay en cada columna, NULL es el valor más seguro para no fallar por el tipo de dato.

¿Qué indica el error? Indica una discrepancia entre las columnas de la consulta original y nuestra consulta inyectada. Ambos lados del UNION deben tener el mismo número de columnas.

¿Qué sigue después? Una vez conocido el número de columnas, el siguiente paso suele ser determinar cuál de ellas acepta datos de tipo texto para extraer información.