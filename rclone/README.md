# Rclone

Contenedor de [rclone](https://rclone.org) con su **Web GUI** oficial y API RC,
para backups/sync a la nube. `rclone` es el primer componente de este repo.

## Comando actual (2026)

`rclone gui` (NO `rclone rcd --rc-web-gui`, que está obsoleto y aborta).

## Levantar

```bash
cd rclone
cp .env.example .env   # ajustar RC_USER / RC_PASS
docker compose up -d
```

## Acceso

- **Web GUI:** `http://<host>:5572`
- **API RC:** `http://<host>:5573`
- Usuario/contraseña: del `.env` (default `admin` / `rclone123` en dev).

> ⚠️ La URL de login debe incluir el parámetro `url=` apuntando a la **IP/dominio
> real** del host (no `[::]`):
> `http://<host>:5572/login?user=<u>&pass=<p>&url=http%3A%2F%2F<host>%3A5573%2F`

## Rutas / volúmenes

Se derivan de la variable **`PATH_TO_DATA`** (se concatena igual que el resto de
los compose del servidor). En `rclone/.env` se define:

```yaml
volumes:
  - ${PATH_TO_DATA}/rclone/config:/config
  - ${PATH_TO_DATA}/rclone:/data
```

| Ruta (host) | Dentro del contenedor | Uso |
|---|---|---|
| `${PATH_TO_DATA}/rclone/config` | `/config` | `rclone.conf` (remotes, persistente) |
| `${PATH_TO_DATA}/rclone` | `/data` | datos/archivos |

En hpserver (test) el valor está en el `.env`; en Lenovo/OMV es la **variable
global OMV** que apunta al disco físico (p. ej. `/srv/dev-disk-...`).

## Seguridad (producción)

- Cambiar `RC_PASS` a una contraseña fuerte.
- La **API RC (5573) equivale a shell** en el usuario rclone → exponer SOLO por
  red interna / Tailscale / proxy con TLS. No abrir a internet directo.
- Los remotes del GUI se guardan en `rclone.conf` (persistido en `/config`).