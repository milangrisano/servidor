# Servidor

Infraestructura de despliegue del servidor de Enrique (hpserver test / Lenovo producción).

Contiene los `docker-compose` y configuraciones para los servicios del servidor.

## Servicios

| Servicio | Carpeta | Estado |
|---|---|---|
| **rclone** (Web GUI + API RC, backups/sync) | [`rclone/`](rclone/) | ✅ en desarrollo |

## Uso

Cada servicio tiene su propio `docker-compose.yml` y documentación en su carpeta.
Los archivos `.env.example` muestran las variables necesarias (copiar a `.env`
con permisos 0600; nunca versionar el `.env` real).