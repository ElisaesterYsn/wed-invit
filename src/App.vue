<script setup>
import TheHero from "./components/TheHero.vue";
import WeddingCountdown from "./components/WeddingCountdown.vue";
import CoupleStory from "./components/CoupleStory.vue";
import EventDetails from "./components/EventDetails.vue";
import PhotoGallery from "./components/PhotoGallery.vue";
import RsvpSection from "./components/RsvpSection.vue";

// ── Wedding config ────────────────────────────────────────────────────────────
const WEDDING_DATE = "30 November 2026";
const WEDDING_DATE_ISO = "2026-11-30"; // used by countdown
const VENUE = "Dewan Suka-Suka";
// ─────────────────────────────────────────────────────────────────────────────
</script>

<template>
  <div class="site-wrapper">
    <!-- 1. Hero + Navigation -->
    <TheHero :weddingDate="WEDDING_DATE" :venue="VENUE" />

    <!-- 2. Live Countdown -->
    <WeddingCountdown :targetDate="WEDDING_DATE_ISO" />

    <!-- 3. Welcome + Couple's Story -->
    <CoupleStory />

    <!-- 4. Event Details -->
    <EventDetails :weddingDate="WEDDING_DATE" :venue="VENUE" />

    <!-- 5. Photo Gallery -->
    <PhotoGallery />

    <!-- 6. RSVP + Share -->
    <RsvpSection :weddingDate="WEDDING_DATE" :venue="VENUE" />

    <!-- 7. Footer -->
    <footer class="site-footer">
      <div class="footer-ornament" aria-hidden="true">✳</div>
      <p class="footer-names">Hermes <span>&amp;</span> Marsha</p>
      <p class="footer-meta">{{ WEDDING_DATE }} · {{ VENUE }}</p>
      <p class="footer-love">MADE WITH LOVE IN SABAH</p>
    </footer>
  </div>
</template>

<style>
/* ── Google Fonts ─────────────────────────────────────────────────────────── */
@import url("https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400;1,500&family=DM+Sans:opsz,wght@9..40,400;9..40,500;9..40,600&display=swap");

/* ── Design tokens ───────────────────────────────────────────────────────── */
:root {
  --cream: #faf7f0;
  --brown: #563a2c;
  --dark: #33251e;
  --gold: #b08a55;
  --muted: #847469;
}

/* ── Base ────────────────────────────────────────────────────────────────── */
html {
  font-family: "DM Sans", sans-serif;
  color: var(--dark);
  background: var(--cream);
  font-size: 16px;
  line-height: 1.6;
  text-rendering: optimizeLegibility;
  scroll-behavior: smooth;
}

/* ── Site wrapper ─────────────────────────────────────────────────────────── */
.site-wrapper {
  min-height: 100svh;
  overflow-x: hidden;
}

/* ── Footer ──────────────────────────────────────────────────────────────── */
.site-footer {
  padding: 50px 20px;
  background: #30231c;
  color: #eee2d1;
  text-align: center;
}

.footer-ornament {
  color: #c6a170;
  font-size: 22px;
  margin-bottom: 12px;
}

.footer-names {
  margin: 0 0 10px;
  font-family: "Cormorant Garamond", serif;
  font-size: 36px;
  font-weight: 400;
}

.footer-names span {
  color: #c6a170;
  font-style: italic;
}

.footer-meta {
  margin: 0 0 22px;
  font-size: 10px;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  opacity: 0.75;
}

.footer-love {
  margin: 0;
  color: #c6a170;
  font-size: 9px;
  letter-spacing: 2px;
}

@media (max-width: 700px) {
  .site-footer {
    padding: 40px 20px;
  }

  .footer-names {
    font-size: 28px;
  }

  .footer-meta {
    font-size: 9px;
    letter-spacing: 1px;
    margin-bottom: 16px;
  }

  .footer-love {
    font-size: 9px;
    letter-spacing: 1.5px;
  }
}
</style>
