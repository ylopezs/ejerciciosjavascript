# Taller: Publicación de proyectos React + Supabase

**Entregable:** un archivo `docs/despliegue.md` (o `DESPLIEGUE.md`) dentro del repositorio `workspace` de cada estudiante, con la investigación de la Parte 1 respondida **con sus propias palabras** y citando las fuentes consultadas.

**Criterio de evaluación:** no se acepta copiar y pegar. Cada respuesta debe tener un ejemplo propio (una captura, un comando ejecutado, un dominio real consultado, etc.).

---

## Parte 1 — Investigación teórica

### 1. Cómo llega un usuario a tu sitio

Investiga y explica:

- Qué pasa, paso a paso, desde que alguien escribe una URL en el navegador hasta que ve la página. Menciona: resolución DNS, conexión TCP, handshake TLS, petición HTTP, respuesta.
- Partes de una URL: esquema, subdominio, dominio, TLD, puerto, ruta, query string, fragmento.
- Diferencia entre **dominio**, **subdominio** y **hosting**. ¿Por qué se contratan por separado?

### 2. DNS

- Qué es el DNS y por qué se le llama "la agenda de contactos de internet".
- Jerarquía: root servers → TLD servers → servidores autoritativos → resolver recursivo.
- Tipos de registro. Explica para qué sirve cada uno y da un ejemplo del valor que llevaría:
  - `A` y `AAAA`
  - `CNAME`
  - `ALIAS` / `ANAME` (y por qué existen si ya hay CNAME)
  - `MX`
  - `TXT` (incluye SPF, DKIM y verificación de propiedad de dominio)
  - `NS`
  - `SOA`
- Qué es el **TTL** y por qué un cambio de DNS "no se ve de inmediato".
- Qué es la **propagación DNS** y cuánto suele tardar realmente.
- **Ejercicio práctico:** ejecuta estos comandos contra un dominio real y pega la salida en tu documento.
  ```bash
  nslookup github.io
  dig github.com A
  dig github.com MX
  dig +trace anthropic.com
  ```
  (En Windows sin `dig`, usa `nslookup -type=MX github.com` o https://dnschecker.org)

### 3. Dominios

- Qué es un **registrador** (registrar) y qué diferencia hay con un proveedor de DNS y con un hosting.
- TLD genéricos vs. de código de país (`.com`, `.dev`, `.app`, `.co`, `.com.co`). ¿Cuáles tienen requisitos especiales?
- Qué es **WHOIS** y qué es la privacidad de dominio.
- Qué son los **nameservers** y qué significa "apuntar el dominio a otro proveedor".
- Investiga precios reales de un `.com` y un `.co` en al menos dos registradores. ¿Por qué el precio de renovación suele ser distinto al del primer año?

### 4. HTTPS y certificados

- Qué es TLS/SSL y qué protege exactamente.
- Qué es una **Autoridad Certificadora (CA)** y qué es **Let's Encrypt**.
- Diferencia entre certificado DV, OV y EV.
- Qué es un certificado **wildcard**.
- Qué significa el error "certificado no válido para este nombre" y por qué aparece al configurar un dominio personalizado.
- Qué es HSTS.

### 5. Modelos de alojamiento

Explica cada modelo, con ventajas, desventajas y un caso típico de uso:

| Modelo | Ejemplos | ¿Cuándo usarlo? |
|---|---|---|
| Hosting compartido | Hostinger, cPanel | |
| VPS | DigitalOcean Droplet, Linode, AWS EC2 | |
| Servidor dedicado | OVH, Hetzner | |
| PaaS | Render, Railway, Heroku, Fly.io | |
| Serverless / Functions | Vercel Functions, AWS Lambda, Supabase Edge Functions | |
| Hosting estático + CDN | GitHub Pages, Netlify, Cloudflare Pages, Vercel | |
| BaaS | Supabase, Firebase, Appwrite | |

Además:

- Qué es un **CDN** y qué problema resuelve (latencia, carga del origen, caché).
- Diferencia entre **sitio estático** y **sitio dinámico/renderizado en servidor**.
- Por qué una app React creada con Vite o CRA es, al compilarse, un **sitio estático**.
- Qué es una **SPA** y por qué el enrutamiento de una SPA da problemas en hostings estáticos (error 404 al recargar en `/dashboard`).

### 6. Comparativa de plataformas

Compara **GitHub Pages, Netlify, Vercel, Cloudflare Pages y Render** en una tabla con estas columnas:

- Plan gratuito: límites de ancho de banda, builds y tamaño.
- ¿Soporta variables de entorno en el build?
- ¿Soporta rutas SPA sin configuración extra?
- ¿Soporta backend / funciones serverless?
- ¿Dominio personalizado gratis? ¿HTTPS automático?
- ¿Preview deployments por Pull Request?

Al final, escribe un párrafo: **¿cuál elegirías para tu proyecto y por qué?**

### 7. Supabase en producción

- Qué es un **BaaS** y qué componentes ofrece Supabase (Postgres, Auth, Storage, Realtime, Edge Functions).
- Diferencia entre la **`anon key`** y la **`service_role` key**. ¿Cuál puede ir en el frontend y cuál **jamás**?
- Qué es **RLS (Row Level Security)**. ¿Por qué es obligatorio activarlo si la `anon key` viaja en el navegador?
- Escribe una política RLS de ejemplo que permita a un usuario leer y editar **solo sus propias filas**.
- Configuración de **CORS** y de **Site URL / Redirect URLs** en Supabase Auth: qué pasa si no configuras la URL de producción.
- Qué límites tiene el plan gratuito de Supabase (proyectos pausados por inactividad, tamaño de base de datos, etc.).

### 8. Variables de entorno y seguridad

- Qué es una variable de entorno y por qué `.env` va en `.gitignore`.
- Por qué en Vite las variables deben empezar con `VITE_` y qué implica eso: **todo lo que lleve el prefijo termina dentro del bundle y es público**.
- Diferencia entre un secreto de build (GitHub Secrets) y un secreto de runtime en servidor.
- Qué hacer si subiste una clave por error a Git (rotar la clave, no basta con borrar el commit).

### 9. Build y despliegue

- Qué hace `npm run build` y qué contiene la carpeta `dist/`.
- Qué son *minificación*, *tree shaking*, *code splitting* y *hashing* de nombres de archivo.
- Qué es **CI/CD** y qué es **GitHub Actions**.
- Diferencia entre desplegar desde una rama (`gh-pages`) y desplegar con un workflow de Actions.

---

## Parte 2 — Tutorial: publicar tu proyecto React + Supabase en GitHub Pages

> Supuestos: proyecto creado con **Vite + React**, repositorio en GitHub, Supabase ya configurado y funcionando en local.

### Paso 0 — Verifica que el proyecto compila

```bash
npm install
npm run build
npm run preview
```

Si `npm run build` falla, **no sigas**. Arregla los errores primero. `preview` te muestra el build real en `http://localhost:4173`.

### Paso 1 — Entiende la URL que vas a tener

GitHub Pages te dará una de estas dos:

| Tipo de repo | URL resultante | `base` de Vite |
|---|---|---|
| `usuario.github.io` | `https://usuario.github.io/` | `/` |
| `usuario/mi-proyecto` | `https://usuario.github.io/mi-proyecto/` | `/mi-proyecto/` |

El segundo caso es el habitual, y es la causa del error más común del taller: **la página carga en blanco porque busca los assets en `/assets/...` en vez de `/mi-proyecto/assets/...`**.

### Paso 2 — Configura `base` en Vite

En `vite.config.js`:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
  plugins: [react()],
  base: '/mi-proyecto/', // ← el nombre EXACTO del repositorio, con barras al inicio y al final
})
```

Verifica con `npm run build` y abre `dist/index.html`: las rutas de `<script>` y `<link>` deben empezar con `/mi-proyecto/`.

### Paso 3 — Arregla el enrutamiento de la SPA

GitHub Pages sirve archivos estáticos: no sabe que `/mi-proyecto/dashboard` debe entregar `index.html`. Si recargas ahí, verás un 404. Tienes dos opciones:

**Opción A — HashRouter (la más simple, recomendada para el taller)**

```jsx
import { HashRouter } from 'react-router-dom'

<HashRouter>
  <App />
</HashRouter>
```

Las URLs quedan como `https://usuario.github.io/mi-proyecto/#/dashboard`. Funciona siempre, sin trucos.

**Opción B — BrowserRouter + copia de `404.html`**

Mantén `BrowserRouter` con el `basename` correcto:

```jsx
<BrowserRouter basename="/mi-proyecto">
```

Y haz que el build genere un `404.html` idéntico al `index.html`, para que GitHub Pages devuelva la app en cualquier ruta. En `package.json`:

```json
"scripts": {
  "build": "vite build && cp dist/index.html dist/404.html"
}
```

(En Windows sin bash, usa el paquete `shx`: `shx cp dist/index.html dist/404.html`.)

### Paso 4 — Variables de entorno de Supabase

En local tienes un `.env` (que **no** está en Git):

```
VITE_SUPABASE_URL=https://xxxxxxxx.supabase.co
VITE_SUPABASE_ANON_KEY=eyJhbGciOi...
```

Y tu cliente:

```js
import { createClient } from '@supabase/supabase-js'

export const supabase = createClient(
  import.meta.env.VITE_SUPABASE_URL,
  import.meta.env.VITE_SUPABASE_ANON_KEY
)
```

Como GitHub Pages solo sirve archivos, estas variables deben inyectarse **durante el build**. Guárdalas como secretos del repositorio:

`Settings` → `Secrets and variables` → `Actions` → `New repository secret`

Crea `VITE_SUPABASE_URL` y `VITE_SUPABASE_ANON_KEY`.

> ⚠️ **Importante:** aunque uses secretos, la `anon key` queda visible en el JavaScript publicado. Eso es normal y está previsto por Supabase: **la seguridad real la da RLS**, no ocultar la clave. Lo que nunca debe salir de tu servidor es la `service_role` key.

### Paso 5 — Activa GitHub Pages con Actions

`Settings` → `Pages` → en **Source**, selecciona **GitHub Actions**.

### Paso 6 — Crea el workflow

Archivo `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - run: npm ci

      - run: npm run build
        env:
          VITE_SUPABASE_URL: ${{ secrets.VITE_SUPABASE_URL }}
          VITE_SUPABASE_ANON_KEY: ${{ secrets.VITE_SUPABASE_ANON_KEY }}

      - uses: actions/configure-pages@v5

      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./dist

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

### Paso 7 — Despliega

```bash
git add .
git commit -m "chore: configurar despliegue en GitHub Pages"
git push origin main
```

Ve a la pestaña **Actions** y observa el workflow. Si todo sale bien, la URL aparece en `Settings` → `Pages`.

### Paso 8 — Configura Supabase para la URL de producción

En el panel de Supabase → `Authentication` → `URL Configuration`:

- **Site URL:** `https://usuario.github.io/mi-proyecto/`
- **Redirect URLs:** añade también `https://usuario.github.io/mi-proyecto/**` y tu `http://localhost:5173/**`.

Si usas `HashRouter`, incluye la variante con `#`. Sin esto, el login por email o por OAuth te devolverá a la URL equivocada.

Revisa además que tus políticas **RLS estén activas** en todas las tablas. Prueba desde una ventana de incógnito, sin sesión, que no puedas leer datos que no deberías.

### Paso 9 — (Opcional) Dominio personalizado

1. Compra el dominio en un registrador (Namecheap, Cloudflare, GoDaddy...).
2. En `Settings` → `Pages` → **Custom domain**, escribe tu dominio. Esto crea un archivo `CNAME` en el repo.
3. Configura el DNS en tu registrador:

   **Para un subdominio** (`www.midominio.com` o `app.midominio.com`):
   ```
   Tipo: CNAME   Nombre: www   Valor: usuario.github.io
   ```

   **Para el dominio raíz** (`midominio.com`), registros `A`:
   ```
   185.199.108.153
   185.199.109.153
   185.199.110.153
   185.199.111.153
   ```
   Y si tu proveedor soporta IPv6, los `AAAA`:
   ```
   2606:50c0:8000::153
   2606:50c0:8001::153
   2606:50c0:8002::153
   2606:50c0:8003::153
   ```
4. Espera la propagación y marca **Enforce HTTPS** cuando GitHub emita el certificado (puede tardar hasta 24 h).
5. Con dominio propio, la app ya vive en la raíz: **cambia `base` a `'/'`** en `vite.config.js` y actualiza las URLs en Supabase.

---

## Errores frecuentes y cómo diagnosticarlos

| Síntoma | Causa probable | Solución |
|---|---|---|
| Página en blanco, 404 en `/assets/index-xxx.js` | `base` mal configurado | Ajusta `base` al nombre del repo |
| 404 al recargar en una ruta interna | Enrutamiento SPA | `HashRouter` o `404.html` |
| `supabaseUrl is required` en producción | Secretos no inyectados en el build | Revisa el bloque `env:` del workflow |
| Login redirige a `localhost` | Site URL sin configurar | Ajusta URL Configuration en Supabase |
| `Failed to fetch` / error CORS | Dominio no permitido | Verifica la URL del proyecto Supabase |
| El sitio no se actualiza | Caché del navegador o del CDN | Recarga forzada (Ctrl+Shift+R) |
| Datos visibles sin iniciar sesión | RLS desactivado | Activa RLS y define políticas |

---

## Entrega

En el repositorio `workspace`, sube:

1. `docs/despliegue.md` con toda la Parte 1 respondida, incluyendo las capturas de los comandos `dig`/`nslookup` y la tabla comparativa de plataformas.
2. La URL pública del proyecto desplegado.
3. Captura del workflow de Actions en verde.
4. Captura de las políticas RLS activas en Supabase.
