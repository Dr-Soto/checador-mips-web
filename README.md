# Checador MIPS con GitHub Pages + Supabase

Esta version no usa Google Apps Script ni Google Sheets. La pagina es HTML estatico para GitHub Pages y los datos se guardan en Supabase.

Estado actual:

- Proyecto Supabase conectado: `https://vlsjbtwqkczarbeiikbk.supabase.co`
- Esquema SQL ejecutado correctamente.
- `index.html` ya tiene configurada la publishable key.

## Archivos

- `index.html`: pagina completa del checador.
- `supabase_schema.sql`: crea tablas, funciones seguras, QR dinamico, panel admin y validacion de horarios.

## 1. Crear Supabase

Este paso ya se hizo para el proyecto actual. Solo repitelo si creas otra base.

La contrasena admin inicial queda como:

```text
Starmedica.26
```

## 2. Pegar llaves publicas en `index.html`

Este paso ya se hizo para el proyecto actual. Solo repitelo si cambias de proyecto.

En Supabase ve a `Project Settings > API` y copia:

- Project URL
- anon public key

En `index.html`, cambia:

```js
const SUPABASE_URL = 'PEGA_AQUI_TU_SUPABASE_URL';
const SUPABASE_ANON_KEY = 'PEGA_AQUI_TU_SUPABASE_ANON_KEY';
```

por tus valores reales.

La `anon public key` puede vivir en el HTML. No pegues ninguna `service_role key`.

## 3. Publicar en GitHub Pages

1. Crea un repositorio en GitHub.
2. Sube el contenido de esta carpeta.
3. En el repositorio ve a `Settings > Pages`.
4. En `Build and deployment`, elige `Deploy from a branch`.
5. Elige rama `main` y carpeta `/root`.
6. Guarda.

GitHub te dara una URL tipo:

```text
https://tuusuario.github.io/tu-repositorio/
```

Esa sera la nueva URL del checador.

## Notas de seguridad

- No necesitas tener abierta tu sesion de Google o Microsoft en ningun ordenador.
- Los MIPS no necesitan iniciar sesion.
- Las tablas tienen RLS activado y no tienen lectura directa publica.
- El HTML llama funciones SQL de Supabase para registrar asistencia y consultar el panel.
- El panel admin usa la contrasena `Starmedica.26`.

## Cambiar contrasena admin

En Supabase SQL Editor ejecuta:

```sql
update app_settings
set value = encode(extensions.digest('NUEVA_CONTRASENA', 'sha256'), 'hex'),
    updated_at = now()
where key = 'admin_password_sha256';
```
