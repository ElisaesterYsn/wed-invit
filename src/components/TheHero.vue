<script setup>
import { ref } from "vue";

const props = defineProps({
  weddingDate: { type: String, required: true },
  venue: { type: String, required: true },
});

const menuOpen = ref(false);

const navLinks = [
  { label: "Home", href: "#home" },
  { label: "Our Story", href: "#story" },
  { label: "Event Details", href: "#details" },
  { label: "Gallery", href: "#gallery" },
  { label: "RSVP", href: "#rsvp" },
];
</script>

<template>
  <!-- ── Navigation ── -->
  <header class="site-header">
    <a class="brand" href="#home" aria-label="Hermes and Marsha home">
      H <span>♥</span> M
    </a>

    <button
      class="menu-toggle"
      type="button"
      :aria-expanded="String(menuOpen)"
      aria-controls="main-nav"
      aria-label="Toggle navigation"
      @click="menuOpen = !menuOpen"
    >
      <span class="toggle-icon" :class="{ open: menuOpen }">
        <span /><span /><span />
      </span>
    </button>

    <nav
      id="main-nav"
      :class="{ 'nav-open': menuOpen }"
      aria-label="Main navigation"
    >
      <a
        v-for="link in navLinks"
        :key="link.href"
        :href="link.href"
        @click="menuOpen = false"
      >
        {{ link.label }}
      </a>
    </nav>
  </header>

  <!-- ── Hero ── -->
  <section id="home" class="hero">
    <div class="hero-overlay" aria-hidden="true" />

    <div class="hero-content">
      <p class="eyebrow">A CELEBRATION OF LOVE</p>
      <p class="intro">Together with our families</p>

      <h1>
        Hermes
        <span class="ampersand">&amp;</span>
        Marsha
      </h1>

      <div class="ornament" aria-hidden="true"><span>✦</span></div>

      <p class="hero-message">
        Two hearts, one beautiful journey.<br />
        We invite you to celebrate our beginning.
      </p>

      <div class="event-summary">
        <span>{{ weddingDate }}</span>
        <i aria-hidden="true" />
        <span>{{ venue }}</span>
      </div>

      <a class="primary-button" href="#details">
        Join Our Celebration <span aria-hidden="true">↗</span>
      </a>
    </div>

    <p class="hero-caption">SABAH, MALAYSIA · WITH LOVE</p>

    <a class="scroll-cue" href="#story" aria-label="Scroll to our story">↓</a>
  </section>
</template>

<style scoped>
/* ── Header ── */
.site-header {
  position: absolute;
  inset: 0 0 auto;
  z-index: 5;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 24px;
  padding: 25px 7%;
  color: white;
}

.brand {
  font-family: "Cormorant Garamond", serif;
  font-size: 27px;
  letter-spacing: 2px;
  white-space: nowrap;
}

.brand span {
  color: #e5bd85;
  font-size: 13px;
}

.site-header nav {
  display: flex;
  align-items: center;
  gap: clamp(14px, 2.8vw, 35px);
}

.site-header nav a {
  font-size: 11px;
  letter-spacing: 1px;
  transition: color 0.2s;
}

.site-header nav a:hover {
  color: #e8c89c;
}

/* ── Hamburger button ── */
.menu-toggle {
  display: none;
  background: transparent;
  border: 1px solid rgba(255 255 255 / 0.5);
  border-radius: 3px;
  padding: 8px 10px;
  color: white;
  line-height: 1;
}

.toggle-icon {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 4px;
  width: 18px;
  height: 14px;
}

.toggle-icon span {
  display: block;
  height: 1.5px;
  background: currentColor;
  transition:
    transform 0.25s,
    opacity 0.25s;
  transform-origin: center;
}

.toggle-icon.open span:nth-child(1) {
  transform: translateY(5.5px) rotate(45deg);
}
.toggle-icon.open span:nth-child(2) {
  opacity: 0;
}
.toggle-icon.open span:nth-child(3) {
  transform: translateY(-5.5px) rotate(-45deg);
}

/* ── Hero ── */
.hero {
  position: relative;
  display: grid;
  min-height: max(650px, 100svh);
  place-items: center;
  overflow: hidden;
  color: #fffaf1;
  text-align: center;
  background:
    linear-gradient(180deg, #251c1640 0%, #33251e28 40%, #211b18a8 100%),
    url("https://images.unsplash.com/photo-1500530855697-b586d89ba3ee?auto=format&fit=crop&w=2200&q=90")
      center 54% / cover no-repeat;
}

@media (min-width: 1400px) {
  .hero {
    min-height: 850px;
  }
}

.hero-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, #241b1733, transparent 55%, #241b1715);
}

.hero-content {
  position: relative;
  z-index: 1;
  width: min(800px, 90%);
  padding: 100px 0 65px;
}

.eyebrow {
  margin: 0 0 20px;
  color: #f2d7ac;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 3px;
  line-height: 1.8;
}

.intro {
  margin: 0 0 15px;
  font-family: "Cormorant Garamond", serif;
  font-size: 18px;
  letter-spacing: 1px;
}

h1 {
  font-family: "Cormorant Garamond", serif;
  font-size: clamp(70px, 12vw, 124px);
  font-weight: 400;
  letter-spacing: -4px;
  line-height: 0.82;
}

.ampersand {
  display: block;
  margin: 15px 0;
  font-size: 0.44em;
  font-style: italic;
  letter-spacing: 0;
}

.ornament {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  margin: 26px auto;
  color: #e5bd85;
}

.ornament::before,
.ornament::after {
  content: "";
  width: 65px;
  height: 1px;
  background: currentColor;
}

.hero-message {
  font-family: "Cormorant Garamond", serif;
  font-size: 19px;
  line-height: 1.8;
}

.event-summary {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
  margin: 28px 0;
  font-size: 10px;
  letter-spacing: 2px;
  text-transform: uppercase;
}

.event-summary i {
  width: 4px;
  height: 4px;
  border-radius: 50%;
  background: #e8c89c;
}

.primary-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  min-height: 49px;
  padding: 0 24px;
  border: 1px solid #c5a274;
  background: #4b3326;
  color: #fffaf1;
  font-size: 10px;
  letter-spacing: 1.5px;
  transition:
    background 0.2s,
    transform 0.2s;
}

.primary-button:hover {
  background: #6c4a35;
  transform: translateY(-2px);
}

.hero-caption {
  position: absolute;
  bottom: 18px;
  left: 7%;
  margin: 0;
  font-size: 9px;
  letter-spacing: 2px;
  opacity: 0.8;
}

.scroll-cue {
  position: absolute;
  right: 7%;
  bottom: 16px;
  display: grid;
  width: 33px;
  height: 33px;
  place-items: center;
  border: 1px solid rgba(255 255 255 / 0.5);
  border-radius: 50%;
}

/* ── Mobile ── */
@media (max-width: 700px) {
  .site-header {
    padding: 20px 6%;
  }

  .menu-toggle {
    display: flex;
    align-items: center;
    justify-content: center;
    min-width: 44px;
    min-height: 44px;
    padding: 0 12px;
  }

  .site-header nav {
    position: absolute;
    top: 68px;
    right: 6%;
    left: 6%;
    display: none;
    flex-direction: column;
    align-items: stretch;
    gap: 0;
    padding: 6px 20px;
    border: 1px solid rgba(255 255 255 / 0.2);
    background: rgba(48 35 28 / 0.97);
  }

  .site-header nav.nav-open {
    display: flex;
  }

  .site-header nav a {
    padding: 14px 0;
    min-height: 44px;
    display: flex;
    align-items: center;
    border-bottom: 1px solid rgba(255 255 255 / 0.12);
    font-size: 12px;
    letter-spacing: 1.2px;
  }

  .site-header nav a:last-child {
    border-bottom: none;
  }

  .hero {
    min-height: 100svh;
    background-position: 55% center;
  }

  .hero-content {
    width: 90%;
    padding: 80px 0 50px;
  }

  h1 {
    font-size: clamp(60px, 16vw, 96px);
    letter-spacing: -2px;
  }

  .hero-message {
    font-size: 16px;
  }

  .intro {
    font-size: 15px;
  }

  .event-summary {
    flex-direction: column;
    gap: 8px;
    font-size: 9px;
    letter-spacing: 1.5px;
  }

  .event-summary i {
    display: none;
  }

  .hero-caption {
    right: 0;
    left: 0;
    text-align: center;
    font-size: 8px;
    letter-spacing: 1.5px;
  }

  .scroll-cue {
    right: 6%;
    width: 44px;
    height: 44px;
  }
}
</style>
