<template>
  <section class="vip-section" id="vip">
    <div class="container">
      <h2 class="section-title">🎟️ Pass VIP Officiel</h2>
      <p class="section-subtitle">
        Obtenez votre accès exclusif à l'événement de l'année
      </p>

      <div class="vip-layout">
        <!-- Formulaire de gauche -->
        <div class="vip-form-wrapper">
          <div class="card vip-form-card">
            <h3 class="form-title">Réservez votre place</h3>

            <div v-if="!revealed" class="form-body">
              <div class="form-group">
                <label for="vip-name" class="form-label">Votre prénom / pseudo</label>
                <input
                  id="vip-name"
                  v-model="name"
                  type="text"
                  class="form-input"
                  placeholder="ex: SuperFan69"
                  maxlength="30"
                  @keyup.enter="generatePass"
                  :disabled="loading"
                />
              </div>

              <div class="form-group">
                <label class="form-label">Type de pass</label>
                <div class="pass-options">
                  <label
                    v-for="opt in passOptions"
                    :key="opt.value"
                    class="pass-option"
                    :class="{ 'pass-option--selected': selectedPass === opt.value }"
                  >
                    <input
                      type="radio"
                      :value="opt.value"
                      v-model="selectedPass"
                      class="sr-only"
                      :disabled="loading"
                    />
                    <span class="pass-option-icon">{{ opt.icon }}</span>
                    <span class="pass-option-name">{{ opt.name }}</span>
                    <span class="pass-option-price">{{ opt.price }}</span>
                  </label>
                </div>
              </div>

              <div class="perks-list">
                <p class="perks-title">✅ Avantages inclus :</p>
                <ul>
                  <li v-for="perk in currentPerks" :key="perk">{{ perk }}</li>
                </ul>
              </div>

              <button
                class="btn btn-primary btn-full"
                @click="generatePass"
                :disabled="!name.trim() || loading"
              >
                <span v-if="loading" class="loader-spin"></span>
                <span v-else>🎟️ Obtenir mon pass</span>
                <span v-if="loading">Génération en cours...</span>
              </button>
            </div>

            <!-- État révélation -->
            <div v-else class="reveal-body">
              <div class="fish-reveal">🐟</div>
              <h3 class="reveal-title">POISSON D'AVRIL !</h3>
              <p class="reveal-text">
                Il n'y a pas de concert,<br>
                <strong>{{ name }}</strong> ! 😂
              </p>
              <p class="reveal-sub">
                Phyla_Miila t'envoie un gros bisou quand même 💜
              </p>
              <button class="btn btn-primary btn-full" @click="reset">
                🔄 Recommencer (autant en profiter)
              </button>
            </div>
          </div>
        </div>

        <!-- Ticket de droite -->
        <div class="vip-ticket-wrapper">
          <div
            class="vip-ticket"
            :class="{
              'vip-ticket--loading': loading,
              'vip-ticket--revealed': revealed,
              [`vip-ticket--${selectedPass}`]: true,
            }"
          >
            <!-- Haut du ticket -->
            <div class="ticket-top">
              <div class="ticket-event">🎵 CONCERT DE PHYLA_MIILA</div>
              <div class="ticket-date">01 AVRIL 2026</div>
              <div class="ticket-venue">Le Salon de Phyla • Paris (Canapé)</div>
            </div>

            <!-- Séparateur perforé -->
            <div class="ticket-separator">
              <div class="ticket-hole ticket-hole--left" aria-hidden="true"></div>
              <div class="ticket-dashes" aria-hidden="true">· · · · · · · · · · · · · · · · · ·</div>
              <div class="ticket-hole ticket-hole--right" aria-hidden="true"></div>
            </div>

            <!-- Bas du ticket -->
            <div class="ticket-bottom">
              <div v-if="!revealed">
                <div class="ticket-pass-type">
                  <span class="ticket-pass-icon">{{ currentPassOption.icon }}</span>
                  <span class="ticket-pass-label">PASS {{ currentPassOption.name.toUpperCase() }}</span>
                </div>
                <div class="ticket-holder">
                  {{ name.trim() || '???' }}
                </div>
                <div class="ticket-id">
                  N° {{ ticketId }}
                </div>
                <div class="ticket-barcode" aria-hidden="true">
                  <div class="barcode-lines">
                    <span v-for="n in 28" :key="n" :style="{ width: (Math.random() * 3 + 1) + 'px' }"></span>
                  </div>
                  <div class="barcode-number">{{ barcodeNumber }}</div>
                </div>
              </div>

              <!-- Révélation -->
              <div v-else class="ticket-april-fools">
                <div class="ticket-fish-anim">🐟🐟🐟</div>
                <div class="ticket-af-text">POISSON D'AVRIL !</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from 'vue'

const name = ref('')
const loading = ref(false)
const revealed = ref(false)
const selectedPass = ref('gold')

const passOptions = [
  {
    value: 'standard',
    name: 'Standard',
    icon: '🎫',
    price: 'Gratuit',
    perks: [
      'Accès au stream',
      'Vue sur le canapé (depuis chez vous)',
      'Droit de spammer dans le chat',
    ],
  },
  {
    value: 'gold',
    name: 'Gold',
    icon: '⭐',
    price: '4,99€',
    perks: [
      'Tout le pass Standard',
      'Emote exclusive 🐟 dans le chat',
      'Mention spéciale dans le stream',
      'Accès aux coulisses (la cuisine de Phyla)',
    ],
  },
  {
    value: 'vip',
    name: 'VIP Platine',
    icon: '💎',
    price: '99,99€',
    perks: [
      'Tout le pass Gold',
      'Votre nom affiché 5 secondes à l\'écran',
      'Photo dédicacée (par email, JPG 40x40px)',
      'Place de parking (non disponible)',
      'Rencontre avec Phyla (en stream)',
    ],
  },
]

const currentPassOption = computed(
  () => passOptions.find(p => p.value === selectedPass.value) || passOptions[1]
)

const currentPerks = computed(() => currentPassOption.value.perks)

const ticketId = computed(() =>
  name.value.trim()
    ? `PHYLA-${btoa(name.value.slice(0, 6)).slice(0, 6).toUpperCase()}-2026`
    : 'PHYLA-??????-2026'
)

const barcodeNumber = computed(() =>
  Array.from({ length: 13 }, () => Math.floor(Math.random() * 10)).join('')
)

async function generatePass() {
  if (!name.value.trim()) return
  loading.value = true
  await new Promise(r => setTimeout(r, 2200))
  loading.value = false
  revealed.value = true
}

function reset() {
  revealed.value = false
  loading.value = false
  name.value = ''
}
</script>

<style scoped>
.vip-section {
  padding: var(--spacing-section) 0;
  background: linear-gradient(180deg, rgba(26, 26, 46, 0.8) 0%, var(--color-bg) 100%);
}

/* ---- Layout ---- */
.vip-layout {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2.5rem;
  align-items: start;
}

/* ---- Formulaire ---- */
.vip-form-card {
  position: sticky;
  top: 2rem;
}

.form-title {
  font-size: 1.25rem;
  font-weight: 800;
  margin-bottom: 1.5rem;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.form-group {
  margin-bottom: 1.25rem;
}

.form-label {
  display: block;
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-text-muted);
  margin-bottom: 0.5rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

.form-input {
  width: 100%;
  padding: 0.75rem 1rem;
  background: rgba(0, 0, 0, 0.3);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius-sm);
  color: var(--color-text);
  font-size: 1rem;
  font-family: var(--font-primary);
  transition: border-color var(--transition-fast), box-shadow var(--transition-fast);
  outline: none;
}

.form-input:focus {
  border-color: var(--color-primary-light);
  box-shadow: 0 0 0 3px rgba(123, 47, 190, 0.2);
}

.form-input::placeholder {
  color: var(--color-text-muted);
  opacity: 0.6;
}

.form-input:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

/* ---- Options de pass ---- */
.pass-options {
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.pass-option {
  display: flex;
  align-items: center;
  gap: 0.75rem;
  padding: 0.75rem 1rem;
  background: rgba(0, 0, 0, 0.2);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius-sm);
  cursor: pointer;
  transition: all var(--transition-fast);
}

.pass-option:hover {
  border-color: var(--color-primary-light);
  background: rgba(123, 47, 190, 0.1);
}

.pass-option--selected {
  border-color: var(--color-accent) !important;
  background: rgba(224, 64, 251, 0.1) !important;
  box-shadow: 0 0 10px rgba(224, 64, 251, 0.2);
}

.pass-option-icon {
  font-size: 1.2rem;
}

.pass-option-name {
  flex: 1;
  font-weight: 600;
  font-size: 0.95rem;
}

.pass-option-price {
  font-size: 0.85rem;
  font-weight: 700;
  color: var(--color-accent);
}

/* ---- Perks ---- */
.perks-list {
  background: rgba(0, 0, 0, 0.2);
  border-radius: var(--border-radius-sm);
  padding: 0.75rem 1rem;
  margin-bottom: 1.25rem;
}

.perks-title {
  font-size: 0.8rem;
  font-weight: 700;
  color: var(--color-text-muted);
  margin-bottom: 0.4rem;
  text-transform: uppercase;
  letter-spacing: 0.08em;
}

.perks-list ul {
  list-style: none;
  display: flex;
  flex-direction: column;
  gap: 0.25rem;
}

.perks-list li {
  font-size: 0.85rem;
  color: var(--color-text-muted);
  padding-left: 0.5rem;
}

/* ---- Bouton full ---- */
.btn-full {
  width: 100%;
  justify-content: center;
}

/* ---- Loader spinner ---- */
.loader-spin {
  display: inline-block;
  width: 18px;
  height: 18px;
  border: 2px solid rgba(255,255,255,0.3);
  border-top-color: white;
  border-radius: 50%;
  animation: spin 0.8s linear infinite;
  flex-shrink: 0;
}

/* ---- Révélation ---- */
.reveal-body {
  text-align: center;
  animation: bounce-in 0.7s ease both;
}

.fish-reveal {
  font-size: 5rem;
  margin-bottom: 0.5rem;
  display: inline-block;
  animation: float 2s ease-in-out infinite;
}

.reveal-title {
  font-size: 2rem;
  font-weight: 900;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  margin-bottom: 0.75rem;
}

.reveal-text {
  font-size: 1.05rem;
  color: var(--color-text);
  margin-bottom: 0.5rem;
  line-height: 1.5;
}

.reveal-sub {
  font-size: 0.9rem;
  color: var(--color-text-muted);
  margin-bottom: 1.5rem;
}

/* ---- Ticket ---- */
.vip-ticket-wrapper {
  display: flex;
  justify-content: center;
}

.vip-ticket {
  width: 100%;
  max-width: 380px;
  background: var(--color-bg-card);
  border-radius: var(--border-radius);
  border: 1px solid var(--color-border);
  overflow: hidden;
  box-shadow: var(--shadow-card);
  transition: all var(--transition-slow);
}

.vip-ticket--loading {
  animation: pulse-glow 0.8s ease-in-out infinite;
}

.vip-ticket--revealed {
  border-color: var(--color-accent);
  box-shadow: var(--shadow-card), var(--shadow-accent-glow);
  animation: bounce-in 0.6s ease both;
}

/* Couleurs par type de pass */
.vip-ticket--gold .ticket-top {
  background: linear-gradient(135deg, #5A1A9A, #9D50E0);
}

.vip-ticket--vip .ticket-top {
  background: linear-gradient(135deg, #2d0070, #E040FB);
}

.vip-ticket--standard .ticket-top {
  background: linear-gradient(135deg, #3a1a6a, #7B2FBE);
}

.ticket-top {
  padding: 1.5rem 1.5rem 1.25rem;
  text-align: center;
  background: var(--gradient-primary);
}

.ticket-event {
  font-size: 0.75rem;
  font-weight: 800;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  margin-bottom: 0.4rem;
  opacity: 0.9;
}

.ticket-date {
  font-size: 1.8rem;
  font-weight: 900;
  letter-spacing: 0.05em;
  line-height: 1;
  margin-bottom: 0.3rem;
}

.ticket-venue {
  font-size: 0.75rem;
  opacity: 0.75;
  letter-spacing: 0.05em;
}

/* ---- Séparateur ---- */
.ticket-separator {
  display: flex;
  align-items: center;
  background: rgba(0,0,0,0.3);
  padding: 0.5rem 0;
  position: relative;
}

.ticket-hole {
  width: 20px;
  height: 20px;
  border-radius: 50%;
  background: var(--color-bg);
  flex-shrink: 0;
  position: relative;
  z-index: 1;
}

.ticket-hole--left  { margin-left: -10px; }
.ticket-hole--right { margin-right: -10px; }

.ticket-dashes {
  flex: 1;
  text-align: center;
  font-size: 0.85rem;
  letter-spacing: 0.1em;
  color: var(--color-text-muted);
  overflow: hidden;
  white-space: nowrap;
  opacity: 0.5;
}

/* ---- Bas du ticket ---- */
.ticket-bottom {
  padding: 1.25rem 1.5rem;
}

.ticket-pass-type {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.75rem;
}

.ticket-pass-icon {
  font-size: 1.2rem;
}

.ticket-pass-label {
  font-size: 0.7rem;
  font-weight: 800;
  letter-spacing: 0.2em;
  color: var(--color-accent);
}

.ticket-holder {
  font-size: 1.5rem;
  font-weight: 900;
  color: var(--color-text);
  margin-bottom: 0.3rem;
  min-height: 2rem;
  word-break: break-word;
}

.ticket-id {
  font-size: 0.7rem;
  color: var(--color-text-muted);
  margin-bottom: 1rem;
  font-family: monospace;
}

/* ---- Barcode ---- */
.ticket-barcode {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 0.3rem;
}

.barcode-lines {
  display: flex;
  gap: 2px;
  height: 48px;
  align-items: stretch;
}

.barcode-lines span {
  display: block;
  background: var(--color-text);
  min-width: 1px;
  opacity: 0.9;
}

.barcode-number {
  font-family: monospace;
  font-size: 0.65rem;
  color: var(--color-text-muted);
  letter-spacing: 0.15em;
}

/* ---- April Fools reveal dans le ticket ---- */
.ticket-april-fools {
  text-align: center;
  padding: 0.5rem 0;
  animation: bounce-in 0.6s ease both;
}

.ticket-fish-anim {
  font-size: 2.5rem;
  animation: float 1.5s ease-in-out infinite;
  margin-bottom: 0.5rem;
}

.ticket-af-text {
  font-size: 1.4rem;
  font-weight: 900;
  background: var(--gradient-primary);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
  letter-spacing: 0.05em;
}

/* ---- Responsive ---- */
@media (max-width: 900px) {
  .vip-layout {
    grid-template-columns: 1fr;
  }

  .vip-ticket-wrapper {
    order: -1;
  }

  .vip-form-card {
    position: static;
  }
}

.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0,0,0,0);
  white-space: nowrap;
  border-width: 0;
}
</style>
