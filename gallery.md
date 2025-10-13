---
layout: page
title: Gallery
permalink: /gallery/
order: 3
---

<div class="page">
  <h1 class="page-title">Gallery</h1>
  <p>Cliquez sur une photo pour l'agrandir. Utilisez les flèches pour naviguer.</p>

  <div id="gallery" class="flickr-gallery"></div>
</div>

<!-- Overlay pour mode agrandi -->
<div id="lightbox" style="display:none;">
  <span id="prev"><</span>
  <img id="lightbox-img" src="" alt="">
  <span id="next">></span>
</div>

<style>
  /* Galerie de photos */
  .flickr-gallery {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(100px, 1fr));
    gap: 10px;
  }

  .flickr-gallery img {
    width: 100%;
    height: auto;
    border-radius: 0 !important; /* supprime les coins arrondis */
    box-shadow: 0 2px 8px rgba(0,0,0,0.15);
    cursor: pointer;
    transition: none;
  }

  .flickr-gallery img:hover {
    transform: none;
  }

  /* Lightbox (agrandissement) */
  #lightbox {
    position: fixed;
    top: 0; left: 0;
    width: 100%; height: 100%;
    background: rgba(0,0,0,0.9);
    display: none;
    align-items: center;
    justify-content: center;
    z-index: 9999;
  }

  #lightbox img {
    max-width: 90%;
    max-height: 85%;
    border-radius: 0 !important;
  }

  #prev, #next {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    color: white;
    font-size: 2rem;
    cursor: pointer;
    opacity: 0.8;
  }

  #prev:hover, #next:hover { opacity: 1; }
  #prev { left: 30px; }
  #next { right: 30px; }
</style>

<script>
  const FLICKR_ID = "203497831@N05"; // ton NSID Flickr
  const url = `https://www.flickr.com/services/feeds/photos_public.gne?id=${FLICKR_ID}&format=json&nojsoncallback=1`;

  let photos = [];
  let currentIndex = 0;

  // Proxy plus stable que AllOrigins
  fetch(`https://api.codetabs.com/v1/proxy/?quest=${encodeURIComponent(url)}`)
    .then(res => res.text())
    .then(text => {
      const feed = JSON.parse(text);
      photos = feed.items.map(item => item.media.m.replace("_m", "_b")); // version HD
      const container = document.getElementById("gallery");

      photos.forEach((src, index) => {
        const img = document.createElement("img");
        img.src = src;
        img.alt = `Photo ${index + 1}`;
        img.loading = "lazy"; // meilleure perf
        img.addEventListener("click", () => openLightbox(index));
        container.appendChild(img);
      });
    })
    .catch(err => {
      console.error("Erreur de chargement Flickr :", err);
      document.getElementById("gallery").innerHTML =
        "<p>⚠️ Impossible de charger la galerie pour le moment.</p>";
    });

  // --- Lightbox ---
  const lightbox = document.getElementById("lightbox");
  const lightboxImg = document.getElementById("lightbox-img");
  const prev = document.getElementById("prev");
  const next = document.getElementById("next");

  function openLightbox(index) {
    currentIndex = index;
    lightboxImg.src = photos[currentIndex];
    lightbox.style.display = "flex";
  }

  function closeLightbox() {
    lightbox.style.display = "none";
  }

  function showPrev() {
    currentIndex = (currentIndex - 1 + photos.length) % photos.length;
    lightboxImg.src = photos[currentIndex];
  }

  function showNext() {
    currentIndex = (currentIndex + 1) % photos.length;
    lightboxImg.src = photos[currentIndex];
  }

  lightboxImg.addEventListener("click", closeLightbox);
  prev.addEventListener("click", showPrev);
  next.addEventListener("click", showNext);

  document.addEventListener("keydown", e => {
    if (lightbox.style.display === "flex") {
      if (e.key === "ArrowLeft") showPrev();
      if (e.key === "ArrowRight") showNext();
      if (e.key === "Escape") closeLightbox();
    }
  });
</script>
