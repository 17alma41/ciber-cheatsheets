# Nikto Web Scanner

Nikto es un escáner de vulnerabilidades web.

## 🚀 1. Sintaxis Básica

La estructura fundamental para lanzar un escaneo es:

```Bash
nikto -h <objetivo> [opciones]
```

    [!TIP] Si el servidor utiliza un puerto no estándar (ej. 8080) o utiliza cifrado HTTPS, es fundamental especificarlo con los parámetros -p o -ssl para evitar errores de conexión.

## 📋 2. Tabla de Comandos Esenciales
Comando	Descripción	Ejemplo de uso
| Opción    | Descripción                                             | Ejemplo            |
|----------|----------------------------------------------------------|-------------------|
| `-h`     | Especifica el host objetivo (IP, Dominio o URL).          | `-h 192.168.1.1`  |
| `-p`     | Define el puerto (por defecto es el 80).                  | `-p 443,8080`     |
| `-ssl`   | Fuerza el escaneo a través de HTTPS.                      | `-h web.com -ssl` |
| `-update`| Actualiza la base de datos de plugins y firmas.           | `nikto -update`   |
| `-v`     | Modo Verbose: muestra todo el tráfico en tiempo real.     | `-h web.com -v`   |
| `-Tuning`| Filtra el tipo de pruebas según la vulnerabilidad.        | `-Tuning 123`     |
| `-o`     | Nombre del archivo para guardar los resultados.           | `-o reporte.txt`  |
| `-Format`| Formato del reporte (csv, json, htm, txt, xml).           | `-Format htm`     |


## ⚙️ 3. Parámetros de "Tuning" (Optimización)

El parámetro -Tuning te permite ahorrar tiempo ejecutando solo los tests específicos que te interesan. Puedes combinar los números sin espacios:

    0: Verificación de archivos mediante subida (File Upload).

    1: Archivos interesantes / Bitácoras (Logs).

    2: Configuraciones erróneas / Archivos por defecto.

    3: Divulgación de información (Information Disclosure).

    4: Inyecciones XSS / Scripting.

    5: Inyecciones SQL.

    6: Denegación de Servicio (DoS).

    x: Pruebas de ejecución remota de comandos (RCE).

    Ejemplo de escaneo enfocado solo en Inyecciones (XSS + SQL): nikto -h mysite.com -Tuning 45

## 🕵️  4. Evasión de Firewalls y WAF

Para intentar pasar desapercibido ante sistemas de detección de intrusos (IDS) o Firewalls de Aplicaciones Web (WAF), Nikto ofrece el parámetro -evasion:

    -evasion 1: Codificación URL aleatoria (no-RFC).

    -evasion 5: Uso de cabeceras "Fake" para confundir al servidor.

    -evasion 8: Uso de parámetros de tipo "Long URL" para desbordar filtros simples.

## ⌨️  5. Atajos de Teclado

Mientras Nikto está trabajando, puedes controlar el proceso en tiempo real con estas teclas:

    v: Activar o desactivar el modo detallado (Verbose).

    p: Pausar el progreso (vuelve a pulsar para reanudar).

    P: Imprimir el porcentaje de progreso actual.

    q: Detener el escaneo de forma segura (Quit).

## 📝 6. Ejemplo de un Escaneo Profesional

Este comando realiza un escaneo profundo en un sitio seguro, aplicando una técnica de evasión y exportando los resultados a un reporte interactivo en HTML:

```Bash
nikto -h https://mi-objetivo.com -evasion 1 -o resultado.html -Format htm
```
