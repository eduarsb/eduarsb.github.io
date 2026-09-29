# CONTEXT.md — eduarsb.github.io

Portafolio profesional de Eduardo Sánchez. HTML estático servido por GitHub Pages
desde `main` (no hay build: lo que está en el repo es lo que se publica).

## Posicionamiento — el contenido no es libre

El sitio sostiene una tesis deliberada: **Ingeniero de Software Senior —
Infraestructura Cloud y Seguridad**, no "frontend developer". Ante cualquier duda
de redacción, la pregunta es *¿esto refuerza el perfil cloud/seguridad ante un
reclutador?*. Consecuencias concretas al editar:

- **El orden de las habilidades ES el mensaje**: Infraestructura y Cloud primero,
  Frontend al final. No reordenar por "relevancia técnica".
- **Ninguna métrica nueva sin que Eduardo la confirme.** Las que están (los ~200
  usuarios internos, las ~100 denuncias diarias, las 196 camas) están validadas y
  son defendibles en entrevista. No inflarlas ni agregar otras.
- **Nada de LinkedIn** — decisión tomada, no es un olvido. Tampoco enlaces a
  escuadra.dev (ex DawnForge, renombrada el 2026-09-27): la única dirección
  permitida es Escuadra → este sitio.
- Identidad pública única: **Eduardo Sánchez**. No debe quedar ninguna referencia
  a "hector" en archivos, enlaces ni nombres de archivo.

## Restricción técnica: la CSP prohíbe inline

Ambas páginas declaran una Content-Security-Policy por `<meta http-equiv>` con
`script-src` limitado a self + los dos CDN, y **sin `'unsafe-inline'`**. Por eso:

- **No agregar `<script>` inline** — el código de animaciones vive en
  `js/scroll-animations.js` precisamente por esto.
- **No agregar atributos `style="…"`** — los estilos que antes estaban inline
  están en clases (`.hero-section`, `.section-header-bg`) en `css/dark-theme.css`.

Un inline nuevo no rompe el build (no hay build): la página se despliega y el
navegador lo bloquea en silencio. Se detecta abriendo la consola, no con `git`.

## El CV se genera, no se edita a mano

- Fuente: `cv/cv-es.html`. Salida: `img/eduardo_sanchez_cv_es.pdf`.
- Regenerar (el comando también está en un comentario dentro del HTML):
  ```bash
  cd cv && google-chrome --headless --disable-gpu --no-pdf-header-footer \
    --print-to-pdf=../img/eduardo_sanchez_cv_es.pdf cv-es.html
  ```
- Después de regenerar, **abrir el PDF y revisar los cortes de página**: los
  títulos de sección usan `break-after: avoid-page` para no quedar huérfanos al
  pie, y eso hay que verificarlo mirando, no asumiendo.

## Estructura

| Ruta | Qué es |
| --- | --- |
| `index.html` · `index_en.html` | Las dos versiones. **Todo cambio de contenido va en las dos.** |
| `css/dark-theme.css` | El tema real. `css/style.css` es la grilla de Bootstrap del template original |
| `js/main.js` | JS del template (jQuery). Contiene código muerto de secciones que ya no existen (portfolio, testimonios, video) |
| `js/scroll-animations.js` | Animaciones de entrada por IntersectionObserver |
| `cv/` | Fuente del CV |
| `lib/` | Librerías del template original |

## Verificación

No hay tests. Antes de dar por buena una edición: servir en local
(`python3 -m http.server`), abrir la página, revisar la consola por errores de
CSP, y comprobar que los enlaces locales responden 200. Sin la extensión de
Chrome, `google-chrome --headless --screenshot --virtual-time-budget=6000`
alcanza para ver el render.
