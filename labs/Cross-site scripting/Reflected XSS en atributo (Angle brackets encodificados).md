## Laboratorio: Reflected XSS en atributo (Angle brackets encodificados)

**Objetivo**: Ejecutar XSS en un entorno donde los caracteres < > están bloqueados, inyectando un gestor de eventos en un atributo HTML.

### 1. Reconocimiento (Fingerprinting & Columnas)

Enviar una cadena al azar y observar en Burp Repeater cómo se refleja en la respuesta:

**Payload**:

```Plaintext
test12345
```

**Explicación**:

Reflexión: El valor se inyecta dentro de un atributo de un input (ej: value="test12345").

Codificación: Al intentar usar < >, el servidor los convierte en &lt; y &gt;, lo que impide crear etiquetas de script propias.

### 2. Explotación (Inyección de Atributo)
Escapar del atributo actual para añadir uno nuevo que ejecute código:

**Payload**:

```js
"onmouseover="alert(1)
```

**Explicación**:

" (Comilla): Sirve para cerrar el valor del atributo original donde se refleja nuestro texto.

*onmouseover*: Inyecta un nuevo atributo de evento. Este ejecuta JavaScript cuando el usuario pasa el ratón por encima del elemento.

### 📝 Resumen Técnico
¿Por qué funciona? Porque aunque el sistema filtra los brackets (< >), permite el uso de comillas, lo que nos deja "romper" la estructura del atributo original.

¿Qué es un Event Handler? Son atributos como onmouseover, onclick o onerror que ejecutan código JS ante acciones del usuario o errores del sistema.

¿Qué es el bypass? Es una técnica para saltar filtros de seguridad; en este caso, saltamos la codificación de etiquetas HTML usando la lógica de los atributos.