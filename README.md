# Ciber cheatsheets

Recopilación de guías rápidas y checklists de utilidad para ciberseguridad, pruebas de penetración y forense.

## Contenido

- [Web hacking cheatsheets](#web-hacking-cheatsheets)
  - [Contenido](#contenido)
    - [Checklist Básico](#checklist-básico)
    - [Inyecciones](#inyecciones)

### Checklist Básico

1. Reconocimiento (Recon)
- [ ] **Identificar stack tecnológico:** Usar Wappalyzer para detectar CMS, lenguaje y servidor.
- [ ] **Enumeración de subdominios:** Buscar activos como `dev.target.com` o `api.target.com`.
- [ ] **Revisión de `robots.txt`:** Buscar rutas ocultas o directorios sensibles.
- [ ] **Fuzzing de directorios:** Localizar carpetas como `/.env`, `/.git` o `/admin`.

2. Configuración y Autenticación
- [ ] **Políticas de contraseñas:** Verificar si permite claves débiles (ej. `123456`).
- [ ] **Enumeración de usuarios:** Comprobar si los mensajes de error revelan usuarios existentes.
- [ ] **Gestión de sesiones:** Verificar que la cookie de sesión tenga los atributos `HttpOnly` y `Secure`.
- [ ] **Fuerza bruta:** Validar si existe un límite de intentos de login (Rate Limiting).

3. Inyección y Manipulación
- [ ] **XSS (Reflejado/Persistente):** Probar payloads en campos de búsqueda y formularios.
- [ ] **SQL Injection:** Intentar romper queries en parámetros URL (ej. `?id=1'`).
- [ ] **Inyección de Cabeceras:** Probar `X-Forwarded-For` o `Host` para envenenamiento de caché.

4. Lógica de Negocio e IDOR
- [ ] **IDOR:** Cambiar IDs en la URL para acceder a datos de otros usuarios.
- [ ] **Salto de pasos:** Intentar acceder a la página de "pago exitoso" sin haber pagado.
- [ ] **Manipulación de valores:** Cambiar precios o cantidades en el cuerpo de la petición (Burp Suite).

### Inyecciones

[SQLi](inyecciones/sqli.md)

[XSS](inyecciones/xss.md)
