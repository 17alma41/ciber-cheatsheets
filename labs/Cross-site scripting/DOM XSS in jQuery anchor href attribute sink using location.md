## Laboratorio: DOM XSS en jQuery (href sink)

**Objetivo**: Ejecutar XSS inyectando código en el atributo href a través del parámetro returnPath.

### 1. Reconocimiento (Fingerprinting & Columnas)

Modificar el parámetro en la URL para ver dónde se refleja:

**Payload:**

```Plaintext
/feedback?returnPath=/test123
```

**Explicación:**

*location.search*: La fuente (source) de los datos es la propia URL.

*href*: Al inspeccionar, el valor /test123 aparece dentro del atributo href del botón "Back".

### 2. Explotación (Ejecución de Script)
Sustituir la ruta por código ejecutable:

**Payload**:

```JavaScript
javascript:alert(document.cookie)
```

**Explicación**:

*javascript*: Protocolo que indica al navegador que ejecute código en lugar de navegar.

*alert():* Función que se dispara al hacer clic en el botón "Back", demostrando la vulnerabilidad.

### 📝 Resumen Técnico
¿Por qué ocurre? El script de la página toma un valor de la URL y lo escribe en un enlace sin filtrarlo.

¿Qué es el Sink? El atributo href, que permite ejecutar código mediante el protocolo javascript:.

¿Qué es la Source? location.search, ya que los datos provienen de los parámetros de la URL.