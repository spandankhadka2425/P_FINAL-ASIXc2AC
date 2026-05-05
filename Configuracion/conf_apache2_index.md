
## Servidor
**Máquina:** SRV2  
**Servidor Web:** Apache2  

## Descripción
Página web que muestra una galería de arte con imágenes de obras famosas. Esta página será monitorizada posteriormente por el SOC.

## Contenido de la Página

<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ArtGalería | SOC Security Project</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0c29 0%, #302b63 50%, #24243e 100%);
            color: #f0f0f0;
            min-height: 100vh;
        }

        /* Header */
        .header {
            background: rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(12px);
            padding: 20px 0;
            position: sticky;
            top: 0;
            z-index: 100;
            border-bottom: 2px solid #e94560;
        }

        .header h1 {
            text-align: center;
            font-size: 2.2rem;
            letter-spacing: 2px;
        }

        .header h1 span {
            color: #e94560;
        }

        .header p {
            text-align: center;
            font-size: 0.85rem;
            opacity: 0.7;
            margin-top: 5px;
        }

        /* Navigation */
        .nav {
            text-align: center;
            margin-top: 15px;
        }

        .nav button {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            padding: 8px 20px;
            margin: 0 8px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 0.9rem;
            transition: all 0.3s ease;
        }

        .nav button:hover, .nav button.active {
            background: #e94560;
            transform: translateY(-2px);
        }

        /* Search & Filter Bar */
        .search-bar {
            max-width: 1200px;
            margin: 20px auto 0;
            padding: 0 20px;
            display: flex;
            gap: 15px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .search-bar input {
            flex: 1;
            max-width: 350px;
            padding: 10px 20px;
            border: none;
            border-radius: 30px;
            background: rgba(255, 255, 255, 0.15);
            color: white;
            font-size: 0.9rem;
        }

        .search-bar input::placeholder {
            color: rgba(255, 255, 255, 0.6);
        }

        .filter-buttons {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .filter-btn {
            background: rgba(255, 255, 255, 0.1);
            border: none;
            color: white;
            padding: 8px 18px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 0.85rem;
            transition: all 0.3s ease;
        }

        .filter-btn:hover, .filter-btn.active {
            background: #e94560;
        }

        /* Main Container */
        .container {
            max-width: 1300px;
            margin: 30px auto;
            padding: 0 20px;
        }

        /* Stats Bar */
        .stats-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            background: rgba(255, 255, 255, 0.05);
            padding: 15px 25px;
            border-radius: 15px;
            margin-bottom: 30px;
            gap: 15px;
        }

        .stat-item {
            text-align: center;
        }

        .stat-number {
            font-size: 1.6rem;
            font-weight: bold;
            color: #e94560;
        }

        .stat-label {
            font-size: 0.75rem;
            opacity: 0.7;
        }

        .random-btn {
            background: #e94560;
            border: none;
            color: white;
            padding: 8px 20px;
            border-radius: 25px;
            cursor: pointer;
            font-size: 0.85rem;
            transition: all 0.3s ease;
        }

        .random-btn:hover {
            background: #ff6b81;
            transform: scale(1.02);
        }

        /* Gallery Grid */
        .art-grid {
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
            gap: 25px;
        }

        .art-card {
            background: rgba(255, 255, 255, 0.08);
            border-radius: 15px;
            overflow: hidden;
            transition: all 0.3s ease;
            cursor: pointer;
            backdrop-filter: blur(5px);
        }

        .art-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.4);
            background: rgba(255, 255, 255, 0.12);
        }

        .art-card img {
            width: 100%;
            height: 200px;
            object-fit: cover;
        }

        .art-info {
            padding: 15px;
        }

        .art-info h3 {
            margin-bottom: 5px;
            color: #e94560;
            font-size: 1.1rem;
        }

        .art-info p {
            font-size: 0.85rem;
            color: #ccc;
        }

        .art-info .category {
            display: inline-block;
            margin-top: 10px;
            font-size: 0.7rem;
            background: rgba(233, 69, 96, 0.3);
            padding: 3px 12px;
            border-radius: 15px;
        }

        /* No Results */
        .no-results {
            text-align: center;
            padding: 50px;
            font-size: 1.2rem;
            opacity: 0.7;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.95);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            max-width: 700px;
            width: 90%;
            background: #1a1a2e;
            border-radius: 20px;
            overflow: hidden;
            animation: fadeIn 0.3s ease;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.95); }
            to { opacity: 1; transform: scale(1); }
        }

        .modal-content img {
            width: 100%;
            height: 350px;
            object-fit: cover;
        }

        .modal-info {
            padding: 25px;
        }

        .modal-info h2 {
            color: #e94560;
            margin-bottom: 10px;
        }

        .modal-info .detail {
            margin: 10px 0;
            font-size: 0.9rem;
        }

        .close-modal {
            background: #e94560;
            border: none;
            color: white;
            padding: 10px 25px;
            border-radius: 30px;
            cursor: pointer;
            margin-top: 15px;
            transition: all 0.3s ease;
        }

        .close-modal:hover {
            background: #ff6b81;
        }

        /* Footer */
        .footer {
            text-align: center;
            padding: 25px;
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            margin-top: 50px;
            font-size: 0.8rem;
            color: #888;
        }

        .footer .admin {
            color: #e94560;
            font-weight: bold;
        }

        .soc-badge {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: #e94560;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.7rem;
            cursor: default;
            opacity: 0.8;
            z-index: 99;
        }

        @media (max-width: 768px) {
            .header h1 { font-size: 1.5rem; }
            .stats-bar { flex-direction: column; text-align: center; }
            .art-grid { grid-template-columns: 1fr; }
        }
    </style>
</head>
<body>

<div class="header">
    <h1>🎨 <span>Art</span>Galería</h1>
    <p>SOC Security Project | Monitorización en Tiempo Real</p>
    <div class="nav">
        <button class="nav-btn active" data-page="gallery">🖼️ Galería</button>
        <button class="nav-btn" data-page="list">📋 Lista de Obras</button>
        <button class="nav-btn" data-page="about">ℹ️ Acerca de</button>
    </div>
</div>

<div class="search-bar" id="searchBar">
    <input type="text" id="searchInput" placeholder="🔍 Buscar por título o artista...">
    <div class="filter-buttons" id="filterButtons">
        <button class="filter-btn active" data-category="all">Todos</button>
        <button class="filter-btn" data-category="Renacimiento">Renacimiento</button>
        <button class="filter-btn" data-category="Romanticismo">Romanticismo</button>
        <button class="filter-btn" data-category="Impresionismo">Impresionismo</button>
    </div>
</div>

<div class="container">
    <div class="stats-bar">
        <div class="stat-item">
            <div class="stat-number" id="artworkCount">0</div>
            <div class="stat-label">Obras de Arte</div>
        </div>
        <div class="stat-item">
            <div class="stat-number" id="visitCount">0</div>
            <div class="stat-label">Visitas</div>
        </div>
        <div class="stat-item">
            <div class="stat-number" id="lastUpdated">2026</div>
            <div class="stat-label">Última Actualización</div>
        </div>
        <button class="random-btn" onclick="showRandomArt()">🎲 Obra Aleatoria</button>
    </div>

    <!-- Gallery Page -->
    <div id="gallery" class="page active">
        <div class="art-grid" id="galleryGrid"></div>
    </div>

    <!-- List Page -->
    <div id="list" class="page">
        <div class="art-grid" id="listGrid"></div>
    </div>

    <!-- About Page -->
    <div id="about" class="page">
        <div style="background: rgba(255,255,255,0.05); border-radius: 20px; padding: 40px; text-align: center;">
            <h2 style="color:#e94560; margin-bottom: 20px;">Sobre este Proyecto</h2>
            <p style="margin-bottom: 20px; line-height: 1.6;">Esta Galería de Arte forma parte del proyecto <strong>SOC Security</strong> del módulo 0379.</p>
            <p style="margin-bottom: 20px; line-height: 1.6;">La página está siendo monitorizada por un Centro de Operaciones de Seguridad utilizando <strong>Wazuh</strong> y <strong>Elastic Stack</strong>.</p>
            <div style="background: rgba(233,69,96,0.2); border-radius: 15px; padding: 20px; margin-top: 20px;">
                <h3 style="margin-bottom: 15px;">👨‍💻 Administradores del Sistema</h3>
                <p style="font-size: 1.2rem;"><strong>Spandan Khadka</strong> & <strong>Anmolpreet Singh Kaur</strong></p>
                <p style="font-size: 0.8rem; margin-top: 10px;">ASIXcAC - Proyecto Final</p>
            </div>
            <div style="margin-top: 30px;">
                <p><strong>Tecnologías utilizadas:</strong></p>
                <p>Apache2 | HAProxy | MySQL | Elasticsearch | Kibana | Wazuh</p>
            </div>
        </div>
    </div>
</div>

<div class="footer">
    <p>© 2026 <span class="admin">Spandan Khadka</span> & <span class="admin">Anmolpreet Singh Kaur</span> | SOC Security Project</p>
    <p>Monitorizado por Wazuh + Elastic Stack</p>
</div>

<div class="soc-badge" title="Esta página está siendo monitorizada por el SOC">
    🛡️ SOC Monitorizada 24/7
</div>

<!-- Modal -->
<div id="modal" class="modal">
    <div class="modal-content">
        <img id="modalImg" src="" alt="">
        <div class="modal-info">
            <h2 id="modalTitle"></h2>
            <p class="detail" id="modalArtist"></p>
            <p class="detail" id="modalYear"></p>
            <p class="detail" id="modalCategory"></p>
            <p id="modalDescription"></p>
            <button class="close-modal" onclick="closeModal()">Cerrar</button>
        </div>
    </div>
</div>

<script>
    // Artworks Database with YOUR local images
    const artworks = [
        { 
            id: 1, 
            title: "Mona Lisa", 
            artist: "Leonardo da Vinci", 
            year: "1503-1519", 
            category: "Renacimiento", 
            description: "La Gioconda, conocida como Mona Lisa, es el retrato más famoso de la historia. Su enigmática sonrisa y la técnica del sfumato la convierten en una obra maestra del Renacimiento italiano. Actualmente se exhibe en el Museo del Louvre, París.",
            image: "img/monalisa.jpg" 
        },
        { 
            id: 2, 
            title: "La Noche Estrellada", 
            artist: "Vincent van Gogh", 
            year: "1889", 
            category: "Impresionismo", 
            description: "Pintada desde el sanatorio de Saint-Rémy-de-Provence, esta obra representa la vista desde la ventana de Van Gogh. Los remolinos en el cielo expresan su estado emocional y su genio artístico.",
            image: "img/la_noche_estraillada.jpg" 
        },
        { 
            id: 3, 
            title: "La Última Cena", 
            artist: "Leonardo da Vinci", 
            year: "1495-1498", 
            category: "Renacimiento", 
            description: "Fresco monumental que representa la última cena de Jesús con sus apóstoles. Destaca por la composición y la expresividad de cada personaje ante el anuncio de la traición.",
            image: "img/ultima_cena.jpg" 
        },
        { 
            id: 4, 
            title: "Almendro en Flor", 
            artist: "Vincent van Gogh", 
            year: "1890", 
            category: "Impresionismo", 
            description: "Van Gogh pintó este cuadro para celebrar el nacimiento de su sobrino. Las ramas floridas contra el cielo azul simbolizan esperanza y renovación, un regalo para su hermano Theo.",
            image: "img/Almendor_de_flor.jpg" 
        },
        { 
            id: 5, 
            title: "El Caminante sobre el Mar de Nubes", 
            artist: "Caspar David Friedrich", 
            year: "1818", 
            category: "Romanticismo", 
            description: "Icono del Romanticismo alemán. Un hombre contempla un paisaje neblinoso de espaldas al espectador, invitando a la reflexión sobre la grandeza de la naturaleza y la soledad del individuo.",
            image: "img/el_caminante_sobre_mar.jpg" 
        },
        { 
            id: 6, 
            title: "Impresión, sol naciente", 
            artist: "Claude Monet", 
            year: "1872", 
            category: "Impresionismo", 
            description: "Esta pintura del puerto de Le Havre dio nombre al movimiento impresionista. Monet captura la luz cambiante y la atmósfera matutina con pinceladas sueltas y colores vibrantes.",
            image: "img/Impression_Sunrise.jpg" 
        }
    ];

    let currentPage = "gallery";
    let currentCategory = "all";
    let currentSearch = "";

    // Visit Counter (localStorage)
    let visitCount = localStorage.getItem("artGalleryVisits") || 0;
    visitCount = parseInt(visitCount) + 1;
    localStorage.setItem("artGalleryVisits", visitCount);
    document.getElementById("visitCount").innerText = visitCount;

    // Last Updated
    document.getElementById("lastUpdated").innerText = "May 2026";
    document.getElementById("artworkCount").innerText = artworks.length;

    function getFilteredArtworks() {
        return artworks.filter(art => {
            const matchesCategory = currentCategory === "all" || art.category === currentCategory;
            const matchesSearch = currentSearch === "" || 
                art.title.toLowerCase().includes(currentSearch.toLowerCase()) ||
                art.artist.toLowerCase().includes(currentSearch.toLowerCase());
            return matchesCategory && matchesSearch;
        });
    }

    function renderGallery() {
        const filtered = getFilteredArtworks();
        const grid = document.getElementById("galleryGrid");
        
        if (filtered.length === 0) {
            grid.innerHTML = '<div class="no-results">❌ No se encontraron obras que coincidan con tu búsqueda.</div>';
            return;
        }
        
        grid.innerHTML = filtered.map(art => `
            <div class="art-card" onclick="showArt(${art.id})">
                <img src="${art.image}" alt="${art.title}" loading="lazy" onerror="this.src='https://via.placeholder.com/300x200?text=Imagen+no+disponible'">
                <div class="art-info">
                    <h3>${art.title}</h3>
                    <p>${art.artist} • ${art.year}</p>
                    <span class="category">${art.category}</span>
                </div>
            </div>
        `).join("");
    }

    function renderList() {
        const filtered = getFilteredArtworks();
        const grid = document.getElementById("listGrid");
        
        if (filtered.length === 0) {
            grid.innerHTML = '<div class="no-results">❌ No se encontraron obras que coincidan con tu búsqueda.</div>';
            return;
        }
        
        grid.innerHTML = filtered.map(art => `
            <div class="art-card" onclick="showArt(${art.id})">
                <div class="art-info">
                    <h3>📌 ${art.title}</h3>
                    <p><strong>${art.artist}</strong> | ${art.year} | ${art.category}</p>
                    <p style="font-size: 0.8rem; margin-top: 8px; opacity: 0.8;">${art.description.substring(0, 100)}...</p>
                </div>
            </div>
        `).join("");
    }

    function showArt(id) {
        const art = artworks.find(a => a.id === id);
        if (!art) return;
        
        document.getElementById("modalImg").src = art.image;
        document.getElementById("modalTitle").innerText = art.title;
        document.getElementById("modalArtist").innerHTML = `<strong>🎨 Artista:</strong> ${art.artist}`;
        document.getElementById("modalYear").innerHTML = `<strong>📅 Año:</strong> ${art.year}`;
        document.getElementById("modalCategory").innerHTML = `<strong>🏷️ Categoría:</strong> ${art.category}`;
        document.getElementById("modalDescription").innerText = art.description;
        document.getElementById("modal").classList.add("active");
    }

    function closeModal() {
        document.getElementById("modal").classList.remove("active");
    }

    function showRandomArt() {
        const randomIndex = Math.floor(Math.random() * artworks.length);
        showArt(artworks[randomIndex].id);
    }

    function navigateTo(page) {
        currentPage = page;
        
        document.getElementById("gallery").classList.remove("active");
        document.getElementById("list").classList.remove("active");
        document.getElementById("about").classList.remove("active");
        
        document.getElementById(page).classList.add("active");
        
        document.querySelectorAll(".nav-btn").forEach(btn => {
            btn.classList.remove("active");
            if (btn.dataset.page === page) {
                btn.classList.add("active");
            }
        });
        
        if (page === "gallery") renderGallery();
        if (page === "list") renderList();
    }

    function applyFilters() {
        if (currentPage === "gallery") renderGallery();
        if (currentPage === "list") renderList();
    }

    // Event Listeners
    document.querySelectorAll(".nav-btn").forEach(btn => {
        btn.addEventListener("click", () => navigateTo(btn.dataset.page));
    });

    document.getElementById("searchInput").addEventListener("input", (e) => {
        currentSearch = e.target.value;
        applyFilters();
    });

    document.querySelectorAll(".filter-btn").forEach(btn => {
        btn.addEventListener("click", () => {
            document.querySelectorAll(".filter-btn").forEach(b => b.classList.remove("active"));
            btn.classList.add("active");
            currentCategory = btn.dataset.category;
            applyFilters();
        });
    });

    document.addEventListener("keydown", (e) => {
        if (e.key === "Escape") closeModal();
    });

    // Initialize
    renderGallery();
    renderList();
</script>

</body>
</html>

- [Index](../Index.md)
