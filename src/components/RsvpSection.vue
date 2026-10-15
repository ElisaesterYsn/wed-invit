<script setup>
import { ref, reactive } from "vue";

const props = defineProps({
  weddingDate: { type: String, required: true },
  venue: { type: String, required: true },
});

// SheetDB API URL — set VITE_SHEETDB_URL in .env.local (dev) and in Vercel
// environment variables (production). Never hardcode the real URL here.
const SHEETDB_URL = import.meta.env.VITE_SHEETDB_URL;

// ── Form state ──────────────────────────────────────────────────────────────
const form = reactive({
  name: "",
  attendance: "yes",
  guests: 1,
  message: "",
});

const errors = reactive({ name: "", guests: "" });
const status = ref("idle"); // 'idle' | 'submitting' | 'success' | 'error'
const errorMsg = ref("");

// ── Validation ──────────────────────────────────────────────────────────────
function validate() {
  errors.name = form.name.trim() ? "" : "Please enter your full name.";
  errors.guests =
    form.attendance === "yes" && (form.guests < 1 || form.guests > 20)
      ? "Please enter a number between 1 and 20."
      : "";
  return !errors.name && !errors.guests;
}

// ── Submit → SheetDB ────────────────────────────────────────────────────────
async function submitRsvp() {
  if (!validate()) return;

  status.value = "submitting";
  errorMsg.value = "";

  const payload = {
    data: {
      Name: form.name.trim(),
      Attendance: form.attendance === "yes" ? "Attending" : "Not Attending",
      Guests: form.attendance === "yes" ? form.guests : 0,
      Message: form.message.trim(),
      Timestamp: new Date().toISOString(),
    },
  };

  try {
    const res = await fetch(SHEETDB_URL, {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
        Accept: "application/json",
      },
      body: JSON.stringify(payload),
    });

    if (!res.ok) throw new Error(`Server responded with ${res.status}`);

    status.value = "success";
  } catch (err) {
    console.error("RSVP submission failed:", err);
    errorMsg.value =
      "Something went wrong — please try again or contact us directly.";
    status.value = "error";
  }
}

function resetForm() {
  form.name = "";
  form.attendance = "yes";
  form.guests = 1;
  form.message = "";
  status.value = "idle";
  errorMsg.value = "";
}
</script>

<template>
  <section id="rsvp" class="rsvp">
    <!-- ── Heading ── -->
    <div class="rsvp-header">
      <p class="eyebrow">YOU ARE PART OF OUR STORY</p>
      <h2>It wouldn't be the same <em>without you.</em></h2>
      <p class="lead">
        Kindly let us know if you'll be joining us. Your response helps us
        prepare the warmest welcome for you.
      </p>

      <p class="privacy-note">
        🔒 This invitation was sent to you personally. Please keep this link
        private and do not share it with others — thank you for understanding.
      </p>
    </div>

    <!-- ── Success state ── -->
    <div
      v-if="status === 'success'"
      class="rsvp-card rsvp-success"
      role="status"
    >
      <!-- Attending -->
      <template v-if="form.attendance === 'yes'">
        <div class="confirm-icon" aria-hidden="true">✦</div>
        <h3>We'll see you there, {{ form.name }}!</h3>
        <p>
          Your RSVP has been received — we are so glad you can make it. Get
          ready for a celebration filled with joy, love, and good food.
        </p>
        <p class="confirm-sub">
          📅 {{ weddingDate }} &nbsp;·&nbsp; 📍 {{ venue }}
        </p>
      </template>

      <!-- Not attending -->
      <template v-else>
        <div class="confirm-icon regret" aria-hidden="true">✿</div>
        <h3>We'll miss you, {{ form.name }}.</h3>
        <p>
          We completely understand and are grateful you took the time to let us
          know. You will be in our hearts and thoughts on the day.
        </p>
        <p class="confirm-sub">
          May our paths cross again soon. With love, Hermes &amp; Marsha.
        </p>
      </template>

      <button class="secondary-button" type="button" @click="resetForm">
        Submit another response
      </button>
    </div>

    <!-- ── Form ── -->
    <form
      v-else
      class="rsvp-card rsvp-form"
      novalidate
      @submit.prevent="submitRsvp"
    >
      <!-- Name -->
      <div class="field-group" :class="{ 'has-error': errors.name }">
        <label for="rsvp-name">
          Full Name <span class="required" aria-hidden="true">*</span>
        </label>
        <input
          id="rsvp-name"
          v-model.trim="form.name"
          type="text"
          autocomplete="name"
          placeholder="e.g. Ahmad bin Abdullah"
          :aria-describedby="errors.name ? 'rsvp-name-error' : undefined"
          :aria-invalid="!!errors.name"
          required
        />
        <span
          v-if="errors.name"
          id="rsvp-name-error"
          class="field-error"
          role="alert"
          >{{ errors.name }}</span
        >
      </div>

      <!-- Attendance -->
      <fieldset class="field-group">
        <legend>Will you be attending?</legend>
        <div class="radio-row">
          <label class="radio-option">
            <input
              v-model="form.attendance"
              type="radio"
              name="attendance"
              value="yes"
            />
            <span>Joyfully accept ✓</span>
          </label>
          <label class="radio-option">
            <input
              v-model="form.attendance"
              type="radio"
              name="attendance"
              value="no"
            />
            <span>Regretfully decline</span>
          </label>
        </div>
      </fieldset>

      <!-- Guest count -->
      <div
        v-if="form.attendance === 'yes'"
        class="field-group"
        :class="{ 'has-error': errors.guests }"
      >
        <label for="rsvp-guests">
          Number of guests
          <span class="label-hint">(including yourself)</span>
        </label>
        <input
          id="rsvp-guests"
          v-model.number="form.guests"
          type="number"
          min="1"
          max="20"
          :aria-describedby="errors.guests ? 'rsvp-guests-error' : undefined"
          :aria-invalid="!!errors.guests"
        />
        <span
          v-if="errors.guests"
          id="rsvp-guests-error"
          class="field-error"
          role="alert"
          >{{ errors.guests }}</span
        >
      </div>

      <!-- Message -->
      <div class="field-group">
        <label for="rsvp-message">
          Leave a message
          <span class="label-hint">(optional)</span>
        </label>
        <textarea
          id="rsvp-message"
          v-model="form.message"
          rows="3"
          placeholder="Congratulations! Can't wait to celebrate with you both."
        />
      </div>

      <!-- API not configured warning (dev only) -->
      <div
        v-if="!SHEETDB_URL || SHEETDB_URL.includes('REPLACE_WITH')"
        class="setup-notice"
        role="note"
      >
        ⚠ SheetDB URL not configured yet. Set <code>VITE_SHEETDB_URL</code> in
        <code>.env.local</code> to enable submissions.
      </div>

      <!-- Submission error -->
      <div v-if="status === 'error'" class="submit-error" role="alert">
        {{ errorMsg }}
      </div>

      <button
        class="primary-button"
        type="submit"
        :disabled="status === 'submitting'"
        :aria-busy="status === 'submitting'"
      >
        <span
          v-if="status === 'submitting'"
          class="spinner"
          aria-hidden="true"
        />
        {{ status === "submitting" ? "Sending…" : "Confirm RSVP" }}
      </button>
    </form>
  </section>
</template>

<style scoped>
.rsvp {
  padding-block: 90px;
  background: #e9dece;
  text-align: center;
}

/* ── Heading ── */
.rsvp-header {
  width: min(600px, 86%);
  margin: 0 auto 48px;
}

.eyebrow {
  margin: 0 0 18px;
  color: #b08a55;
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 3px;
}

h2 {
  margin: 0 0 20px;
  font-family: "Cormorant Garamond", serif;
  font-size: clamp(34px, 5vw, 52px);
  font-weight: 400;
  line-height: 1.12;
  color: #33251e;
}

h2 em {
  color: #97704e;
  font-weight: 400;
}

.lead {
  color: #847469;
  font-size: 13px;
  line-height: 2;
  margin: 0;
}

.privacy-note {
  margin: 22px auto 0;
  max-width: 420px;
  padding: 12px 18px;
  border: 1px solid #d9c9b3;
  background: rgba(250 247 240 / 0.8);
  color: #847469;
  font-size: 11px;
  line-height: 1.8;
  letter-spacing: 0.2px;
}

/* ── Shared card shell ── */
.rsvp-card {
  width: min(520px, 86%);
  margin: 0 auto;
  background: #faf7f0;
  border: 1px solid #d9c9b3;
  padding: 48px 44px;
}

/* ── Form layout ── */
.rsvp-form {
  text-align: left;
  display: flex;
  flex-direction: column;
  gap: 26px;
}

.field-group {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.field-group > label,
.field-group > legend {
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: #563a2c;
}

fieldset {
  border: none;
  padding: 0;
  margin: 0;
}

.required {
  color: #b08a55;
}

.label-hint {
  font-weight: 400;
  text-transform: none;
  letter-spacing: 0;
  color: #847469;
  font-size: 11px;
}

input[type="text"],
input[type="number"],
textarea {
  width: 100%;
  padding: 13px 14px;
  border: 1px solid #c9b89f;
  background: white;
  color: #33251e;
  font: inherit;
  font-size: 15px;
  outline: none;
  transition:
    border-color 0.2s,
    box-shadow 0.2s;
  resize: vertical;
  border-radius: 0;
}

input[type="text"]:focus,
input[type="number"]:focus,
textarea:focus {
  border-color: #b08a55;
  box-shadow: 0 0 0 3px rgba(176 138 85 / 0.15);
}

input[type="number"] {
  max-width: 110px;
}

.has-error input,
.has-error textarea {
  border-color: #c0392b;
}

.field-error {
  font-size: 12px;
  color: #c0392b;
}

/* ── Radio ── */
.radio-row {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-top: 4px;
}

.radio-option {
  display: flex;
  align-items: center;
  gap: 9px;
  padding: 13px 18px;
  min-height: 44px;
  border: 1px solid #c9b89f;
  background: white;
  cursor: pointer;
  font-size: 13px;
  font-weight: 400;
  text-transform: none;
  letter-spacing: 0;
  color: #33251e;
  transition:
    border-color 0.2s,
    background 0.2s;
  user-select: none;
}

.radio-option:has(input:checked) {
  border-color: #b08a55;
  background: rgba(176 138 85 / 0.08);
}

.radio-option input {
  accent-color: #b08a55;
  flex-shrink: 0;
}

/* ── Notices ── */
.setup-notice {
  padding: 12px 16px;
  background: #fff8e1;
  border: 1px solid #ffe082;
  font-size: 12px;
  color: #6d4c00;
  text-align: left;
  line-height: 1.7;
}

.setup-notice code {
  font-family: ui-monospace, Consolas, monospace;
  font-size: 11px;
  background: rgba(0 0 0 / 0.06);
  padding: 1px 5px;
  border-radius: 3px;
}

.submit-error {
  padding: 12px 16px;
  background: #fff0f0;
  border: 1px solid #f5c6c6;
  font-size: 12px;
  color: #c0392b;
  text-align: left;
  line-height: 1.7;
}

/* ── Buttons ── */
.primary-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 10px;
  min-height: 52px;
  padding: 0 32px;
  border: 1px solid #4b3326;
  background: #4b3326;
  color: #fffaf1;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  cursor: pointer;
  transition:
    background 0.2s,
    transform 0.2s,
    opacity 0.2s;
  align-self: center;
  margin-top: 4px;
}

.primary-button:hover:not(:disabled) {
  background: #6c4a35;
  transform: translateY(-2px);
}

.primary-button:disabled {
  opacity: 0.65;
  cursor: not-allowed;
}

/* Loading spinner */
.spinner {
  display: inline-block;
  width: 14px;
  height: 14px;
  border: 2px solid rgba(255 255 255 / 0.35);
  border-top-color: white;
  border-radius: 50%;
  animation: spin 0.7s linear infinite;
}

@keyframes spin {
  to {
    transform: rotate(360deg);
  }
}

.secondary-button {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
  padding: 0 26px;
  min-height: 44px;
  border: 1px solid #b08a55;
  background: transparent;
  color: #563a2c;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 1px;
  text-transform: uppercase;
  cursor: pointer;
  transition: background 0.2s;
}

.secondary-button:hover {
  background: rgba(176 138 85 / 0.12);
}

/* ── Success card ── */
.rsvp-success {
  text-align: center;
  padding: 52px 40px;
}

.confirm-icon {
  font-size: 30px;
  color: #b08a55;
  margin-bottom: 18px;
}

.rsvp-success h3 {
  margin: 0 0 14px;
  font-family: "Cormorant Garamond", serif;
  font-size: 32px;
  font-weight: 400;
  color: #33251e;
}

.rsvp-success p {
  color: #847469;
  font-size: 13px;
  line-height: 1.9;
  margin: 0;
}

.confirm-sub {
  margin-top: 14px !important;
  font-size: 11px !important;
  letter-spacing: 0.5px;
  color: #b08a55 !important;
}

.confirm-icon.regret {
  color: #97704e;
  font-size: 26px;
}

/* ── Mobile ── */
@media (max-width: 700px) {
  .rsvp {
    padding-block: 60px;
  }

  .rsvp-header {
    margin-bottom: 32px;
  }

  .rsvp-card {
    padding: 32px 20px;
  }

  .rsvp-success {
    padding: 36px 20px;
  }

  .radio-row {
    flex-direction: column;
  }

  .radio-option {
    width: 100%;
  }
}
</style>
