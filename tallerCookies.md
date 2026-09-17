# Taller: Cookies en aplicaciones web (React + Supabase)

**Entregable:** un archivo `docs/cookies.md` dentro del repositorio `workspace`, con la investigación de la Parte 1 respondida con sus propias palabras, más las capturas y el código pedidos en la Parte 2.

**Criterio de evaluación:** no se acepta copiar y pegar. Cada respuesta debe tener una captura propia (DevTools) o un ejemplo de código propio.

---

## Parte 1 — Investigación teórica

### 1. Qué es una cookie

- Definición: un pequeño par clave-valor que el servidor pide al navegador guardar y reenviar en peticiones futuras.
- ¿En qué se diferencia de `localStorage` y `sessionStorage`? Completa esta tabla con tus propias palabras:

| | Cookie | localStorage | sessionStorage |
|---|---|---|---|
| ¿Se envía automáticamente al servidor? | | | |
| ¿Sobrevive a cerrar el navegador? | | | |
| ¿Tiene expiración configurable? | | | |
| ¿Tamaño máximo aproximado? | | | |
| ¿Accesible desde JavaScript? | | | |

- ¿Por qué el HTTP es "sin estado" (*stateless*) y qué problema resuelven las cookies frente a eso?

### 2. Cómo se crea una cookie

- Qué encabezado HTTP usa el servidor para pedirle al navegador que guarde una cookie, y qué encabezado usa el navegador para reenviarla.
- Cómo se crea una cookie desde JavaScript en el navegador (`document.cookie`) y por qué esa API es incómoda de usar directamente.
- Diferencia entre una cookie creada por el **servidor** y una creada por **JavaScript del lado del cliente**.

### 3. Atributos de una cookie

Investiga y explica para qué sirve cada atributo, qué pasa si lo omites, y da un ejemplo de la línea completa del encabezado:

- `Expires` y `Max-Age`. ¿Qué es una **cookie de sesión** (sin ninguno de los dos)?
- `Domain` y `Path`. ¿Qué diferencia hay entre no poner `Domain` y ponerlo explícitamente?
- `Secure`. ¿Qué pasa si intentas usarla en un sitio servido por `http://`?
- `HttpOnly`. ¿Por qué esto es clave para mitigar ataques XSS? ¿Puede leerla `document.cookie`?
- `SameSite` (`Strict`, `Lax`, `None`). Explica con un ejemplo concreto de cada valor: ¿la cookie viaja si el usuario llega desde un enlace externo? ¿Y si un `<iframe>` de otro sitio te embebe? ¿Qué requisito extra exige el navegador cuando `SameSite=None`?
- `Partitioned` (CHIPS). ¿Qué problema nuevo intenta resolver?

### 4. Cookies de primera parte vs. terceros

- Qué es una **cookie de primera parte** (first-party) y una de **terceros** (third-party).
- Por qué los navegadores (Safari, Firefox, y ahora Chrome) han ido bloqueando cookies de terceros por defecto.
- Da un ejemplo real de uso legítimo de cookies de terceros y uno de uso para rastreo publicitario.

### 5. Sesiones y autenticación

- Qué es una **sesión** del lado del servidor y cómo se relaciona con la cookie que recibe el navegador (normalmente solo contiene un ID de sesión, no los datos).
- Diferencia entre autenticación basada en **sesión con cookie** y autenticación basada en **token JWT**.
  - ¿Dónde se suele guardar un JWT: cookie, `localStorage`, memoria?
  - ¿Cuáles son los riesgos de guardar un JWT en `localStorage` (pista: XSS) frente a guardarlo en una cookie `HttpOnly`?
  - ¿Qué riesgo tiene en cambio una cookie frente a **CSRF**, y por qué `localStorage` no tiene ese riesgo?
- Qué es un **refresh token** y por qué normalmente se guarda de forma más protegida que el token de acceso.

### 6. Seguridad: los dos ataques clásicos

- **XSS (Cross-Site Scripting):** explica cómo un script inyectado podría robar una cookie, y por qué `HttpOnly` lo evita.
- **CSRF (Cross-Site Request Forgery):** explica, con un ejemplo paso a paso, cómo un sitio malicioso podría aprovechar que el navegador envía cookies automáticamente para ejecutar una acción no deseada en otro sitio donde la víctima tiene sesión iniciada.
- ¿Cómo ayuda `SameSite=Lax` o `Strict` a mitigar CSRF? ¿Por qué no es una protección completa por sí sola (menciona los *CSRF tokens* como complemento)?

### 7. Cookies y privacidad / normativa

- Qué exige, en términos generales, el **RGPD (GDPR)** europeo y leyes similares (Colombia: Ley 1581 de 2012 y su reglamentación sobre tratamiento de datos) respecto al consentimiento de cookies.
- Diferencia entre cookies **estrictamente necesarias** (no requieren consentimiento) y cookies de **analítica/marketing** (sí lo requieren).
- Qué es un **banner/gestor de consentimiento de cookies** (cookie consent manager) y qué debe permitir hacer, como mínimo, a un usuario.

### 8. Cookies en el mundo real: Supabase

- Investiga cómo maneja Supabase Auth la sesión en el navegador: ¿usa `localStorage` por defecto o cookies? ¿Qué paquete (`@supabase/ssr` u otro) existe para manejar cookies cuando hay renderizado del lado del servidor (Next.js, por ejemplo)?
- ¿Por qué en una SPA pura de React (sin servidor propio) es más común que Supabase guarde el token en `localStorage` en vez de en una cookie `HttpOnly`?
- ¿Qué cabecera debe configurar Supabase para permitir que tu dominio de GitHub Pages haga peticiones (relación con el taller anterior de CORS)?

### 9. Herramientas del navegador

- ¿Dónde se ven las cookies de la pestaña activa en Chrome DevTools y en Firefox DevTools? (nombre exacto del panel)
- ¿Qué columnas muestra esa tabla y qué información da cada una?
- ¿Cómo se borra una cookie específica desde DevTools sin borrar todas?

---

## Parte 2 — Ejercicio práctico

### Ejercicio A — Inspeccionar cookies reales

1. Abre tu proyecto desplegado en GitHub Pages (o cualquier sitio con login, por ejemplo github.com) con DevTools abierto.
2. Ve a **Application** (Chrome) o **Storage** (Firefox) → **Cookies**.
3. Toma una captura mostrando al menos una cookie con sus columnas: `Name`, `Value`, `Domain`, `Path`, `Expires`, `Size`, `HttpOnly`, `Secure`, `SameSite`.
4. Identifica y anota en tu documento: ¿cuál cookie parece ser de sesión (`Session` en `Expires`) y cuál tiene fecha fija? ¿Alguna tiene `HttpOnly` marcado? ¿Puedes verla con `document.cookie` en la consola? Compruébalo y pega el resultado.

### Ejercicio B — Crear y leer cookies con JavaScript

En la consola de DevTools de cualquier página, ejecuta y documenta el resultado de cada línea:

```js
// Crear una cookie simple
document.cookie = "tema=oscuro; path=/; max-age=3600";

// Leer todas las cookies visibles a JS
console.log(document.cookie);

// Crear una cookie que expira en el pasado (forma de "borrarla")
document.cookie = "tema=; path=/; max-age=0";

console.log(document.cookie);
```

Responde: ¿por qué no existe un `document.cookie = ""` que borre todo de una vez? ¿Por qué tuviste que repetir el mismo `path`?

### Ejercicio C — Un mini helper de cookies en React

Crea un archivo `src/utils/cookies.js` en tu proyecto con funciones propias (no uses una librería) para `setCookie`, `getCookie` y `deleteCookie`, y decláralas con JSDoc explicando qué atributos aceptan. Luego, en un componente, úsalas para guardar la preferencia de tema claro/oscuro del usuario y que persista al recargar la página.

Requisitos mínimos del helper:
- `setCookie(nombre, valor, diasExpiracion, opciones)` — `opciones` debe permitir pasar `secure` y `sameSite`.
- `getCookie(nombre)` — debe devolver `null` si no existe.
- `deleteCookie(nombre)` — debe reutilizar `setCookie` con expiración pasada.

Pega el código final y una captura del selector de tema funcionando (cookie visible en DevTools tras recargar).

### Ejercicio D — Simular el problema de CSRF (conceptual, sin atacar nada real)

Sin ejecutar nada contra un sitio ajeno, escribe en tu documento el HTML que **hipotéticamente** podría usar un sitio malicioso para explotar CSRF contra un endpoint como `POST /api/transferir-dinero` que confía solo en la cookie de sesión (sin token CSRF ni verificación de `SameSite`). Es un ejercicio de análisis, no de ejecución.

Luego responde: si ese mismo endpoint estuviera protegido con `SameSite=Strict`, ¿el ataque seguiría funcionando? ¿Y con `SameSite=Lax`?

### Ejercicio E — Auditoría de tu propio proyecto

Revisa tu proyecto React + Supabase del taller anterior y responde:

1. ¿Dónde guarda Supabase el token de tu sesión actual? Búscalo en DevTools (Application → Local Storage y Application → Cookies).
2. ¿Qué pasaría con la sesión del usuario si alguien lograra inyectar un `<script>` en tu página (XSS) y el token estuviera donde lo encontraste? Justifica si tu proyecto está expuesto a ese riesgo y qué harías para mitigarlo (Content-Security-Policy, sanitizar inputs, etc.).

---

## Tabla resumen para pegar al final del documento

| Atributo/concepto | ¿Lo usa tu proyecto? | ¿Dónde lo verificaste? |
|---|---|---|
| `Secure` | | |
| `HttpOnly` | | |
| `SameSite` | | |
| Cookies de terceros | | |
| Banner de consentimiento | | |
| Token de Supabase: ¿cookie o localStorage? | | |

## Entrega

En `docs/cookies.md`:

1. Parte 1 completa, con la tabla comparativa cookie/localStorage/sessionStorage llena.
2. Capturas de los ejercicios A y C.
3. Código del helper de cookies (Ejercicio C).
4. Respuestas escritas de los ejercicios B, D y E.
5. La tabla resumen final llena.
