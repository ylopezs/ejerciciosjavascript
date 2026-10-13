# Lineamientos para la exposición del proyecto

Proyecto desarrollado con **React** (frontend), **Supabase** (backend), creado en **GitHub Codespaces** y publicado en **GitHub Pages**.

Estos lineamientos asumen que ya tienen sus diapositivas listas con la marca de agua de la universidad. Se enfocan en **qué contar, en qué orden y cómo demostrarlo**.

---

## 1. Formato sugerido

- **Duración:** 10 a 12 minutos de exposición + 3 a 5 minutos de preguntas (ajustar según la rúbrica del curso).
- **Participación:** todos los integrantes deben hablar. Definan quién presenta cada parte antes del día de la exposición.
- **Proporción recomendada:** 40 % explicación (diapositivas) y 60 % demostración y arquitectura. Es un proyecto técnico: el producto funcionando es lo más importante.

---

## 2. Estructura de la presentación

| # | Sección | Qué debe quedar claro | Tiempo aprox. |
|---|---------|----------------------|---------------|
| 1 | **Portada y equipo** | Nombre del proyecto, integrantes, curso, docente | 30 s |
| 2 | **Problema y objetivo** | ¿Qué necesidad resuelve? ¿Para quién? | 1 min |
| 3 | **Solución propuesta** | Funcionalidades principales (máximo 4 o 5) | 1 min |
| 4 | **Arquitectura** | Diagrama: React (front) ↔ Supabase (back) ↔ GitHub Pages (despliegue) | 2 min |
| 5 | **Tecnologías y herramientas** | React, Supabase, GitHub, Codespaces, GitHub Pages: qué hace cada una y por qué se eligió | 1 min |
| 6 | **Demo en vivo** | Recorrido por el sitio publicado | 3 a 4 min |
| 7 | **Retos y aprendizajes** | Problemas reales y cómo los resolvieron | 1 a 2 min |
| 8 | **Mejoras futuras y cierre** | Qué harían con más tiempo; conclusiones | 1 min |

---

## 3. Qué explicar sobre la arquitectura

Este es el punto donde más se evalúa el entendimiento técnico. Deben poder responder con claridad:

### Frontend (React)

- Estructura de componentes y manejo de estado.
- Cómo se comunica con Supabase (cliente `supabase-js`).
- Enrutamiento, y cómo lo resolvieron en GitHub Pages, que no maneja rutas del lado del servidor (por ejemplo con `HashRouter` o configurando `basename`).

### Backend (Supabase)

- Servicios utilizados: base de datos PostgreSQL, autenticación, almacenamiento (Storage), funciones, etc.
- Modelo de datos: tablas principales y sus relaciones (un diagrama entidad-relación en una diapositiva suma mucho).
- **Seguridad:** políticas **RLS (Row Level Security)** y por qué son necesarias, ya que la clave `anon` es pública en el frontend.

### Desarrollo y despliegue

- **GitHub Codespaces:** entorno de desarrollo en la nube; ventajas (mismo entorno para todo el equipo, sin instalaciones locales).
- **GitHub Pages:** cómo se publicó (rama `gh-pages`, GitHub Actions o paquete `gh-pages`) y que es hosting estático.
- **Variables de entorno:** manejo de la URL y la clave `anon` de Supabase, y por qué **nunca** se sube la `service_role key` al repositorio.

---

## 4. Cómo hacer la demo

1. **Usen el sitio publicado en GitHub Pages**, no `localhost`. Eso demuestra que el despliegue funciona.
2. **Preparen un guion** de 3 a 4 flujos clave (por ejemplo: registro/login → crear un registro → consultar → editar o eliminar).
3. **Tengan datos de prueba cargados** de antemano para no perder tiempo.
4. **Muestren Supabase en paralelo:** abran el *Table Editor* y evidencien que lo hecho en la interfaz se refleja en la base de datos. Es una forma muy efectiva de explicar la conexión front-back.
5. **Muestren brevemente el repositorio:** estructura de carpetas, historial de commits y la participación de cada integrante.
6. **Plan B:** tengan capturas de pantalla o un video corto de respaldo por si falla el internet o el servicio.

---

## 5. Lista de verificación previa (un día antes)

- [ ] El sitio de GitHub Pages carga correctamente y funciona de punta a punta.
- [ ] El proyecto de Supabase está **activo** (los proyectos gratuitos se pausan por inactividad; reactívenlo con anticipación).
- [ ] Hay un usuario de prueba y datos de ejemplo.
- [ ] El repositorio es accesible (público o con permisos para el docente) y el `README` explica el proyecto, la instalación y el enlace al sitio.
- [ ] No hay claves secretas expuestas en el código.
- [ ] Las diapositivas con la marca de agua de la universidad están revisadas: sin errores de ortografía y con texto legible.
- [ ] Se hizo al menos un ensayo completo cronometrado.

---

## 6. Buenas prácticas en las diapositivas

- **Poco texto, más imágenes:** diagramas, capturas y esquemas en vez de párrafos.
- **Un mensaje por diapositiva.**
- Letra legible desde el fondo del salón (mínimo 24 pt para el cuerpo).
- Incluyan el **enlace y un código QR** del sitio y del repositorio en la diapositiva final.
- No lean las diapositivas: úsenlas como apoyo.

---

## 7. Preguntas que pueden hacerles (prepárense)

- ¿Por qué eligieron Supabase en lugar de otro backend?
- ¿Cómo protegen los datos si la clave de Supabase está en el frontend?
- ¿Qué pasa si dos usuarios editan el mismo dato al mismo tiempo?
- ¿Cómo manejan la autenticación y los roles de usuario?
- ¿Qué limitaciones tiene GitHub Pages para este tipo de proyecto?
- ¿Cómo se organizó el trabajo en equipo en GitHub (ramas, commits, pull requests)?
- ¿Qué escalarían o cambiarían si el proyecto tuviera miles de usuarios?

---

## 8. Criterios que probablemente se evaluarán

| Criterio | Qué se busca |
|----------|--------------|
| Claridad del problema y la solución | Se entiende qué hace y por qué importa |
| Dominio técnico | Explican la arquitectura y pueden defender decisiones |
| Funcionamiento | La demo corre sin errores en el sitio publicado |
| Trabajo colaborativo | Evidencia en GitHub, participación equilibrada |
| Comunicación | Manejo del tiempo, lenguaje técnico claro, seguridad al hablar |
| Presentación | Diapositivas limpias, con la identidad de la universidad |
