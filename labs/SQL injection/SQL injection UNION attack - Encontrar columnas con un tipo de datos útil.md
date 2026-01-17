## SQL injection UNION attack - Encontrar columnas con un tipo de datos útil

**Objetivo**: Determinar cuál de las columnas devueltas por la consulta original acepta datos de tipo texto para poder extraer información.

### 1. Reconocimiento (Confirmación de columnas)
Interceptar la petición y confirmar que la consulta devuelve exactamente tres columnas:

Payload:

```SQL
' UNION SELECT NULL,NULL,NULL--
```

**Explicación**:

*UNION SELECT*: Prepara la unión de los resultados.

*NULL,NULL,NULL*: Se usan tres valores nulos para confirmar la cantidad de columnas. Si la página carga sin errores (HTTP 200), sabemos que hay 3 columnas.

### 2. Explotación (Prueba de tipo de datos)

Probar cada columna individualmente reemplazando el NULL por una cadena de texto (ej. 'a') hasta encontrar la que no genere un error:

**Payload**:

```SQL
' UNION SELECT 'a',NULL,NULL--
```

**Explicación**:

*'a'*: Es el valor de prueba. Si la columna no es de tipo texto, la base de datos lanzará un error de conversión de tipos.

Prueba secuencial: Si el primer payload falla, se prueba en la segunda posición (NULL,'a',NULL--) y así sucesivamente.

Éxito: La columna que acepte el texto mostrará el valor 'abcdef' en algún lugar de la página web.

### 📝 Resumen Técnico
¿Por qué da error? Porque en SQL, las columnas en una operación UNION deben tener tipos de datos compatibles. Intentar meter texto en una columna de números rompe la consulta.

¿Por qué es necesario este paso? Antes de intentar sacar nombres de usuarios o contraseñas, necesitamos saber en qué posición "pintará" la web nuestro texto inyectado.

¿Qué es el Data Type Mismatch? Es el error que ocurre cuando los tipos de datos de las dos consultas del UNION no coinciden.