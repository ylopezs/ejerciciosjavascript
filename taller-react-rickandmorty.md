# Taller: Aplicación React con Bootstrap y la API de Rick and Morty

**Nivel:** Intermedio  
**Duración estimada:** 2–3 horas  
**Requisitos previos:** Conocimiento básico de JavaScript, haber completado el taller de funciones `obtenerPersonaje(id)` y `obtenerPersonajes()`

---

## Objetivos del taller

Al finalizar este taller podrás:

- Crear un proyecto React desde cero usando GitHub Codespaces y **Vite**
- Integrar Bootstrap en una aplicación React
- Consumir la API de Rick and Morty usando hooks de React (`useState`, `useEffect`)
- Construir un layout de dos columnas: lista a la izquierda, detalle a la derecha
- Manejar estados de carga y errores en la UI

---

## Parte 1 — Crear el proyecto en GitHub Codespaces

### 1.1 Crear el repositorio

1. Ve a [github.com](https://github.com) e inicia sesión.
2. Haz clic en **New repository**.
3. Nómbralo `rick-and-morty-react`.
4. Marca **Add a README file** y haz clic en **Create repository**.

### 1.2 Abrir Codespaces

1. Dentro del repositorio, haz clic en el botón verde **Code**.
2. Selecciona la pestaña **Codespaces**.
3. Haz clic en **Create codespace on main**.

Espera a que el entorno termine de cargar (puede tomar 1–2 minutos).

### 1.3 Crear la aplicación React con Vite

En la terminal integrada de Codespaces, ejecuta:

```bash
npm create vite@latest . -- --template react
```

> **Nota:** El punto `.` indica que el proyecto se crea en la carpeta actual. Cuando Vite pregunte si deseas continuar porque la carpeta no está vacía (hay un README), escribe `y` y presiona Enter.

Vite generará el proyecto al instante. Luego instala las dependencias:

```bash
npm install
```

Cuando termine, ejecuta:

```bash
npm run dev
```

Codespaces detectará el puerto automáticamente y te mostrará un aviso para abrirlo. Haz clic en **Open in Browser** para ver la app corriendo.

> **¿Por qué Vite en lugar de Create React App?**  
> Vite es significativamente más rápido: arranca el servidor de desarrollo en menos de un segundo y sus actualizaciones en caliente (HMR) son casi instantáneas. Create React App fue la herramienta estándar durante años, pero la comunidad migró a Vite por su velocidad y menor configuración.

---

## Parte 2 — Instalar e integrar Bootstrap

### 2.1 Instalar Bootstrap

Detén el servidor con `Ctrl + C` y ejecuta:

```bash
npm install bootstrap
```

### 2.2 Importar Bootstrap en la app

Con Vite, el punto de entrada es `src/main.jsx` (no `index.js` como en Create React App). Abre ese archivo y agrega la importación de Bootstrap **antes** de `index.css`:

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

### 2.3 Limpiar los archivos de ejemplo

Vite genera algunos archivos de ejemplo que no necesitamos. Reemplaza el contenido de `src/App.jsx` con un componente vacío por ahora:

```jsx
function App() {
  return <div>Hola Vite + React</div>
}

export default App
```

También puedes borrar `src/App.css` y vaciar `src/index.css` (o dejarlo con tus propios estilos globales).

---

## Parte 3 — Estructura del proyecto

Antes de escribir código, organiza los archivos. En la carpeta `src/`, crea las siguientes carpetas y archivos:

```
src/
├── components/
│   ├── ListaPersonajes.jsx
│   └── DetallePersonaje.jsx
├── services/
│   └── rickAndMortyService.js
├── App.jsx
└── main.jsx
```

Puedes crearlos con los siguientes comandos en la terminal:

```bash
mkdir src/components src/services
touch src/components/ListaPersonajes.jsx
touch src/components/DetallePersonaje.jsx
touch src/services/rickAndMortyService.js
```

> **Nota:** Con Vite los componentes de React usan la extensión `.jsx` por convención. Asegúrate de usar esa extensión al crear los archivos.

---

## Parte 4 — El servicio de la API

Este archivo contiene las dos funciones que ya conoces del taller anterior, pero exportadas para poder usarlas en toda la app.

Abre `src/services/rickAndMortyService.js` y escribe:

```javascript
const BASE_URL = 'https://rickandmortyapi.com/api';

/**
 * Obtiene la lista de todos los personajes (primera página)
 * @returns {Promise<Array>} Array de personajes
 */
export async function obtenerPersonajes() {
  const respuesta = await fetch(`${BASE_URL}/character`);
  if (!respuesta.ok) {
    throw new Error('Error al obtener los personajes');
  }
  const datos = await respuesta.json();
  return datos.results;
}

/**
 * Obtiene un personaje por su ID
 * @param {number} id - ID del personaje
 * @returns {Promise<Object>} Objeto con los datos del personaje
 */
export async function obtenerPersonaje(id) {
  const respuesta = await fetch(`${BASE_URL}/character/${id}`);
  if (!respuesta.ok) {
    throw new Error(`Error al obtener el personaje con ID ${id}`);
  }
  return respuesta.json();
}
```

> **¿Qué cambió respecto al taller anterior?**  
> Las funciones son exactamente las mismas en lógica. Solo se agregan las palabras `export` al principio para que otros archivos del proyecto puedan importarlas.

---

## Parte 5 — Componente: Lista de Personajes

Este componente muestra la lista de personajes en la columna izquierda.

Abre `src/components/ListaPersonajes.jsx`:

```jsx
function ListaPersonajes({ personajes, onSeleccionar, personajeSeleccionadoId }) {
  return (
    <div className="list-group">
      {personajes.map((personaje) => (
        <button
          key={personaje.id}
          type="button"
          className={`list-group-item list-group-item-action d-flex align-items-center gap-3 ${
            personaje.id === personajeSeleccionadoId ? 'active' : ''
          }`}
          onClick={() => onSeleccionar(personaje.id)}
        >
          <img
            src={personaje.image}
            alt={personaje.name}
            width="48"
            height="48"
            className="rounded-circle"
          />
          <div className="text-start">
            <div className="fw-bold">{personaje.name}</div>
            <small className={personaje.id === personajeSeleccionadoId ? 'text-white-50' : 'text-muted'}>
              {personaje.species} — {personaje.status}
            </small>
          </div>
        </button>
      ))}
    </div>
  );
}

export default ListaPersonajes;
```

> **Nota:** Con Vite y React 17+, ya no es obligatorio escribir `import React from 'react'` al inicio de cada componente. El JSX se transforma automáticamente.

### ¿Qué hace este componente?

| Prop | Descripción |
|---|---|
| `personajes` | Array de personajes recibido desde `App.jsx` |
| `onSeleccionar` | Función que se ejecuta al hacer clic en un personaje |
| `personajeSeleccionadoId` | ID del personaje activo (para resaltarlo en la lista) |

---

## Parte 6 — Componente: Detalle del Personaje

Este componente muestra la información completa del personaje seleccionado en la columna derecha.

Abre `src/components/DetallePersonaje.jsx`:

```jsx
function DetallePersonaje({ personaje, cargando }) {
  if (cargando) {
    return (
      <div className="d-flex justify-content-center align-items-center h-100">
        <div className="spinner-border text-success" role="status">
          <span className="visually-hidden">Cargando...</span>
        </div>
      </div>
    );
  }

  if (!personaje) {
    return (
      <div className="d-flex flex-column justify-content-center align-items-center h-100 text-muted">
        <span className="fs-1">👈</span>
        <p className="mt-3">Selecciona un personaje de la lista para ver su detalle</p>
      </div>
    );
  }

  const badgeColor = {
    Alive: 'success',
    Dead: 'danger',
    unknown: 'secondary',
  };

  return (
    <div className="card h-100 border-0 shadow-sm">
      <img
        src={personaje.image}
        className="card-img-top"
        alt={personaje.name}
        style={{ objectFit: 'cover', maxHeight: '320px' }}
      />
      <div className="card-body">
        <h3 className="card-title fw-bold">{personaje.name}</h3>
        <span className={`badge bg-${badgeColor[personaje.status] || 'secondary'} mb-3`}>
          {personaje.status}
        </span>

        <table className="table table-borderless table-sm">
          <tbody>
            <tr>
              <th scope="row" className="text-muted">Especie</th>
              <td>{personaje.species}</td>
            </tr>
            <tr>
              <th scope="row" className="text-muted">Género</th>
              <td>{personaje.gender}</td>
            </tr>
            <tr>
              <th scope="row" className="text-muted">Origen</th>
              <td>{personaje.origin?.name}</td>
            </tr>
            <tr>
              <th scope="row" className="text-muted">Ubicación actual</th>
              <td>{personaje.location?.name}</td>
            </tr>
            <tr>
              <th scope="row" className="text-muted">Episodios</th>
              <td>{personaje.episode?.length}</td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  );
}

export default DetallePersonaje;
```

---

## Parte 7 — Componente principal: App.jsx

Este es el cerebro de la aplicación. Aquí se manejan los estados y se unen los dos componentes.

Reemplaza todo el contenido de `src/App.jsx` con:

```jsx
import { useState, useEffect } from 'react';
import { obtenerPersonajes, obtenerPersonaje } from './services/rickAndMortyService';
import ListaPersonajes from './components/ListaPersonajes';
import DetallePersonaje from './components/DetallePersonaje';

function App() {
  const [personajes, setPersonajes] = useState([]);
  const [personajeSeleccionado, setPersonajeSeleccionado] = useState(null);
  const [cargandoLista, setCargandoLista] = useState(true);
  const [cargandoDetalle, setCargandoDetalle] = useState(false);
  const [error, setError] = useState(null);

  // Cargar la lista al montar el componente
  useEffect(() => {
    obtenerPersonajes()
      .then((datos) => {
        setPersonajes(datos);
        setCargandoLista(false);
      })
      .catch((err) => {
        setError(err.message);
        setCargandoLista(false);
      });
  }, []);

  // Cargar el detalle cuando el usuario hace clic
  const manejarSeleccion = (id) => {
    setCargandoDetalle(true);
    setPersonajeSeleccionado(null);
    obtenerPersonaje(id)
      .then((datos) => {
        setPersonajeSeleccionado(datos);
        setCargandoDetalle(false);
      })
      .catch((err) => {
        setError(err.message);
        setCargandoDetalle(false);
      });
  };

  return (
    <div className="container-fluid">
      {/* Encabezado */}
      <nav className="navbar navbar-dark bg-dark mb-4">
        <div className="container">
          <span className="navbar-brand fw-bold fs-4">🛸 Rick and Morty Explorer</span>
        </div>
      </nav>

      {/* Mensaje de error global */}
      {error && (
        <div className="container">
          <div className="alert alert-danger" role="alert">
            {error}
          </div>
        </div>
      )}

      {/* Layout principal */}
      <div className="container">
        <div className="row g-4">
          {/* Columna izquierda: Lista */}
          <div className="col-12 col-md-4">
            <h5 className="fw-bold mb-3">Personajes</h5>
            {cargandoLista ? (
              <div className="d-flex justify-content-center mt-5">
                <div className="spinner-border text-success" role="status">
                  <span className="visually-hidden">Cargando...</span>
                </div>
              </div>
            ) : (
              <ListaPersonajes
                personajes={personajes}
                onSeleccionar={manejarSeleccion}
                personajeSeleccionadoId={personajeSeleccionado?.id}
              />
            )}
          </div>

          {/* Columna derecha: Detalle */}
          <div className="col-12 col-md-8" style={{ minHeight: '400px' }}>
            <h5 className="fw-bold mb-3">Detalle</h5>
            <DetallePersonaje
              personaje={personajeSeleccionado}
              cargando={cargandoDetalle}
            />
          </div>
        </div>
      </div>
    </div>
  );
}

export default App;
```

---

## Parte 8 — Estilos opcionales

Si quieres que la lista sea scrolleable y no ocupe toda la página, agrega esto en `src/index.css`:

```css
/* Hace la columna de la lista scrolleable */
.list-group {
  max-height: 80vh;
  overflow-y: auto;
}
```

---

## Parte 9 — Ejecutar y probar la aplicación

Inicia el servidor con:

```bash
npm run dev
```

> **Diferencia importante con Create React App:** el comando es `npm run dev`, no `npm start`.

Codespaces expondrá el puerto automáticamente. Deberías ver:

1. La lista de personajes cargando en la columna izquierda.
2. Un mensaje indicando que selecciones un personaje en la columna derecha.
3. Al hacer clic en un personaje, aparece su información detallada a la derecha.

---

## Parte 10 — Explicación de conceptos clave

### ¿Qué es Vite?

Vite es una herramienta de construcción moderna para proyectos web. A diferencia de Create React App, no empaqueta todo el código antes de servir la app: aprovecha los módulos ES nativos del navegador para entregar solo lo que se necesita en cada momento. Esto lo hace extremadamente rápido.

| | Create React App | Vite |
|---|---|---|
| Comando de inicio | `npm start` | `npm run dev` |
| Punto de entrada | `src/index.js` | `src/main.jsx` |
| Velocidad de arranque | Lenta (segundos) | Muy rápida (< 1 segundo) |
| Soporte actual | Sin mantenimiento activo | Mantenido activamente |

### ¿Qué es un Hook?

Los Hooks son funciones especiales de React que permiten "enganchar" funcionalidades al ciclo de vida del componente.

| Hook | ¿Para qué sirve? |
|---|---|
| `useState` | Guarda y actualiza datos dentro del componente |
| `useEffect` | Ejecuta código cuando el componente se monta o cuando cambia un valor |

### El flujo de datos (props hacia abajo, eventos hacia arriba)

```
App.jsx
 ├── Lee datos con useEffect → guarda en estado
 ├── Pasa personajes → ListaPersonajes (prop)
 ├── Recibe id seleccionado ← ListaPersonajes (callback)
 └── Pasa detalle → DetallePersonaje (prop)
```

### Renderizado condicional

```jsx
// Si está cargando, muestra spinner. Si no, muestra el contenido.
{cargando ? <Spinner /> : <Contenido />}
```

---

## Retos adicionales (opcionales)

Estos retos son para quienes quieran ir más allá:

1. **Paginación:** La API devuelve personajes en páginas. Agrega botones "Anterior" y "Siguiente" para navegar entre páginas usando el campo `info.next` y `info.prev` de la respuesta.

2. **Búsqueda:** Agrega un campo de texto que filtre la lista por nombre usando el parámetro `?name=` de la API: `https://rickandmortyapi.com/api/character?name=rick`

3. **Filtro por estado:** Agrega un `<select>` que filtre personajes por `Alive`, `Dead` o `unknown`.

4. **Responsive mejorado:** En mobile, oculta el detalle hasta que se seleccione un personaje y agrega un botón "Volver a la lista".

---

## Resumen

En este taller:

- Creaste un proyecto React con **Vite** en GitHub Codespaces usando `npm create vite@latest`
- Aprendiste las diferencias clave entre Vite y Create React App
- Integraste Bootstrap para el diseño responsivo
- Separaste la lógica en **servicios**, **componentes** y **estado**
- Usaste `useEffect` para cargar datos al iniciar la app
- Usaste `useState` para manejar la lista, el personaje seleccionado y los estados de carga
- Construiste un layout de dos columnas comunicado entre componentes

¡Felicitaciones! Ahora tienes una aplicación React funcional construida con las herramientas que usa la industria hoy. 🚀
