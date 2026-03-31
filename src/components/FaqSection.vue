<template>
  <section class="faq-section" id="faq">
    <div class="container">
      <h2 class="section-title">❓ Foire Aux Questions</h2>
      <p class="section-subtitle">
        Toutes les réponses aux questions que vous vous posez
        <span class="subtitle-small"
          >(et celles que vous ne vous posez pas)</span
        >
      </p>

      <div class="faq-list">
        <div
          v-for="(item, index) in faqItems"
          :key="index"
          class="faq-item"
          :class="{ 'faq-item--open': openIndex === index }"
        >
          <button
            class="faq-question"
            @click="toggle(index)"
            :aria-expanded="openIndex === index"
            :aria-controls="`faq-answer-${index}`"
          >
            <span class="faq-q-icon">{{ item.icon }}</span>
            <span class="faq-q-text">{{ item.question }}</span>
            <span class="faq-chevron" aria-hidden="true">
              {{ openIndex === index ? "▲" : "▼" }}
            </span>
          </button>

          <div
            :id="`faq-answer-${index}`"
            class="faq-answer"
            :class="{ 'faq-answer--visible': openIndex === index }"
          >
            <div class="faq-answer-inner">
              <p class="answer-text">{{ item.answer }}</p>
              <p v-if="item.bonus" class="answer-bonus">{{ item.bonus }}</p>
            </div>
          </div>
        </div>
      </div>

      <!-- Easter egg footer FAQ -->
      <div class="faq-easter-egg">
        <p>Vous avez d'autres questions ? 🤔</p>
        <p class="easter-egg-answer">Non ? Bien. Merci d'être venu.</p>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref } from "vue";

const openIndex = ref(0);

function toggle(index) {
  openIndex.value = openIndex.value === index ? null : index;
}

const faqItems = ref([
  {
    icon: "🏟️",
    question: "Où se déroule le concert ?",
    answer:
      "Dans le salon de Phyla_Miila. Le rang 1 correspond à son canapé, et le rang 2 est la table basse. Les places debout sont disponibles en cuisine.",
    bonus:
      "🚪 Accès par la porte principale uniquement. La fenêtre est réservée aux VIP Platine.",
  },
  {
    icon: "👔",
    question: "Y a-t-il un dress code ?",
    answer:
      "Oui. Le pyjama est obligatoire. Les pantoufles sont vivement recommandées. Les costumes de poisson seront appréciés à leur juste valeur.",
    bonus: "⛔ Les smokings et robes de soirée sont formellement interdits.",
  },
  {
    icon: "🍿",
    question: "Peut-on apporter des snacks ?",
    answer:
      "C'est même fortement recommandé. Phyla en aura besoin pour tenir le show. Les chips sont préférées aux pop-corns pour éviter le bruit de fond sur le micro.",
  },
  {
    icon: "✍️",
    question: "Et si je veux un autographe ?",
    answer:
      'Il faudra sub pendant le stream. Un sub de base donne droit à un "merci" prononcé en direct. Un sub Prime donne droit à un bisou virtuel.',
    bonus: "💜 Les Tier 3 reçoivent un GG WP personnel.",
  },
  {
    icon: "🎧",
    question: "Quelle est la qualité sonore attendue ?",
    answer:
      "Phyla utilisera un micro de gamer à 29,99€ avec filtre anti-pop fait maison (une chaussette). Attendez-vous à un son chaleureux et authentique.",
    bonus: "🎵 Possible souffle de fond si le PC fait du bruit.",
  },
  {
    icon: "🚗",
    question: "Y a-t-il un parking disponible ?",
    answer:
      "Oui ! Vous pouvez vous garer devant votre propre domicile, puisque le concert est en ligne. Les abonnés ont une place garantie dans leur salon.",
  },
  {
    icon: "🌡️",
    question: "Et en cas d'urgence médicale ?",
    answer:
      "Un médecin de renom (le chat de Menth, Dr. Shoun) sera présent. En cas de rire incontrôlable, allongez-vous et respirez. C'est normal.",
  },
  {
    icon: "🎟️",
    question: "Le pass VIP est-il vraiment réel ?",
    answer: "...",
    bonus:
      "Venez dans les mp de Menth, et lâchez un 'Je te fais l'amour tout les jours de la semaine pendant 24h non-stop' et ne le crier pas trop fort à Phyla ♥",
  },
]);
</script>

<style scoped>
.faq-section {
  padding: var(--spacing-section) 0;
  background: linear-gradient(
    180deg,
    var(--color-bg) 0%,
    rgba(26, 26, 46, 0.6) 100%
  );
}

/* ---- Liste FAQ ---- */
.faq-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  max-width: 800px;
  margin: 0 auto 2.5rem;
}

/* ---- Item ---- */
.faq-item {
  background: var(--gradient-card);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  overflow: hidden;
  transition:
    border-color var(--transition-med),
    box-shadow var(--transition-med);
}

.faq-item--open {
  border-color: var(--color-border-accent);
  box-shadow:
    var(--shadow-card),
    0 0 15px rgba(123, 47, 190, 0.2);
}

/* ---- Question (bouton) ---- */
.faq-question {
  width: 100%;
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 1.1rem 1.5rem;
  background: none;
  border: none;
  cursor: pointer;
  text-align: left;
  color: var(--color-text);
  transition: background var(--transition-fast);
}

.faq-question:hover {
  background: rgba(123, 47, 190, 0.1);
}

.faq-item--open .faq-question {
  background: rgba(123, 47, 190, 0.15);
}

.faq-q-icon {
  font-size: 1.3rem;
  flex-shrink: 0;
}

.faq-q-text {
  flex: 1;
  font-size: 1rem;
  font-weight: 600;
  line-height: 1.4;
}

.faq-chevron {
  font-size: 0.7rem;
  color: var(--color-text-muted);
  transition: transform var(--transition-med);
  flex-shrink: 0;
}

/* ---- Réponse (collapsible) ---- */
.faq-answer {
  display: grid;
  grid-template-rows: 0fr;
  transition: grid-template-rows var(--transition-med);
  overflow: hidden;
}

.faq-answer--visible {
  grid-template-rows: 1fr;
}

.faq-answer-inner {
  overflow: hidden;
  padding: 0 1.5rem;
}

.faq-answer--visible .faq-answer-inner {
  padding: 0 1.5rem 1.25rem;
}

.answer-text {
  font-size: 0.95rem;
  color: var(--color-text-muted);
  line-height: 1.7;
  margin-bottom: 0.4rem;
}

.answer-bonus {
  font-size: 0.85rem;
  color: var(--color-accent);
  font-style: italic;
  line-height: 1.5;
  padding: 0.5rem 0.75rem;
  background: rgba(224, 64, 251, 0.08);
  border-left: 2px solid var(--color-accent);
  border-radius: 0 4px 4px 0;
}

/* ---- Sous-titre ---- */
.subtitle-small {
  font-size: 0.85em;
  color: var(--color-text-muted);
  font-style: italic;
}

/* ---- Easter egg ---- */
.faq-easter-egg {
  text-align: center;
  padding: 2rem;
  background: var(--gradient-card);
  border: 1px dashed var(--color-border);
  border-radius: var(--border-radius);
  max-width: 500px;
  margin: 0 auto;
}

.faq-easter-egg p {
  font-size: 1rem;
  color: var(--color-text-muted);
  margin-bottom: 0.4rem;
}

.easter-egg-answer {
  font-size: 1.2rem !important;
  font-weight: 700;
  color: var(--color-accent) !important;
  margin-top: 0.5rem !important;
}
</style>
