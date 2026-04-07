# Ejercitación: Pizzería DDS — migrando a Bootstrap

## Descripción

Se parte de un sitio web simple de tres páginas con HTML y CSS vanilla. El objetivo es migrar el sitio, paso a paso, para usar Bootstrap 5, eliminando el CSS propio que Bootstrap reemplaza.

**Archivos de inicio:**

| Archivo | Contenido |
|---------|-----------|
| `index.html` | Home con banner y tres columnas de presentación |
| `horarios.html` | Tabla de horarios de atención semanal |
| `pedidos.html` | Formulario para adelantar pedidos |
| `estilos.css` | Estilos propios (se reducirá progresivamente) |

El archivo `estilos.css` tiene comentarios que indican en qué paso se elimina cada sección. Leerlos antes de empezar.

---

## Paso 1 — Incluir Bootstrap

**Archivos:** `index.html`, `horarios.html`, `pedidos.html`

**Objetivo:** conectar Bootstrap al proyecto y observar los cambios que Reboot produce en los estilos base.

### Qué hacer

En el `<head>` de los tres archivos, agregar el link al CSS de Bootstrap **antes** del link a `estilos.css`:

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>...</title>

  <!-- Agregar esta línea ANTES de estilos.css -->
  <link rel="stylesheet"
        href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css">

  <!-- El CSS propio siempre DESPUÉS de Bootstrap -->
  <link rel="stylesheet" href="estilos.css">
</head>
```

Agregar también el script de Bootstrap al final del `<body>` de los tres archivos:

```html
  <!-- Justo antes de </body> -->
  <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/js/bootstrap.bundle.min.js"></script>
</body>
```

### Qué observar

Abrir `index.html` en el navegador y comparar con la versión anterior:

- La **tipografía cambia**: Bootstrap aplica `font-family: system-ui, sans-serif` en el body. Como `estilos.css` sobreescribe con `Georgia`, la diferencia se ve en elementos sin CSS propio (por ejemplo, el texto dentro de la tabla si se abre `horarios.html`).
- Los **márgenes del body desaparecen**: Bootstrap establece `margin: 0` en el body vía Reboot.
- Los **links dentro del texto** pueden cambiar de color: Bootstrap los hace azules por defecto, pero `estilos.css` los sobreescribe en el nav.
- En `horarios.html`, la tabla puede verse levemente diferente: Bootstrap aplica un reset global a `<table>`.

> **Para reflexionar:** ¿por qué el CSS propio no "se rompió" con Bootstrap? Porque `estilos.css` se carga *después* de Bootstrap. Sus reglas tienen precedencia cuando la especificidad es igual o mayor.

### CSS que se puede eliminar

El bloque del reset en `estilos.css` ya no es necesario — Bootstrap lo hace con Reboot:

```css
/* Eliminar esto: */
* {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
```

Se puede dejar por ahora sin problemas; no produce conflictos.

---

## Paso 2 — Aplicar `.container`

**Archivos:** `index.html`, `horarios.html`, `pedidos.html`

**Objetivo:** reemplazar la clase `.pagina` (wrapper de contenido centrado) por `.container` de Bootstrap.

### Qué hacer

En los tres archivos HTML, buscar todos los `<div class="pagina">` y cambiar la clase:

**Antes:**
```html
<div class="pagina">
  ...
</div>
```

**Después:**
```html
<div class="container">
  ...
</div>
```

> **Atención en `index.html`:** el `<div class="hero">` queda fuera del container (es correcto: el hero ocupa todo el ancho de la pantalla). Solo el div que envuelve las `.columnas` tiene la clase `.pagina` que se reemplaza.

### Qué eliminar de `estilos.css`

Eliminar el bloque marcado como "PASO 2":

```css
/* Eliminar esto: */
.pagina {
  max-width: 960px;
  margin: 0 auto;
  padding: 0 24px;
}
```

### Qué observar

- El contenido sigue centrado y con ancho máximo, igual que antes.
- La diferencia principal: el ancho máximo ahora es **responsivo** — cambia en cada breakpoint (540px, 720px, 960px, 1140px según el viewport). Con el `.pagina` propio era siempre 960px fijo.
- En pantallas angostas, el `.container` tiene padding lateral automático de `0.75rem` a cada lado.

---

## Paso 3 — Grilla en la home

**Archivos:** `index.html`

**Objetivo:** reemplazar el layout de tres columnas con `display: flex` por la grilla de Bootstrap (`.row` + `.col-*`).

### Qué hacer

Dentro del `.container` de `index.html`, reemplazar la estructura `.columnas` / `.columna` por `.row` / `.col-md-4`:

**Antes:**
```html
<div class="container">
  <div class="columnas">
    <div class="columna">
      <h2>🍕 Nuestras especialidades</h2>
      <p>...</p>
      <p><a href="pedidos.html">Ver el menú completo →</a></p>
    </div>
    <div class="columna">
      <h2>👨‍🍳 Quiénes somos</h2>
      <p>...</p>
    </div>
    <div class="columna">
      <h2>📦 Pedidos para retirar</h2>
      <p>...</p>
      <p><a href="pedidos.html">Adelantar pedido →</a></p>
    </div>
  </div>
</div>
```

**Después:**
```html
<div class="container">
  <div class="row py-4 g-3">
    <div class="col-md-4">
      <div class="card h-100 p-4">
        <h2 class="h5 text-danger">🍕 Nuestras especialidades</h2>
        <p>...</p>
        <p><a href="pedidos.html">Ver el menú completo →</a></p>
      </div>
    </div>
    <div class="col-md-4">
      <div class="card h-100 p-4">
        <h2 class="h5 text-danger">👨‍🍳 Quiénes somos</h2>
        <p>...</p>
      </div>
    </div>
    <div class="col-md-4">
      <div class="card h-100 p-4">
        <h2 class="h5 text-danger">📦 Pedidos para retirar</h2>
        <p>...</p>
        <p><a href="pedidos.html">Adelantar pedido →</a></p>
      </div>
    </div>
  </div>
</div>
```

**Clases usadas y su significado:**

| Clase | Qué hace |
|-------|---------|
| `row` | Activa flexbox y crea la fila de la grilla |
| `py-4` | Padding vertical de `1.5rem` arriba y abajo |
| `g-3` | Gutter (espacio entre columnas) de `1rem` |
| `col-md-4` | Ocupa 4 de 12 columnas cuando el viewport es ≥ 768px; en mobile ocupa el 100% |
| `card` | Componente card de Bootstrap (borde, fondo blanco, border-radius) |
| `h-100` | `height: 100%` — hace que todas las cards tengan la misma altura |
| `p-4` | Padding interno de `1.5rem` |
| `h5` | Aplica el tamaño visual de `<h5>` a cualquier elemento |
| `text-danger` | Color rojo (Bootstrap's `danger`) |

### Qué eliminar de `estilos.css`

Eliminar el bloque marcado como "PASO 3":

```css
/* Eliminar esto: */
.columnas { ... }
.columna { ... }
.columna h2 { ... }
.columna p { ... }
.columna a { ... }
```

### Qué observar

- **En pantalla grande (≥ 768px):** tres columnas en fila, igual que antes pero con el espaciado de Bootstrap.
- **Achicar la ventana a menos de 768px:** las columnas se apilan verticalmente de forma automática. Con el CSS propio (flex), se comprimían en fila y se veían muy angostas. Bootstrap lo resuelve sin escribir ningún `@media query`.
- Las cards tienen todas la misma altura gracias a `h-100`, aunque el contenido sea diferente.

> **Para explorar:** ¿qué pasa si se cambia `col-md-4` por `col-sm-6`? ¿Y si se combina `col-12 col-md-4`? Probar achicando y agrandando la ventana del navegador.

---

## Paso 4 — Tabla de horarios

**Archivos:** `horarios.html`

**Objetivo:** aplicar clases Bootstrap a la tabla y eliminar todos los estilos de tabla del CSS propio.

### Qué hacer

#### 4.1 — Clase base de la tabla

Agregar clases Bootstrap al `<table>`:

**Antes:**
```html
<table>
```

**Después:**
```html
<table class="table table-bordered table-hover">
```

#### 4.2 — Encabezado oscuro

Agregar clase al `<thead>`:

**Antes:**
```html
<thead>
```

**Después:**
```html
<thead class="table-dark">
```

#### 4.3 — Celdas abiertas con color de Bootstrap

Reemplazar `class="celda-abierta"` por `class="table-success"` en **todas** las celdas `<td>` que la tienen. Esto incluye las celdas con `rowspan="2"` de Sábado y Domingo:

**Antes:**
```html
<td class="celda-abierta">✓ Abierto</td>
<td class="celda-abierta" rowspan="2">✓ Abierto<br>12:00 – 23:30</td>
```

**Después:**
```html
<td class="table-success">✓ Abierto</td>
<td class="table-success" rowspan="2">✓ Abierto<br>12:00 – 23:30</td>
```

> **Nota:** el atributo `rowspan="2"` es HTML puro y no cambia. Bootstrap no altera la estructura de la tabla, solo su presentación.

#### 4.4 — Título de la página

Reemplazar la clase `.titulo-pagina` por utilidades Bootstrap (en `horarios.html` y también en `pedidos.html` para mantener consistencia):

**Antes:**
```html
<h1 class="titulo-pagina">Horarios de atención</h1>
```

**Después:**
```html
<h1 class="text-center text-danger mt-4 mb-3">Horarios de atención</h1>
```

### Qué eliminar de `estilos.css`

Eliminar los bloques marcados como "PASO 4":

```css
/* Eliminar esto: */
.titulo-pagina { ... }
table { ... }
thead th { ... }
tbody td { ... }
tbody tr:nth-child(even) td { ... }
.celda-abierta { ... }
```

### Qué observar

- El `<thead>` tiene fondo oscuro por `table-dark` sin ningún CSS propio.
- El `table-hover` hace que al pasar el mouse sobre una fila se resalte automáticamente.
- Las celdas verdes usan el color `success` de Bootstrap, consistente con el sistema de colores del resto del sitio.
- El `rowspan="2"` sigue funcionando exactamente igual — Bootstrap no lo toca.
- El CSS propio de la tabla fue reemplazado por tres palabras: `table table-bordered table-hover`.

> **Para explorar:** agregar `table-striped` a la clase del `<table>`. ¿Qué cambia? ¿Se combina bien con `table-success` en las celdas?

---

## Paso 5 — Formulario de pedidos

**Archivos:** `pedidos.html`

**Objetivo:** reemplazar los estilos custom del formulario por clases Bootstrap.

### Qué hacer

#### 5.1 — Contenedor del formulario

Reemplazar `.caja-formulario` por utilidades Bootstrap:

**Antes:**
```html
<div class="caja-formulario">
  <h1 class="titulo-pagina" style="text-align: left; margin-top: 0;">Adelantar pedido</h1>
  <form>
```

**Después:**
```html
<div class="card p-4 mt-4 mb-5 mx-auto" style="max-width: 620px;">
  <h1 class="h3 text-danger mb-4">Adelantar pedido</h1>
  <form>
```

#### 5.2 — Campos de texto, `select` y `textarea`

Para cada `<div class="campo">` con sus elementos internos, aplicar clases Bootstrap:

**Antes (ejemplo — repetir para todos los campos):**
```html
<div class="campo">
  <label for="nombre">Nombre completo</label>
  <input type="text" id="nombre" name="nombre" placeholder="Ej: María González">
</div>
```

**Después:**
```html
<div class="mb-3">
  <label for="nombre" class="form-label">Nombre completo</label>
  <input type="text" class="form-control" id="nombre" name="nombre"
         placeholder="Ej: María González">
</div>
```

**Tabla de reemplazos:**

| HTML original | HTML con Bootstrap | Descripción |
|--------------|-------------------|-------------|
| `<div class="campo">` | `<div class="mb-3">` | Margen inferior de `1rem` entre campos |
| `<label>` | `<label class="form-label">` | Espaciado adecuado sobre el campo |
| `<input>` | `<input class="form-control">` | Input estilizado con bordes, padding y focus azul |
| `<select>` | `<select class="form-select">` | Select con flecha personalizada |
| `<textarea>` | `<textarea class="form-control">` | Textarea con mismos estilos que input |

#### 5.3 — Radio buttons

Bootstrap tiene una estructura específica para radios y checkboxes:

**Antes:**
```html
<div class="campo">
  <label>Tamaño</label>
  <div class="opciones-radio">
    <label><input type="radio" name="tamanio" value="chica"> Chica (4 porciones)</label>
    <label><input type="radio" name="tamanio" value="grande" checked> Grande (8 porciones)</label>
  </div>
</div>
```

**Después:**
```html
<div class="mb-3">
  <label class="form-label">Tamaño</label>
  <div class="form-check form-check-inline">
    <input class="form-check-input" type="radio" name="tamanio" id="chica" value="chica">
    <label class="form-check-label" for="chica">Chica (4 porciones)</label>
  </div>
  <div class="form-check form-check-inline">
    <input class="form-check-input" type="radio" name="tamanio" id="grande" value="grande" checked>
    <label class="form-check-label" for="grande">Grande (8 porciones)</label>
  </div>
</div>
```

> **Nota:** ahora el `<label>` usa `for="id"` para vincular al input. Es buena práctica de accesibilidad y Bootstrap lo requiere para que `.form-check-label` funcione correctamente.

#### 5.4 — Botón de envío

**Antes:**
```html
<button type="submit" class="boton-enviar">Enviar pedido</button>
```

**Después:**
```html
<button type="submit" class="btn btn-danger">Enviar pedido</button>
```

### Qué eliminar de `estilos.css`

Eliminar los bloques marcados como "PASO 5":

```css
/* Eliminar esto: */
.caja-formulario { ... }
.campo { ... }
.campo label { ... }
.campo input, .campo select, .campo textarea { ... }
.campo textarea { ... }
.opciones-radio { ... }
.boton-enviar { ... }
.boton-enviar:hover { ... }
```

### Qué observar

- Los inputs tienen bordes redondeados y un **focus azul con sombra** al hacer clic, sin ningún CSS propio.
- El `<select>` tiene una flecha personalizada de Bootstrap en lugar de la flecha del navegador.
- El botón tiene hover integrado.
- El formulario se ve profesional sin haber escrito una sola línea de CSS propio.

> **Para explorar:** agregar `class="is-invalid"` a un input y debajo un `<div class="invalid-feedback">El campo es obligatorio.</div>`. ¿Qué muestra Bootstrap?

---

## Estado final del CSS

Después de completar los cinco pasos, el archivo `estilos.css` se redujo de ~120 líneas a solo los estilos del nav y el hero:

```css
/* =============================================
   Pizzería DDS — Estilos propios (versión final)
   ============================================= */

body {
  font-family: Georgia, serif;
  background-color: #fef9f5;
  color: #2c2c2c;
  line-height: 1.6;
}

/* --- Navegación --- */
nav {
  background-color: #b91c1c;
  padding: 14px 32px;
  display: flex;
  align-items: center;
  gap: 28px;
}

nav .marca {
  font-size: 22px;
  font-weight: bold;
  color: white;
  text-decoration: none;
  margin-right: auto;
}

nav a {
  color: rgba(255, 255, 255, 0.85);
  text-decoration: none;
  font-size: 15px;
}

nav a:hover {
  color: white;
  text-decoration: underline;
}

/* --- Hero --- */
.hero {
  background-color: #7f1d1d;
  color: white;
  text-align: center;
  padding: 72px 24px;
}

.hero h1 { font-size: 2.8rem; margin-bottom: 14px; }
.hero p { font-size: 1.2rem; margin-bottom: 28px; }

.hero .boton-cta {
  display: inline-block;
  background-color: white;
  color: #b91c1c;
  padding: 10px 28px;
  border-radius: 4px;
  text-decoration: none;
  font-weight: bold;
}
```

Eso es aproximadamente el 30% del CSS original. El resto lo maneja Bootstrap.

---

## Paso 6 (bonus) — La navegación con Bootstrap

La navegación todavía usa CSS propio. Para migrarla a Bootstrap Navbar, la estructura HTML cambia bastante. Este es un paso más complejo, ideal para una segunda clase o para exploración autónoma.

La estructura de un navbar Bootstrap equivalente al nav actual sería:

```html
<nav class="navbar navbar-expand-lg" style="background-color: #b91c1c;">
  <div class="container-fluid">

    <a class="navbar-brand text-white fw-bold" href="index.html">🍕 DDS </a>

    <button class="navbar-toggler" type="button"
            data-bs-toggle="collapse"
            data-bs-target="#navPrincipal">
      <span class="navbar-toggler-icon"></span>
    </button>

    <div class="collapse navbar-collapse" id="navPrincipal">
      <ul class="navbar-nav ms-auto">
        <li class="nav-item">
          <a class="nav-link text-white" href="index.html">Inicio</a>
        </li>
        <li class="nav-item">
          <a class="nav-link text-white" href="horarios.html">Horarios</a>
        </li>
        <li class="nav-item">
          <a class="nav-link text-white" href="pedidos.html">Hacer un pedido</a>
        </li>
      </ul>
    </div>

  </div>
</nav>
```

**Ventaja principal:** al achicar la ventana, aparece automáticamente el botón "hamburguesa" que muestra/oculta el menú. Sin escribir JavaScript ni CSS.

Con esto, los estilos del nav también se pueden eliminar de `estilos.css`.

---

## Resumen de los pasos

| Paso | Acción | CSS eliminado |
|------|--------|--------------|
| 1 | Incluir Bootstrap CDN | Reset (`*`) |
| 2 | Reemplazar `.pagina` por `.container` | `.pagina` |
| 3 | Grilla en la home | `.columnas`, `.columna` y variantes |
| 4 | Clases de tabla y colores | `table`, `thead th`, `tbody td`, `.celda-abierta`, `.titulo-pagina` |
| 5 | Clases de formulario | `.caja-formulario`, `.campo`, `.boton-enviar` y variantes |
| 6 (bonus) | Bootstrap Navbar | `nav`, `nav .marca`, `nav a` |
