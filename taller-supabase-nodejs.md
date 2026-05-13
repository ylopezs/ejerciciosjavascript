# Taller Supabase + Node.js — API REST y Console.log

# Taller Básico de Supabase + Node.js

## Objetivos

Aprender a:

- Crear tablas en Supabase
- Relacionar tablas
- Insertar datos
- Consultar datos SQL
- Usar la API REST automática de Supabase
- Consumir la API usando Node.js
- Mostrar resultados con `console.log()`

---

# Parte 1 — Crear proyecto en Supabase

Ir a:

https://supabase.com

Pasos:

1. Crear cuenta
2. Crear nuevo proyecto
3. Esperar que la base de datos termine de configurarse

---

# Parte 2 — Crear tablas

## Abrir SQL Editor

En el menú lateral:

- SQL Editor
- New Query

---

# Tabla `cursos`

```sql
CREATE TABLE cursos (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    profesor VARCHAR(100) NOT NULL,
    activo BOOLEAN DEFAULT true
);
```

---

# Tabla `estudiantes`

```sql
CREATE TABLE estudiantes (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    nombre VARCHAR(100) NOT NULL,
    edad INT,
    email VARCHAR(150) UNIQUE,
    curso_id BIGINT,

    CONSTRAINT fk_curso
        FOREIGN KEY(curso_id)
        REFERENCES cursos(id)
);
```

---

# Parte 3 — Insertar datos

## Insertar cursos

```sql
INSERT INTO cursos (nombre, profesor)
VALUES
('React Básico', 'Carlos'),
('Base de Datos', 'Laura'),
('JavaScript Avanzado', 'Andrés');
```

---

## Insertar estudiantes

```sql
INSERT INTO estudiantes (nombre, edad, email, curso_id)
VALUES
('Ana', 20, 'ana@email.com', 1),
('Luis', 22, 'luis@email.com', 2),
('María', 19, 'maria@email.com', 1),
('Pedro', 25, 'pedro@email.com', 3);
```

---

# Parte 4 — Consultas SQL

## Ver cursos

```sql
SELECT * FROM cursos;
```

---

## Ver estudiantes

```sql
SELECT * FROM estudiantes;
```

---

## JOIN estudiantes + cursos

```sql
SELECT 
    estudiantes.nombre AS estudiante,
    estudiantes.edad,
    cursos.nombre AS curso,
    cursos.profesor
FROM estudiantes
INNER JOIN cursos
ON estudiantes.curso_id = cursos.id;
```

---

# Parte 5 — Obtener credenciales API

Ir a:

- Settings
- API

Copiar:

- Project URL
- anon public key

---

# Parte 6 — Crear proyecto Node.js

## Crear carpeta

```bash
mkdir supabase-api
```

---

## Entrar a la carpeta

```bash
cd supabase-api
```

---

## Inicializar Node.js

```bash
npm init -y
```

---

# Parte 7 — Crear archivo JavaScript

Crear archivo:

```txt
app.js
```

---

# Parte 8 — Consumir API REST

## Código completo

```javascript
const url =
  "https://TU-PROYECTO.supabase.co/rest/v1/estudiantes";

const headers = {
  apikey: "TU_ANON_KEY",
  Authorization: "Bearer TU_ANON_KEY",
  "Content-Type": "application/json",
};

async function obtenerEstudiantes() {
  try {

    console.log("Consultando API...");

    const response = await fetch(url, {
      method: "GET",
      headers,
    });

    const data = await response.json();

    console.log("Respuesta de la API:");
    console.log(data);

  } catch (error) {

    console.log("Error:");
    console.log(error);

  }
}

obtenerEstudiantes();
```

---

# Parte 9 — Ejecutar el programa

## Ejecutar Node.js

```bash
node app.js
```

---

# Resultado esperado

```bash
Consultando API...

Respuesta de la API:

[
  {
    id: 1,
    nombre: 'Ana',
    edad: 20,
    email: 'ana@email.com',
    curso_id: 1
  }
]
```

---

# Parte 10 — Insertar datos desde Node.js

```javascript
const url =
  "https://TU-PROYECTO.supabase.co/rest/v1/estudiantes";

const headers = {
  apikey: "TU_ANON_KEY",
  Authorization: "Bearer TU_ANON_KEY",
  "Content-Type": "application/json",
  Prefer: "return=representation",
};

async function crearEstudiante() {

  const estudiante = {
    nombre: "Camila",
    edad: 23,
    email: "camila@email.com",
    curso_id: 1,
  };

  try {

    console.log("Insertando estudiante...");

    const response = await fetch(url, {
      method: "POST",
      headers,
      body: JSON.stringify(estudiante),
    });

    const data = await response.json();

    console.log("Estudiante creado:");
    console.log(data);

  } catch (error) {

    console.log("Error:");
    console.log(error);

  }
}

crearEstudiante();
```

---

# Parte 11 — Actualizar datos

```javascript
const url =
  "https://TU-PROYECTO.supabase.co/rest/v1/estudiantes?id=eq.1";

const headers = {
  apikey: "TU_ANON_KEY",
  Authorization: "Bearer TU_ANON_KEY",
  "Content-Type": "application/json",
};

async function actualizarEstudiante() {

  try {

    console.log("Actualizando estudiante...");

    const response = await fetch(url, {
      method: "PATCH",
      headers,
      body: JSON.stringify({
        edad: 30,
      }),
    });

    console.log("Estudiante actualizado");

  } catch (error) {

    console.log(error);

  }
}

actualizarEstudiante();
```

---

# Parte 12 — Eliminar datos

```javascript
const url =
  "https://TU-PROYECTO.supabase.co/rest/v1/estudiantes?id=eq.1";

const headers = {
  apikey: "TU_ANON_KEY",
  Authorization: "Bearer TU_ANON_KEY",
};

async function eliminarEstudiante() {

  try {

    console.log("Eliminando estudiante...");

    await fetch(url, {
      method: "DELETE",
      headers,
    });

    console.log("Estudiante eliminado");

  } catch (error) {

    console.log(error);

  }
}

eliminarEstudiante();
```

---

# Parte 13 — Ejercicios

## Ejercicio 1

Insertar 3 estudiantes nuevos desde Node.js.

---

## Ejercicio 2

Consultar únicamente estudiantes mayores de 20 años.

Pista:

```txt
?edad=gt.20
```

---

## Ejercicio 3

Actualizar el nombre de un estudiante.

---

## Ejercicio 4

Eliminar un estudiante específico.

---

## Ejercicio 5

Mostrar los resultados usando:

```javascript
console.table(data)
```

---

# Parte 14 — Próximo Taller

Más adelante podrán aprender:

- React + Supabase
- Login
- Registro de usuarios
- Roles
- Permisos
- RLS
