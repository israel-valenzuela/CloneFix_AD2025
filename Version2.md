# Ccodigo de la vercion 2

### Descripción general

En esta segunda versión del proyecto se integraron funcionalidades dinámicas que permiten visualizar, 
buscar y obtener información detallada de películas y series utilizando la API de TVMaze. Estas mejoras
hacen que la aplicación sea más interactiva y ofrezca una experiencia de usuario más completa en 
comparación con la primera versión.

---

## Codgigo:
```js
// api a tvmaze
const API = "https://api.tvmaze.com"

// Elementos del DOM
const rowsContainer = document.getElementById('rowsContainer' )
const hero = document.getElementById('hero')
const heroTitle = document.getElementById ('heroTitle')
const heroDesc = document.getElementById('heroDesc' )
const heroPlay = document.getElementById('heroPlay' )

const init = async () => {
const trending = await fetchJSON(`${API}/shows?page=1`)
renderHero(trending[Math.floor(Math.random() * trending.length)]);
renderRow("Tendencias", trending.slice(0,20))
console ('@@@ trending => ', trending)
}

const viewSearch = () => {
    const form = document.getElementById('searchForm');
    const input = document.getElementById('searchInput');
    form.addEventListener('submit', async (e) => {
        e.preventDefault();
        const movie = input.value.trim();
        if (!movie) {
            return;
        }
        const results = await fetchJSON(`${API}/search/shows?q=${encodeURIComponent(movie)}`);
        const shows = results.map(r => r.show);
        rowsContainer.innerHTML = '';
        renderRow(`Resultados para ${movie}`, shows);
    });
}

const renderRow = (title, shows) => {
    const section = document.createElement('section');
    section.classList = 'mb-3';
    section.innerHTML = 
    `
    <h3 class="rowTitle">${title}</h3>
    <div class="rail" data-rail></div>
    `;
    const rail = section.querySelector('[data-rail]');
    shows.forEach((show) => {
        rail.appendChild(posterCard(show));
    });
    rowsContainer.appendChild(section);
}

const posterCard = show => {
    const card = document.createElement('div');
    card.className = 'card card-poster';
    const img = show?.image?.medium || 'https://placehold.co/600x400?text=Sin+Imagen';
    
    card.innerHTML =
    `
    <img class="card-img-top" src="${img}">
    <div class="card-body p-2">
        <div>
            ${(show.genres || []).slice(0,2).join(".")}
        </div>
        <div class="fw-semibold">
            ${escapeHTML(show.name)}
        </div>
    </div>
    `;
    card.addEventListener('click', () => openDetail(show.id));
    return card;
}

const escapeHTML = s => {
    return (s||"").replace(/[&<>"']/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m]));
}

const fetchJSON = async (url) => {
    const res = await fetch(url);
    if (!res.ok) {
        throw new Error('Error al cargar datos: ' + url);
    }
    return await res.json();
}

const renderHero = show => {
    if (!show || !hero) {
        return;
    }
    const bg = show?.image?.original || show?.image?.medium || 'https://placehold.co/600x400?text=Sin+Imagen';
    
    hero.style.backgroundImage = bg ? `url(${bg})` : 'none';
    heroTitle.textContent = show.name || '';
    heroDesc.innerHTML = stripHTML(show.summary || '').slice(0,200) + '...';
    heroPlay.onclick = () => openDetail(show.id);
}

const stripHTML = html => {
    return (html||"").replace(/<[^>]*>/g,""); 
}

const openDetail = async (id) => {
    const modalEL = document.getElementById('detailModal');
    const modalBody = document.getElementById('detailBody');
    const modalTitle = document.getElementById('detailTitle');
    modalTitle.textContent = 'Cargando...';
    modalBody.innerHTML = 'Cargando...';

    const modal = bootstrap.Modal.getOrCreateInstance(modalEL);

    const show = await fetchJSON(`${API}/shows/${id}`);
    modalTitle.textContent = show.name;
    modalBody.innerHTML = 
    `
    <div class="row g-4">
        <div class="col-md-4">
            <img class="img-fluid rounded" src="${show?.image?.original || show?.image?.medium || 'https://placehold.co/600x400?text=Sin+Imagen'}" />
        </div>
        <div class="col-md-8">
            <div class="mb-2">
                ${(show.genres || []).map(g => 
                    `<span class="badge badge-genre me-1">${g}</span>`
                ).join("")}
            </div>
            <p class="text-secondary small">
                ${show.summary || "Sin Sinopsis"}
            </p>
            <p class="text-secondary small">
                ⭐${show?.rating?.average ?? 'N/A'}. Lenguaje: ${show?.language ?? 'N/A'}. Status: ${show?.status ?? 'N/A'}
            </p>
            <a class="btn btn-outline-light me-2" href="${show?.officialSite || show?.url}" 
            target="_blank" rel="noopener">
                Web Site
            </a>
        </div>
    </div>
    `;
    modal.show();
}

init()
```
---
## Búsqueda de películas o series (viewSearch)

Se agregó un formulario interactivo que permite buscar títulos por nombre:
- Captura el evento submit del formulario.
- Envía la consulta a la API (/search/shows?q=).
- Limpia los resultados anteriores y muestra las coincidencias en pantalla.

## Renderización de resultados (renderRow y posterCard)

Cada conjunto de resultados (ya sean tendencias o búsquedas) se organiza en secciones:
- renderRow() crea una fila con el título de la categoría y una galería de tarjetas.
- posterCard() genera dinámicamente las tarjetas de cada serie/película, incluyendo su imagen, géneros y nombre.

## Visualización de detalles (openDetail)

Cuando el usuario selecciona una película o serie, la función:
- Llama al endpoint /shows/{id}.
- Muestra en un modal información completa: imagen, géneros, resumen, calificación, idioma, estado y enlace al sitio oficial.

# Adición del identificador id="hero" en el <header>

---

Antes, el encabezado (header) solo contenía los elementos visuales del título y botón, pero no tenía un identificador que el código JavaScript pudiera reconocer

```html
<header id="hero" class="hero bg-black">
    <div class="container">
        <h1 id="heroTitle" class="display-5 fw-bold"></h1>
        <p id="heroDesc" class="lead col-lg-6"></p>
        <button id="heroPlay" class="btn btn-danger btn-lg">
            Ver Ahora
        </button>
    </div>
</header>
```
## Navegacion
- 🔗 [home](README.md)
- ❌ [Version_3](arvhivo/Version3)
