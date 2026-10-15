<script setup>
// No props — copy is fixed; can be made data-driven later
</script>

<template>
  <!-- ── Welcome ── -->
  <section class="welcome">
    <div class="shell">
      <p class="eyebrow">A HOLY MATRIMONY</p>
      <h2>A new chapter, <em>a lifetime of love.</em></h2>
      <p class="body-copy">
        With grateful hearts, we would be delighted to have you with us as we
        begin this wonderful chapter together.
      </p>
      <div class="ornament" aria-hidden="true"><span>❋</span></div>
    </div>
  </section>

  <!-- ── Story ── -->
  <section id="story" class="story">
    <div class="story-inner shell">
      <figure class="story-figure">
        <img
          src="https://images.unsplash.com/photo-1519741497674-611481863552?auto=format&fit=crop&w=1200&q=85"
          alt="Romantic wedding celebration"
          width="600"
          height="750"
          loading="lazy"
        />
        <figcaption>A little story, a lifetime together</figcaption>
      </figure>

      <div class="story-copy">
        <p class="eyebrow">OUR STORY</p>
        <h2>And so, <em>our forever begins.</em></h2>
        <p class="body-copy">
          Every beautiful journey starts with a moment. Today, we look forward
          to creating new memories, sharing laughter, and building a life filled
          with love — right here in the beautiful land of Sabah.
        </p>
        <p class="signature">With love, Hermes &amp; Marsha</p>
      </div>
    </div>
  </section>
</template>

<style scoped>
.shell {
  width: min(1080px, 86%);
  margin-inline: auto;
}

/* ── Welcome ── */
.welcome {
  padding-block: 95px 85px;
  text-align: center;
}

.eyebrow {
  margin: 0 0 20px;
  color: #b08a55;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 3px;
  line-height: 1.8;
}

h2 {
  margin: 0 0 20px;
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

.body-copy {
  max-width: 500px;
  margin: 0 auto;
  color: #847469;
  font-size: 13px;
  line-height: 2;
}

.ornament {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  margin: 30px auto 0;
  color: #b08a55;
}

.ornament::before,
.ornament::after {
  content: "";
  width: 65px;
  height: 1px;
  background: currentColor;
}

/* ── Story ── */
.story {
  padding-block: 30px 105px;
}

.story-inner {
  display: grid;
  grid-template-columns: 1fr 0.9fr;
  align-items: center;
  gap: 8%;
}

.story-figure {
  position: relative;
  margin: 0;
}

.story-figure img {
  width: 100%;
  aspect-ratio: 4 / 5;
  object-fit: cover;
}

.story-figure figcaption {
  position: absolute;
  right: -18px;
  bottom: 20px;
  padding: 15px 20px;
  background: #faf7f0;
  color: #563a2c;
  font-family: "Cormorant Garamond", serif;
  font-size: 17px;
  font-style: italic;
}

.story-copy .body-copy {
  max-width: none;
  line-height: 2.1;
}

.signature {
  margin-top: 32px;
  color: #563a2c;
  font-family: "Cormorant Garamond", serif;
  font-size: 22px;
  font-style: italic;
}

@media (max-width: 700px) {
  .welcome {
    padding-block: 60px 50px;
  }

  .story {
    padding-bottom: 60px;
  }

  .story-inner {
    grid-template-columns: 1fr;
    gap: 32px;
  }

  .story-figure img {
    aspect-ratio: 5 / 4;
  }

  .story-figure figcaption {
    position: static;
    display: block;
    margin-top: -1px;
    text-align: right;
    font-size: 14px;
    padding: 10px 14px;
  }
}
</style>
