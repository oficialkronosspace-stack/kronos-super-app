# Despliegue en Cloudflare Pages

Este documento explica cómo configurar Cloudflare Pages para desplegar el frontend (Create React App) del monorepo.

Resumen rápido
- Install command: npm ci --prefix client
- Build command: npm run build --prefix client
- Output directory: client/build
- Node: 18 (se incluye `.nvmrc` en la raíz)

Pasos en la UI de Cloudflare Pages
1. En Pages > Deployments > Settings > Build settings:
   - Build command: `npm run build --prefix client`
   - Install command: `npm ci --prefix client`
   - Output directory: `client/build`
2. Asegúrate de seleccionar Node 18 si la UI lo permite. Cloudflare respeta `.nvmrc` en la raíz.

Variables de entorno y secrets
- Variables públicas para Create React App deben usar el prefijo `REACT_APP_` (p. ej. `REACT_APP_API_URL`).
- Si necesitas instalar paquetes privados desde npm, crea un secret llamado `NPM_TOKEN` en Cloudflare Pages y usa el archivo `.npmrc` (plantilla adjunta) para autenticar.

Archivo `.npmrc` (uso)
- No comitees tu token en texto plano.
- Añade `//registry.npmjs.org/:_authToken=${NPM_TOKEN}` en un archivo `.npmrc` en la raíz durante el build (Cloudflare Pages puede usar variables de entorno para sustituir `NPM_TOKEN`).

Notas sobre el backend (server)
- Cloudflare Pages es para sitios estáticos (frontend). El backend (directorio `server/`) no cuenta con un script `build` y debe ejecutarse en un servicio separado (por ejemplo: Render, Railway, Fly, Heroku) o convertirse a Workers si quieres mantener todo en Cloudflare.

Prueba local rápida
1. Desde la raíz del repo:
   - `npm ci`
   - `npm run build --prefix client`
2. Verifica que `client/build/index.html` existe.

Si quieres, puedo añadir una GitHub Action de verificación (opcional) que haga `npm ci` y `npm run build --prefix client` para chequear que el build pasa en CI.
