# Cómo publicar en arquetipos.kimeltun.app

Tiempo estimado: 20 minutos de trabajo + hasta 24 h de propagación DNS
(normalmente 10–30 minutos).

---

## Antes de empezar

Necesitas dos cosas:

1. **Acceso al panel DNS de `kimeltun.app`** — donde compraste el dominio
   (Cloudflare, Namecheap, GoDaddy, NIC Chile, Porkbun, etc.).
2. **Una cuenta de GitHub.** Si no tienes, créala en github.com — es gratis y el
   plan gratuito permite GitHub Pages en repositorios públicos.

> **Sobre el dominio `.app`:** los dominios `.app` están en la lista HSTS
> preload de Google, lo que significa que **el navegador exige HTTPS
> obligatoriamente**. No es un problema — GitHub Pages emite el certificado
> automáticamente — pero implica que el sitio no funcionará hasta que el
> certificado esté activo. Ten paciencia con el paso 5.
> Fuente: https://get.app/anatomy-of-app/

---

## Ruta A — GitHub Pages (recomendada)

Gratis, con historial de versiones, y otros profesores pueden ver y adaptar el
código. Es la opción que mejor calza con una app bajo licencia abierta.

### Paso 1 — Crear el repositorio

1. Entra a github.com y haz clic en **New repository**.
2. Nombre: `arquetipos`
3. Visibilidad: **Public** (obligatorio para Pages en el plan gratuito).
4. **No** marques "Add a README file" — ya lo tenemos.
5. **Create repository**.

### Paso 2 — Subir los archivos

La forma más simple, sin usar terminal:

1. En el repositorio recién creado, haz clic en **uploading an existing file**.
2. Arrastra **todo el contenido** de la carpeta `kimeltun-arquetipos`
   (los archivos sueltos y la carpeta `img` completa).
   Arrastra el *contenido*, no la carpeta contenedora.
3. Escribe un mensaje de commit: `Versión inicial de la app`
4. **Commit changes**.

Verifica que en la raíz del repositorio se vean `index.html`, `arquetipos.html`,
`CNAME`, `LICENSE` y la carpeta `img/`.

### Paso 3 — Activar GitHub Pages

1. En el repositorio: **Settings** → menú lateral **Pages**.
2. En "Build and deployment" → Source: **Deploy from a branch**.
3. Branch: **main**, carpeta: **/ (root)** → **Save**.
4. Espera 1–2 minutos. Aparecerá la URL provisoria
   `https://TU-USUARIO.github.io/arquetipos/`. Ábrela y comprueba que la app
   funciona antes de seguir.

### Paso 4 — Configurar el dominio personalizado

**Primero en GitHub, después en el DNS.** Este orden importa: si creas el
registro DNS antes de reclamar el dominio en GitHub, otra persona podría
publicar un sitio en tu subdominio (advertencia explícita en la documentación
de GitHub).

1. En **Settings → Pages → Custom domain**, escribe:

   ```
   arquetipos.kimeltun.app
   ```

   y haz clic en **Save**. (El archivo `CNAME` que incluí ya contiene ese valor,
   así que probablemente el campo aparezca lleno.)

2. Ahora ve al panel DNS de `kimeltun.app` y crea **un** registro:

   | Campo | Valor |
   |---|---|
   | Tipo | `CNAME` |
   | Nombre / Host | `arquetipos` |
   | Valor / Apunta a | `TU-USUARIO.github.io` |
   | TTL | Automático (o 3600) |

   Reemplaza `TU-USUARIO` por tu nombre de usuario de GitHub.

   **Importante:** el valor es `TU-USUARIO.github.io` — **sin** el nombre del
   repositorio y **sin** `https://`. Es el error más común.

   > Si usas **Cloudflare**: pon el registro en modo **DNS only** (nube gris),
   > no proxy (nube naranja). El proxy interfiere con la emisión del
   > certificado de GitHub. Puedes activarlo después, si quieres.

### Paso 5 — Activar HTTPS

1. Vuelve a **Settings → Pages**. GitHub verificará el DNS (puede tardar de
   minutos a algunas horas).
2. Cuando la casilla **Enforce HTTPS** deje de estar en gris, **márcala**.
3. Listo: https://arquetipos.kimeltun.app

Si tras 24 horas sigue sin funcionar, comprueba el DNS desde tu Terminal:

```bash
dig arquetipos.kimeltun.app +nostats +nocomments +nocmd
```

Debería mostrar una línea `CNAME TU-USUARIO.github.io.`

---

## Ruta B — Netlify (más rápida, sin Git)

Si prefieres no tocar GitHub:

1. Entra a https://app.netlify.com/drop
2. Arrastra la carpeta `kimeltun-arquetipos` completa. En segundos tendrás una
   URL provisoria funcionando.
3. **Site configuration → Domain management → Add a domain** →
   `arquetipos.kimeltun.app`
4. Netlify te mostrará el registro CNAME a crear (algo como
   `nombre-aleatorio.netlify.app`). Créalo en el panel DNS de `kimeltun.app`.
5. El certificado HTTPS (Let's Encrypt) se emite solo.

**Ventaja:** más rápido y permite actualizar arrastrando de nuevo la carpeta.
**Desventaja:** sin historial de versiones y sin código visible para colegas.
Si más adelante quieres migrar a GitHub, no pierdes nada.

---

## Actualizar la app más adelante

- **GitHub Pages:** edita el archivo en GitHub (lápiz ✏️) o sube la versión
  nueva. Se publica solo en 1–2 minutos.
- **Netlify:** arrastra la carpeta actualizada en **Deploys**.

---

## Pensando en las próximas apps

Si `kimeltun.app` va a ser tu paraguas de herramientas docentes, conviene
decidir ahora el patrón:

- Un repositorio por app, cada uno con su subdominio
  (`arquetipos.kimeltun.app`, `otra.kimeltun.app`). Es lo más ordenado y lo que
  estamos haciendo aquí.
- Una portada en `kimeltun.app` (la raíz) que liste todas las herramientas. Para
  eso necesitarías un repositorio llamado `TU-USUARIO.github.io` y registros `A`
  apuntando a las IP de GitHub Pages (185.199.108.153, .109.153, .110.153,
  .111.153).

Vale la pena montar la portada cuando tengas la segunda o tercera app.

---

## Qué revisar antes de usarla en clase

- [ ] Abrir la app en el móvil — los alumnos la usarán ahí.
- [ ] Probar la descarga del PDF en Chrome y en Safari.
- [ ] Probarla en la red de la universidad, no solo en casa.
- [ ] Confirmar que las 12 imágenes cargan.

> **Nota técnica:** la app compila JSX en el navegador con Babel standalone.
> Funciona bien, pero añade aproximadamente medio segundo de carga inicial y
> muestra un aviso en la consola del navegador. Es irrelevante para el uso en
> clase; si algún día quieres eliminarlo, hay que precompilar el JSX.

---

## Fuentes

- GitHub. *Managing a custom domain for your GitHub Pages site.*
  https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site/managing-a-custom-domain-for-your-github-pages-site
- GitHub. *Securing your GitHub Pages site with HTTPS.*
  https://docs.github.com/en/pages/getting-started-with-github-pages/securing-your-github-pages-site-with-https
- Netlify. *Custom domains.* https://docs.netlify.com/domains/domains-fundamentals/domains-glossary/
- Creative Commons. *CC BY-NC-SA 4.0.* https://creativecommons.org/licenses/by-nc-sa/4.0/deed.es
