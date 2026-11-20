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

Siguen la convención del plugin de docker del servidor, con **dos carpetas base
de propósito distinto** (desde `rclone/.env`):

| Variable | Propósito | Uso en rclone |
|---|---|---|
| `PATH_TO_APPDATA` | **Configuración** de los servicios | `${PATH_TO_APPDATA}/rclone/config` → `/config` (`rclone.conf`, remotes) |
| `PATH_TO_DATA` | **Datos** que guardan/generan las apps | `${PATH_TO_DATA}/rclone` → `/data` (archivos, montajes) |

En hpserver (test) los valores van en el `.env`; en Lenovo/OMV son **variables
globales OMV** que apuntan al disco físico.

## Seguridad (producción)

- Cambiar `RC_PASS` a una contraseña fuerte.
- La **API RC (5573) equivale a shell** en el usuario rclone → exponer SOLO por
  red interna / Tailscale / proxy con TLS. No abrir a internet directo.
- Los remotes del GUI se guardan en `rclone.conf` (persistido en `/config`).