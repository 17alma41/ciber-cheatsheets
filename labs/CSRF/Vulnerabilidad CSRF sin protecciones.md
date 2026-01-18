## Vulnerabilidad CSRF sin protecciones

**Objetivo**: Realizar un cambio de correo electrónico de la víctima mediante una petición falsificada aprovechando la ausencia total de medidas de seguridad.

### 1. Reconocimiento (Análisis de la Petición)
Interceptar la petición de "Update email" en Burp Suite y analizar su estructura:

**Petición capturada**:

```HTTP
POST /my-account/change-email HTTP/1.1
Host: YOUR-LAB-ID.web-security-academy.net
Content-Type: application/x-www-form-urlencoded

email=test%40test.com
```

**Explicación**:

*POST Request*: La acción se realiza mediante un método POST, lo cual es estándar, pero no garantiza seguridad por sí solo.

Ausencia de Tokens: No existe ningún parámetro impredecible (como un token CSRF). La petición es totalmente predecible.

*Cookies*: El navegador adjunta automáticamente las cookies de sesión del usuario al realizar la petición, lo que permite al atacante actuar en nombre de la víctima.

### 2. Explotación (Generación de PoC)
Crear un documento HTML malicioso que obligue al navegador del usuario a enviar la petición de cambio de email automáticamente:

**Payload (HTML)**:

```HTML
<form method="POST" action="https://YOUR-LAB-ID.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="pwned@attacker.com">
</form>
<script>
        document.forms[0].submit();
</script>
```

**Explicación**:

*form method="POST"*: Imita la estructura exacta de la petición original de la aplicación.

*input type="hidden"*: Define el nuevo correo electrónico que queremos imponer a la víctima sin que esta vea campos de texto sospechosos.

*document.forms[0].submit()*: Script que dispara el envío del formulario de forma inmediata en cuanto la víctima carga la página del atacante.

### 📝 Resumen Técnico
¿Por qué funciona? Porque la aplicación confía exclusivamente en las cookies de sesión para validar la identidad, y estas se envían automáticamente aunque la petición se origine en un sitio externo.

¿Qué falta en la defensa? No hay un CSRF Token (un valor único y secreto por sesión) que valide que la petición fue generada por el usuario de forma legítima desde la web oficial.

¿Cómo se completa el ataque? El atacante aloja el HTML en su servidor y engaña a la víctima (mediante phishing o un enlace) para que lo visite mientras tiene su sesión abierta en el laboratorio.