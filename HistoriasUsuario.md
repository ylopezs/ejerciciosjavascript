# Taller: De lo que ya tienen (login, Supabase, mapa del sitio) a Historias de Usuario

**Duración total:** ~1h30min (ajustable)
**Modalidad:** Grupos de 3-4 estudiantes, cada grupo trabaja sobre su propio proyecto
**Materiales que cada grupo debe traer:**
- Su mapa del sitio (sitemap) ya hecho
- Acceso a su proyecto Supabase (tablas, políticas RLS, auth)
- Su flujo de login funcionando (o su diseño/mockup si aún no está desplegado)

**Objetivo del taller:** que cada grupo termine con un backlog inicial de historias de usuario, trazables a pantallas concretas del sitio y a tablas concretas de Supabase, listo para empezar a desarrollar.

---

## 0. Marco teórico rápido (15 min)

Explicar solo lo esencial antes de la práctica:

- **Historia de usuario (formato estándar):**
  `Como [rol/actor], quiero [acción/funcionalidad], para [beneficio/objetivo]`
- **Criterios de aceptación** (qué hace que la historia se considere "hecha"), en formato Gherkin:
  `Dado [contexto], cuando [acción], entonces [resultado esperado]`
- **INVEST**: una buena historia es Independiente, Negociable, Valiosa, Estimable, pequeña (Small) y Testeable.
- Idea clave del taller: **el mapa del sitio te da las pantallas, Supabase te da los datos, y el cruce entre ambos te da las historias de usuario.**

---

## Actividad 1 — Inventario de lo que ya existe (20 min)

Cada grupo llena esta tabla usando lo que YA tienen (no lo que falta):

### 1A. Roles/actores identificados en el login
¿Qué tipos de usuario maneja su sistema de autenticación? (ej: usuario normal, administrador, invitado, moderador...)

| Rol | ¿Cómo se identifica en Supabase? (tabla/campo/rol de RLS) |
|---|---|
| | |

### 1B. Tablas de Supabase
Lista rápida de las tablas que ya tienen creadas (pueden sacarlo del Table Editor de Supabase).

| Tabla | Campos clave | ¿Qué representa en el negocio? |
|---|---|---|
| | | |

### 1C. Pantallas del mapa del sitio
Lista cada nodo/pantalla de su sitemap.

| Pantalla | ¿A qué rol(es) le aparece? | ¿Con qué tabla(s) de Supabase se conecta? |
|---|---|---|
| | | |

> 🎯 Este inventario es el insumo directo para las siguientes actividades. Nadie avanza sin llenarlo.

---

## Actividad 2 — De pantallas a historias de usuario (30 min)

Para **cada pantalla** del sitemap (columna 1C), el grupo redacta al menos **una historia de usuario** siguiendo el formato:

```
Como [rol de la tabla 1A]
quiero [acción que se puede hacer en esa pantalla]
para [beneficio]
```

**Regla del taller:** ninguna historia puede mencionar una pantalla que no esté en el sitemap, ni un dato que no exista en Supabase. Si la historia necesita un dato que no tienen, deben anotarlo aparte (ver "Deuda de datos" al final).

**Ejemplo guía (mostrar antes de que empiecen):**

> Pantalla: `/perfil`
> Tabla relacionada: `profiles`
>
> *Como usuario autenticado, quiero editar mi nombre y foto de perfil, para que mi información esté actualizada cuando otros usuarios la vean.*

---

## Actividad 3 — De tablas Supabase a historias CRUD (30 min)

Ahora al revés: para **cada tabla** importante (columna 1B), pensar en las 4 operaciones básicas y ver cuáles necesitan historia propia:

| Operación | ¿Aplica? | Historia de usuario | Pantalla donde ocurre |
|---|---|---|---|
| Crear (Create) | | | |
| Leer (Read) | | | |
| Actualizar (Update) | | | |
| Eliminar (Delete) | | | |

Esto casi siempre revela historias que no salieron en la Actividad 2 (ej: "eliminar cuenta", "recuperar contraseña", historias de administrador que no tienen pantalla visible todavía).

**Pregunta guía para el grupo:** ¿esta operación la puede hacer cualquier rol, o solo algunos? Esto luego se conecta directo con las políticas RLS de Supabase.

---

## Actividad 4 — Criterios de aceptación (30 min)

Tomar **5 historias** (las que el grupo considere más importantes) y escribirles criterios de aceptación en Gherkin.

**Plantilla:**

```
Historia: Como [rol], quiero [acción], para [beneficio]

Criterio 1:
  Dado que [estado inicial]
  Cuando [el usuario hace X]
  Entonces [pasa Y]

Criterio 2 (caso de error):
  Dado que [estado inicial]
  Cuando [el usuario hace algo inválido]
  Entonces [se muestra este mensaje / no se guarda el dato]
```

**Ejemplo:**

```
Historia: Como usuario autenticado, quiero editar mi nombre y foto de perfil,
para que mi información esté actualizada.

Criterio 1:
  Dado que estoy en /perfil y he iniciado sesión
  Cuando cambio mi nombre y presiono "Guardar"
  Entonces el campo "name" en la tabla profiles se actualiza
  y veo un mensaje de confirmación

Criterio 2:
  Dado que estoy en /perfil
  Cuando dejo el nombre vacío y presiono "Guardar"
  Entonces no se guarda el cambio y veo un mensaje de error
```

> Este paso es clave porque conecta directo con los tests y con las políticas RLS de Supabase (¿quién puede hacer UPDATE en `profiles`? ¿solo el dueño de la fila?).

---

## Actividad 5 — Priorización y backlog (20 min)

Con todas las historias generadas (Actividades 2 y 3), cada grupo las clasifica usando **MoSCoW**:

| Historia | Must (imprescindible) | Should | Could | Won't (por ahora) |
|---|---|---|---|---|
| | | | | |

Luego arman un backlog ordenado en 3 columnas (esto puede quedar como tablero físico, en un Excel, o en Trello/Jira/GitHub Projects):

**Sprint 1 (login + navegación básica)** → **Sprint 2 (funcionalidad principal)** → **Sprint 3 (extras / roles avanzados)**

---

## Actividad 6 — Revisión cruzada entre grupos (15 min)

Cada grupo intercambia su backlog con otro grupo. El grupo revisor responde por escrito:

1. ¿Cada historia tiene un rol claro (no genérico como "el usuario")?
2. ¿Cada historia se puede rastrear a una pantalla real del sitemap?
3. ¿Cada historia se puede rastrear a una tabla/campo real de Supabase?
4. ¿Hay alguna historia que en realidad son dos historias mezcladas?
5. ¿Falta alguna historia obvia (login, logout, recuperar contraseña, error 404, etc.)?

---

## Cierre — "Deuda de datos" (5 min)

Cada grupo anota en una lista aparte: **¿qué historias necesitan una tabla o campo de Supabase que todavía no existe?** Esto se convierte en la primera tarea técnica antes de programar esas historias específicas.

```
Historia: ...
Falta en Supabase: tabla / campo / relación
```

---

## Entregable final del taller

Cada grupo entrega:
1. Tabla de inventario (Actividad 1)
2. Backlog de historias de usuario con formato estándar
3. 5 historias con criterios de aceptación completos
4. Backlog priorizado (MoSCoW) organizado en sprints
5. Lista de "deuda de datos"

Con esto ya tienen literalmente el punto de partida para empezar a codear: saben qué pantalla construir primero, qué tabla de Supabase usar, y cómo van a saber si "ya funciona".
