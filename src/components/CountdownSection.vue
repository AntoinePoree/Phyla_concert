<template>
  <section class="countdown-section" id="countdown">
    <div class="container">
      <h2 class="section-title">⏳ Le grand jour approche...</h2>
      <p class="section-subtitle">Préparez-vous pour l'événement ultime</p>

      <div class="countdown-wrapper">
        <div v-if="!isAprilFools" class="countdown-grid">
          <div class="countdown-unit" v-for="unit in units" :key="unit.label">
            <div class="countdown-number" :class="{ flip: unit.flipping }">
              {{ String(unit.value).padStart(2, "0") }}
            </div>
            <div class="countdown-label">{{ unit.label }}</div>
          </div>
        </div>

        <div v-else class="april-fools-msg">
          <div class="fish-big">🐟</div>
          <p class="af-text">C'EST LE JOUR J !</p>
          <p class="af-sub">POISSON D'AVRIL ! 🎉</p>
        </div>
      </div>

      <div class="countdown-extras">
        <div class="extra-card">
          <span class="extra-icon">🏟️</span>
          <span class="extra-label">Salle</span>
          <span class="extra-value">Le Salon de Phyla</span>
        </div>
        <div class="extra-card">
          <span class="extra-icon">🎤</span>
          <span class="extra-label">Artiste</span>
          <span class="extra-value">Phyla_Miila</span>
        </div>
        <div class="extra-card">
          <span class="extra-icon">🎟️</span>
          <span class="extra-label">Places</span>
          <span class="extra-value">Illimitées*</span>
        </div>
      </div>

      <p class="disclaimer">
        * Limité à la capacité du canapé (2-3 personnes max)
      </p>
    </div>
  </section>
</template>

<script setup>
import { ref, computed, onMounted, onUnmounted } from "vue";

const TARGET_DATE = new Date("2026-04-30T20:00:00");

const timeLeft = ref(calculateTimeLeft());
const flipping = ref({});
let interval = null;

function calculateTimeLeft() {
  const now = new Date();
  const diff = TARGET_DATE - now;
  if (diff <= 0)
    return { days: 0, hours: 0, minutes: 0, seconds: 0, done: true };
  return {
    days: Math.floor(diff / (1000 * 60 * 60 * 24)),
    hours: Math.floor((diff / (1000 * 60 * 60)) % 24),
    minutes: Math.floor((diff / (1000 * 60)) % 60),
    seconds: Math.floor((diff / 1000) % 60),
    done: false,
  };
}

const isAprilFools = computed(() => timeLeft.value.done);

const units = computed(() => [
  { label: "Jours", value: timeLeft.value.days, flipping: flipping.value.days },
  {
    label: "Heures",
    value: timeLeft.value.hours,
    flipping: flipping.value.hours,
  },
  {
    label: "Minutes",
    value: timeLeft.value.minutes,
    flipping: flipping.value.minutes,
  },
  {
    label: "Secondes",
    value: timeLeft.value.seconds,
    flipping: flipping.value.seconds,
  },
]);

onMounted(() => {
  interval = setInterval(() => {
    const prev = { ...timeLeft.value };
    timeLeft.value = calculateTimeLeft();
    // Déclenche l'animation flip quand une valeur change
    if (prev.seconds !== timeLeft.value.seconds) flipping.value.seconds = true;
    if (prev.minutes !== timeLeft.value.minutes) flipping.value.minutes = true;
    if (prev.hours !== timeLeft.value.hours) flipping.value.hours = true;
    if (prev.days !== timeLeft.value.days) flipping.value.days = true;
    setTimeout(() => {
      flipping.value = {};
    }, 400);
  }, 1000);
});

onUnmounted(() => clearInterval(interval));
</script>

<style scoped>
.countdown-section {
  padding: var(--spacing-section) 0;
  background: linear-gradient(
    180deg,
    var(--color-bg) 0%,
    rgba(26, 26, 46, 0.8) 100%
  );
}

/* ---- Grille countdown ---- */
.countdown-wrapper {
  display: flex;
  justify-content: center;
  margin-bottom: 3rem;
}

.countdown-grid {
  display: flex;
  gap: 1.5rem;
  flex-wrap: wrap;
  justify-content: center;
}

.countdown-unit {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.countdown-number {
  min-width: 100px;
  padding: 1rem 0.5rem;
  background: var(--gradient-card);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  font-size: clamp(2.5rem, 7vw, 4.5rem);
  font-weight: 900;
  text-align: center;
  color: var(--color-text);
  font-variant-numeric: tabular-nums;
  position: relative;
  overflow: hidden;
  box-shadow:
    var(--shadow-card),
    inset 0 1px 0 rgba(255, 255, 255, 0.05);
  transition: box-shadow 0.3s;
}

.countdown-number::before {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(
    180deg,
    rgba(157, 80, 224, 0.08) 0%,
    transparent 50%
  );
  pointer-events: none;
}

.countdown-number.flip {
  animation: flip-number 0.4s ease;
  border-color: var(--color-accent);
  box-shadow:
    var(--shadow-card),
    0 0 15px var(--color-accent-glow);
}

@keyframes flip-number {
  0% {
    transform: scaleY(1);
  }
  40% {
    transform: scaleY(0);
  }
  41% {
    transform: scaleY(0);
  }
  100% {
    transform: scaleY(1);
  }
}

.countdown-label {
  font-size: 0.8rem;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--color-text-muted);
}

/* ---- April Fools message ---- */
.april-fools-msg {
  text-align: center;
  animation: bounce-in 0.8s ease both;
}

.fish-big {
  font-size: 6rem;
  animation: fish-swim 3s ease-in-out infinite;
  display: inline-block;
}

.af-text {
  font-size: clamp(2rem, 6vw, 4rem);
  font-weight: 900;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.af-sub {
  font-size: 1.5rem;
  color: var(--color-accent);
  margin-top: 0.5rem;
}

/* ---- Extras ---- */
.countdown-extras {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 1rem;
}

.extra-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
  background: var(--gradient-card);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  padding: 1rem 1.5rem;
  min-width: 140px;
  transition: all var(--transition-med);
}

.extra-card:hover {
  border-color: var(--color-border-accent);
  transform: translateY(-2px);
}

.extra-icon {
  font-size: 1.8rem;
}

.extra-label {
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--color-text-muted);
}

.extra-value {
  font-size: 0.95rem;
  font-weight: 700;
  color: var(--color-text);
  text-align: center;
}

/* ---- Disclaimer ---- */
.disclaimer {
  text-align: center;
  font-size: 0.75rem;
  color: var(--color-text-muted);
  opacity: 0.6;
  font-style: italic;
}

/* ---- Responsive ---- */
@media (max-width: 480px) {
  .countdown-grid {
    gap: 0.75rem;
  }

  .countdown-number {
    min-width: 70px;
    padding: 0.75rem 0.25rem;
  }
}
</style>
