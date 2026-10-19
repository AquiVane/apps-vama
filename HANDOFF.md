# HANDOFF — apps-vama (frontend de la Tienda de apps VAMA)

Actualizado: 2026-10-19.

## Qué es
Frontend estático de `apps.vamatienda.com.ar` (repo público, Cloudflare Pages). Regla de este repo: nunca claves, secretos ni lógica de cobro o licencia; todo eso se valida en el servidor (Workers de `AquiVane/cosmart-workers`). Solo valores públicos por diseño (public key de MP, client-id de PayPal, clave PÚBLICA de firma de licencias).

## Verano en Juego (`juegos/`) — rama `claude/great-einstein-odu0bs`, SIN PUBLICAR (main no se toca hasta cargar los secrets del Worker)
App de 17 juegos offline, pago único ARS 3.500 / USD 2,28. Detalle en `juegos/README.md`. Backend ya desplegado (18/10): `verano-en-juego` + `mp-productos-ganadores` en `cosmart-workers`.
- Rutas: `/juegos/` (categoría), `/juegos/verano-en-juego/` (landing), `…/checkout/`, `…/ya-la-compre/`, `…/gracias/`, `/juegos/jugar/` (la app, UN archivo generado por `npm run build` desde `juegos/src/jugar/`; el que se commitea es SIEMPRE el de producción).
- Esto es una excepción a "sin build": solo `juegos/` usa Node (build de la app, marca blanca `npm run build:marca -- --cliente=x`, tests).
- **Para publicar** (en este orden): (1) cargar `INTERNAL_SECRET` y `BREVO_API_KEY` en el Worker `verano-en-juego` (workflow "Set Worker Secret" de cosmart-workers); (2) `curl https://verano-en-juego.conglomeradocosmart.workers.dev/api/license/public-key` → pegar `publica` en `juegos/config/firma-publica.txt` → `cd juegos && npm run build` → commit; (3) probar un cobro de punta a punta; (4) mergear a `main`.
- El repo `AquiVane/vama` (Academy) tiene una copia vieja de `juegos/` en su rama `claude/great-einstein-odu0bs`: NO mergearla, quedó obsoleta.
