<script setup>
const props = defineProps({
  weddingDate: { type: String, required: true },
  venue: { type: String, required: true },
});

const events = [
  {
    icon: "✝",
    eyebrow: "HOLY MATRIMONY",
    title: "09:00 AM",
    sub: "Wedding Ceremony",
  },
  {
    icon: "✳",
    eyebrow: "THE DATE",
    title: props.weddingDate,
    sub: "Monday · 2026",
  },
  {
    icon: "⌖",
    eyebrow: "THE VENUE",
    title: props.venue,
    sub: "Sabah, Malaysia",
  },
  {
    icon: "◈",
    eyebrow: "WEDDING RECEPTION",
    title: "11:00 AM – 3:00 PM",
    sub: "Lunch & Celebration",
  },
];
</script>

<template>
  <section id="details" class="details">
    <div class="shell">
      <p class="eyebrow">SAVE THE DATE</p>
      <h2>Celebrate <em>with us.</em></h2>

      <div class="details-grid">
        <article v-for="ev in events" :key="ev.eyebrow" class="detail-card">
          <span class="detail-icon" aria-hidden="true">{{ ev.icon }}</span>
          <p class="card-eyebrow">{{ ev.eyebrow }}</p>
          <h3>{{ ev.title }}</h3>
          <p class="card-sub">{{ ev.sub }}</p>
        </article>
      </div>

      <p class="details-note">
        Dress code: Smart Casual — Earth tones &amp; cream are warmly welcomed.
      </p>
    </div>
  </section>
</template>

<style scoped>
.details {
  padding-block: 90px;
  background: #f0e8da;
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
  margin: 0 0 45px;
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

.details-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
}

.detail-card {
  padding: 35px 18px;
  border: 1px solid #d9c9b3;
  background: rgba(250 247 240 / 0.7);
  transition:
    transform 0.25s,
    box-shadow 0.25s;
}

.detail-card:hover {
  transform: translateY(-4px);
  box-shadow: 0 8px 24px rgba(51 37 30 / 0.08);
}

.detail-icon {
  display: block;
  margin-bottom: 20px;
  color: #b08a55;
  font-size: 28px;
}

.card-eyebrow {
  margin: 0 0 10px;
  color: #b08a55;
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 2.5px;
}

h3 {
  margin: 0 0 8px;
  font-family: "Cormorant Garamond", serif;
  font-size: 22px;
  font-weight: 500;
  color: #33251e;
  line-height: 1.3;
}

.card-sub {
  color: #847469;
  font-size: 11px;
  letter-spacing: 0.5px;
}

.details-note {
  margin-top: 28px;
  color: #847469;
  font-size: 12px;
}

@media (max-width: 900px) {
  .details-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 14px;
  }
}

@media (max-width: 700px) {
  .details {
    padding-block: 60px;
  }

  .details-grid {
    grid-template-columns: 1fr;
    gap: 10px;
  }

  .detail-card {
    padding: 24px 16px;
  }

  h3 {
    font-size: 19px;
  }
}
</style>
