# Guía: Login con Supabase + React (Vite) en Codespaces

Esta guía está pensada para el trabajo del día. Cada estudiante debe crear su proyecto dentro de la carpeta de la fecha correspondiente en el repositorio `workspace`.

---

## 0. Preparar la carpeta del día en el repositorio

Dentro de tu Codespace, en la raíz del repositorio `workspace`:

```bash
# Ejemplo: si hoy es 14 de julio de 2026
mkdir -p 2026-07-14/login-supabase
cd 2026-07-14/login-supabase
```

> Ajusta el nombre de la carpeta al formato de fecha que use tu curso (por ejemplo `14-07-2026` o `2026-07-14`).

---

## 1. Crear el proyecto con Vite

```bash
npm create vite@latest . -- --template react
```

- Si la carpeta no está vacía, Vite puede preguntar si deseas continuar; confirma con `y`.
- Instala las dependencias:

```bash
npm install
```

---

## 2. Instalar el cliente de Supabase

```bash
npm install @supabase/supabase-js
```

---

## 3. Configuración en Supabase (paso a paso)

### 3.1 Crear la cuenta y el proyecto

1. Ve a [supabase.com](https://supabase.com) y crea una cuenta (puedes usar GitHub para entrar más rápido).
2. Haz clic en **New project**.
3. Completa:
   - **Name**: por ejemplo `login-supabase-nombre`
   - **Database Password**: guárdala en un lugar seguro (no la necesitarás para este login, pero sí para otras integraciones)
   - **Region**: elige la más cercana (por ejemplo `South America (São Paulo)`)
4. Haz clic en **Create new project** y espera 1-2 minutos mientras se aprovisiona.

### 3.2 Obtener la URL y la llave (API Keys)

1. En el menú lateral, ve a **Project Settings** (ícono de engranaje) → **API**.
2. Copia y guarda:
   - **Project URL** → va en `VITE_SUPABASE_URL`
   - **anon public** key → va en `VITE_SUPABASE_ANON_KEY`
3. **Nunca copies la `service_role` key** para el frontend; esa es solo para uso en backend/servidor.

### 3.3 Configurar la URL del sitio y redirects (importante en Codespaces)

Codespaces genera una URL pública distinta cada vez que reconstruyes el entorno (algo como `https://xxxxx-5173.app.github.dev`). Supabase necesita conocerla para que la confirmación de email y los links de recuperación funcionen:

1. Ve a **Authentication → URL Configuration**.
2. En **Site URL**, coloca la URL pública de tu Codespace (la que aparece en la pestaña **Ports** al correr `npm run dev`), por ejemplo:
   ```
   https://xxxxx-5173.app.github.dev
   ```
3. En **Redirect URLs**, agrega esa misma URL (y `http://localhost:5173` como respaldo para pruebas locales fuera de Codespaces).
4. Guarda los cambios.

> Si el Codespace se reconstruye y cambia la URL, deberás actualizar este valor de nuevo.

### 3.4 Habilitar y configurar el proveedor de Email

1. Ve a **Authentication → Providers**.
2. Verifica que **Email** esté habilitado (viene activado por defecto).
3. Ve a **Authentication → Settings** (o **Sign In / Providers** según la versión del panel):
   - **Confirm email**: si lo dejas **activado**, el usuario debe confirmar su correo antes de poder iniciar sesión (más realista, pero requiere revisar el correo).
   - Para agilizar las pruebas en clase, puedes **desactivar** temporalmente "Confirm email"; así el registro deja la sesión activa de inmediato.

### 3.5 (Opcional) Revisar los usuarios registrados

1. Ve a **Authentication → Users**.
2. Ahí verás cada cuenta creada desde el formulario de registro, su email y estado de confirmación.
3. Desde aquí también puedes eliminar usuarios de prueba manualmente.

### 3.6 (Opcional) Tabla de perfiles con RLS

Si más adelante quieres guardar datos adicionales del usuario (nombre, avatar, etc.), crea una tabla `profiles` protegida con Row Level Security:

1. Ve a **Table Editor → New table**, nómbrala `profiles`, con columnas como `id` (uuid, referencia a `auth.users.id`), `full_name` (text).
2. Ve a **Authentication → Policies** (o la pestaña de RLS de la tabla) y **activa RLS**.
3. Crea una policy que permita a cada usuario leer y editar solo su propia fila (`auth.uid() = id`).

> Este paso es opcional para el login básico; solo es necesario si vas a guardar más datos del usuario.

---

## 4. Configurar variables de entorno

En la raíz del proyecto (`login-supabase`), crea un archivo `.env`:

```bash
touch .env
```

Contenido de `.env`:

```env
VITE_SUPABASE_URL=tu_project_url_aqui
VITE_SUPABASE_ANON_KEY=tu_anon_key_aqui
```

⚠️ **Importante:** agrega `.env` a `.gitignore` para no subir las llaves al repositorio.

```bash
echo ".env" >> .gitignore
```

---

## 5. Crear el cliente de Supabase

Crea el archivo `src/supabaseClient.js`:

```javascript
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

---

## 6. Crear el componente de Login

Crea `src/Login.jsx`:

```jsx
import { useState } from 'react'
import { supabase } from './supabaseClient'

export default function Login({ onLogin }) {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const [isSignUp, setIsSignUp] = useState(false)
  const [errorMsg, setErrorMsg] = useState('')
  const [loading, setLoading] = useState(false)

  const handleSubmit = async (e) => {
    e.preventDefault()
    setErrorMsg('')
    setLoading(true)

    const { data, error } = isSignUp
      ? await supabase.auth.signUp({ email, password })
      : await supabase.auth.signInWithPassword({ email, password })

    setLoading(false)

    if (error) {
      setErrorMsg(error.message)
      return
    }

    if (data.session) {
      onLogin(data.session)
    } else if (isSignUp) {
      setErrorMsg('Revisa tu correo para confirmar la cuenta.')
    }
  }

  return (
    <div style={{ maxWidth: 320, margin: '40px auto' }}>
      <h2>{isSignUp ? 'Crear cuenta' : 'Iniciar sesión'}</h2>
      <form onSubmit={handleSubmit}>
        <div>
          <label>Email</label>
          <input
            type="email"
            value={email}
            onChange={(e) => setEmail(e.target.value)}
            required
          />
        </div>
        <div>
          <label>Contraseña</label>
          <input
            type="password"
            value={password}
            onChange={(e) => setPassword(e.target.value)}
            required
            minLength={6}
          />
        </div>
        {errorMsg && <p style={{ color: 'red' }}>{errorMsg}</p>}
        <button type="submit" disabled={loading}>
          {loading ? 'Cargando...' : isSignUp ? 'Registrarme' : 'Entrar'}
        </button>
      </form>
      <button onClick={() => setIsSignUp(!isSignUp)}>
        {isSignUp ? '¿Ya tienes cuenta? Inicia sesión' : '¿No tienes cuenta? Regístrate'}
      </button>
    </div>
  )
}
```

---

## 7. Integrar el login en `App.jsx`

```jsx
import { useEffect, useState } from 'react'
import { supabase } from './supabaseClient'
import Login from './Login'

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
    <div style={{ maxWidth: 400, margin: '40px auto' }}>
      <h2>Bienvenido, {session.user.email}</h2>
      <button onClick={handleLogout}>Cerrar sesión</button>
    </div>
  )
}

export default App
```

---

## 8. Ejecutar el proyecto en Codespaces

```bash
npm run dev -- --host
```

- Codespaces mostrará una notificación para abrir el puerto (usualmente `5173`).
- Haz clic en **Abrir en el navegador** o ve a la pestaña **Ports**.

---

## 9. Probar el flujo

1. Regístrate con un correo y contraseña (mínimo 6 caracteres).
2. Si desactivaste la confirmación de email, deberías entrar directo.
3. Si la dejaste activa, revisa el correo, confirma, y luego inicia sesión.
4. Verifica que el botón **Cerrar sesión** funcione correctamente.

---

## 10. Subir el trabajo al repositorio `workspace`

Desde la raíz del repo (asegúrate de estar en la carpeta del día):

```bash
cd /workspaces/workspace   # o la ruta de tu repo
git add 2026-07-14/login-supabase
git commit -m "Login con Supabase - React + Vite"
git push origin main
```

> Reemplaza `main` por el nombre de tu rama si tu curso usa otra convención (por ejemplo `nombre-apellido` o una rama por estudiante).

---

## Checklist final

- [ ] Proyecto creado dentro de la carpeta de la fecha del día
- [ ] `.env` configurado y **no subido** al repositorio (`.gitignore`)
- [ ] Registro de usuario funcionando
- [ ] Login funcionando
- [ ] Logout funcionando
- [ ] Cambios subidos (`git push`) a la carpeta correcta del repositorio `workspace`

---

## Recursos

- Documentación oficial de Auth en Supabase: https://supabase.com/docs/guides/auth
- Documentación de Vite: https://vitejs.dev/guide/
