<template>
  <div id="phyla-app">
    <!-- Barre de navigation fixe -->
    <nav class="navbar" :class="{ 'navbar--scrolled': scrolled }">
      <div class="container navbar-inner">
        <a href="#" class="navbar-logo"> 🎵 <span>Phyla_Miila</span> </a>
        <div class="navbar-links">
          <a href="#countdown" class="navbar-link">⏳ Compte à rebours</a>
          <a href="#setlist" class="navbar-link">🎵 Setlist</a>
          <a href="#vip" class="navbar-link">🎟️ VIP</a>
          <a href="#faq" class="navbar-link">❓ FAQ</a>
        </div>
        <!-- Menu burger mobile -->
        <button
          class="burger"
          @click="mobileMenuOpen = !mobileMenuOpen"
          :aria-expanded="mobileMenuOpen"
          aria-label="Menu"
        >
          <span></span><span></span><span></span>
        </button>
      </div>

      <!-- Menu mobile -->
      <div class="mobile-menu" :class="{ 'mobile-menu--open': mobileMenuOpen }">
        <a href="#countdown" class="mobile-link" @click="mobileMenuOpen = false"
          >⏳ Compte à rebours</a
        >
        <a href="#setlist" class="mobile-link" @click="mobileMenuOpen = false"
          >🎵 Setlist</a
        >
        <a href="#vip" class="mobile-link" @click="mobileMenuOpen = false"
          >🎟️ Pass VIP</a
        >
        <a href="#faq" class="mobile-link" @click="mobileMenuOpen = false"
          >❓ FAQ</a
        >
      </div>
    </nav>

    <!-- Sections principales -->
    <main>
      <HeroSection />
      <CountdownSection />
      <SetlistSection />
      <VipSection />
      <FaqSection />
    </main>

    <FooterSection />

    <!-- Poisson flottant qui traverse l'écran de temps en temps -->
    <div v-if="showFlyingFish" class="flying-fish" aria-hidden="true">🐟</div>

    <!-- Bouton retour en haut -->
    <button
      v-if="scrolled"
      class="scroll-top-btn"
      @click="scrollToTop"
      aria-label="Retour en haut"
      title="Retour en haut"
    >
      ↑
    </button>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";
import HeroSection from "./components/HeroSection.vue";
import CountdownSection from "./components/CountdownSection.vue";
import SetlistSection from "./components/SetlistSection.vue";
import VipSection from "./components/VipSection.vue";
import FaqSection from "./components/FaqSection.vue";
import FooterSection from "./components/FooterSection.vue";

const scrolled = ref(false);
const mobileMenuOpen = ref(false);
const showFlyingFish = ref(false);

// Scroll listener
function onScroll() {
  scrolled.value = window.scrollY > 80;
}

// Poisson qui traverse l'écran toutes les 30 secondes
let fishTimer = null;
function triggerFlyingFish() {
  showFlyingFish.value = true;
  setTimeout(() => {
    showFlyingFish.value = false;
  }, 4000);
}

function scrollToTop() {
  window.scrollTo({ top: 0, behavior: "smooth" });
}

onMounted(() => {
  window.addEventListener("scroll", onScroll, { passive: true });
  // Premier poisson après 8 secondes
  setTimeout(() => {
    triggerFlyingFish();
    fishTimer = setInterval(triggerFlyingFish, 30_000);
  }, 8_000);
});

onUnmounted(() => {
  window.removeEventListener("scroll", onScroll);
  clearInterval(fishTimer);
});
</script>

<style scoped>
/* ---- Navbar ---- */
.navbar {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  padding: 1rem 0;
  transition:
    background var(--transition-med),
    backdrop-filter var(--transition-med),
    box-shadow var(--transition-med);
}

.navbar--scrolled {
  background: rgba(13, 13, 26, 0.92);
  backdrop-filter: blur(12px);
  box-shadow: 0 2px 20px rgba(0, 0, 0, 0.4);
  padding: 0.6rem 0;
}

.navbar-inner {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 1.5rem;
}

.navbar-logo {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  font-size: 1.1rem;
  font-weight: 800;
  color: var(--color-text);
  text-decoration: none;
  white-space: nowrap;
}

.navbar-logo span {
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.navbar-links {
  display: flex;
  gap: 0.25rem;
  align-items: center;
}

.navbar-link {
  padding: 0.4rem 0.8rem;
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-text-muted);
  text-decoration: none;
  border-radius: 50px;
  transition: all var(--transition-fast);
}

.navbar-link:hover {
  color: var(--color-accent);
  background: rgba(224, 64, 251, 0.1);
}

/* ---- Burger ---- */
.burger {
  display: none;
  flex-direction: column;
  gap: 5px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 0.4rem;
}

.burger span {
  display: block;
  width: 24px;
  height: 2px;
  background: var(--color-text);
  border-radius: 2px;
  transition: all var(--transition-fast);
}

/* ---- Menu mobile ---- */
.mobile-menu {
  display: none;
  flex-direction: column;
  gap: 0;
  background: rgba(13, 13, 26, 0.97);
  backdrop-filter: blur(12px);
  border-top: 1px solid var(--color-border);
  padding: 0;
  max-height: 0;
  overflow: hidden;
  transition:
    max-height var(--transition-med),
    padding var(--transition-med);
}

.mobile-menu--open {
  max-height: 300px;
  padding: 0.5rem 0;
}

.mobile-link {
  display: block;
  padding: 0.85rem 1.5rem;
  font-size: 1rem;
  font-weight: 600;
  color: var(--color-text-muted);
  text-decoration: none;
  transition: all var(--transition-fast);
  border-bottom: 1px solid rgba(157, 80, 224, 0.1);
}

.mobile-link:last-child {
  border-bottom: none;
}

.mobile-link:hover {
  color: var(--color-accent);
  background: rgba(224, 64, 251, 0.08);
  padding-left: 2rem;
}

/* ---- Poisson volant ---- */
.flying-fish {
  position: fixed;
  bottom: 20%;
  left: 0;
  font-size: 2.5rem;
  z-index: 999;
  pointer-events: none;
  animation: fish-swim 4s ease-in-out forwards;
  filter: drop-shadow(0 0 8px rgba(224, 64, 251, 0.5));
}

/* ---- Bouton retour en haut ---- */
.scroll-top-btn {
  position: fixed;
  bottom: 1.5rem;
  right: 1.5rem;
  z-index: 50;
  width: 44px;
  height: 44px;
  border-radius: 50%;
  background: var(--gradient-primary);
  color: white;
  border: none;
  font-size: 1.2rem;
  font-weight: 700;
  cursor: pointer;
  box-shadow: var(--shadow-glow);
  transition:
    transform var(--transition-fast),
    box-shadow var(--transition-fast);
  display: flex;
  align-items: center;
  justify-content: center;
}

.scroll-top-btn:hover {
  transform: translateY(-3px) scale(1.1);
  box-shadow: var(--shadow-glow), var(--shadow-accent-glow);
}

/* ---- Responsive ---- */
@media (max-width: 768px) {
  .navbar-links {
    display: none;
  }

  .burger {
    display: flex;
  }

  .mobile-menu {
    display: flex;
  }
}
</style>
