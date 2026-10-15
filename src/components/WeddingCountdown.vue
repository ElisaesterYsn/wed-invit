<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

const props = defineProps({
  targetDate: { type: String, required: true }, // ISO date string, e.g. "2026-11-30"
});

// Target: ceremony starts 09:00 local time (Malaysia, UTC+8)
const target = new Date(`${props.targetDate}T09:00:00+08:00`);

function calcTimeLeft() {
  const diff = target - Date.now();
  if (diff <= 0) return { days: 0, hours: 0, minutes: 0, seconds: 0 };
  return {
    days: Math.floor(diff / 86_400_000),
    hours: Math.floor((diff % 86_400_000) / 3_600_000),
    minutes: Math.floor((diff % 3_600_000) / 60_000),
    seconds: Math.floor((diff % 60_000) / 1_000),
  };
}

const timeLeft = ref(calcTimeLeft());
const hasPassed = computed(() => target - Date.now() <= 0);

let timer;
onMounted(() => {
  timer = setInterval(() => {
    timeLeft.value = calcTimeLeft();
  }, 1000);
});
onUnmounted(() => clearInterval(timer));

function pad(n) {
  return String(n).padStart(2, "0");
}

const units = computed(() => [
  { value: timeLeft.value.days, label: "Days" },
  { value: timeLeft.value.hours, label: "Hours" },
  { value: timeLeft.value.minutes, label: "Minutes" },
  { value: timeLeft.value.seconds, label: "Seconds" },
]);
</script>

<template>
  <section class="countdown-section" aria-label="Wedding countdown timer">
    <p class="eyebrow">COUNTING DOWN</p>
    <h2>The big day <em>is almost here.</em></h2>

    <div
      v-if="!hasPassed"
      class="countdown-grid"
      role="timer"
      aria-live="polite"
      aria-atomic="true"
    >
      <div v-for="unit in units" :key="unit.label" class="tick-box">
        <span class="tick-number" :aria-label="`${unit.value} ${unit.label}`">
          {{ pad(unit.value) }}
        </span>
        <span class="tick-label">{{ unit.label }}</span>
      </div>
    </div>

    <p v-else class="celebration-msg">
      Today is the day! <em>Congratulations, Hermes &amp; Marsha! 🎉</em>
    </p>

    <div class="ornament" aria-hidden="true"><span>❋</span></div>
  </section>
</template>

<style scoped>
.countdown-section {
  padding-block: 65px 70px;
  background: #30231c;
  color: #fffaf1;
  text-align: center;
}

.eyebrow {
  margin: 0 0 18px;
  color: #c6a170;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 3px;
}

h2 {
  margin: 0 0 36px;
  font-family: "Cormorant Garamond", serif;
  font-size: clamp(28px, 5vw, 50px);
  font-weight: 400;
  line-height: 1.12;
}

h2 em {
  color: #c6a170;
  font-weight: 400;
}

.countdown-grid {
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  gap: clamp(16px, 3vw, 40px);
}

.tick-box {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 10px;
  min-width: 90px;
}

.tick-number {
  display: block;
  width: 90px;
  padding-block: 16px;
  border: 1px solid #c6a17050;
  font-family: "Cormorant Garamond", serif;
  font-size: clamp(44px, 6vw, 64px);
  font-weight: 400;
  line-height: 1;
  letter-spacing: -1px;
  background: rgba(255 255 255 / 0.04);
}

.tick-label {
  font-size: 9px;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: #c6a170;
}

.celebration-msg {
  font-family: "Cormorant Garamond", serif;
  font-size: 26px;
  line-height: 1.5;
}

.celebration-msg em {
  display: block;
  font-size: 0.8em;
  color: #c6a170;
}

.ornament {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
  margin: 40px auto 0;
  color: #c6a170;
}

.ornament::before,
.ornament::after {
  content: "";
  width: 65px;
  height: 1px;
  background: currentColor;
}

@media (max-width: 480px) {
  .countdown-section {
    padding-block: 50px 55px;
  }

  .countdown-grid {
    gap: 10px;
  }

  .tick-box {
    min-width: 68px;
  }

  .tick-number {
    width: 68px;
    padding-block: 10px;
  }

  .tick-label {
    letter-spacing: 1px;
  }

  h2 {
    margin-bottom: 28px;
  }
}
</style>
