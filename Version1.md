# Codigo de la Version 1

La versión 1 de CloneFlix representa la fase inicial del desarrollo de una plataforma web inspirada en los servicios de streaming más populares, se construyó la estructura base del sitio utilizando HTML, CSS y JavaScript, para lograr un diseño responsivo y visualmente atractivo.

---

## Archivo HTML

Estructura principal del sitio y conexión con los estilos y scripts.

```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>🎃 CloneFlix</title>
        <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-sRIl4kxILFvY47J16cr9ZwB07vP4J8+LH7qKQnuqkuIAvNWLzeN8tE5YBujZqJLB" crossorigin="anonymous">
        <link rel="stylesheet" href="CSS/app.css">
    </head>
    <body>
        <nav class="navbar navbar-expand-lg border-bottom bg-black">
            <div class="container">
                <a href="#" class="navbar-brand">
                    <b>Clone</b>Flix
                </a>
                <form id="searchForm" class="d-flex ms-auto">
                    <input id="searchInput" type="text" class="form-control me-2" placeholder="Busca tu movie...">
                    <button type="submit" class="btn btn-danger">Buscar</button>
                </form>
            </div>
        </nav>

        <header class="hero bg-black">
            <div class="container">
                <h1 id="heroTitle" class="display-5 fw-bold"></h1>
                <p id="heroDesc" class="lead col-lg-6"></p>
                <button id="heroPlay" class="btn btn-danger btn-lg">
                    Ver Ahora
                </button>
            </div>
        </header>

        <main id="rowsContainer" class="container my-4">
        </main>

        <footer class="footer py-4 mt-5">
            <div class="container small">
                Hecho con ❤️ utilizando TVMaze
            </div>
        </footer>

        <!-- Modal para el detalle de la película -->
        <div id="detailModal" class="modal fade" tabindex="-1" aria-hidden="true">
            <div class="modal-dialog modal-dialog-centered modal-xl">
                <div class="modal-content bg-dark text-light">
                    <div class="modal-header border-secondary">
                        <h5 id="detailTitle" class="modal-title">Detalle</h5>
                        <button class="btn-close btn-close-white" data-bs-dismiss="modal"></button>
                    </div>
                    <div id="detailBody" class="modal-body">
                        Cargando...
                    </div>
                </div>
            </div>
        </div>

        <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.8/dist/js/bootstrap.bundle.min.js" integrity="sha384-FKyoEForCGlyvwx9Hj09JcYn3nv7wiPVlz7YYwJrWVcXK/BmnVDxM+D2scQbITxI" crossorigin="anonymous"></script>
        <script src="JS/app.js"></script>
    </body>
</html>
```
---

## Archivo CSS

Estilos personalizados del sitio para darle identidad visual

```css
body {
    background-color: #0B0D10;
}
.navbar-brand b {
    color: #E50914;
}
.hero {
    min-height: 55vh;
    background-size: cover;
    background-position: center;
    display: flex;
    align-items: end;
    padding: 3rem 1rem;
    position: relative;
}
.hero::after {
    content: "";
    position: absolute;
    inset: 0;
    background: linear-gradient(180deg, rgba(0, 0, 0, 0.0), rgba(0, 0, 0, 0.7));
}
.hero > .container {
    position: relative;
    z-index: 2;
}
.row-title {
    font-weight: 700;
    margin: 1rem 0 .5rem;
}
.rail {
    display: flex;
    gap: 1rem;
    overflow-x: auto;
    padding-bottom: .5em;
    scroll-snap-type: x mandatory;
}
.card-poster {
    min-width: 160px;
    scroll-snap-align: start;
    background: #111;
    border: none;
}
.card-poster img {
    aspect-ratio: 2 / 3;
    object-fit: cover;
    border-radius: .5rem;
}
.badge-genre {
    background-color: #E50914;
}
.footer {
    border-top: 1px solid #222;
    color: #9AA4AD;
}
::-webkit-scrollbar {
    height: 8px;
}
::-webkit-scrollbar-thumb {
    background-color: #333;
    border-radius: 4px;
}
```

---

## Archivo JavaScript

Controla la lógica, conexión con la API y generación dinámica del contenido.

```js
// api a tvmaze
const API = "https://api.tvmaze.com"


const rowsContainer = document.getElementById('rowsContainer' )
const hero = document.getElementById('hero')
const heroTitle = document.getElementById ('heroTitle')
const heroDesc = document.getElementById('heroDesc' )
const heroPlay = document.getElementById('heroPlay' )

const init = async () => {
const trending = await fetchJSON(`${API}/shows?page=1`)
renderRow("Tendencias", trending.slice(0,20))
console ('@@@ trending => ', trending)
}

const renderRow = (title, shows) => {
    const section = document. createElement ('section')
    section. classList = 'mb-3'
    section. innerHTML =
    `
    <h3 class="rowTitle">${title}</h3>
    <div class="rail" data-rail></div>
    `
    // funcion para crear los poster mini y pegarlos
    rowsContainer.appendChild(section)
    }


const fetchJSON = async (url) => {
const res = await fetch(url)
if (!res.ok) {
throw new Error('Error al cargar datos: ', url)
}

return await res.json()
}

init()
```

---

## Navegacion
- 🔗 [home](README.md)
- ❌ [Version_2](archivo/version2)
