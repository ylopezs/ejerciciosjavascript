# Taller de Debugging en React
### Consola del navegador, DevTools y `console.log`

**Duración sugerida:** 90–120 minutos
**Requisito previo:** saber crear componentes React y correr un proyecto (Vite o CRA)

---

## Objetivos de aprendizaje

Al terminar este taller, cada estudiante va a poder:

1. Abrir las DevTools del navegador y moverse entre las pestañas **Elements/Inspector**, **Console** y **Sources**.
2. Leer un mensaje de error de React/JavaScript y ubicar la línea del código que lo causó.
3. Usar `console.log`, `console.warn`, `console.error`, `console.table` y `console.group` para entender qué pasa "por dentro" de un componente.
4. Reconocer y explicar 5 errores comunes en React: props/valores `undefined`, comparaciones de tipos incorrectas, mutación de estado, efectos sin dependencias y errores async no controlados.
5. Poner un *breakpoint* en el código desde la pestaña Sources y pausar la ejecución para inspeccionar variables.

---

## Preparación (en Codespaces)

Cada estudiante ya tiene su Codespace con un proyecto de React dentro de una carpeta con la fecha de hoy. Pasos:

1. Abrir la terminal del Codespace y confirmar que el proyecto corre:
   ```
   npm run dev
   ```
2. Abrir la app en el navegador (Codespaces va a ofrecer un puerto/preview).
3. Reemplazar el contenido de `src/App.jsx` por el código de la sección **"Código del taller"** más abajo, y crear/reemplazar también `src/App.css` con el CSS incluido ahí.
4. Guardar y confirmar que el proyecto recarga solo (hot reload).

> Nota para el profesor: a propósito la app **no va a cargar bien** apenas la reemplacen. Ese es el primer ejercicio.

---

## Parte 1 — Tour rápido por las DevTools (15 min)

Abrir las DevTools con `F12` o clic derecho → *Inspeccionar*.

| Pestaña | Para qué sirve |
|---|---|
| **Elements / Inspector** | Ver el HTML real que React generó, y editarlo temporalmente para probar cambios de CSS |
| **Console** | Ver mensajes de `console.log`, errores y warnings; también se puede escribir código JS ahí mismo |
| **Sources** | Ver el código fuente tal cual lo escribió el navegador, poner *breakpoints* y pausar la ejecución |
| **Network** | Ver las peticiones que hace la app (fetch, imágenes, etc.) — útil más adelante con APIs |

**Ejercicio rápido:** en la pestaña Elements, hacer clic en el ícono de flecha (seleccionar elemento) y tocar cualquier parte de la página. Ver cómo las DevTools resaltan el HTML correspondiente.

---

## Parte 2 — Los distintos `console.*` (10 min)

Antes de entrar a los bugs, mostrar en vivo (o pedir que prueben en la consola del navegador):

```js
console.log("mensaje normal");
console.warn("una advertencia, sale en amarillo");
console.error("un error, sale en rojo");
console.table([{ nombre: "Ana", edad: 21 }, { nombre: "Luis", edad: 23 }]);
console.group("Detalles de la tarea");
console.log("id:", 1);
console.log("texto:", "Aprender React");
console.groupEnd();
```

Preguntar: ¿cuándo usarían `console.table` en vez de `console.log`? ¿Cuándo `console.error` en vez de `console.log`?

---

## Parte 3 — Cacería de bugs (50–60 min)

El código de más abajo tiene **5 bugs intencionales**, marcados con comentarios `🐛 BUG N`. Los estudiantes deben resolverlos **en este orden**, porque cada uno destraba el siguiente.

Para cada bug: leer la consola, formular una hipótesis, agregar `console.log` para confirmarla, y recién después corregir el código.

### 🐛 Bug 1 — La app ni siquiera carga

Al reemplazar `App.jsx`, la pantalla se pone en blanco o muestra una superposición roja de error.

- Leer el mensaje de error completo en la consola. ¿Qué tipo de error es? ¿En qué archivo y línea ocurre?
- Buscar esa línea en el código. ¿Qué variable es `undefined`?
- Pista: agregar `console.log(tarea)` justo antes de la línea que falla, dentro del `.map()`, para ver los datos de cada tarea una por una.
- **Pregunta guía:** ¿por qué falla justo con una tarea y no con las otras tres?

### 🐛 Bug 2 — La consola no para de imprimir

Con la app ya cargando, abrir la consola y observar cómo un mensaje se repite sin parar.

- ¿Qué hook de React se está usando? ¿Qué le falta como segundo argumento?
- Pista: comparar este `useEffect` con el de `PerfilUsuario`, que sí tiene un arreglo `[]` al final.
- **Pregunta guía:** ¿qué hace el arreglo de dependencias de `useEffect`? ¿Qué pasa si está vacío, si no está, o si tiene variables adentro?

### 🐛 Bug 3 — Los filtros "Pendientes" y "Completadas" no muestran nada

- Agregar `console.log(typeof tarea.completada, tarea.completada)` dentro del `.filter()`.
- Mirar el resultado en consola: ¿qué tipo de dato es realmente `completada`?
- **Pregunta guía:** ¿por qué `"true" === true` da `false` en JavaScript?

### 🐛 Bug 4 — Agregar una tarea no la muestra en la lista

- Agregar dos `console.log(tareas.length)`: uno antes y otro después de `agregarTarea`.
- El número cambia, pero la pantalla no. ¿Por qué React "no se entera" del cambio?
- Pista: buscar en el código si se está usando `push()` sobre el arreglo original o si se está creando un arreglo nuevo con `setTareas([...tareas, nuevaTarea])`.

### 🐛 Bug 5 — "Cargando perfil..." se queda pegado

- Este bug no siempre aparece: hay que recargar la página varias veces (50% de probabilidad).
- Cuando se queda pegado, mirar la consola: ¿aparece algún error en rojo?
- **Pregunta guía:** ¿por qué ese error no afecta visualmente a la app, y cómo se darían cuenta de que ocurrió si no miraran la consola?
- Reto extra: envolver el `throw` en un `try/catch` y usar `console.error` (o un estado de error) para mostrar algo en pantalla.

---

## Parte 4 — Breakpoints en Sources (15 min)

1. Ir a la pestaña **Sources**, buscar `App.jsx` (o el archivo compilado equivalente).
2. Hacer clic en el número de línea de `completarTarea` para poner un *breakpoint*.
3. Tocar el botón ✔ de una tarea en la app: la ejecución se pausa ahí mismo.
4. Pasar el mouse sobre las variables (`id`, `tareas`) para ver sus valores en ese instante, o usar el panel **Scope** a la derecha.
5. Usar los botones de *Step over / Step into* para avanzar línea por línea.

**Pregunta de cierre:** ¿en qué se diferencia usar un breakpoint de usar `console.log`? ¿Cuándo conviene cada uno?

---

## Entregable del taller

Cada estudiante entrega (puede ser una captura de pantalla o un documento corto) por cada uno de los 5 bugs:

1. El mensaje de error o comportamiento raro observado.
2. El `console.log` que usaron para confirmar la causa.
3. La línea de código corregida.
4. Una frase explicando, en sus palabras, por qué pasaba el bug.

---

## Código del taller

### `src/App.jsx`

```jsx
import { useState, useEffect } from 'react';
import './App.css';

// Datos iniciales de tareas.
// 👀 Miren con atención: una de estas tareas es distinta a las demás...
const tareasIniciales = [
  { id: 1, texto: 'Aprender React', categoria: 'estudio', completada: false },
  { id: 2, texto: 'Hacer ejercicio', categoria: 'salud', completada: true },
  { id: 3, texto: 'Leer un libro', categoria: 'ocio', completada: false },
  { id: 4, texto: 'Practicar debugging', completada: false },
];

function App() {
  const [tareas, setTareas] = useState(tareasIniciales);
  const [filtro, setFiltro] = useState('todas');
  const [contador, setContador] = useState(0);

  // 🐛 BUG 2 — useEffect SIN arreglo de dependencias.
  // Este efecto se ejecuta después de CADA render, y como adentro
  // llamamos a setContador, provocamos otro render... y otro... y otro.
  // Pista: abran la consola y cuenten cuántas veces se imprime esto.
  useEffect(() => {
    console.log('Renderizando App, contador:', contador);
    setContador(contador + 1);
  });

  // Filtra las tareas según el botón elegido
  const tareasFiltradas = tareas.filter((tarea) => {
    if (filtro === 'todas') return true;
    // 🐛 BUG 3 — 'completada' es un booleano (true/false),
    // pero acá se compara contra el STRING "true"/"false".
    // Agreguen un console.log(typeof tarea.completada, tarea.completada)
    // para ver qué tipo de dato es en realidad.
    if (filtro === 'completadas') return tarea.completada === 'true';
    if (filtro === 'pendientes') return tarea.completada === 'false';
    return true;
  });

  // Agrega una tarea nueva a la lista
  function agregarTarea(texto) {
    if (!texto.trim()) return;
    // 🐛 BUG 4 — Se está MUTANDO el arreglo original con push()
    // en vez de crear uno nuevo. React compara referencias, así que
    // no detecta el cambio y la lista no se actualiza en pantalla.
    // Prueben: console.log('tareas antes:', tareas.length) aquí arriba
    // y otra vez después del push, van a ver que sí cambia el arreglo...
    // pero la interfaz no se entera.
    tareas.push({ id: Date.now(), texto, categoria: 'general', completada: false });
    setTareas(tareas);
  }

  // Marca una tarea como completada
  function completarTarea(id) {
    const nuevasTareas = tareas.map((tarea) =>
      tarea.id === id ? { ...tarea, completada: true } : tarea
    );
    setTareas(nuevasTareas);
  }

  return (
    <div className="app">
      <h1>Mis Tareas</h1>

      <div className="filtros">
        <button onClick={() => setFiltro('todas')}>Todas</button>
        <button onClick={() => setFiltro('pendientes')}>Pendientes</button>
        <button onClick={() => setFiltro('completadas')}>Completadas</button>
      </div>

      <ul className="lista-tareas">
        {tareasFiltradas.map((tarea) => (
          <li key={tarea.id} className={tarea.completada ? 'completada' : ''}>
            <span>{tarea.texto}</span>
            {/* 🐛 BUG 1 — La tarea con id 4 no tiene la propiedad 'categoria',
                así que tarea.categoria es undefined, y undefined.toUpperCase()
                no existe: la app se rompe apenas carga.
                Este es el primer error que van a ver: la pantalla se pone
                en blanco (o roja) con un mensaje de error. Léanlo con calma. */}
            <span className="categoria">{tarea.categoria.toUpperCase()}</span>
            <button onClick={() => completarTarea(tarea.id)}>✔</button>
          </li>
        ))}
      </ul>

      <AgregarTarea onAgregar={agregarTarea} />
      <PerfilUsuario />
    </div>
  );
}

function AgregarTarea({ onAgregar }) {
  const [texto, setTexto] = useState('');

  function manejarEnvio(e) {
    e.preventDefault();
    onAgregar(texto);
    setTexto('');
  }

  return (
    <form onSubmit={manejarEnvio} className="form-agregar">
      <input
        value={texto}
        onChange={(e) => setTexto(e.target.value)}
        placeholder="Nueva tarea"
      />
      <button type="submit">Agregar</button>
    </form>
  );
}

function PerfilUsuario() {
  const [usuario, setUsuario] = useState(null);

  useEffect(() => {
    obtenerUsuario();
  }, []);

  // Simula una llamada a una API que a veces falla (como pasa en la vida real)
  function obtenerUsuario() {
    const exito = Math.random() > 0.5;

    setTimeout(() => {
      if (exito) {
        setUsuario({ nombre: 'Estudiante React' });
      } else {
        // 🐛 BUG 5 — Se lanza un error pero nadie lo atrapa (no hay try/catch)
        // y nadie usa console.error para avisar. El resultado: la pantalla
        // se queda en "Cargando perfil..." para siempre y el único rastro
        // del problema es un error en rojo en la consola que nadie mira.
        throw new Error('No se pudo cargar el usuario');
      }
    }, 1000);
  }

  if (!usuario) return <p className="perfil">Cargando perfil...</p>;

  return <p className="perfil">Perfil: {usuario.nombre}</p>;
}

export default App;
```

### `src/App.css`

```css
.app {
  max-width: 480px;
  margin: 40px auto;
  padding: 24px;
  font-family: 'Segoe UI', system-ui, sans-serif;
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
}

h1 {
  margin-top: 0;
  color: #1f2937;
}

.filtros {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

.filtros button {
  flex: 1;
  padding: 8px;
  border: 1px solid #d1d5db;
  background: #f9fafb;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
}

.filtros button:hover {
  background: #e5e7eb;
}

.lista-tareas {
  list-style: none;
  padding: 0;
  margin: 0 0 20px;
}

.lista-tareas li {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 8px;
  padding: 10px 12px;
  border: 1px solid #e5e7eb;
  border-radius: 8px;
  margin-bottom: 8px;
}

.lista-tareas li.completada span:first-child {
  text-decoration: line-through;
  color: #9ca3af;
}

.categoria {
  font-size: 11px;
  font-weight: 700;
  color: #6366f1;
  background: #eef2ff;
  padding: 2px 8px;
  border-radius: 999px;
}

.form-agregar {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}

.form-agregar input {
  flex: 1;
  padding: 8px 10px;
  border: 1px solid #d1d5db;
  border-radius: 6px;
}

.form-agregar button {
  padding: 8px 14px;
  border: none;
  background: #6366f1;
  color: white;
  border-radius: 6px;
  cursor: pointer;
}

.perfil {
  font-size: 14px;
  color: #4b5563;
  border-top: 1px solid #e5e7eb;
  padding-top: 12px;
}
```

---

