# 📘 Tutorial: Cómo crear el README.md de tu proyecto

Este tutorial es para que **tú y tu equipo** creen su propio archivo `README.md`, explicando su proyecto de React + Supabase desarrollado en Codespaces. No es un README ya hecho — es la guía para que ustedes lo construyan.

---

## 🎯 ¿Para qué sirve un README?

Es la primera página que ve cualquier persona que entra a tu repositorio de GitHub. Debe responder tres preguntas básicas:

1. ¿Qué hace este proyecto?
2. ¿Cómo lo pongo a funcionar?
3. ¿Cómo puedo contribuir o usarlo?

Un buen README hace que cualquier compañero (o el profesor) pueda clonar el proyecto y ejecutarlo sin tener que preguntarles nada.

---

## 1️⃣ Crear el archivo

Dentro de Codespaces, en la raíz del proyecto, abre una terminal y ejecuta:

```bash
touch README.md
```

Si el archivo ya existe (muchos proyectos de React lo crean automáticamente), simplemente ábranlo desde el explorador de archivos de VS Code.

---

## 2️⃣ Estructura recomendada

Un README completo normalmente sigue este orden. Cópienlo como plantilla y llénenlo con la información real de su proyecto:

```markdown
# Nombre del Proyecto

Breve descripción de qué hace la aplicación (2-3 líneas).

## 🖼️ Capturas de pantalla
(opcional, pero se ve muy bien)

## ⚙️ Tecnologías usadas
- React
- Supabase
- Vite / Create React App
- (otras librerías que usen)

## 🚀 Instalación y ejecución
Pasos para clonar y correr el proyecto localmente o en Codespaces.

## 🔑 Variables de entorno
Qué variables necesita el proyecto (sin exponer las claves reales).

## 📁 Estructura del proyecto
Breve explicación de las carpetas principales.

## ✨ Funcionalidades
Lista de lo que la app permite hacer.

## 👥 Autores
Nombres del equipo.

## 📄 Licencia
(opcional)
```

---

## 3️⃣ Cómo redactar cada sección

### Nombre y descripción
Escriban el nombre del proyecto como un título con `#` y debajo una descripción corta pero clara. Ejemplo:

```markdown
# TaskFlow

Aplicación web para gestionar tareas en equipo, con autenticación de usuarios y base de datos en tiempo real usando Supabase.
```

### Tecnologías usadas
Usen una lista con guiones (`-`). Pueden agregar el logo o badge de cada tecnología si quieren que se vea más profesional (opcional, no obligatorio).

### Instalación y ejecución
Aquí van los **comandos exactos** que alguien debe correr. Usen bloques de código con triple backtick para que se vean formateados:

````markdown
```bash
git clone https://github.com/usuario/repositorio.git
cd repositorio
npm install
npm run dev
```
````

### Variables de entorno
Expliquen **qué** variables se necesitan, pero **nunca** pongan las claves reales de Supabase en el README. Ejemplo correcto:

```markdown
Crea un archivo `.env` en la raíz con:

VITE_SUPABASE_URL=tu_url_de_supabase
VITE_SUPABASE_ANON_KEY=tu_clave_publica
```

### Estructura del proyecto
Una lista simple de carpetas importantes, por ejemplo:

```markdown
- `src/components` → componentes reutilizables de React
- `src/pages` → vistas principales de la app
- `src/supabaseClient.js` → configuración de conexión a Supabase
```

### Funcionalidades
Lista de lo que hace la app, en formato de checklist si quieren:

```markdown
- [x] Registro e inicio de sesión
- [x] Crear, editar y eliminar tareas
- [ ] Notificaciones en tiempo real (en desarrollo)
```

### Autores
Simplemente los nombres del equipo, y opcionalmente su usuario de GitHub.

---

## 4️⃣ Sintaxis básica de Markdown que necesitan

| Elemento | Sintaxis |
|---|---|
| Título | `# Título` `## Subtítulo` |
| Negrita | `**texto**` |
| Cursiva | `*texto*` |
| Lista | `- item` |
| Lista numerada | `1. item` |
| Enlace | `[texto](url)` |
| Imagen | `![alt](url-de-la-imagen)` |
| Código en línea | `` `código` `` |
| Bloque de código | ```` ```lenguaje ... ``` ```` |
| Checklist | `- [ ] pendiente` / `- [x] hecho` |

---

## 5️⃣ Cómo ver el resultado antes de subirlo

En VS Code (dentro de Codespaces), abran el archivo `README.md` y presionen:

```
Ctrl + Shift + V
```

Esto abre una **vista previa** con el formato ya renderizado, para que revisen que todo se vea bien antes de subirlo a GitHub.

---

## 6️⃣ Subir el README al repositorio

```bash
git add README.md
git commit -m "Agrega README del proyecto"
git push origin main
```

Una vez subido, GitHub lo mostrará automáticamente en la página principal del repositorio.

---

## ✅ Checklist final antes de entregar

- [ ] El nombre y la descripción del proyecto están claros
- [ ] Están listadas las tecnologías usadas
- [ ] Los pasos de instalación funcionan si alguien más los sigue desde cero
- [ ] No hay claves ni contraseñas reales expuestas
- [ ] El README se ve bien en la vista previa de Markdown
- [ ] Todos los integrantes del equipo aparecen como autores
