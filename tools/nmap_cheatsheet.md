# Nmap Cheatsheet - Comandos Esenciales

Guía rápida de los parámetros más útiles de Nmap.

## 🚀 1. Escaneo de Puertos (Técnicas)
Define **cómo** se conecta Nmap a la máquina.

| Parámetro | Nombre | Para qué sirve |
| :--- | :--- | :--- |
| `-sS` | **SYN Scan** (Stealth) | **El estándar.** Rápido y "sigiloso". No completa la conexión TCP. Requiere `sudo`. |
| `-sT` | **Connect Scan** | Úsalo si **no tienes sudo**. Es más ruidoso (deja logs en la víctima). |
| `-sU` | **UDP Scan** | Para escanear servicios UDP (DNS, SNMP, NTP). **Ojo:** Es muy lento. |

## 🔍 2. Enumeración y Versiones
Saca información detallada de lo que has encontrado.

| Parámetro | Nombre | Para qué sirve |
| :--- | :--- | :--- |
| `-sV` | **Version Detection** | Intenta averiguar la **versión exacta** del servicio (ej. Apache 2.4.41). Vital para buscar exploits. |
| `-O` | **OS Detection** | Intenta adivinar qué **Sistema Operativo** corre la víctima (Windows, Linux, etc). |
| `-A` | **Aggressive Scan** | El "Todo en Uno". Activa detección de SO (`-O`), Versiones (`-sV`), Scripts (`-sC`) y Traceroute. |

## 🎯 3. Definición de Objetivos y Puertos
Define **qué** vas a escanear.

| Parámetro | Nombre | Para qué sirve |
| :--- | :--- | :--- |
| `-p-` | **All Ports** | Escanea los **65.535 puertos**. Si no lo pones, solo escanea los 1000 más comunes. |
| `-p 80,443` | **Specific Ports** | Escanea solo los puertos que tú le digas (separados por coma). |
| `-Pn` | **No Ping** | Asume que la máquina está encendida. Úsalo si Nmap dice *"Note: Host seems down"*. |
| `--top-ports 100` | **Top Ports** | Escanea solo los 100 puertos más populares (muy rápido). |

## 📜 4. Scripts (NSE)
El verdadero poder de Nmap.

| Parámetro | Nombre | Para qué sirve |
| :--- | :--- | :--- |
| `-sC` | **Default Scripts** | Lanza los scripts básicos y seguros (FTP anónimo, títulos HTTP, claves SSH por defecto...). |
| `--script vuln` | **Vuln Scan** | Busca vulnerabilidades conocidas (CVEs) en los servicios encontrados. |
| `--script "smb*"` | **Wildcard** | Ejecuta todos los scripts que empiecen por "smb" (útil para enumerar Windows). |

## ⚡ 5. Velocidad y Salida
Controla el tiempo y guarda tu trabajo.

| Parámetro | Nombre | Para qué sirve |
| :--- | :--- | :--- |
| `-T4` | **Timing 4** | Acelera el escaneo. Es el estándar para CTFs. (T1 es paranoico, T5 es demasiado agresivo). |
| `--min-rate 5000` | **Min Rate** | Fuerza a enviar paquetes muy rápido (útil si `-p-` tarda mucho). |
| `-oA nombre` | **Output All** | Guarda el resultado en 3 formatos: `.nmap` (texto), `.gnmap` (grepable) y `.xml`. **Úsalo siempre.** |
| `-v` | **Verbose** | Te muestra en pantalla lo que va encontrando en tiempo real, sin esperar al final. |

---

### 1. Escaneo Inicial (Rápido)
Para ver qué hay abierto rápidamente.
```bash
nmap -p- --open -sS --min-rate 5000 -n -Pn <IP>
```

### 2. Escaneo detallado (Clásico)
Una vez sabemos los puertos abiertos, hacemos un escaneo detallado.
```bash
nmap -p 22,80 -sC -sV -n -Pn <IP> -oN escaneo_detallado
```

### 3. Escaneo de vulnerabilidades
Para buscar fallos de seguridad automáticamente.
```bash
nmap -p 445 --script vuln -Pn <IP>
```