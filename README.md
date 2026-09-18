# uptime

Monitor de disponibilidad de los dominios publicos de SecretarIA.

Vive aqui y no en `Nodus-Development/secretar-ia` por una razon de facturacion:
los repos privados consumen minutos de Actions de la cuota de la organizacion,
los publicos no. Con cron cada 15 minutos son ~96 ejecuciones al dia y GitHub
factura un minimo de 1 minuto por job aunque la comprobacion tarde 10 segundos:
~1300 minutos al mes, el 44% de los 3000 que incluye el plan Team.

El 2026-09-18 la organizacion agoto la cuota (3013/3000) y GitHub dejo de
arrancar jobs en **todos** los repos. El monitor se apago solo, en silencio,
que es justo el modo de fallo que existe para evitar.

## Que comprueba

| Check | Esperado |
|---|---|
| `https://secretar-ia.org/healthz` | 200 |
| `https://app.secretar-ia.org/healthz` | 200 |
| `https://convex.secretar-ia.org/version` | 200 |
| `https://secretar-ia.org/precios` | 200 (no 404) |
| `https://secretar-ia.org/privacy` | 200 (no 404) — requisito OAuth Google |
| `https://secretar-ia.org/terms` | 200 (no 404) |
| `POST https://secretar-ia.org/` | 403 o 405 |

No golpea `/` nunca: la raiz esta cacheada en el borde de Cloudflare y
devolveria 200 con el origen muerto. `/healthz` tiene regla de bypass de cache.

## Contexto

El postmortem del incidente que motivo este monitor (landing caida 18 dias,
2026-08-07 → 2026-08-25) esta en el repo privado `secretar-ia`, en
`docs/LANDING_RAILWAY_INCIDENTE.md`. Referencia operativa en
`docs/landing-infra.md`.

Este repo es publico y no contiene secretos: solo peticiones GET/POST a
dominios que ya son publicos.
