# Manifiesto por el Agua, la Vida y el Futuro de los Territorios
## Landing page de adhesiones ciudadanas — Repositorio local

Este archivo le da a Claude Code el contexto completo del proyecto para que pueda
editar, corregir y ampliar la landing page sin necesidad de explicaciones adicionales
en cada sesión.

---

## Descripción del proyecto

Landing page estática de recolección de firmas ciudadanas en apoyo a los candidatos
Iván Cepeda y Aida Quilcué, impulsada por egresados de Administración Ambiental de
universidades colombianas. El contenido es un manifiesto de 6 puntos sobre política
ambiental, seguido de un formulario de adhesión con lista de firmas en tiempo real.

**URL de producción:** GitHub Pages (rama `main` o `gh-pages` según configuración)
**Repositorio:** github.com/[usuario]/manifiesto-cepeda ← actualizar con URL real

---

## Stack técnico

- **HTML / CSS / JS vanilla** — sin frameworks, sin bundlers, sin npm
- **Hosting:** GitHub Pages (archivos estáticos, deploy automático desde rama main)
- **Backend de firmas:** Supabase (PostgreSQL + API REST)
- **Tipografías:** Playfair Display + Inter — Google Fonts, cargadas en `<head>`
- **Sin node_modules.** No ejecutar `npm install`. No agregar dependencias externas.

---

## Estructura de archivos

```
/
├── index.html          ← página principal, contiene todo el HTML
├── css/
│   └── styles.css      ← todos los estilos, usa variables CSS (ver paleta abajo)
├── js/
│   └── main.js         ← lógica de firmas: fetch Supabase, render lista, formulario
├── images/
│   ├── IMAGE1__1_.jpeg   → Hero (río verde)
│   ├── IMAGE1__4_.jpeg   → Sección 4 col izq (valle montañoso)
│   ├── IMAGE1__5_.jpeg   → Sección 6 col der (palafito costero)
│   ├── IMAGE1__6_.jpeg   → Sección 5 col izq (canoa con plátanos)
│   ├── IMAGE1__7_.jpeg   → Sección 3 col der (cascada en selva)
│   ├── IMAGE1__13_.jpeg  → Sección 9 full-bleed (ciervo en selva)
│   ├── IMAGE1__17_.jpeg  → Sección 7 col izq (cocos cosechados)
│   ├── IMAGE1__19_.jpeg  → Sección 12 full-bleed (ciudad costera panorámica)
│   └── IMAGE1__20_.jpeg  → Sección 8 col der (río en valle desde altura)
└── CLAUDE.md           ← este archivo
```

---

## Arquitectura HTML — 13 secciones

Cada sección tiene un `id` que es su referencia canónica. No cambiar los IDs
sin actualizar también todos los `href="#..."` que los apuntan.

| id | Sección | Fondo | Layout |
|---|---|---|---|
| `#hero` | Hero | `IMAGE1__1_.jpeg` full-bleed + overlay | Texto centrado sobre imagen |
| `#intro` | ¿Por qué participamos? | blanco | Texto con borde izq verde 4px |
| `#punto-1` | Punto 1 — Agua y ecosistemas | beige | `.two-col`: `.col-text` izq / `.col-image` der |
| `#punto-2` | Punto 2 — Economía siglo XXI | blanco | `.two-col`: `.col-image` izq / `.col-text` der |
| `#punto-3` | Punto 3 — Institucionalidad | beige | `.two-col`: `.col-image` izq / `.col-text` der |
| `#punto-4` | Punto 4 — Ciencia y participación | blanco | `.two-col`: `.col-text` izq / `.col-image` der |
| `#punto-5` | Punto 5 — Naturaleza y desarrollo | beige | `.two-col`: `.col-image` izq / `.col-text` der |
| `#punto-6` | Punto 6 — Liderazgo internacional | blanco | `.two-col`: `.col-text` izq / `.col-image` der |
| `#interlude` | Interlude biodiversidad | `IMAGE1__13_.jpeg` full-bleed + overlay | Cita centrada sobre imagen |
| `#formulario` | Formulario de firmas | `--color-green-dark` | Tabs + campos + submit |
| `#firmas` | Lista de firmas en vivo | blanco | Grid 2 col de `.signature-card` |
| `#cierre` | Cierre | `IMAGE1__19_.jpeg` full-bleed + overlay | Cita + 2 botones CTA |
| `#footer` | Footer | `--color-green-dark` | Logo + social + disclaimer |

---

## Variables CSS — paleta completa

Definidas en `:root` en `css/styles.css`. Usar siempre estas variables,
nunca valores hexadecimales hardcodeados en propiedades individuales.

```css
:root {
  --color-green-dark:   #1B4332;  /* headings oscuros, footer, formulario bg */
  --color-green-mid:    #2D6A4F;  /* accents, botones secundarios, bordes */
  --color-beige:        #F5F0E8;  /* fondo secciones impares del manifiesto */
  --color-white:        #FFFFFF;  /* fondo secciones pares, superficies card */
  --color-amber:        #B7791F;  /* CTA primario, highlights de datos */
  --color-amber-light:  #FEF3C7;  /* fondo callout boxes */
  --color-text:         #1A1A1A;  /* body text principal */
  --color-text-muted:   #6B7280;  /* metadatos, timestamps, notas secundarias */
  --color-border:       #E5E7EB;  /* bordes de cards y separadores */
}
```

---

## Tipografía

```css
/* Headings H1–H3 */
font-family: 'Playfair Display', serif;
font-weight: 700;

/* Body, labels, UI */
font-family: 'Inter', sans-serif;
font-weight: 400;  /* body */
font-weight: 600;  /* subtítulos, labels, small-caps */

/* Números decorativos de sección (.section-number) */
font-family: 'Playfair Display', serif;
font-size: 80px;
color: var(--color-green-mid);
opacity: 0.15;
position: absolute;
z-index: 0;        /* siempre detrás del heading */
```

---

## Clases CSS principales

```
Layouts
.container          max-width 1200px, centrado con auto margins
.container--narrow  max-width 780px, para secciones de texto puro
.two-col            grid 2 columnas 50/50, gap 60px, colapsa a 1 col en mobile
.col-text           columna de texto dentro de .two-col
.col-image          columna de imagen dentro de .two-col

Fondos de sección
.bg-beige           background: var(--color-beige)
.bg-dark            background: var(--color-green-dark), text blanco

Componentes de contenido
.section-number     número decorativo grande semitransparente (01–06)
.callout-box        caja destacada fondo amber-light, borde izq amber 4px
.stat-box           caja de dato estadístico, número grande + descripción
.two-stats          wrapper flex para poner 2 .stat-box lado a lado

Formulario
.form-tabs          contenedor de los tabs Profesional / Ciudadano
.tab-btn            botón de tab individual
.tab-btn.active     tab activo (bg verde, texto blanco)
.form-panel         contenedor de campos, se muestra/oculta con JS
.btn-primary        botón CTA principal (amber filled)
.btn-secondary      botón CTA secundario (outlined)
.field-error        mensaje de error inline bajo un campo inválido

Lista de firmas
.signatures-grid    grid 2 columnas de cards
.signature-card     card individual de firma
.signature-avatar   círculo de iniciales generado por JS
.tag-profesional    pill verde claro para egresados
.tag-ciudadano      pill teal claro para ciudadanos

Imágenes full-bleed
.hero-bg            imagen de fondo del hero con overlay CSS
.section-fullbleed  sección con imagen full-bleed y overlay oscuro
```

---

## JavaScript — main.js

Funciones principales que existen en `main.js`. No renombrarlas sin actualizar
todos los event listeners que las referencian.

```javascript
// Inicialización
initFormTabs()          // activa el sistema de tabs del formulario
initSmoothScroll()      // scroll suave para todos los href="#..."
initSignatureForm()     // valida y envía el formulario a Supabase
initSignaturesList()    // carga firmas al iniciar y configura el polling

// Supabase
fetchSignatures()       // GET a Supabase, devuelve array ordenado por created_at desc
submitSignature(data)   // POST a Supabase con los datos del formulario
renderSignatures(list)  // actualiza el DOM de #firmas con el array recibido
updateCounter(n)        // actualiza todos los elementos .counter-badge con el total

// Utilidades
generateAvatar(name)    // genera el HTML del círculo de iniciales
getRelativeTime(date)   // convierte timestamp ISO a "hace X minutos/horas/días"
showSuccess()           // muestra el mensaje de éxito post-submit
showFieldError(field, msg) // agrega .field-error bajo un input específico
clearErrors()           // limpia todos los .field-error del formulario
```

---

## Integración Supabase

Las credenciales se declaran al inicio de `main.js` como constantes:

```javascript
const SUPABASE_URL      = 'https://[project-ref].supabase.co';
const SUPABASE_ANON_KEY = 'eyJ...';  // anon/public key, no la service key
```

**Tabla:** `firmas`

| Columna | Tipo | Nullable | Notas |
|---|---|---|---|
| `id` | uuid | no | generado por Supabase |
| `nombre` | text | no | nombre público del firmante |
| `tipo` | text | no | `"profesional"` o `"ciudadano"` |
| `universidad` | text | sí | solo para tipo profesional |
| `anio_egreso` | integer | sí | solo para tipo profesional |
| `ciudad` | text | no | |
| `organizacion` | text | sí | solo para tipo ciudadano |
| `created_at` | timestamptz | no | generado automáticamente |

**El correo electrónico no se guarda.** Se usa solo para validación en el
frontend y se descarta antes del POST.

El polling de actualización corre cada 30 segundos con `setInterval`.

---

## Reglas para Claude Code

**Antes de editar cualquier archivo:**
1. Leer el archivo completo con `Read` para entender el contexto real
2. No asumir que el código generado por Stitch coincide exactamente con estas
   convenciones — verificar siempre los nombres reales de clases e IDs

**Al editar CSS:**
- Usar siempre variables CSS (`var(--color-amber)`) nunca hex directo
- Los breakpoints mobile están en `max-width: 768px`
- Las secciones `.two-col` colapsan a columna única en mobile automáticamente

**Al editar JS:**
- No introducir dependencias externas ni import/export (el JS es vanilla sin módulos)
- Mantener la inicialización dentro de `DOMContentLoaded`
- El polling de firmas usa `setInterval` — si se modifica `initSignaturesList`,
  verificar que el intervalo se limpia correctamente para evitar memory leaks

**Al editar HTML:**
- No cambiar los `id` de sección sin actualizar los `href` correspondientes
- Los textos del manifiesto son contenido político sensible — no parafrasear
  ni resumir sin instrucción explícita del usuario
- Las imágenes en `/images/` tienen nombres exactos con doble guión bajo,
  verificar el nombre antes de referenciar en `src`

**Deploy:**
- El deploy a GitHub Pages es manual: `git add . && git commit -m "..." && git push`
- No hay CI/CD configurado salvo que el usuario lo indique expresamente
- GitHub Pages sirve desde la raíz de `main` — no hay carpeta `dist`

---

## Tareas frecuentes — referencia rápida

**Cambiar un color:** editar la variable en `:root` en `css/styles.css`

**Cambiar texto del manifiesto:** editar el párrafo correspondiente en `index.html`
dentro de la sección `#punto-N`

**Agregar un campo al formulario:** editar el `.form-panel` en `index.html` +
agregar validación en `initSignatureForm()` en `main.js` + agregar la columna
en la tabla de Supabase si corresponde

**Cambiar imagen de una sección:** reemplazar el archivo en `/images/` manteniendo
el mismo nombre, o actualizar el `src` / `background-image` en HTML o CSS

**Modificar el polling:** buscar `setInterval` en `main.js`

**Agregar animación de entrada:** usar `IntersectionObserver` — ya hay un patrón
establecido en el código si se implementó para las secciones del manifiesto