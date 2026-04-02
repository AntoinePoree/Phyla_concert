<template>
  <section class="hero">
    <!-- Étoiles animées en arrière-plan -->
    <div class="stars-container" aria-hidden="true">
      <span
        v-for="star in stars"
        :key="star.id"
        class="star"
        :style="{
          left: star.x + '%',
          top: star.y + '%',
          width: star.size + 'px',
          height: star.size + 'px',
          animationDelay: star.delay + 's',
          animationDuration: star.duration + 's',
        }"
        >✦</span
      >
    </div>

    <!-- Orbes lumineuses -->
    <div class="orb orb-1" aria-hidden="true"></div>
    <div class="orb orb-2" aria-hidden="true"></div>

    <!-- Contenu principal -->
    <div class="container hero-content">
      <div class="badge-live">
        <span class="live-dot"></span>
        ÉVÉNEMENT EXCLUSIF
      </div>

      <h1 class="hero-title">
        <span class="title-line-1">Le Concert de</span>
        <span class="title-name">Phyla_Miila</span>
      </h1>

      <div class="hero-date">
        <span class="date-icon">📅</span>
        <span class="date-text">1 Avril 2026</span>
      </div>

      <p class="hero-subtitle">🎵 L'événement de l'année 🎵</p>

      <p class="hero-tagline">
        Une nuit inoubliable, une voix légendaire,<br />
        un moment dans l'histoire du streaming français.
      </p>

      <div class="hero-cta">
        <a href="#vip" class="btn btn-primary hero-btn">
          🎟️ Obtenir mon pass VIP
        </a>
        <a href="#setlist" class="btn btn-outline hero-btn">
          🎵 Voir la setlist
        </a>
      </div>

      <div class="hero-scroll-hint" aria-hidden="true">
        <span>↓</span>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";

// Génération des étoiles/étincelles
const stars = ref(
  Array.from({ length: 60 }, (_, i) => ({
    id: i,
    x: Math.random() * 100,
    y: Math.random() * 100,
    size: Math.random() * 14 + 6,
    delay: Math.random() * 6,
    duration: Math.random() * 4 + 3,
  })),
);
</script>

<style scoped>
/* ---- Hero wrapper ---- */
.hero {
  position: relative;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  overflow: hidden;
  background: var(--gradient-hero);
  padding: 6rem 0 4rem;
}

/* ---- Orbes lumineuses ---- */
.orb {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  pointer-events: none;
  animation: pulse-glow 4s ease-in-out infinite;
}

.orb-1 {
  width: 500px;
  height: 500px;
  background: radial-gradient(circle, rgba(123, 47, 190, 0.5), transparent 70%);
  top: -150px;
  left: -100px;
  animation-delay: 0s;
}

.orb-2 {
  width: 400px;
  height: 400px;
  background: radial-gradient(
    circle,
    rgba(224, 64, 251, 0.35),
    transparent 70%
  );
  bottom: -100px;
  right: -80px;
  animation-delay: 2s;
}

/* ---- Étoiles ---- */
.stars-container {
  position: absolute;
  inset: 0;
  pointer-events: none;
  z-index: 0;
}

.star {
  position: absolute;
  color: var(--color-accent);
  font-size: 1em;
  animation: float linear infinite;
  opacity: 0.7;
  text-shadow: 0 0 8px var(--color-accent);
  user-select: none;
}

/* ---- Contenu ---- */
.hero-content {
  position: relative;
  z-index: 1;
  text-align: center;
  animation: fade-in-up 0.9s ease both;
}

/* ---- Badge ---- */
.badge-live {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: rgba(224, 64, 251, 0.15);
  border: 1px solid var(--color-accent);
  border-radius: 50px;
  padding: 0.4rem 1.2rem;
  font-size: 0.8rem;
  font-weight: 700;
  letter-spacing: 0.12em;
  color: var(--color-accent);
  margin-bottom: 2rem;
  text-transform: uppercase;
}

.live-dot {
  display: inline-block;
  width: 8px;
  height: 8px;
  background: var(--color-accent);
  border-radius: 50%;
  animation: pulse-dot 1.4s ease-in-out infinite;
}

@keyframes pulse-dot {
  0%,
  100% {
    opacity: 1;
    transform: scale(1);
  }
  50% {
    opacity: 0.3;
    transform: scale(0.6);
  }
}

/* ---- Titre ---- */
.hero-title {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
  margin-bottom: 1.5rem;
  line-height: 1.1;
}

.title-line-1 {
  font-size: clamp(1.5rem, 4vw, 2.4rem);
  color: var(--color-text-muted);
  font-weight: 400;
  letter-spacing: 0.08em;
  text-transform: uppercase;
}

.title-name {
  font-size: clamp(3rem, 10vw, 7rem);
  font-weight: 900;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  background-size: 200% auto;
  animation: shimmer 3s linear infinite;
  text-shadow: none;
  letter-spacing: -0.02em;
  line-height: 1;
}

/* ---- Date ---- */
.hero-date {
  display: inline-flex;
  align-items: center;
  gap: 0.75rem;
  background: rgba(123, 47, 190, 0.2);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  padding: 0.75rem 1.75rem;
  margin-bottom: 1.5rem;
  animation: pulse-glow 3s ease-in-out infinite;
}

.date-icon {
  font-size: 1.4rem;
}

.date-text {
  font-size: clamp(1.2rem, 3vw, 1.8rem);
  font-weight: 800;
  color: var(--color-text);
  letter-spacing: 0.04em;
}

.date-text sup {
  font-size: 0.6em;
  vertical-align: super;
}

/* ---- Sous-titres ---- */
.hero-subtitle {
  font-size: clamp(1.2rem, 3vw, 1.8rem);
  font-weight: 700;
  color: var(--color-accent);
  margin-bottom: 1rem;
  text-shadow: 0 0 20px var(--color-accent-glow);
}

.hero-tagline {
  font-size: 1.05rem;
  color: var(--color-text-muted);
  max-width: 520px;
  margin: 0 auto 2.5rem;
  line-height: 1.7;
}

/* ---- CTA ---- */
.hero-cta {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 3rem;
}

.hero-btn {
  font-size: 1rem;
}

.btn-outline {
  background: transparent;
  color: var(--color-primary-light);
  border: 2px solid var(--color-primary-light);
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.85rem 2rem;
  border-radius: 50px;
  font-size: 1rem;
  font-weight: 700;
  cursor: pointer;
  transition: all var(--transition-med);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  text-decoration: none;
}

.btn-outline:hover {
  background: rgba(157, 80, 224, 0.15);
  border-color: var(--color-accent);
  color: var(--color-accent);
  transform: translateY(-2px);
}

/* ---- Scroll hint ---- */
.hero-scroll-hint {
  font-size: 1.5rem;
  color: var(--color-text-muted);
  animation: bounce-scroll 2s ease-in-out infinite;
}

@keyframes bounce-scroll {
  0%,
  100% {
    transform: translateY(0);
    opacity: 0.6;
  }
  50% {
    transform: translateY(8px);
    opacity: 1;
  }
}

/* ---- Responsive ---- */
@media (max-width: 600px) {
  .hero-cta {
    flex-direction: column;
    align-items: center;
  }

  .hero-btn,
  .btn-outline {
    width: 100%;
    max-width: 320px;
    justify-content: center;
  }
}
</style>
