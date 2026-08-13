# Taller: `useEffect` y el Ciclo de Vida en React
### Cuándo se ejecuta cada cosa, y por qué eso importa

**Duración sugerida:** 90–120 minutos
**Requisito previo:** haber hecho el taller de debugging con la consola (DevTools, `console.log`, lectura de errores)

---

## Objetivos de aprendizaje

Al terminar este taller, cada estudiante va a poder:

1. Explicar las tres fases del ciclo de vida de un componente: **montaje**, **actualización** y **desmontaje**.
2. Predecir cuándo se ejecuta un `useEffect` según su arreglo de dependencias (sin arreglo, arreglo vacío `[]`, arreglo con variables).
3. Reconocer y corregir 4 bugs clásicos de `useEffect`: falta de *cleanup*, *stale closures* (closures obsoletas), listeners duplicados y dependencias faltantes.
4. Usar `console.log` para "ver" el ciclo de vida en acción, no solo intuirlo.
5. Crear el proyecto desde cero en la terminal de Codespaces, y subir el trabajo con `git` al finalizar.

---

## Preparación (en Codespaces)

Este taller se trabaja en una carpeta nueva, con la fecha de hoy, dentro del mismo repositorio de workspace que ya vienen usando.

1. Abrir la terminal del Codespace y ubicarse en la raíz del repositorio.
2. Crear la carpeta del día (usen el mismo formato de fecha que ya vienen usando, por ejemplo `2026-08-13`) y entrar en ella:
   ```bash
   mkdir 2026-08-13
   cd 2026-08-13
   ```
3. Crear el proyecto de React con Vite **dentro de esa carpeta**:
   ```bash
   npm create vite@latest . -- --template react
   ```
   > El `.` le dice a Vite que use la carpeta actual en vez de crear una nueva. Cuando pregunte, confirmen que la carpeta no está vacía (va a tener el `.git` del repo, eso es normal).
4. Instalar dependencias y correr el proyecto:
   ```bash
   npm install
   npm run dev
   ```
5. Abrir la app desde el preview/puerto que ofrece Codespaces.
6. Reemplazar `src/App.jsx` y `src/App.css` por el código de la sección **"Código del taller"** más abajo.

> No hace falta correr `git init`: ya están dentro del repositorio del workspace. Al final del taller van a hacer `git add`, `commit` y `push` normal.

---

## Parte 1 — Las tres fases, en teoría (10 min)

Todo componente de React pasa por tres momentos:

| Fase | ¿Cuándo pasa? | ¿Qué se hace ahí típicamente? |
|---|---|---|
| **Montaje** | La primera vez que el componente aparece en pantalla | Pedir datos a una API, iniciar un timer, suscribirse a un evento |
| **Actualización** | Cada vez que cambia el estado o las props que el efecto "vigila" | Volver a pedir datos si cambió un id, recalcular algo |
| **Desmontaje** | Cuando el componente desaparece de la pantalla | Cancelar el timer, quitar el listener, cancelar la petición pendiente — esto es el **cleanup** |

El arreglo de dependencias de `useEffect` es lo que controla cuándo se repite el efecto:

```js
useEffect(() => { /* ... */ });          // se ejecuta en CADA render (casi nunca es lo que quieren)
useEffect(() => { /* ... */ }, []);      // se ejecuta UNA sola vez, al montar
useEffect(() => { /* ... */ }, [algo]);  // se ejecuta al montar, y de nuevo cada vez que 'algo' cambia
```

Y el *cleanup* es la función que devuelven (opcionalmente) desde adentro del efecto:

```js
useEffect(() => {
  const id = setInterval(() => console.log('tick'), 1000);
  return () => clearInterval(id); // esto corre antes del próximo efecto, y al desmontar
}, []);
```

---

## Parte 2 — A predecir antes de correr (10 min)

Antes de tocar código, en grupos de 2, para cada línea de la tabla anterior, que escriban en un papel o chat: *"esto se ejecuta cuando..."*. Después lo comparan corriendo el `ExperimentoFases` de la Parte 4.

---

## Parte 3 — Cacería de bugs (50–60 min)

El código de más abajo tiene **4 bugs intencionales** relacionados con el ciclo de vida, marcados con `🐛 BUG N`. No dependen unos de otros como en el taller anterior — pueden resolverlos en cualquier orden, pero se sugiere este.

Para cada uno: observar el comportamiento raro, agregar `console.log` donde haga falta para confirmar la hipótesis, y recién después corregir.

### 🐛 Bug 1 — El reloj sigue corriendo aunque lo oculten

- Hacer clic en "Ocultar reloj" y mirar la consola: ¿los logs de "tick" paran o siguen?
- Pista: buscar el `setInterval` dentro de `Reloj` y fijarse si el efecto devuelve algo al final.
- **Pregunta guía:** si el componente ya no está en pantalla, ¿por qué el intervalo seguiría corriendo? ¿Qué problema real causaría esto en una app grande (pista: memory leak)?

### 🐛 Bug 2 — El contador automático se queda pegado en 1

- Observar: el número en pantalla sube a 1 y no avanza más, pero la consola sigue imprimiendo un mensaje cada segundo.
- Agregar `console.log('valor real vs valor del efecto')` comparando lo que muestra la pantalla contra lo que loguea el efecto.
- Pista: esto se llama **stale closure** (closure obsoleta) — el efecto "recuerda" el valor de `contador` que existía cuando se creó, no el valor actual.
- Dos formas de arreglarlo: usar la forma funcional `setContador(c => c + 1)`, o agregar `contador` a las dependencias (con su cleanup correspondiente). Prueben ambas y comparen.

### 🐛 Bug 3 — El ancho de ventana duplica los mensajes

- Abrir la consola, y cambiar el tamaño de la ventana del navegador (o del panel) varias veces.
- ¿Cuántas veces se imprime el mismo evento de resize la segunda vez, comparado con la primera?
- Pista: mirar el arreglo de dependencias de `RastreadorVentana`. ¿Debería el efecto "reiniciarse" cada vez que cambia el ancho?
- **Pregunta guía:** ¿qué falta al final del efecto para que no se acumulen listeners?

### 🐛 Bug 4 — El nombre no cambia al elegir otro usuario

- Hacer clic en "Usuario 2". El botón cambia de estado, pero el nombre en pantalla sigue mostrando el del Usuario 1.
- Agregar un `console.log(id)` dentro del efecto de `PerfilUsuario` y ver si se imprime de nuevo al cambiar de usuario.
- Pista: revisar el arreglo de dependencias del efecto. ¿Incluye la prop `id`?

---

## Parte 4 — Experimento guiado: viendo las fases en acción (15–20 min)

El componente `ExperimentoFases` (al final del código) usa `console.log` para mostrar exactamente cuándo pasa cada fase. Hagan lo siguiente, probando **una variante a la vez** y anotando qué pasa:

1. Tal como está ahora, con `[clics]` como dependencia: hacer clic varias veces en el botón y leer la consola. ¿Cuándo aparece 🟢 MONTADO, 🔵 ACTUALIZADO y 🔴 LIMPIEZA?
2. Cambiar la dependencia a un arreglo vacío `[]`. Antes de correrlo, predecir: ¿va a aparecer 🔵 ACTUALIZADO alguna vez?
3. Quitar el arreglo de dependencias por completo. Predecir de nuevo antes de correr: ¿qué va a pasar con 🔴 LIMPIEZA?
4. Envolver el `<ExperimentoFases />` en el mismo `mostrar/ocultar` que usaron para el reloj (pueden copiar ese patrón), para ver el 🔴 LIMPIEZA que corresponde al desmontaje real.

**Pregunta de cierre:** ¿por qué el *cleanup* se ejecuta tanto al desmontar como justo antes de que el efecto se repita? ¿Qué relación tiene esto con el Bug 3?

---

## Código del taller

### `src/App.jsx`

```jsx
import { useState, useEffect, useRef } from 'react';
import './App.css';

function App() {
  const [mostrarReloj, setMostrarReloj] = useState(true);
  const [usuarioId, setUsuarioId] = useState(1);

  return (
    <div className="app">
      <h1>useEffect y Ciclo de Vida</h1>

      <section>
        <h2>1. Reloj</h2>
        <button onClick={() => setMostrarReloj(!mostrarReloj)}>
          {mostrarReloj ? 'Ocultar reloj' : 'Mostrar reloj'}
        </button>
        {mostrarReloj && <Reloj />}
      </section>

      <section>
        <h2>2. Contador automático</h2>
        <ContadorAutomatico />
      </section>

      <section>
        <h2>3. Ancho de ventana</h2>
        <RastreadorVentana />
      </section>

      <section>
        <h2>4. Perfil de usuario</h2>
        <div className="botones-usuario">
          <button onClick={() => setUsuarioId(1)}>Usuario 1</button>
          <button onClick={() => setUsuarioId(2)}>Usuario 2</button>
        </div>
        <PerfilUsuario id={usuarioId} />
      </section>

      <section>
        <h2>5. Experimento: fases del ciclo de vida</h2>
        <ExperimentoFases />
      </section>
    </div>
  );
}

function Reloj() {
  const [segundos, setSegundos] = useState(0);

  // 🐛 BUG 1 — Este efecto arranca un setInterval pero nunca lo limpia.
  // Cuando el componente se desmonta (al ocultar el reloj), el intervalo
  // sigue corriendo en segundo plano. Abran la consola, oculten el reloj
  // con el botón, y fíjense si los logs de "tick" siguen apareciendo.
  useEffect(() => {
    console.log('⏰ Reloj montado');
    const id = setInterval(() => {
      setSegundos((s) => {
        console.log('tick, segundos:', s + 1);
        return s + 1;
      });
    }, 1000);
    // Falta el cleanup acá: return () => clearInterval(id);
  }, []);

  return <p>Segundos: {segundos}</p>;
}

function ContadorAutomatico() {
  const [contador, setContador] = useState(0);

  useEffect(() => {
    const id = setInterval(() => {
      // 🐛 BUG 2 — 'contador' quedó "congelado" en el valor que tenía
      // cuando el efecto se creó (stale closure), porque el arreglo de
      // dependencias está vacío pero acá adentro se lee 'contador'
      // directamente en vez de usar la forma funcional de setState.
      console.log('El contador según el efecto es:', contador);
      setContador(contador + 1);
    }, 1000);
    return () => clearInterval(id);
  }, []);

  return <p>Contador: {contador}</p>;
}

function RastreadorVentana() {
  const [ancho, setAncho] = useState(window.innerWidth);

  // 🐛 BUG 3 — El efecto depende de 'ancho', así que cada vez que la
  // ventana cambia de tamaño el efecto se vuelve a ejecutar y agrega
  // OTRO listener de resize, sin haber quitado el anterior. Prueben
  // achicar/agrandar la ventana varias veces y cuenten cuántas veces
  // se repite el mismo mensaje en consola.
  useEffect(() => {
    function manejarResize() {
      console.log('Resize detectado, ancho:', window.innerWidth);
      setAncho(window.innerWidth);
    }
    window.addEventListener('resize', manejarResize);
    // Falta el cleanup acá: return () => window.removeEventListener('resize', manejarResize);
  }, [ancho]);

  return <p>Ancho actual: {ancho}px</p>;
}

function PerfilUsuario({ id }) {
  const [nombre, setNombre] = useState('');

  // 🐛 BUG 4 — El efecto usa 'id' pero no lo incluye en el arreglo de
  // dependencias. Por eso, aunque cambien de usuario con los botones,
  // el efecto no se vuelve a ejecutar y el nombre no se actualiza.
  useEffect(() => {
    console.log('Buscando datos del usuario', id);
    const nombres = { 1: 'Ana', 2: 'Luis' };
    setNombre(nombres[id]);
  }, []);

  return <p>Nombre: {nombre}</p>;
}

function ExperimentoFases() {
  const [clics, setClics] = useState(0);
  const esPrimeraVez = useRef(true);

  // Este componente no tiene ningún bug: es para experimentar cambiando
  // el arreglo de dependencias (ver Parte 4 de la guía) y observar
  // cuándo se ejecuta cada log.
  useEffect(() => {
    if (esPrimeraVez.current) {
      console.log('🟢 MONTADO');
      esPrimeraVez.current = false;
    } else {
      console.log('🔵 ACTUALIZADO, clics:', clics);
    }

    return () => {
      console.log('🔴 LIMPIEZA (antes del próximo efecto, o al desmontar)');
    };
  }, [clics]);

  return (
    <div>
      <p>Clics: {clics}</p>
      <button onClick={() => setClics(clics + 1)}>Clickeame</button>
    </div>
  );
}

export default App;
```

### `src/App.css`

```css
.app {
  max-width: 560px;
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

section {
  border-top: 1px solid #e5e7eb;
  padding: 16px 0;
}

section:first-of-type {
  border-top: none;
}

h2 {
  font-size: 15px;
  color: #6366f1;
  margin: 0 0 10px;
}

button {
  padding: 8px 14px;
  border: 1px solid #d1d5db;
  background: #f9fafb;
  border-radius: 6px;
  cursor: pointer;
  font-size: 14px;
  margin-right: 8px;
}

button:hover {
  background: #e5e7eb;
}

.botones-usuario {
  margin-bottom: 8px;
}

p {
  color: #374151;
}
```

---

## Entregable del taller

Por cada uno de los 4 bugs:

1. Descripción del comportamiento raro observado.
2. El `console.log` que usaron para confirmar la causa.
3. La corrección aplicada.
4. Una frase explicando en qué fase del ciclo de vida estaba el problema (montaje, actualización o falta de limpieza al desmontar).

Más las anotaciones del Experimento guiado (Parte 4): qué predijeron vs. qué pasó realmente con cada variante del arreglo de dependencias.

---

## Cierre: commit y push

Terminado el taller, desde la terminal del Codespace, parados en la raíz del repositorio (no dentro de la carpeta del proyecto):

```bash
git add 2026-08-13
git commit -m "Taller useEffect y ciclo de vida"
git push
```

> Si Codespaces les pide configurar `user.name` y `user.email` de git, es la primera vez que hacen commit en ese Codespace — es normal, solo hay que completarlo una vez.

---

## Para el profesor: soluciones de referencia

<details>
<summary>Ver soluciones (clic para expandir)</summary>

1. Agregar `return () => clearInterval(id);` al final del efecto de `Reloj`.
2. Cambiar `setContador(contador + 1)` por `setContador((c) => c + 1)` dentro del `setInterval` (así no depende del `contador` capturado por el closure).
3. Agregar `return () => window.removeEventListener('resize', manejarResize);` al final del efecto de `RastreadorVentana`. (Punto extra: también se puede discutir por qué convendría usar `[]` como dependencia en vez de `[ancho]`, ya que el listener no necesita reiniciarse.)
4. Cambiar `}, [])` por `}, [id])` en el efecto de `PerfilUsuario`.

</details>
