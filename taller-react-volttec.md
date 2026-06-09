# Taller: Portafolio Web con React y Bootstrap
### Empresa VoltTec — Servicios Eléctricos Residenciales y Comerciales

---

## Información general

| | |
|---|---|
| **Tema** | Componentes React + Bootstrap |
| **Duración estimada** | 3 horas |
| **Prerequisitos** | HTML, CSS básico |
| **Repositorio** | `workspace/` → carpeta con la fecha del día (ej. `2025-06-09/`) |

---

## Objetivos de aprendizaje

Al finalizar este taller el estudiante será capaz de:

1. Crear y usar **componentes funcionales** en React
2. Renderizar listas de datos con el método **`.map()`**
3. Manejar **estado local** con el hook `useState`
4. Integrar **Bootstrap 5** en un proyecto React (Vite)
5. Entender la **composición de componentes** (cómo una app se arma de piezas)

---

## Configuración del proyecto con Vite

Antes de recibir los archivos del taller, debes crear la aplicación base con Vite y dejarla lista para trabajar.

### Requisitos previos

Verifica que tienes Node.js instalado. Abre una terminal y ejecuta:

```bash
node -v
npm -v
```

Debes ver algo como `v18.x.x` y `9.x.x` o superior. Si no tienes Node.js, descárgalo desde [nodejs.org](https://nodejs.org).

---

### Paso 1 — Crear la app con Vite

En tu terminal, navega a la carpeta `workspace/` y crea la app dentro de una carpeta con la fecha de hoy:

```bash
npm create vite@latest 2025-06-09 -- --template react
```

> Reemplaza `2025-06-09` con la fecha real del día en formato `YYYY-MM-DD`.

Vite te confirmará que creó el proyecto. Verás una salida como esta:

```
✔ Project name: 2025-06-09
✔ Select a framework: React
✔ Select a variant: JavaScript

Scaffolding project in ./2025-06-09...
Done.
```

---

### Paso 2 — Entrar a la carpeta e instalar dependencias

```bash
cd 2025-06-09
npm install
```

Esto descarga React y Vite. Toma unos segundos.

---

### Paso 3 — Instalar Bootstrap

Con Bootstrap como dependencia de npm, se importa directo en el código sin necesitar un `<link>` en el HTML:

```bash
npm install bootstrap
```

---

### Paso 4 — Limpiar los archivos de ejemplo

Vite genera una app de demostración que no necesitamos. Limpia lo siguiente:

**Reemplaza todo el contenido de `src/App.jsx`** con esto:

```jsx
function App() {
  return (
    <div>
      <h1>Hola VoltTec</h1>
    </div>
  );
}

export default App;
```

**Reemplaza todo el contenido de `src/main.jsx`** con esto:

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import 'bootstrap/dist/css/bootstrap.min.css'
import './index.css'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
  <StrictMode>
    <App />
  </StrictMode>,
)
```

> La línea `import 'bootstrap/dist/css/bootstrap.min.css'` es todo lo que se necesita para activar Bootstrap en el proyecto.

**Borra el archivo `src/App.css`** (ya no lo usaremos).

**Vacía el archivo `src/index.css`** (déjalo en blanco por ahora; el profesor lo reemplazará con los estilos del proyecto).

---

### Paso 5 — Verificar que todo funciona

```bash
npm run dev
```

Abre el navegador en `http://localhost:5173`. Debes ver el texto **"Hola VoltTec"** en pantalla.

Si lo ves, ✅ tu entorno está listo.

---

### Paso 6 — Crear la carpeta de componentes

Dentro de `src/`, crea la carpeta `components/`:

```bash
mkdir src/components
```

En Windows (si no usas Git Bash):

```
md src\components
```

---

### Paso 7 — Copiar los archivos del taller

El profesor entregará los siguientes archivos. Cópialos en las rutas indicadas:

| Archivo entregado | Destino en tu proyecto |
|---|---|
| `App.jsx` | `src/App.jsx` (reemplaza el tuyo) |
| `index.css` | `src/index.css` (reemplaza el tuyo) |
| `Navbar.jsx` | `src/components/Navbar.jsx` |
| `Header.jsx` | `src/components/Header.jsx` |
| `Footer.jsx` | `src/components/Footer.jsx` |
| `Services.jsx` | `src/components/Services.jsx` |
| `WhyUs.jsx` | `src/components/WhyUs.jsx` |
| `Contact.jsx` | `src/components/Contact.jsx` |

Después de copiar los archivos, ejecuta de nuevo:

```bash
npm run dev
```

Debes ver la página completa de VoltTec con el hero, el navbar y el footer ya funcionando.

---

### ¿Qué hace cada archivo que generó Vite?

| Archivo | Para qué sirve |
|---|---|
| `index.html` | Página HTML base. Tiene el `<div id="root">` donde React monta la app. |
| `vite.config.js` | Configuración de Vite (no necesitas tocarlo). |
| `package.json` | Lista de dependencias y scripts del proyecto. |
| `src/main.jsx` | Punto de entrada: conecta React con el `index.html`. |
| `src/App.jsx` | Componente raíz que ensambla todos los demás. |
| `src/index.css` | Estilos globales de la marca. |
| `src/components/` | Carpeta donde viven todos los componentes. |

---

## Estructura del proyecto

Al abrir el repositorio encontrarás esta estructura:

```
workspace/
├── index.html
├── package.json
├── vite.config.js
└── src/
    ├── main.jsx          ← punto de entrada (no modificar)
    ├── App.jsx           ← componente raíz (no modificar)
    ├── index.css         ← estilos globales de la marca (no modificar)
    └── components/
        ├── Navbar.jsx    ✅ ENTREGADO — estudiar, no modificar
        ├── Header.jsx    ✅ ENTREGADO — estudiar, no modificar
        ├── Footer.jsx    ✅ ENTREGADO — estudiar, no modificar
        ├── Services.jsx  🛠️ COMPLETAR — Actividad 1
        ├── WhyUs.jsx     🛠️ COMPLETAR — Actividad 2
        └── Contact.jsx   🛠️ COMPLETAR — Actividad 3
```

---

## Cómo correr el proyecto (días siguientes)

Si ya tienes el proyecto configurado y solo quieres volver a abrirlo, entra a la carpeta del proyecto y ejecuta:

```bash
npm run dev
```

Luego abre tu navegador en `http://localhost:5173`.

> Cada vez que guardes un archivo, el navegador se actualiza automáticamente. Esto se llama **Hot Module Replacement (HMR)**, una de las principales ventajas de Vite.

---

## Antes de empezar — Componentes entregados

Antes de tocar cualquier actividad, **lee** los tres componentes entregados. No los modifiques, pero sí analiza cómo están escritos.

### Navbar.jsx — Menú de navegación

Abre el archivo y responde en tu cuaderno:

1. ¿Qué hace el hook `useState` en este componente? ¿Qué valores puede tomar la variable `menuAbierto`?
2. ¿Para qué sirve el `useEffect`? ¿Qué evento del navegador está escuchando?
3. ¿Cómo se pasa una función como prop a un evento en JSX? Busca un ejemplo en el código.
4. ¿Qué diferencia hay entre `class` en HTML y `className` en JSX?

### Header.jsx — Jumbotron / Hero

Abre el archivo y responde:

1. ¿Dónde está el array `estadisticas`? ¿Qué forma tiene cada objeto dentro de él?
2. ¿Cómo se usa `.map()` para renderizar las tarjetas de estadísticas?
3. ¿Para qué sirve el parámetro `index` en el `.map()`? ¿Dónde se usa en el JSX?

### Footer.jsx — Pie de página

Abre el archivo y responde:

1. ¿Cuántos arrays de datos tiene este componente? Nómbralos.
2. ¿Qué hace `new Date().getFullYear()`? ¿Por qué es útil usarlo así?
3. ¿Puedes encontrar un `.map()` anidado dentro de otro? ¿Dónde?

---

## Actividad 1 — Sección de Servicios (`Services.jsx`)

**Archivo:** `src/components/Services.jsx`

### Contexto

VoltTec ofrece servicios eléctricos tanto para hogares como para empresas. Tu tarea es poblar y mostrar la sección de servicios de su portafolio.

### Paso 1 — Define el array de datos

Dentro del archivo, completa el array `servicios`. Debe tener **mínimo 6 objetos**, uno por cada servicio. Cada objeto tiene esta forma:

```js
{
  icono: '⚡',
  titulo: 'Instalaciones residenciales',
  descripcion: 'Realizamos instalaciones eléctricas completas para viviendas nuevas y remodelaciones, cumpliendo la normativa RETIE vigente.',
}
```

Algunos servicios que puedes incluir (puedes inventar otros):

- Instalaciones residenciales
- Instalaciones comerciales e industriales
- Mantenimiento preventivo y correctivo
- Tableros eléctricos y breakers
- Iluminación LED y domótica
- Plantas eléctricas y UPS
- Redes de voz y datos
- Puestas a tierra

### Paso 2 — Renderiza las tarjetas con `.map()`

Dentro del `return()`, en el `<div className="row g-4">`, escribe el código para recorrer el array y mostrar una tarjeta Bootstrap por cada servicio.

Estructura sugerida para cada tarjeta:

```jsx
<div className="col-md-6 col-lg-4">
  <div className="card h-100 border-0 shadow-sm">
    <div className="card-body p-4">
      <div style={{ fontSize: '2.5rem' }}>{/* icono */}</div>
      <h5 className="card-title fw-bold mt-2">{/* titulo */}</h5>
      <p className="card-text text-muted">{/* descripcion */}</p>
    </div>
  </div>
</div>
```

### Paso 3 — Escribe el subtítulo de la sección

En el párrafo del encabezado, escribe una frase que describa brevemente lo que hace VoltTec. Máximo 2 oraciones.

### Criterios de entrega

- [ ] El array tiene mínimo 6 servicios con los 3 campos requeridos
- [ ] Se usa `.map()` para renderizar (no HTML copiado y pegado 6 veces)
- [ ] Cada elemento del `.map()` tiene la prop `key`
- [ ] Las tarjetas se ven en 3 columnas en pantallas grandes y 2 en tabletas

---

## Actividad 2 — Sección "¿Por qué elegirnos?" (`WhyUs.jsx`)

**Archivo:** `src/components/WhyUs.jsx`

### Contexto

Esta sección debe convencer al visitante de contratar a VoltTec. Tiene dos columnas: una imagen representativa y un texto con las ventajas competitivas de la empresa.

### Paso 1 — Define el array de razones

Completa el array `razones` con **mínimo 4 objetos**:

```js
{
  icono: '🏆',
  titulo: 'Técnicos certificados',
  descripcion: 'Todo nuestro personal cuenta con certificación RETIE y se actualiza constantemente.',
}
```

Algunas ideas:

- Certificación RETIE
- Garantía de 1 año en mano de obra
- Respuesta en menos de 24 horas
- Materiales de primera calidad
- Presupuesto sin costo
- Atención 24/7 para emergencias

### Paso 2 — Agrega la imagen

En la columna izquierda, agrega una imagen con la etiqueta `<img>`. Puedes usar este placeholder mientras consigues una imagen real:

```jsx
<img
  src="https://placehold.co/500x400/1a1e2e/f5c518?text=VoltTec"
  alt="Técnicos de VoltTec trabajando"
  className="img-fluid rounded shadow"
/>
```

### Paso 3 — Escribe los párrafos de la empresa

En la columna derecha, escribe 2 párrafos presentando a VoltTec. Habla de:
- Cuántos años lleva en el mercado
- En qué ciudades opera
- Qué tipo de clientes atiende

### Paso 4 — Renderiza las razones con `.map()`

Recorre el array y muestra cada razón. Estructura sugerida:

```jsx
<li className="d-flex gap-3 mb-3">
  <span style={{ fontSize: '1.5rem' }}>{/* icono */}</span>
  <div>
    <strong>{/* titulo */}</strong>
    <p className="text-muted mb-0 small">{/* descripcion */}</p>
  </div>
</li>
```

### Criterios de entrega

- [ ] Se muestra una imagen con `img-fluid`
- [ ] Hay mínimo 2 párrafos con texto real de la empresa
- [ ] El array tiene mínimo 4 razones
- [ ] Se usa `.map()` para renderizar la lista
- [ ] El layout es de 2 columnas en pantallas grandes

---

## Actividad 3 — Formulario de Contacto (`Contact.jsx`)

**Archivo:** `src/components/Contact.jsx`

### Contexto

El formulario es el punto de conversión del portafolio: donde los visitantes piden una cotización. Debe recoger la información suficiente para que VoltTec pueda responder.

### Paso 1 — Completa la información de contacto

En la columna izquierda, agrega los datos de la empresa usando una lista con emojis o íconos:

```jsx
<ul className="list-unstyled">
  <li className="mb-3">
    <span className="me-2">📍</span>
    Calle 10 # 43A-15, El Poblado, Medellín
  </li>
  {/* Agrega: teléfono, email, horario */}
</ul>
```

### Paso 2 — Completa los campos del formulario

Llena cada `<div>` con su campo correspondiente. En React, los atributos cambian así:

| HTML estándar | JSX en React |
|---|---|
| `class="..."` | `className="..."` |
| `for="campo"` | `htmlFor="campo"` |

Ejemplo de un campo completo:

```jsx
<div className="col-md-6">
  <label htmlFor="nombre" className="form-label fw-semibold">
    Nombre completo
  </label>
  <input
    type="text"
    id="nombre"
    className="form-control"
    placeholder="Tu nombre"
  />
</div>
```

El campo **Tipo de servicio** debe ser un `<select>` con al menos 5 opciones:

```jsx
<select id="servicio" className="form-select">
  <option value="">Selecciona un servicio...</option>
  <option value="residencial">Instalación residencial</option>
  {/* Agrega más opciones */}
</select>
```

El campo **Mensaje** debe ser un `<textarea>`:

```jsx
<textarea
  id="mensaje"
  className="form-control"
  rows="4"
  placeholder="Describe brevemente tu necesidad..."
></textarea>
```

### Paso 3 — Escribe el subtítulo de la sección

En el `<p>` del encabezado, escribe una frase invitando al contacto.

### Criterios de entrega

- [ ] Los 5 campos están completos: nombre, email, teléfono, tipo de servicio, mensaje
- [ ] El `<select>` tiene mínimo 5 opciones
- [ ] Todos los `<label>` tienen `htmlFor` apuntando al `id` correcto
- [ ] Se usa `className` en lugar de `class` en todos los elementos
- [ ] La información de contacto de la empresa está visible

---

## Reto adicional — Estado en el formulario

Si terminas las 3 actividades antes de tiempo, implementa el manejo de estado en el formulario de contacto.

### Paso 1 — Importa `useState`

Al inicio de `Contact.jsx` agrega:

```js
import { useState } from 'react';
```

### Paso 2 — Crea el estado del formulario

Dentro de la función `Contact`, antes del `return`:

```js
const [formulario, setFormulario] = useState({
  nombre: '',
  email: '',
  telefono: '',
  servicio: '',
  mensaje: '',
});

const [enviado, setEnviado] = useState(false);
```

### Paso 3 — Conecta los inputs al estado

Agrega `value` y `onChange` a cada campo:

```jsx
<input
  type="text"
  id="nombre"
  className="form-control"
  value={formulario.nombre}
  onChange={(e) => setFormulario({ ...formulario, nombre: e.target.value })}
/>
```

### Paso 4 — Maneja el envío

Agrega una función y conéctala al botón:

```js
const manejarEnvio = () => {
  if (formulario.nombre && formulario.email) {
    setEnviado(true);
  }
};
```

```jsx
<button className="btn btn-voltec w-100 py-2" onClick={manejarEnvio}>
  Enviar solicitud
</button>

{enviado && (
  <div className="alert alert-success mt-3">
    ✅ ¡Gracias {formulario.nombre}! Te contactaremos pronto.
  </div>
)}
```

---

## Entrega

Sube tu trabajo al repositorio `workspace/` en la carpeta con la fecha de hoy. La carpeta debe contener **todo el proyecto** (incluyendo `node_modules` no es necesario, sí todo lo demás).

Estructura esperada en el repositorio:

```
workspace/
└── 2025-06-09/        ← fecha del día del taller
    ├── index.html
    ├── package.json
    ├── vite.config.js
    └── src/
        ├── main.jsx
        ├── App.jsx
        ├── index.css
        └── components/
            ├── Navbar.jsx
            ├── Header.jsx
            ├── Footer.jsx
            ├── Services.jsx    ← tu trabajo
            ├── WhyUs.jsx       ← tu trabajo
            └── Contact.jsx     ← tu trabajo
```

### Lista de verificación final

Antes de hacer el commit, revisa:

- [ ] El proyecto corre sin errores en la consola (`npm run dev`)
- [ ] La página se ve bien en pantalla completa y en móvil (usa las DevTools del navegador)
- [ ] Los tres componentes tienen contenido real (no `// TODO` sin completar)
- [ ] Se usa `.map()` en Services y WhyUs (no HTML repetido manualmente)
- [ ] El formulario de contacto tiene todos los campos y usan `className` / `htmlFor`
- [ ] Reto adicional completado *(opcional)*

---

## Conceptos clave del taller

| Concepto | ¿Qué es? | ¿Dónde lo viste? |
|---|---|---|
| **Componente funcional** | Función de JS que retorna JSX | Todos los archivos `.jsx` |
| **JSX** | Sintaxis parecida a HTML dentro de JS | Todo el `return()` |
| **Props** | Datos que se pasan entre componentes | `key` en los `.map()` |
| **`useState`** | Hook para guardar estado local | `Navbar.jsx`, reto adicional |
| **`useEffect`** | Hook para efectos secundarios | `Navbar.jsx` (listener de scroll) |
| **`.map()`** | Renderizar listas desde arrays | `Header.jsx`, tus actividades |
| **`className`** | Equivalente de `class` en JSX | Todos los elementos HTML |
| **Bootstrap** | Librería CSS de componentes | Clases como `btn`, `card`, `row` |

---

*Taller elaborado para el programa de desarrollo web — VoltTec Portfolio Project*
