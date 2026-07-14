# Guía 2: CRUD de Productos con Supabase + React (Vite)

Esta guía es una **extensión** de la guía anterior (`guia-login-supabase-react-vite.md`). Se asume que ya tienes:

- El proyecto React + Vite creado (`login-supabase`)
- `@supabase/supabase-js` instalado
- El cliente `src/supabaseClient.js` configurado
- El login funcionando (registro / inicio de sesión / cierre de sesión)

Ahora vamos a agregar un **CRUD de productos** que solo esté disponible para usuarios autenticados.

---

## 0. Ubicación en el repositorio

Sigue trabajando dentro de la carpeta del día y del mismo proyecto de la guía anterior:

```bash
cd 2026-07-14/login-supabase
```

No es necesario crear un proyecto nuevo: el CRUD se agrega sobre el mismo proyecto donde ya está el login.

---

## 1. Crear la tabla `productos` en Supabase

### 1.1 Desde el Table Editor

1. En el panel de Supabase, ve a **Table Editor → New table**.
2. Nombre de la tabla: `productos`.
3. Desmarca "Enable Row Level Security" por ahora (lo activaremos manualmente en el paso 1.3, para entenderlo bien).
4. Agrega las columnas:

| Columna       | Tipo        | Configuración                                  |
|---------------|-------------|-------------------------------------------------|
| `id`          | `uuid`      | Primary key, default: `gen_random_uuid()`        |
| `user_id`     | `uuid`      | default: `auth.uid()` (o se asigna desde la app) |
| `nombre`      | `text`      | not null                                         |
| `descripcion` | `text`      | nullable                                         |
| `precio`      | `numeric`   | not null, default `0`                            |
| `created_at`  | `timestamp` | default: `now()`                                 |

5. Haz clic en **Save**.

### 1.2 (Alternativa) Desde el SQL Editor

Si prefieres crear la tabla con SQL, ve a **SQL Editor → New query** y ejecuta:

```sql
create table productos (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users(id) default auth.uid(),
  nombre text not null,
  descripcion text,
  precio numeric not null default 0,
  created_at timestamp with time zone default now()
);
```

### 1.3 Activar Row Level Security (RLS) y crear políticas

Con RLS activado, cada usuario solo puede ver y modificar **sus propios productos**.

1. Ve a **Authentication → Policies** (o a la pestaña **RLS** dentro de la tabla `productos` en el Table Editor).
2. Activa **Enable RLS** para la tabla `productos`.
3. Crea las siguientes 4 políticas (puedes usar el asistente visual o el SQL Editor):

```sql
-- Ver solo mis productos
create policy "Los usuarios pueden ver sus propios productos"
on productos for select
using (auth.uid() = user_id);

-- Crear productos propios
create policy "Los usuarios pueden crear sus propios productos"
on productos for insert
with check (auth.uid() = user_id);

-- Editar solo mis productos
create policy "Los usuarios pueden editar sus propios productos"
on productos for update
using (auth.uid() = user_id);

-- Eliminar solo mis productos
create policy "Los usuarios pueden eliminar sus propios productos"
on productos for delete
using (auth.uid() = user_id);
```

> Sin estas políticas, con RLS activado **nadie** podrá leer ni escribir en la tabla, aunque esté autenticado. Es un error común: activar RLS y olvidar crear las políticas.

### 1.4 Probar la tabla desde el panel

1. Ve a **Table Editor → productos → Insert row**.
2. Intenta insertar una fila manualmente sin `user_id`: debería fallar o quedar sin dueño según cómo configuraste el default.
3. Esto confirma que la tabla y las políticas están activas antes de conectarla con React.

---

## 2. Estructura de archivos en el proyecto React

Vamos a agregar estos archivos nuevos dentro de `src/`:

```
src/
├── supabaseClient.js     (ya existe)
├── Login.jsx             (ya existe)
├── App.jsx               (ya existe, lo vamos a modificar)
└── Productos.jsx         (nuevo)
```

---

## 3. Crear el componente `Productos.jsx`

Este componente incluye las 4 operaciones del CRUD: **crear, leer, actualizar y eliminar**.

```jsx
import { useEffect, useState } from 'react'
import { supabase } from './supabaseClient'

export default function Productos({ session }) {
  const [productos, setProductos] = useState([])
  const [loading, setLoading] = useState(true)
  const [errorMsg, setErrorMsg] = useState('')

  // Formulario (creación / edición)
  const [nombre, setNombre] = useState('')
  const [descripcion, setDescripcion] = useState('')
  const [precio, setPrecio] = useState('')
  const [editingId, setEditingId] = useState(null) // null = creando, id = editando

  const cargarProductos = async () => {
    setLoading(true)
    const { data, error } = await supabase
      .from('productos')
      .select('*')
      .order('created_at', { ascending: false })

    if (error) {
      setErrorMsg(error.message)
    } else {
      setProductos(data)
    }
    setLoading(false)
  }

  useEffect(() => {
    cargarProductos()
  }, [])

  const limpiarFormulario = () => {
    setNombre('')
    setDescripcion('')
    setPrecio('')
    setEditingId(null)
  }

  // CREATE y UPDATE usan el mismo formulario
  const handleSubmit = async (e) => {
    e.preventDefault()
    setErrorMsg('')

    if (editingId) {
      // UPDATE
      const { error } = await supabase
        .from('productos')
        .update({ nombre, descripcion, precio: Number(precio) })
        .eq('id', editingId)

      if (error) {
        setErrorMsg(error.message)
        return
      }
    } else {
      // CREATE
      const { error } = await supabase.from('productos').insert({
        nombre,
        descripcion,
        precio: Number(precio),
        user_id: session.user.id,
      })

      if (error) {
        setErrorMsg(error.message)
        return
      }
    }

    limpiarFormulario()
    cargarProductos()
  }

  const handleEditar = (producto) => {
    setEditingId(producto.id)
    setNombre(producto.nombre)
    setDescripcion(producto.descripcion || '')
    setPrecio(producto.precio)
  }

  const handleEliminar = async (id) => {
    const confirmar = window.confirm('¿Seguro que deseas eliminar este producto?')
    if (!confirmar) return

    const { error } = await supabase.from('productos').delete().eq('id', id)

    if (error) {
      setErrorMsg(error.message)
      return
    }
    cargarProductos()
  }

  return (
    <div style={{ maxWidth: 600, margin: '20px auto' }}>
      <h2>Mis Productos</h2>

      <form onSubmit={handleSubmit} style={{ marginBottom: 24 }}>
        <div>
          <label>Nombre</label>
          <input
            type="text"
            value={nombre}
            onChange={(e) => setNombre(e.target.value)}
            required
          />
        </div>
        <div>
          <label>Descripción</label>
          <input
            type="text"
            value={descripcion}
            onChange={(e) => setDescripcion(e.target.value)}
          />
        </div>
        <div>
          <label>Precio</label>
          <input
            type="number"
            step="0.01"
            value={precio}
            onChange={(e) => setPrecio(e.target.value)}
            required
          />
        </div>

        {errorMsg && <p style={{ color: 'red' }}>{errorMsg}</p>}

        <button type="submit">
          {editingId ? 'Guardar cambios' : 'Crear producto'}
        </button>
        {editingId && (
          <button type="button" onClick={limpiarFormulario}>
            Cancelar edición
          </button>
        )}
      </form>

      {loading ? (
        <p>Cargando productos...</p>
      ) : productos.length === 0 ? (
        <p>Aún no tienes productos registrados.</p>
      ) : (
        <table width="100%" cellPadding="6" style={{ borderCollapse: 'collapse' }}>
          <thead>
            <tr style={{ borderBottom: '1px solid #ccc' }}>
              <th align="left">Nombre</th>
              <th align="left">Descripción</th>
              <th align="left">Precio</th>
              <th align="left">Acciones</th>
            </tr>
          </thead>
          <tbody>
            {productos.map((p) => (
              <tr key={p.id} style={{ borderBottom: '1px solid #eee' }}>
                <td>{p.nombre}</td>
                <td>{p.descripcion}</td>
                <td>${Number(p.precio).toFixed(2)}</td>
                <td>
                  <button onClick={() => handleEditar(p)}>Editar</button>{' '}
                  <button onClick={() => handleEliminar(p.id)}>Eliminar</button>
                </td>
              </tr>
            ))}
          </tbody>
        </table>
      )}
    </div>
  )
}
```

---

## 4. Conectar `Productos.jsx` con `App.jsx`

Modifica tu `App.jsx` (el que ya tiene el login) para mostrar el CRUD una vez que el usuario esté autenticado:

```jsx
import { useEffect, useState } from 'react'
import { supabase } from './supabaseClient'
import Login from './Login'
import Productos from './Productos'

function App() {
  const [session, setSession] = useState(null)

  useEffect(() => {
    supabase.auth.getSession().then(({ data: { session } }) => {
      setSession(session)
    })

    const { data: listener } = supabase.auth.onAuthStateChange(
      (_event, session) => {
        setSession(session)
      }
    )

    return () => listener.subscription.unsubscribe()
  }, [])

  const handleLogout = async () => {
    await supabase.auth.signOut()
    setSession(null)
  }

  if (!session) {
    return <Login onLogin={setSession} />
  }

  return (
    <div>
      <div style={{ display: 'flex', justifyContent: 'space-between', maxWidth: 600, margin: '0 auto', paddingTop: 20 }}>
        <h3>Bienvenido, {session.user.email}</h3>
        <button onClick={handleLogout}>Cerrar sesión</button>
      </div>
      <Productos session={session} />
    </div>
  )
}

export default App
```

---

## 5. Ejecutar y probar

```bash
npm run dev -- --host
```

1. Inicia sesión con un usuario (creado en la guía anterior).
2. **Crear**: llena el formulario y haz clic en **Crear producto**.
3. **Leer**: los productos deben aparecer automáticamente en la tabla al cargar.
4. **Actualizar**: haz clic en **Editar**, modifica los campos y haz clic en **Guardar cambios**.
5. **Eliminar**: haz clic en **Eliminar** y confirma.
6. Prueba con **dos usuarios distintos**: cada uno debe ver **solo sus propios productos** (gracias a las políticas RLS del paso 1.3).

---

## 6. Verificación desde Supabase

1. Ve a **Table Editor → productos** en el panel de Supabase.
2. Confirma que cada fila tenga el `user_id` correcto según quién la creó desde la app.
3. Si un producto no aparece en la app pero sí en la tabla, revisa que su `user_id` coincida con el usuario que inició sesión (es la causa más común de "no veo mis datos").

---

## 7. Subir el trabajo al repositorio `workspace`

```bash
cd /workspaces/workspace   # o la ruta de tu repo
git add 2026-07-14/login-supabase
git commit -m "CRUD de productos con Supabase - React + Vite"
git push origin main
```

---

## Checklist final

- [ ] Tabla `productos` creada en Supabase con las columnas correctas
- [ ] RLS activado y las 4 políticas (select/insert/update/delete) creadas
- [ ] Componente `Productos.jsx` creado y conectado en `App.jsx`
- [ ] Crear producto funcionando
- [ ] Listar productos funcionando
- [ ] Editar producto funcionando
- [ ] Eliminar producto funcionando
- [ ] Verificado que cada usuario solo ve sus propios productos
- [ ] Cambios subidos (`git push`) a la carpeta correcta del repositorio `workspace`

---

## Recursos

- Documentación de la API de datos de Supabase: https://supabase.com/docs/guides/database/overview
- Row Level Security en Supabase: https://supabase.com/docs/guides/auth/row-level-security
