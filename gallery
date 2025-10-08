---
layout: page
title: Gallery
permalink: /gallery/
order: 3
---

<div class="page">
  <h1 class="page-title">Gallery</h1>
  <p>Cliquez sur une photo pour l'agrandir. Utilisez les flèches pour naviguer.</p>

  <div id="gallery"></div>
</div>

<!-- Overlay pour mode agrandi -->
<div id="lightbox" style="display:none;">
  <span id="prev"><</span>
  <img id="lightbox-img" src="" alt="">
  <span id="next">></span>
</div>

<style>
  #gallery {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 15px;
  }

  #gallery img {
    width: 100%;
    height: auto;
    box-shadow: 0 2px 8px rgba(0,0,0,0.2);
    cursor: pointer;
    transition: transform 0.3s ease;
  }

  /*#gallery img:hover {
    transform: scale(1.05);
  } */

  /* Lightbox overlay */
  #lightbox {
    position: fixed;
    top:0; left:0;
    width:100%; height:100%;
    background: rgba(0,0,0,0.9);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 999;
  }

  #lightbox img {
    max-width: 90%;
    max-height: 80%;
    cursor: pointer;
  }

  #prev, #next {
    color: white;
    font-size: 2rem;
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    cursor: pointer;
    user-select: none;
    opacity: 0.7;
    transition: opacity 0.2s;
  }

  #prev:hover, #next:hover {
    opacity: 1;
  }

  #prev { left: 20px; }
  #next { right: 20px; }
</style>

<script>
  const FLICKR_ID = "203497831@N05"; // Remplace par ton NSID
  const url = `https://www.flickr.com/services/feeds/photos_public.gne?id=${FLICKR_ID}&format=json&nojsoncallback=1`;

  let photos = [];
  let currentIndex = 0;

  fetch(`https://api.allorigins.win/get?url=${encodeURIComponent(url)}`)
    .then(res => res.json())
    .then(data => {
      const feed = JSON.parse(data.contents);
      photos = feed.items.map(item => item.media.m.replace("_m","_b")); // grande version
      const container = document.getElementById("gallery");

      photos.forEach((src, index) => {
        const img = document.createElement("img");
        img.src = src;
        img.alt = `Photo ${index+1}`;
        img.addEventListener("click", () => openLightbox(index));
        container.appendChild(img);
      });
    });

  const lightbox = document.getElementById("lightbox");
  const lightboxImg = document.getElementById("lightbox-img");
  const prev = document.getElementById("prev");
  const next = document.getElementById("next");

  function openLightbox(index){
    currentIndex = index;
    lightboxImg.src = photos[currentIndex];
    lightbox.style.display = "flex";
  }

  function closeLightbox(){
    lightbox.style.display = "none";
  }

  function showPrev(){
    currentIndex = (currentIndex - 1 + photos.length) % photos.length;
    lightboxImg.src = photos[currentIndex];
  }

  function showNext(){
    currentIndex = (currentIndex + 1) % photos.length;
    lightboxImg.src = photos[currentIndex];
  }

  lightboxImg.addEventListener("click", closeLightbox);
  prev.addEventListener("click", showPrev);
  next.addEventListener("click", showNext);

  // Gestion clavier : flèches gauche/droite pour navigation, échap pour fermer
  document.addEventListener("keydown", e => {
    if(lightbox.style.display === "flex"){
      if(e.key === "ArrowLeft") showPrev();
      if(e.key === "ArrowRight") showNext();
      if(e.key === "Escape") closeLightbox();
    }
  });
</script>
