# 🧪 Taller React --- Lista de Estudiantes

## 📁 1. Estructura inicial

En su repositorio `workspace`, creen una carpeta con la fecha de hoy:

    workspace/
      05-05

Entren a esa carpeta desde la terminal.

------------------------------------------------------------------------

## ⚛️ 2. Creación del proyecto

Usen Vite (recomendado por rapidez):

``` bash
cd 05-05
npm create vite@latest
```

-   Nombre del proyecto: `lista-estudiantes`
-   Framework: `React`
-   Lenguaje: `JavaScript`

Luego ejecuten:

``` bash
cd lista-estudiantes
npm install
npm run dev
```

------------------------------------------------------------------------

## 🧹 3. Limpieza del proyecto

Eliminar o modificar:

-   ❌ `App.css`
-   ❌ contenido de `index.css` (opcional)
-   ❌ imágenes innecesarias

Modificar `App.jsx`:

``` jsx
function App() {
  return (
    <>
      <h1>Lista de Estudiantes</h1>
    </>
  );
}

export default App;
```

------------------------------------------------------------------------

## 👤 4. Crear componente `Estudiante`

Crear carpeta `components` y dentro el archivo:

📄 `Estudiante.jsx`

``` jsx
function Estudiante(props) {
  return (
    <div>
      <p>Nombre: {props.nombre}</p>
      <p>Edad: {props.edad}</p>
    </div>
  );
}

export default Estudiante;
```

------------------------------------------------------------------------

## 🔌 5. Uso de Props

Editar `App.jsx`:

``` jsx
import Estudiante from "./components/Estudiante";

function App() {
  return (
    <>
      <h1>Lista de Estudiantes</h1>

      <Estudiante nombre="Juan" edad={20} />
      <Estudiante nombre="Maria" edad={22} />
    </>
  );
}

export default App;
```

------------------------------------------------------------------------

## 📋 6. Crear componente `Lista`

📄 `Lista.jsx`

``` jsx
import Estudiante from "./Estudiante";

function Lista() {
  return (
    <div>
      <Estudiante nombre="Carlos" edad={21} />
      <Estudiante nombre="Ana" edad={19} />
    </div>
  );
}

export default Lista;
```

------------------------------------------------------------------------

## 🔁 7. Inyección de props con un vector

Editar `Lista.jsx`:

``` jsx
import Estudiante from "./Estudiante";

function Lista() {
  const estudiantes = [
    { nombre: "Carlos", edad: 21 },
    { nombre: "Ana", edad: 19 },
    { nombre: "Luis", edad: 23 },
  ];

  return (
    <div>
      {estudiantes.map((est, index) => (
        <Estudiante
          key={index}
          nombre={est.nombre}
          edad={est.edad}
        />
      ))}
    </div>
  );
}

export default Lista;
```

------------------------------------------------------------------------

## 🔗 8. Usar `Lista` en `App`

``` jsx
import Lista from "./components/Lista";

function App() {
  return (
    <>
      <h1>Lista de Estudiantes</h1>
      <Lista />
    </>
  );
}

export default App;
```

------------------------------------------------------------------------

## 🎯 Objetivo del taller

Al finalizar, el estudiante debe entender:

-   ✔ Creación de un proyecto en React
-   ✔ Estructura básica
-   ✔ Componentes reutilizables
-   ✔ Uso de props
-   ✔ Renderizado dinámico con arrays (`map`)

------------------------------------------------------------------------

## 🚀 Extra (opcional)

-   Agregar campo `carrera`
-   Agregar estilos básicos
-   Mostrar estudiantes mayores de edad (condicional)
