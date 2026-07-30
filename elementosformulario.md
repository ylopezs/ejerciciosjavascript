# Ejercicio: Formulario completo en React

## 📁 Organización en el repo `workspace`

```
workspace/
  dia-01/
  dia-02/
  ...
  dia-XX-formulario-completo/    ← el de hoy
```

## 🚀 Pasos en GitHub Codespaces

1. Abrir el repo `workspace` → botón **Code → Codespaces → Create codespace on main**.
2. En la terminal del Codespace:

```bash
mkdir dia-XX-formulario-completo
cd dia-XX-formulario-completo
npm create vite@latest . -- --template react
npm install
```

3. Correr el proyecto:

```bash
npm run dev -- --host
```

4. Codespaces detecta el puerto (5173) y muestra un pop-up **"Open in Browser"**, o aparece en la pestaña **PORTS** de VS Code. Ahí se abre.

5. Al terminar:

```bash
git add .
git commit -m "dia XX: formulario completo"
git push
```

## 📝 Enunciado

Construir un formulario de **"Registro de estudiante"** que use **todos** estos tipos de elementos (no solo `input type="text"`):

- `input type="text"` → nombre
- `input type="email"` → correo
- `input type="password"` → contraseña
- `input type="number"` → edad
- `input type="date"` → fecha de nacimiento
- `input type="range"` → nivel de experiencia (1-10)
- `input type="checkbox"` (uno solo) → aceptar términos
- `input type="checkbox"` (varios) → lenguajes que conoce (JS, Python, Java...)
- `input type="radio"` (grupo) → modalidad (presencial/virtual)
- `select` con `option` → país
- `textarea` → comentarios
- `input type="file"` → foto de perfil
- `input type="color"` → color favorito
- `button type="submit"` → enviar

## ✅ Requisitos funcionales

1. Cada campo debe estar controlado con `useState`.
2. Al enviar el formulario (`onSubmit`, con `e.preventDefault()`), mostrar en pantalla un resumen de todos los datos ingresados.
3. El checkbox de "aceptar términos" es obligatorio: si no está marcado, el botón de enviar debe estar deshabilitado.
4. Mostrar en tiempo real el valor del `range` al lado del slider.
5. Validar que el email tenga formato válido antes de permitir enviar (puede ser básico, con una expresión regular simple).

## 🌟 Nivel extra (opcional)

- Validar que la edad sea mayor a 0.
- Mostrar una vista previa de la imagen subida con `URL.createObjectURL(foto)`.

## 📦 Entregable

- Código fuente dentro de `dia-XX-formulario-completo/` en el repo `workspace`, con commit y push realizados desde el Codespace.
