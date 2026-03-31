<template>
  <footer class="footer">
    <!-- Vague décorative -->
    <div class="footer-wave" aria-hidden="true">
      <svg
        viewBox="0 0 1440 80"
        xmlns="http://www.w3.org/2000/svg"
        preserveAspectRatio="none"
      >
        <path
          d="M0,40 C360,80 720,0 1080,40 C1260,60 1380,50 1440,45 L1440,80 L0,80 Z"
          fill="var(--color-bg-card)"
        />
      </svg>
    </div>

    <div class="footer-body">
      <div class="container">
        <!-- Note de musique animée -->
        <div class="footer-fish" aria-hidden="true">🎵</div>

        <!-- Titre principal -->
        <h2 class="footer-title">Concert d'Avril 2026</h2>
        <p class="footer-subtitle">
          Fait avec ❤️ pour <strong>Phyla_Miila</strong>
        </p>

        <!-- Ligne décorative -->
        <div class="footer-divider" aria-hidden="true"></div>

        <!-- Liens de navigation -->
        <nav class="footer-nav" aria-label="Navigation footer">
          <a href="#countdown" class="footer-link">⏳ Compte à rebours</a>
          <a href="#setlist" class="footer-link">🎵 Setlist</a>
          <a href="#vip" class="footer-link">🎟️ Pass VIP</a>
          <a href="#faq" class="footer-link">❓ FAQ</a>
        </nav>

        <!-- Message d'aveu -->
        <div class="footer-confession">
          <p>
            <span class="confession-label">AVEUX OFFICIELS</span><br />
            Phyla_Miila est la plus grande chanteuse de l'univers.<br />
            Donc achetez vos places dès maintenant. 😄
          </p>
        </div>

        <!-- Footer bottom -->
        <div class="footer-bottom">
          <button v-if="!revealed" class="reveal-btn" @click="reveal">
            NE PAS Cliquez ici pour NE PAS VOIR les mentions légales
          </button>

          <Transition name="fade-bottom">
            <div v-if="revealed" class="footer-bottom-content">
              <p class="footer-copy">
                🐟 Poisson d'Avril 2026 · Phyla_Miila · Tous droits réservés
                (surtout le droit de troller)
              </p>
              <p class="footer-tech">
                Construit par Menth · Aucun concert n'a été blessé lors de la
                création de ce site
              </p>
            </div>
          </Transition>
        </div>

        <!-- Avalanche de poissons (portail fixed) -->
        <Teleport to="body">
          <div class="fish-avalanche" aria-hidden="true">
            <span
              v-for="f in fishList"
              :key="f.id"
              class="fish-particle"
              :style="f.style"
              >🐟</span
            >
          </div>
        </Teleport>
      </div>
    </div>
  </footer>
</template>

<script setup>
import { ref } from "vue";

const revealed = ref(false);
const fishList = ref([]);

function reveal() {
  revealed.value = true;
  launchFishAvalanche();
}

function launchFishAvalanche() {
  fishList.value = [];
  const count = 30;
  for (let i = 0; i < count; i++) {
    const top = Math.random() * 100; // % vertical
    const duration = 1.2 + Math.random() * 1; // 1.2s – 2.2s
    const delay = i * 0.06; // départ en cascade
    const size = 1.2 + Math.random() * 1.8; // 1.2rem – 3rem
    const flip = Math.random() > 0.5 ? "scaleX(-1)" : ""; // alterner sens
    fishList.value.push({
      id: i,
      style: {
        top: `${top}vh`,
        animationDuration: `${duration}s`,
        animationDelay: `${delay}s`,
        fontSize: `${size}rem`,
        transform: flip,
      },
    });
  }
  // Nettoyer après la dernière vague
  setTimeout(
    () => {
      fishList.value = [];
    },
    count * 60 + 2200 + 200,
  );
}
</script>

<style scoped>
.footer {
  position: relative;
  background: var(--color-bg);
}

/* ---- Vague ---- */
.footer-wave {
  width: 100%;
  line-height: 0;
  overflow: hidden;
}

.footer-wave svg {
  width: 100%;
  height: 80px;
  display: block;
}

/* ---- Corps du footer ---- */
.footer-body {
  background: var(--color-bg-card);
  padding: 4rem 0 2rem;
  text-align: center;
}

/* ---- Poisson animé ---- */
.footer-fish {
  font-size: 4rem;
  display: inline-block;
  animation: float 3s ease-in-out infinite;
  margin-bottom: 1rem;
  filter: drop-shadow(0 0 15px rgba(224, 64, 251, 0.4));
}

/* ---- Titre ---- */
.footer-title {
  font-size: clamp(2rem, 5vw, 3rem);
  font-weight: 900;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  background-size: 200% auto;
  animation: shimmer 4s linear infinite;
  margin-bottom: 0.5rem;
}

.footer-subtitle {
  font-size: 1.1rem;
  color: var(--color-text-muted);
  margin-bottom: 2rem;
}

.footer-subtitle strong {
  color: var(--color-accent);
}

/* ---- Divider ---- */
.footer-divider {
  width: 80px;
  height: 2px;
  background: var(--gradient-primary);
  border-radius: 2px;
  margin: 0 auto 2rem;
}

/* ---- Navigation ---- */
.footer-nav {
  display: flex;
  gap: 1rem;
  justify-content: center;
  flex-wrap: wrap;
  margin-bottom: 2rem;
}

.footer-link {
  font-size: 0.9rem;
  font-weight: 600;
  color: var(--color-text-muted);
  text-decoration: none;
  padding: 0.4rem 0.8rem;
  border-radius: 50px;
  border: 1px solid var(--color-border);
  transition: all var(--transition-fast);
}

.footer-link:hover {
  color: var(--color-accent);
  border-color: var(--color-accent);
  background: rgba(224, 64, 251, 0.08);
}

/* ---- Confession ---- */
.footer-confession {
  max-width: 600px;
  margin: 0 auto 2.5rem;
  background: rgba(123, 47, 190, 0.1);
  border: 1px dashed var(--color-border);
  border-radius: var(--border-radius);
  padding: 1.5rem;
}

.footer-confession p {
  font-size: 0.9rem;
  color: var(--color-text-muted);
  line-height: 1.8;
}

.confession-label {
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 0.2em;
  color: var(--color-accent);
  text-transform: uppercase;
  display: inline-block;
  margin-bottom: 0.25rem;
}

/* ---- Bottom ---- */
.footer-bottom {
  border-top: 1px solid var(--color-border);
  padding-top: 1.5rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.5rem;
}

.reveal-btn {
  background: var(--gradient-primary);
  color: var(--color-text);
  border: none;
  border-radius: 50px;
  padding: 0.6rem 1.6rem;
  font-size: 0.9rem;
  font-weight: 700;
  cursor: pointer;
  transition:
    opacity 0.2s,
    transform 0.2s;
}
.reveal-btn:hover {
  opacity: 0.85;
  transform: scale(1.04);
}

.footer-bottom-content {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  text-align: center;
}

.footer-copy {
  font-size: 0.85rem;
  color: var(--color-text-muted);
}

.footer-tech {
  font-size: 0.75rem;
  color: var(--color-text-muted);
  opacity: 0.5;
  font-style: italic;
}

/* ---- Transition reveal ---- */
.fade-bottom-enter-active {
  transition:
    opacity 0.5s,
    transform 0.5s;
}
.fade-bottom-enter-from {
  opacity: 0;
  transform: translateY(12px);
}

/* ---- Avalanche de poissons (fixed, hors scoped) ---- */
.fish-avalanche {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 9999;
  overflow: hidden;
}

.fish-particle {
  position: absolute;
  left: -4rem;
  animation: fish-cross linear forwards;
  will-change: transform;
  display: inline-block;
}

@keyframes fish-cross {
  from {
    transform: translateX(calc(100vw + 8rem));
  }
  to {
    transform: translateX(0);
  }
}
</style>
