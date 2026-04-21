
## Servidor
**Máquina:** SRV2  
**Servidor Web:** Apache2  

## Descripción
Página web temporal que muestra una galería de arte con imágenes de obras famosas. Esta página será monitorizada posteriormente por el SOC.

## Contenido de la Página

```html
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Galería de Arte</title>

<style>
    body {
        margin: 0;
        font-family: 'Segoe UI', sans-serif;
        background: #111;
        color: white;
        text-align: center;
    }

    header {
        padding: 20px;
        font-size: 28px;
        letter-spacing: 2px;
        background: #000;
        border-bottom: 1px solid #333;
    }

    .gallery-container {
        position: relative;
        max-width: 900px;
        margin: 40px auto;
        overflow: hidden;
        border-radius: 10px;
        box-shadow: 0 0 20px rgba(0,0,0,0.6);
    }

    .gallery {
        display: flex;
        transition: transform 0.5s ease-in-out;
    }

    .gallery img {
        width: 100%;
        flex-shrink: 0;
        object-fit: cover;
    }

    .arrow {
        position: absolute;
        top: 50%;
        transform: translateY(-50%);
        font-size: 40px;
        background: rgba(0,0,0,0.5);
        border: none;
        color: white;
        cursor: pointer;
        padding: 10px;
        z-index: 10;
        transition: background 0.3s;
    }

    .arrow:hover {
        background: rgba(255,255,255,0.2);
    }

    .left {
        left: 10px;
    }

    .right {
        right: 10px;
    }

    .caption {
        margin-top: 15px;
        font-size: 18px;
        color: #ccc;
    }
</style>
</head>

<body>

<header>Galería de Arte</header>

<div class="gallery-container">
    <button class="arrow left" onclick="prevSlide()">❮</button>

    <div class="gallery" id="gallery">
        <img src="https://upload.wikimedia.org/wikipedia/commons/6/6a/Mona_Lisa.jpg">
        <img src="https://upload.wikimedia.org/wikipedia/commons/a/a3/The_Starry_Night.jpg">
        <img src="https://upload.wikimedia.org/wikipedia/commons/4/4c/The_Scream.jpg">
        <img src="https://upload.wikimedia.org/wikipedia/commons/0/0c/Las_Meninas_01.jpg">
    </div>

    <button class="arrow right" onclick="nextSlide()">❯</button>
</div>

<div class="caption" id="caption"></div>

<script>
    const gallery = document.getElementById('gallery');
    const images = document.querySelectorAll('.gallery img');
    const caption = document.getElementById('caption');

    let index = 0;

    const titles = [
        "La Mona Lisa - Leonardo da Vinci",
        "La Noche Estrellada - Vincent van Gogh",
        "El Grito - Edvard Munch",
        "Las Meninas - Diego Velázquez"
    ];

    function updateGallery() {
        gallery.style.transform = `translateX(-${index * 100}%)`;
        caption.textContent = titles[index];
    }

    function nextSlide() {
        index = (index + 1) % images.length;
        updateGallery();
    }

    function prevSlide() {
        index = (index - 1 + images.length) % images.length;
        updateGallery();
    }

    // Inicializar
    updateGallery();
</script>

</body>
</html>

```
- [Index](../Index.md)
