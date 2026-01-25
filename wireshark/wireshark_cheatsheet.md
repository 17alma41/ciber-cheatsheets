# 🦈 Wireshark Cheatsheet - Filtros Esenciales

Guía rápida para analizar tráfico de red.

## 🎯 1. Filtros Básicos (IP y Puertos)
Lo primero es aislar la conversación que te interesa.

| Filtro | Para qué sirve |
| :--- | :--- |
| `ip.addr == 10.10.10.15` | Muestra todo el tráfico de esa IP (tanto si entra como si sale). |
| `ip.src == 10.10.10.15` | Muestra solo el tráfico **enviado por** esa IP. |
| `ip.dst == 10.10.10.15` | Muestra solo el tráfico **recibido por** esa IP. |
| `tcp.port == 80` | Filtra todo el tráfico TCP en el puerto 80 (Web). |
| `udp.port == 53` | Filtra todo el tráfico UDP en el puerto 53 (DNS). |

## 🌐 2. Web y HTTP (Caza de Credenciales)
Vital para ver qué páginas se visitan y qué datos se envían.

| Filtro | Para qué sirve |
| :--- | :--- |
| `http` | Muestra solo paquetes HTTP (ignora el ruido TCP de conexión). |
| `http.request.method == "POST"` | **EL MEJOR.** Muestra formularios enviados (Login, Subida de archivos). ¡Aquí suelen estar las contraseñas! |
| `http.request.method == "GET"` | Muestra las peticiones de visualización de páginas o archivos. |
| `http.response.code == 200` | Muestra solo las respuestas exitosas del servidor. |
| `http.user_agent contains "Nmap"` | Detecta si alguien está escaneando con Nmap. |

## 🕵️ 3. Búsqueda de Texto y Archivos (Forensics)
Para encontrar "agujas en el pajar" (contraseñas, flags, usuarios).

| Filtro | Para qué sirve |
| :--- | :--- |
| `frame contains "password"` | Busca la palabra "password" dentro del contenido de **cualquier** paquete. |
| `frame contains "admin"` | Busca referencias al usuario admin. |
| `frame contains "HTB{"` | **CTF MODE.** Busca el formato de la bandera directamente en el tráfico. |
| `ftp` | Muestra tráfico FTP (donde las contraseñas viajan en texto plano). |
| `telnet` | Muestra tráfico Telnet (también texto plano). |

## ⚡ 4. Protocolos Windows (SMB/DNS)
Muy útil en máquinas Windows de HackTheBox.

| Filtro | Para qué sirve |
| :--- | :--- |
| `smb2` | Muestra tráfico de carpetas compartidas modernas. |
| `dns` | Muestra resoluciones de nombres (qué dominios se están buscando). |
| `kerberos` | Muestra autenticación de Windows (útil para ataques avanzados). |

## 🛠️ 5. Operadores Lógicos
Para combinar filtros y ser más preciso.

| Operador | Significado | Ejemplo |
| :--- | :--- | :--- |
| `&&` | **Y** (Ambas cosas deben ser verdad) | `ip.src == 10.10.10.5 && http` |
| `||` | **O** (Una de las dos es verdad) | `tcp.port == 80 || tcp.port == 443` |
| `!` | **NO** (Excluir algo) | `ip.addr == 10.10.10.5 && !arp` |

---

## Trucos de "Click Derecho" (Sin comandos)

A veces es mejor usar el ratón que escribir filtros:

### 1. Follow TCP/HTTP Stream (Seguir flujo)
Es la función más importante. Reconstruye la conversación completa como si fuera un chat.
* **Cómo:** Click derecho en un paquete interesante -> `Follow` -> `TCP Stream` (o `HTTP Stream`).
* **Resultado:** Ves el texto completo (HTML, contraseñas, scripts) limpio y ordenado.

### 2. Export Objects (Extraer archivos)
Si alguien descargó una imagen o un `.exe` por HTTP, Wireshark puede recuperarlo.
* **Cómo:** Menú `File` -> `Export Objects` -> `HTTP` (o SMB).
* **Resultado:** Una lista de archivos listos para guardar en tu disco.

### 3. Protocol Hierarchy (Estadísticas)
Para tener una vista de pájaro de qué está pasando.
* **Cómo:** Menú `Statistics` -> `Protocol Hierarchy`.
* **Resultado:** Te dice porcentajes (ej: "El 80% del tráfico es HTTP, el 20% es DNS"). Útil para detectar anomalías.