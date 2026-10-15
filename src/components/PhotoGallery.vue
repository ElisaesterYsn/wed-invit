<script setup>
import { ref } from "vue";

const photos = [
  {
    src: "https://images.unsplash.com/photo-1537633552985-df8429e8048b?auto=format&fit=crop&w=800&q=85",
    alt: "Wedding couple sharing a quiet moment",
  },
  {
    src: "https://images.unsplash.com/photo-1511285560929-80b456fea0bc?auto=format&fit=crop&w=800&q=85",
    alt: "Elegant wedding ceremony details",
  },
  {
    src: "https://images.unsplash.com/photo-1465495976277-4387d4b0e4a6?auto=format&fit=crop&w=800&q=85",
    alt: "Romantic floral wedding arrangement",
  },
  {
    src: "https://images.unsplash.com/photo-1519225421980-715cb0215aed?auto=format&fit=crop&w=800&q=85",
    alt: "Bride and groom walking together",
  },
  {
    src: "https://images.unsplash.com/photo-1478146059778-26028b07395a?auto=format&fit=crop&w=800&q=85",
    alt: "Sunset wedding venue in golden hour",
  },
  {
    src: "https://images.unsplash.com/photo-1522413452208-996ff3f3e740?auto=format&fit=crop&w=800&q=85",
    alt: "Beautiful wedding table setting",
  },
];

const lightboxSrc = ref(null);
const lightboxAlt = ref("");

function openLightbox(photo) {
  lightboxSrc.value = photo.src;
  lightboxAlt.value = photo.alt;
}

function closeLightbox() {
  lightboxSrc.value = null;
}

function handleKeydown(e) {
  if (e.key === "Escape") closeLightbox();
}
</script>

<template>
  <section id="gallery" class="gallery">
    <div class="shell">
      <p class="eyebrow">MOMENTS TO TREASURE</p>
      <h2>Love, in <em>little frames.</em></h2>

      <div class="gallery-grid">
        <button
          v-for="(photo, i) in photos"
          :key="i"
          class="gallery-item"
          type="button"
          :aria-label="`View photo: ${photo.alt}`"
          @click="openLightbox(photo)"
        >
          <img
            :src="photo.src"
            :alt="photo.alt"
            loading="lazy"
            width="400"
            height="533"
          />
          <span class="gallery-hover-overlay" aria-hidden="true">
            <span class="zoom-icon">⊕</span>
          </span>
        </button>
      </div>

      <p class="gallery-note">
        A curated preview — the couple's own photos can be added later.
      </p>
    </div>

    <!-- Lightbox -->
    <Teleport to="body">
      <div
        v-if="lightboxSrc"
        class="lightbox"
        role="dialog"
        aria-modal="true"
        :aria-label="lightboxAlt"
        tabindex="-1"
        @click.self="closeLightbox"
        @keydown="handleKeydown"
      >
        <button
          class="lightbox-close"
          type="button"
          aria-label="Close photo"
          @click="closeLightbox"
        >
          ✕
        </button>
        <img :src="lightboxSrc" :alt="lightboxAlt" class="lightbox-img" />
      </div>
    </Teleport>
  </section>
</template>

<style scoped>
.gallery {
  padding-block: 100px;
  text-align: center;
}

.shell {
  width: min(1080px, 86%);
  margin-inline: auto;
}

.eyebrow {
  margin: 0 0 20px;
  color: #b08a55;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 3px;
}

h2 {
  margin: 0 0 38px;
  font-family: "Cormorant Garamond", serif;
  font-size: clamp(37px, 5vw, 55px);
  font-weight: 400;
  line-height: 1.12;
  color: #33251e;
}

h2 em {
  color: #97704e;
  font-weight: 400;
}

.gallery-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 14px;
}

.gallery-item {
  position: relative;
  padding: 0;
  border: none;
  background: none;
  overflow: hidden;
  cursor: zoom-in;
}

.gallery-item img {
  width: 100%;
  aspect-ratio: 3 / 4;
  object-fit: cover;
  transition: transform 0.4s ease;
  display: block;
}

.gallery-hover-overlay {
  position: absolute;
  inset: 0;
  display: grid;
  place-items: center;
  background: rgba(51 37 30 / 0);
  transition: background 0.3s;
}

.zoom-icon {
  color: white;
  font-size: 32px;
  opacity: 0;
  transform: scale(0.7);
  transition:
    opacity 0.3s,
    transform 0.3s;
}

.gallery-item:hover img {
  transform: scale(1.04);
}

.gallery-item:hover .gallery-hover-overlay {
  background: rgba(51 37 30 / 0.35);
}

.gallery-item:hover .zoom-icon {
  opacity: 1;
  transform: scale(1);
}

.gallery-note {
  margin-top: 22px;
  color: #847469;
  font-size: 11px;
}

/* ── Lightbox ── */
.lightbox {
  position: fixed;
  inset: 0;
  z-index: 100;
  display: grid;
  place-items: center;
  background: rgba(20 14 10 / 0.92);
  padding: 20px;
  animation: lb-in 0.2s ease;
}

@keyframes lb-in {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

.lightbox-close {
  position: absolute;
  top: 16px;
  right: 16px;
  background: none;
  border: 1px solid rgba(255 255 255 / 0.3);
  color: white;
  font-size: 16px;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  display: grid;
  place-items: center;
  transition: background 0.2s;
}

.lightbox-close:hover {
  background: rgba(255 255 255 / 0.1);
}

.lightbox-img {
  max-width: 90vw;
  max-height: 88vh;
  object-fit: contain;
  box-shadow: 0 20px 60px rgba(0 0 0 / 0.6);
}

@media (max-width: 700px) {
  .gallery {
    padding-block: 60px;
  }

  .gallery-grid {
    grid-template-columns: 1fr 1fr;
    gap: 8px;
  }

  .gallery-grid .gallery-item:last-child {
    grid-column: 1 / -1;
  }

  .gallery-grid .gallery-item:last-child img {
    aspect-ratio: 16 / 9;
  }
}
</style>
