# 08 — Despliegue y arquitectura Web ↔ Colyseus

## Qué se despliega dónde

| Pieza | Despliegue | Tipo de servicio |
|---|---|---|
| App web (interfaz) | Contenedor web Nitro / SSR | Estático o SSR con Node/Nitro |
| Servidor de juego Colyseus | Proceso Node persistente con WebSocket | Servicio de larga duración persistente |
| PostgreSQL | Servicio de base de datos | Persistencia de partidas y catálogo |

El servidor de juego requiere un proceso de larga duración para mantener salas en memoria y conexiones WebSocket activas. Por eso la app web y el servidor Colyseus se despliegan como servicios dedicados (p. ej. orquestados con Docker Compose).

## Conexión

- App web: una única variable pública `VITE_GAME_SERVER_URL` (p. ej. `https://game.midominio.com`). El SDK deriva `wss://`.
- Sin pasarela intermedia: el navegador habla directo con el servidor de juego.
- El servidor admite solo orígenes en `ALLOWED_ORIGINS` (dominio publicado, previsualización, `http://localhost:8080`).
- Si el servidor no responde, la app muestra un error real; nunca datos simulados.

## Variables de entorno (servidor, privadas)

`PORT`, `DATABASE_URL`, `TOKEN_SECRET`, `ALLOWED_ORIGINS`, `MEDIA_DIR`, `CONTENT_DIR`, `LOG_LEVEL`. Se entrega `deploy/.env.example` sin valores reales.

## Opciones de alojamiento del servidor

| Opción | Pros | Contras |
|---|---|---|
| **VPS + Docker Compose + Caddy/Nginx** (recomendada) | 100 % open source, reproducible, TLS automático, económico | Mantenimiento propio |
| Fly.io | Despliegue sencillo, WebSocket OK | Plataforma externa |
| Railway / Render | Muy sencillo, Postgres incluido | Coste variable |

Una sola instancia con muchas salas aisladas. Sin escalado horizontal hasta medir necesidad.

## Entornos

- Local: `docker compose up` (Postgres + servidor) + `bun dev` (web).
- Staging y producción: `deploy/docker-compose.yml` con Caddy/reverse proxy.
- Criterio final: partida completa con varios dispositivos reales en un despliegue independiente y autohospedado.
