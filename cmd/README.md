# innernet cmd — documentación

Terminal interactiva para InnerNet. Accesible en `cmd.inn` e `innernet.qzz.io/cmd`.

---

## Auth

| Comando | Descripción |
|---|---|
| `login <user> <pass>` | Iniciar sesión |
| `register <user> <pass>` | Crear cuenta nueva |
| `logout` | Cerrar sesión |
| `whoami` | Ver usuario y balance actual |

La sesión se guarda en `localStorage` y persiste entre recargas.

---

## LUCKS ⚡

| Comando | Descripción |
|---|---|
| `balance` | Ver saldo actual |
| `transfer <user> <amount>` | Transferir LUCKS a otro usuario |
| `addlucks <user> <amount> <apikey>` | Añadir LUCKS (requiere API key de administrador) |

---

## Dominios

| Comando | Descripción |
|---|---|
| `domains` | Listar tus dominios registrados |
| `register <domain>` | Registrar dominio (cuesta 100 ⚡) |
| `cname <domain> <url>` | Configurar CNAME record |
| `mx <domain> <record>` | Configurar MX record |
| `whois <domain>` | Ver info de cualquier dominio |
| `search <query>` | Buscar dominios (máx. 20 resultados) |
| `tlds` | Listar extensiones disponibles |

Extensiones disponibles: `.lbc` `.green` `.party` `.cc` `.inn` `.abc` `.ai`

---

## Hosting

| Comando | Descripción |
|---|---|
| `visit <domain>` | Ver página en iframe embebido |
| `visit <domain> --text` | Ver página como texto plano |

El dominio debe tener CNAME configurado apuntando a un proyecto de host.lbc.

---

## Proyectos (estilo git)

Los proyectos funcionan como repositorios: editas localmente, luego haces push.

| Comando | Descripción |
|---|---|
| `projects` | Listar proyectos |
| `init <name>` | Crear proyecto nuevo y activarlo |
| `cd <name\|id>` | Cambiar de proyecto activo |
| `ls` | Listar archivos del proyecto activo |
| `add <path> <html>` | Stagear archivo localmente |
| `push` | Subir todos los archivos staged a own-net |

### Flujo típico

```
innernet ❯ cd mi-sitio
switched to: mi-sitio

mi-sitio ❯ add index.html <h1>Hola InnerNet</h1>
staged: index.html

mi-sitio ❯ push
pushed: index.html
1/1 files pushed
```

Los archivos staged se guardan en memoria (se pierden al recargar). Hacer `push` los sube al servidor y limpia el stage.

---

## KV Store

Requiere un proyecto activo (`cd <name>`).

| Comando | Descripción |
|---|---|
| `kv get <key>` | Leer valor |
| `kv set <key> <value>` | Escribir valor |
| `kv del <key>` | Eliminar clave |

Límite por proyecto: 100KB (o 1MB con extra activado por @Luciano).

---

## Mail

| Comando | Descripción |
|---|---|
| `mail create <addr> <pass>` | Crear cuenta de correo |
| `mail inbox <addr> <pass>` | Ver bandeja de entrada |
| `mail send <from> <pass> <to> <subj> <body>` | Enviar correo |

El dominio del correo debe ser tuyo y tener MX record configurado a `mail.host`.

---

## Atajos de teclado

| Tecla | Acción |
|---|---|
| `↑` / `↓` | Navegar historial de comandos |
| `Tab` | Autocompletar comando |
| `Enter` | Ejecutar |

---

## Notas técnicas

- Backend: `own-net.vercel.app`
- Auth: JWT de 30 días guardado en `localStorage`
- Los comandos que requieren login fallan con `not logged in` si no hay sesión activa
- `addlucks` nunca expone la API key — se pasa directamente al header `x-api-key`
