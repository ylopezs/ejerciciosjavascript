# Taller de Funciones en JavaScript y React — Calculadora

## Objetivo

Los estudiantes crearán una calculadora en dos etapas:

1. **Node.js**
   - Crear funciones matemáticas.
   - Ejecutar el archivo desde terminal usando Node.
   - Completar el código interno de las funciones.

2. **React + Vite**
   - Crear una interfaz gráfica.
   - Reutilizar las funciones creadas anteriormente.
   - Integrar lógica y eventos en React.

---

# Parte 1 — Calculadora en Node.js

## Objetivos de aprendizaje

- Crear funciones en JavaScript.
- Entender parámetros y retorno.
- Ejecutar archivos con Node.
- Organizar código.

---

# Paso 1 — Crear el proyecto

Crear una carpeta:

```bash
mkdir 14-05
cd 14-05
mkdir calculadora-funciones
cd calculadora-funciones
```

Crear el archivo:

```bash
touch app.js
```

---

# Paso 2 — Código base

## Archivo `app.js`

> Los estudiantes deben completar el interior de las funciones.

```js
// =====================================
// CALCULADORA CON FUNCIONES
// =====================================

// Función suma
function sumar(a, b) {

}

// Función resta
function restar(a, b) {

}

// Función multiplicación
function multiplicar(a, b) {

}

// Función división
function dividir(a, b) {

}

// =====================================
// LLAMADO DE FUNCIONES
// =====================================

console.log("Resultado suma:");
console.log(sumar(10, 5));

console.log("----------------");

console.log("Resultado resta:");
console.log(restar(10, 5));

console.log("----------------");

console.log("Resultado multiplicación:");
console.log(multiplicar(10, 5));

console.log("----------------");

console.log("Resultado división:");
console.log(dividir(10, 5));
```

---

# Paso 3 — Ejecutar el programa

En terminal:

```bash
node app.js
```

---

# Resultado esperado

```bash
Resultado suma:
15

----------------

Resultado resta:
5

----------------

Resultado multiplicación:
50

----------------

Resultado división:
2
```

---

# Retos adicionales

## Reto 1 — Potencia

Crear una función:

```js
function potencia(a, b) {

}
```

Ejemplo:

```js
potencia(2, 3)
```

Resultado:

```bash
8
```

---

## Reto 2 — Número mayor

Crear una función que retorne el número mayor.

```js
function mayor(a, b) {

}
```

---

## Reto 3 — Validación de división

Evitar dividir por cero.

Ejemplo esperado:

```bash
No se puede dividir por cero
```

---

# Parte 2 — Calculadora en React con Vite

## Objetivos de aprendizaje

- Crear proyectos React.
- Entender componentes.
- Manejar estados.
- Usar eventos.
- Reutilizar funciones JavaScript.

---

# Paso 1 — Crear el proyecto

Crear proyecto:

```bash
npm create vite@latest
```

Nombre:

```bash
calculadora-react
```

Seleccionar:

```bash
React
```

Luego:

```bash
cd calculadora-react
npm install
npm run dev
```

---

# Paso 2 — Limpiar el proyecto

Eliminar contenido innecesario de:

- `App.css`
- `index.css`

---

# Paso 3 — Crear la calculadora

## Archivo `src/App.jsx`

> Los estudiantes deben completar únicamente las funciones matemáticas.

```jsx
import { useState } from "react";

function App() {

  const [numero1, setNumero1] = useState("");
  const [numero2, setNumero2] = useState("");

  // =====================================
  // FUNCIONES
  // =====================================

  function sumar(a, b) {

  }

  function restar(a, b) {

  }

  function multiplicar(a, b) {

  }

  function dividir(a, b) {

  }

  // =====================================
  // EVENTOS
  // =====================================

  function handleSuma() {
    alert(sumar(Number(numero1), Number(numero2)));
  }

  function handleResta() {
    alert(restar(Number(numero1), Number(numero2)));
  }

  function handleMultiplicacion() {
    alert(multiplicar(Number(numero1), Number(numero2)));
  }

  function handleDivision() {
    alert(dividir(Number(numero1), Number(numero2)));
  }

  return (
    <div style={{ padding: "40px" }}>
      <h1>Calculadora React</h1>

      <input
        type="number"
        placeholder="Número 1"
        value={numero1}
        onChange={(e) => setNumero1(e.target.value)}
      />

      <br />
      <br />

      <input
        type="number"
        placeholder="Número 2"
        value={numero2}
        onChange={(e) => setNumero2(e.target.value)}
      />

      <br />
      <br />

      <button onClick={handleSuma}>
        Sumar
      </button>

      <button onClick={handleResta}>
        Restar
      </button>

      <button onClick={handleMultiplicacion}>
        Multiplicar
      </button>

      <button onClick={handleDivision}>
        Dividir
      </button>
    </div>
  );
}

export default App;
```

---

# Resultado esperado

La aplicación debe:

- Recibir dos números.
- Ejecutar operaciones matemáticas.
- Mostrar resultados usando `alert()`.

---

# Retos adicionales React

## Reto 1 — Resultado en pantalla

En vez de usar `alert`, mostrar el resultado en un `<h2>`.

---

## Reto 2 — Botón limpiar

Agregar un botón:

```bash
Limpiar
```

Debe borrar los inputs.

---

## Reto 3 — Diseño

Agregar estilos:

- Colores
- Bordes
- Espaciado
- Hover en botones

---

# Preguntas de reflexión

1. ¿Qué es una función?
2. ¿Qué son los parámetros?
3. ¿Qué hace `return`?
4. ¿Qué diferencia hay entre Node y React?
5. ¿Qué hace `useState`?
6. ¿Por qué usamos `Number()` en React?
