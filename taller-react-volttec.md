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

Crea cada archivo en la ruta indicada y copia exactamente el contenido que aparece a continuación.

---

#### `src/main.jsx`

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

---

#### `src/App.jsx`

```jsx
import Navbar from './components/Navbar';
import Header from './components/Header';
import Services from './components/Services';
import WhyUs from './components/WhyUs';
import Contact from './components/Contact';
import Footer from './components/Footer';

function App() {
  return (
    <>
      {/* ✅ ENTREGADO: Navbar con menú de navegación */}
      <Navbar />

      {/* ✅ ENTREGADO: Header / Jumbotron principal */}
      <Header />

      {/* 🛠️ TAREA: Sección de servicios - ustedes deben completar el HTML interno */}
      <Services />

      {/* 🛠️ TAREA: Sección ¿Por qué nosotros? */}
      <WhyUs />

      {/* 🛠️ TAREA: Formulario de contacto */}
      <Contact />

      {/* ✅ ENTREGADO: Footer */}
      <Footer />
    </>
  );
}

export default App;
```

---

#### `src/index.css`

```css
/* =====================================================
   VoltTec — Estilos Globales
   ===================================================== */

:root {
  --voltec-amarillo: #f5c518;
  --voltec-amarillo-oscuro: #d4a800;
  --voltec-negro: #0f1117;
  --voltec-gris-oscuro: #1a1e2e;
  --voltec-gris-medio: #2c3147;
  --voltec-texto-claro: #f0f2f8;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
  color: #1a1e2e;
}

/* ----- NAVBAR ----- */
.navbar-transparente {
  background: rgba(15, 17, 23, 0.3);
  backdrop-filter: blur(4px);
}

.navbar-scrolled {
  background: var(--voltec-negro) !important;
}

.transition-all {
  transition: background 0.3s ease, box-shadow 0.3s ease;
}

.logo-icono { font-size: 1.6rem; line-height: 1; }
.logo-texto { font-size: 1.4rem; font-weight: 800; letter-spacing: -0.5px; }
.logo-principal { color: var(--voltec-amarillo); }
.logo-secundario { color: #ffffff; }

.nav-link-custom {
  color: rgba(255, 255, 255, 0.85) !important;
  font-weight: 500;
  padding: 0.4rem 0.8rem !important;
  border-radius: 6px;
  transition: color 0.2s, background 0.2s;
}
.nav-link-custom:hover {
  color: var(--voltec-amarillo) !important;
  background: rgba(245, 197, 24, 0.1);
}

.toggler-icono { color: white; font-size: 1.4rem; }

/* ----- BOTÓN DE MARCA ----- */
.btn-voltec {
  background: var(--voltec-amarillo);
  color: var(--voltec-negro);
  font-weight: 700;
  border: none;
  border-radius: 8px;
  transition: background 0.2s, transform 0.15s;
}
.btn-voltec:hover {
  background: var(--voltec-amarillo-oscuro);
  color: var(--voltec-negro);
  transform: translateY(-1px);
}

/* ----- HERO ----- */
.header-hero {
  background: linear-gradient(135deg, #0a0d16 0%, #1a1e2e 40%, #0f2040 100%);
  position: relative;
  overflow: hidden;
}
.header-hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background-image:
    radial-gradient(circle at 20% 50%, rgba(245,197,24,0.08) 0%, transparent 50%),
    radial-gradient(circle at 80% 20%, rgba(245,197,24,0.05) 0%, transparent 40%);
}
.hero-overlay {
  position: absolute;
  inset: 0;
  background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23f5c518' fill-opacity='0.03'%3E%3Cpath d='M36 34v-4h-2v4h-4v2h4v4h2v-4h4v-2h-4zm0-30V0h-2v4h-4v2h4v4h2V6h4V4h-4zM6 34v-4H4v4H0v2h4v4h2v-4h4v-2H6zM6 4V0H4v4H0v2h4v4h2V6h4V4H6z'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
}
.hero-badge {
  display: inline-block;
  background: rgba(245,197,24,0.15);
  border: 1px solid rgba(245,197,24,0.3);
  color: var(--voltec-amarillo);
  padding: 0.35rem 1rem;
  border-radius: 50px;
  font-size: 0.85rem;
  font-weight: 600;
}
.hero-titulo { line-height: 1.1; letter-spacing: -1px; }
.hero-titulo-acento { color: var(--voltec-amarillo); }
.hero-descripcion { color: rgba(255,255,255,0.75); max-width: 550px; line-height: 1.7; }

.stat-card {
  background: rgba(255,255,255,0.07);
  border: 1px solid rgba(255,255,255,0.1);
  border-radius: 10px;
  backdrop-filter: blur(4px);
}
.stat-numero { font-size: 1.6rem; font-weight: 800; color: var(--voltec-amarillo); line-height: 1; }
.stat-etiqueta { font-size: 0.7rem; color: rgba(255,255,255,0.6); margin-top: 4px; text-transform: uppercase; letter-spacing: 0.5px; }

/* ----- TÍTULOS DE SECCIÓN ----- */
.section-titulo {
  font-size: 2.2rem;
  font-weight: 800;
  letter-spacing: -0.5px;
  position: relative;
  display: inline-block;
}
.section-titulo::after {
  content: '';
  display: block;
  width: 50px;
  height: 4px;
  background: var(--voltec-amarillo);
  border-radius: 2px;
  margin: 0.5rem auto 0;
}
.section-subtitulo { font-size: 1.1rem; max-width: 600px; margin: 0 auto; }

/* ----- FOOTER ----- */
.footer-principal { background: var(--voltec-negro); }
.footer-titulo { color: var(--voltec-amarillo); font-weight: 700; text-transform: uppercase; letter-spacing: 1px; font-size: 0.8rem; }
.footer-link { color: rgba(255,255,255,0.6); text-decoration: none; font-size: 0.9rem; transition: color 0.2s; }
.footer-link:hover { color: var(--voltec-amarillo); }
.footer-bottom { background: rgba(0,0,0,0.3); border-top: 1px solid rgba(255,255,255,0.06); }
```

---

#### `src/components/Navbar.jsx` ✅ ENTREGADO

```jsx
// ============================================================
//  ✅ COMPONENTE ENTREGADO — NO MODIFICAR
//  Navbar.jsx
//
//  OBJETIVO DE APRENDIZAJE:
//  Observa cómo se usa el estado (useState) para controlar
//  el menú móvil, y cómo se manejan eventos en React.
// ============================================================

import { useState, useEffect } from 'react';

function Navbar() {
  const [menuAbierto, setMenuAbierto] = useState(false);
  const [scrolled, setScrolled] = useState(false);

  useEffect(() => {
    const manejarScroll = () => {
      setScrolled(window.scrollY > 50);
    };
    window.addEventListener('scroll', manejarScroll);
    return () => window.removeEventListener('scroll', manejarScroll);
  }, []);

  const toggleMenu = () => {
    setMenuAbierto(!menuAbierto);
  };

  const cerrarMenu = () => {
    setMenuAbierto(false);
  };

  return (
    <nav
      className={`navbar navbar-expand-lg fixed-top transition-all ${
        scrolled ? 'navbar-scrolled shadow' : 'navbar-transparente'
      }`}
    >
      <div className="container">

        {/* Logo */}
        <a className="navbar-brand d-flex align-items-center gap-2" href="#inicio">
          <span className="logo-icono">⚡</span>
          <span className="logo-texto">
            <span className="logo-principal">VOLT</span>
            <span className="logo-secundario">TEC</span>
          </span>
        </a>

        {/* Botón hamburguesa para móvil */}
        <button
          className={`navbar-toggler border-0 ${menuAbierto ? '' : 'collapsed'}`}
          type="button"
          onClick={toggleMenu}
          aria-label="Abrir menú"
        >
          <span className="toggler-icono">
            {menuAbierto ? '✕' : '☰'}
          </span>
        </button>

        {/* Links de navegación */}
        <div className={`collapse navbar-collapse ${menuAbierto ? 'show' : ''}`}>
          <ul className="navbar-nav ms-auto gap-1">
            <li className="nav-item">
              <a className="nav-link nav-link-custom" href="#inicio" onClick={cerrarMenu}>
                Inicio
              </a>
            </li>
            <li className="nav-item">
              <a className="nav-link nav-link-custom" href="#servicios" onClick={cerrarMenu}>
                Servicios
              </a>
            </li>
            <li className="nav-item">
              <a className="nav-link nav-link-custom" href="#nosotros" onClick={cerrarMenu}>
                Nosotros
              </a>
            </li>
            <li className="nav-item">
              <a className="nav-link nav-link-custom" href="#contacto" onClick={cerrarMenu}>
                Contacto
              </a>
            </li>
            <li className="nav-item ms-lg-2">
              <a
                className="btn btn-voltec px-4"
                href="tel:+573001234567"
                onClick={cerrarMenu}
              >
                📞 Llamar ahora
              </a>
            </li>
          </ul>
        </div>

      </div>
    </nav>
  );
}

export default Navbar;
```

---

#### `src/components/Header.jsx` ✅ ENTREGADO

```jsx
// ============================================================
//  ✅ COMPONENTE ENTREGADO — NO MODIFICAR
//  Header.jsx  (Jumbotron / Hero)
//
//  OBJETIVO DE APRENDIZAJE:
//  Observa cómo se estructuran los datos como un array
//  de objetos y se renderizan con .map() en JSX.
// ============================================================

const estadisticas = [
  { numero: '15+', etiqueta: 'Años de experiencia' },
  { numero: '3.200', etiqueta: 'Proyectos completados' },
  { numero: '98%', etiqueta: 'Clientes satisfechos' },
  { numero: '24/7', etiqueta: 'Soporte disponible' },
];

function Header() {
  return (
    <header id="inicio" className="header-hero">
      <div className="hero-overlay"></div>

      <div className="container position-relative">
        <div className="row min-vh-100 align-items-center">
          <div className="col-lg-7 text-white py-5">

            <div className="hero-badge mb-3">
              <span>⚡ Certificados RETIE · Colombia</span>
            </div>

            <h1 className="hero-titulo display-3 fw-bold mb-3">
              Soluciones eléctricas
              <span className="hero-titulo-acento d-block">
                para tu hogar y empresa
              </span>
            </h1>

            <p className="hero-descripcion lead mb-4">
              Instalaciones, mantenimiento y modernización eléctrica con
              los más altos estándares de seguridad. Atendemos Medellín y el
              Área Metropolitana.
            </p>

            <div className="d-flex flex-wrap gap-3 mb-5">
              <a href="#servicios" className="btn btn-voltec btn-lg px-5">
                Ver servicios
              </a>
              <a href="#contacto" className="btn btn-outline-light btn-lg px-5">
                Solicitar cotización
              </a>
            </div>

            <div className="row g-3">
              {estadisticas.map((stat, index) => (
                <div key={index} className="col-6 col-sm-3">
                  <div className="stat-card text-center p-3">
                    <div className="stat-numero">{stat.numero}</div>
                    <div className="stat-etiqueta">{stat.etiqueta}</div>
                  </div>
                </div>
              ))}
            </div>

          </div>
        </div>
      </div>
    </header>
  );
}

export default Header;
```

---

#### `src/components/Footer.jsx` ✅ ENTREGADO

```jsx
// ============================================================
//  ✅ COMPONENTE ENTREGADO — NO MODIFICAR
//  Footer.jsx
//
//  OBJETIVO DE APRENDIZAJE:
//  Observa cómo se divide el footer en columnas con Bootstrap
//  y cómo se separa la lógica de datos del HTML (JSX).
// ============================================================

const serviciosFooter = [
  'Instalaciones residenciales',
  'Instalaciones comerciales',
  'Mantenimiento preventivo',
  'Tableros eléctricos',
  'Iluminación LED',
  'Plantas eléctricas',
];

const linksRapidos = [
  { label: 'Inicio', href: '#inicio' },
  { label: 'Servicios', href: '#servicios' },
  { label: 'Nosotros', href: '#nosotros' },
  { label: 'Contacto', href: '#contacto' },
];

function Footer() {
  const anioActual = new Date().getFullYear();

  return (
    <footer className="footer-principal pt-5 pb-0">
      <div className="container">
        <div className="row g-4 pb-4">

          {/* Columna 1: Logo y descripción */}
          <div className="col-lg-4 col-md-6">
            <div className="d-flex align-items-center gap-2 mb-3">
              <span className="logo-icono">⚡</span>
              <span className="logo-texto">
                <span className="logo-principal">VOLT</span>
                <span className="logo-secundario text-warning">TEC</span>
              </span>
            </div>
            <p className="text-light opacity-75 mb-3">
              Empresa certificada en instalaciones eléctricas residenciales
              y comerciales. Más de 15 años brindando seguridad y confianza
              a hogares y empresas de Medellín.
            </p>
            <div className="d-flex gap-2">
              {['Facebook', 'Instagram', 'WhatsApp'].map((red) => (
                <a key={red} href="#" className="btn btn-sm btn-outline-light">
                  {red === 'WhatsApp' ? '💬' : red === 'Instagram' ? '📸' : '👥'}
                </a>
              ))}
            </div>
          </div>

          {/* Columna 2: Links rápidos */}
          <div className="col-lg-2 col-md-6 col-6">
            <h6 className="footer-titulo mb-3">Navegación</h6>
            <ul className="list-unstyled">
              {linksRapidos.map((link) => (
                <li key={link.label} className="mb-2">
                  <a href={link.href} className="footer-link">
                    {link.label}
                  </a>
                </li>
              ))}
            </ul>
          </div>

          {/* Columna 3: Servicios */}
          <div className="col-lg-3 col-md-6 col-6">
            <h6 className="footer-titulo mb-3">Servicios</h6>
            <ul className="list-unstyled">
              {serviciosFooter.map((servicio) => (
                <li key={servicio} className="mb-2">
                  <a href="#servicios" className="footer-link">
                    {servicio}
                  </a>
                </li>
              ))}
            </ul>
          </div>

          {/* Columna 4: Contacto */}
          <div className="col-lg-3 col-md-6">
            <h6 className="footer-titulo mb-3">Contacto</h6>
            <ul className="list-unstyled">
              <li className="mb-2 text-light opacity-75">
                📍 Calle 10 # 43A-15, El Poblado, Medellín
              </li>
              <li className="mb-2">
                <a href="tel:+573001234567" className="footer-link">
                  📞 (300) 123-4567
                </a>
              </li>
              <li className="mb-2">
                <a href="mailto:info@volttec.com.co" className="footer-link">
                  📧 info@volttec.com.co
                </a>
              </li>
              <li className="mb-2 text-light opacity-75">
                🕐 Lun–Vie 7am–6pm · Sáb 8am–2pm
              </li>
              <li className="mt-3">
                <span className="badge bg-success">✅ Certificado RETIE</span>
              </li>
            </ul>
          </div>

        </div>
      </div>

      <div className="footer-bottom text-center py-3 mt-2">
        <small className="text-light opacity-50">
          © {anioActual} VoltTec Servicios Eléctricos S.A.S. — Todos los derechos reservados.
          &nbsp;·&nbsp; NIT 900.123.456-7
        </small>
      </div>
    </footer>
  );
}

export default Footer;
```

---

#### `src/components/Services.jsx` 🛠️ TAREA

```jsx
// ============================================================
//  🛠️ COMPONENTE PARA COMPLETAR — TAREA ESTUDIANTE
//  Services.jsx
//
//  INSTRUCCIONES:
//  1. Define el array "servicios" con al menos 6 servicios.
//     Cada objeto debe tener: icono, titulo, descripcion.
//  2. Completa el JSX usando tarjetas Bootstrap (card).
//  3. Usa .map() para renderizar — NO copies el HTML 6 veces.
//
//  PISTAS:
//  - Usa <div className="row g-4"> para el grid
//  - Cada tarjeta va en <div className="col-md-6 col-lg-4">
//  - Clases Bootstrap útiles: card, card-body, card-title, card-text
// ============================================================

// 🛠️ PASO 1: Define aquí tu array de servicios
const servicios = [
  // Ejemplo de la estructura:
  // {
  //   icono: '⚡',
  //   titulo: 'Instalaciones residenciales',
  //   descripcion: 'Instalamos sistemas eléctricos completos...',
  // },

  // TODO: Agrega aquí tus 6 servicios
];

function Services() {
  return (
    <section id="servicios" className="py-5 bg-light">
      <div className="container">

        <div className="text-center mb-5">
          <h2 className="section-titulo">Nuestros Servicios</h2>
          <p className="section-subtitulo text-muted">
            {/* 🛠️ TODO: Escribe una descripción corta de la sección */}
          </p>
        </div>

        {/* 🛠️ PASO 2: Renderiza las tarjetas con servicios.map(...) */}
        <div className="row g-4">

          {/* TODO: Tu código .map() va aquí */}

        </div>

      </div>
    </section>
  );
}

export default Services;
```

---

#### `src/components/WhyUs.jsx` 🛠️ TAREA

```jsx
// ============================================================
//  🛠️ COMPONENTE PARA COMPLETAR — TAREA ESTUDIANTE
//  WhyUs.jsx  (¿Por qué elegirnos?)
//
//  INSTRUCCIONES:
//  1. Define el array "razones" con al menos 4 objetos.
//     Cada objeto debe tener: icono, titulo, descripcion.
//  2. Agrega una imagen en la columna izquierda.
//  3. Escribe 2 párrafos sobre la empresa.
//  4. Usa .map() para renderizar la lista de razones.
//
//  PISTAS:
//  - <div className="row align-items-center"> para las columnas
//  - className="list-unstyled" en <ul> quita los bullets
//  - Placeholder: https://placehold.co/500x400/1a1e2e/f5c518?text=VoltTec
// ============================================================

// 🛠️ PASO 1: Define tu array de razones
const razones = [
  // TODO: Agrega aquí tus razones
  // Ejemplo:
  // { icono: '🏆', titulo: 'Experiencia certificada', descripcion: '...' },
];

function WhyUs() {
  return (
    <section id="nosotros" className="py-5">
      <div className="container">
        <div className="row align-items-center g-5">

          {/* Columna izquierda: imagen */}
          <div className="col-lg-5">
            {/* 🛠️ TODO: Agrega <img> con className="img-fluid rounded shadow" */}
          </div>

          {/* Columna derecha: texto y razones */}
          <div className="col-lg-7">
            <h2 className="section-titulo mb-3">¿Por qué elegirnos?</h2>

            {/* 🛠️ TODO: Escribe 2-3 párrafos sobre la empresa */}
            <p className="text-muted mb-4">
              {/* Tu texto aquí */}
            </p>

            {/* 🛠️ PASO 2: Renderiza las razones con .map() */}
            <ul className="list-unstyled">
              {/* TODO: razones.map(...) */}
            </ul>
          </div>

        </div>
      </div>
    </section>
  );
}

export default WhyUs;
```

---

#### `src/components/Contact.jsx` 🛠️ TAREA

```jsx
// ============================================================
//  🛠️ COMPONENTE PARA COMPLETAR — TAREA ESTUDIANTE
//  Contact.jsx
//
//  INSTRUCCIONES:
//  1. Completa el formulario: nombre, email, teléfono,
//     tipo de servicio (select), mensaje (textarea), botón.
//  2. Agrega la información de contacto en la columna izquierda.
//  3. Usa las clases Bootstrap: form-label, form-control,
//     form-select, btn.
//
//  ⚠️ DIFERENCIAS JSX vs HTML:
//     class="..."  →  className="..."
//     for="campo"  →  htmlFor="campo"
//
//  RETO ADICIONAL: usa useState para manejar el formulario
//  y mostrar una alerta al enviarlo.
// ============================================================

function Contact() {
  return (
    <section id="contacto" className="py-5 bg-dark text-white">
      <div className="container">

        <div className="text-center mb-5">
          <h2 className="section-titulo text-white">Contáctenos</h2>
          <p className="text-light opacity-75">
            {/* 🛠️ TODO: Escribe un texto invitando al contacto */}
          </p>
        </div>

        <div className="row g-5">

          {/* Columna izquierda: información de contacto */}
          <div className="col-lg-4">
            <h4 className="mb-4">Información de contacto</h4>
            {/* 🛠️ TODO: Lista con 📍 dirección, 📞 teléfono, 📧 email, 🕐 horario */}
          </div>

          {/* Columna derecha: formulario */}
          <div className="col-lg-8">
            <div className="bg-white text-dark rounded-3 p-4">
              <div className="row g-3">

                {/* Campo: Nombre */}
                <div className="col-md-6">
                  {/* TODO */}
                </div>

                {/* Campo: Email */}
                <div className="col-md-6">
                  {/* TODO */}
                </div>

                {/* Campo: Teléfono */}
                <div className="col-md-6">
                  {/* TODO */}
                </div>

                {/* Campo: Tipo de servicio — usa <select> con 5+ opciones */}
                <div className="col-md-6">
                  {/* TODO */}
                </div>

                {/* Campo: Mensaje — usa <textarea> */}
                <div className="col-12">
                  {/* TODO */}
                </div>

                {/* Botón enviar */}
                <div className="col-12">
                  {/* TODO */}
                </div>

              </div>
            </div>
          </div>

        </div>
      </div>
    </section>
  );
}

export default Contact;
```

---

Después de crear todos los archivos, ejecuta de nuevo:

```bash
npm run dev
```

Debes ver la página completa de VoltTec con el navbar, el hero y el footer ya funcionando.

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
