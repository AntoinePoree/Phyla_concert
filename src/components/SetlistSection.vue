<template>
  <section class="setlist-section" id="setlist">
    <div class="container">
      <h2 class="section-title">🎵 La Setlist Officielle</h2>
      <p class="section-subtitle">
        Programme soigneusement préparé après des mois de répétition
        <span class="disclaimer-inline">(dans sa tête)</span>
      </p>

      <div class="setlist-card">
        <!-- En-tête du programme -->
        <div class="setlist-header">
          <div class="setlist-header-info">
            <span class="header-label">ARTISTE</span>
            <span class="header-value">Phyla_Miila</span>
          </div>
          <div class="setlist-header-info">
            <span class="header-label">DATE</span>
            <span class="header-value">30 / 04 / 2026</span>
          </div>
          <div class="setlist-header-info">
            <span class="header-label">DURÉE TOTALE</span>
            <span class="header-value">{{ totalDuration }}</span>
          </div>
        </div>

        <!-- Liste des chansons -->
        <ol class="song-list">
          <li
            v-for="(song, index) in setlist"
            :key="index"
            class="song-item"
            :class="{ 'song-item--active': hoveredIndex === index }"
            @mouseenter="hoveredIndex = index"
            @mouseleave="hoveredIndex = null"
          >
            <span class="song-number">{{
              String(index + 1).padStart(2, "0")
            }}</span>
            <span class="song-icon">{{ song.icon }}</span>
            <div class="song-info">
              <span class="song-title">{{ song.title }}</span>
              <span v-if="song.note" class="song-note">{{ song.note }}</span>
            </div>
            <span class="song-duration">{{ song.duration }}</span>
            <span class="song-play-icon" aria-hidden="true">▶</span>
          </li>
        </ol>

        <!-- Rappel -->
        <div class="encore-section">
          <div class="encore-divider">
            <span class="encore-label">✨ RAPPEL ✨</span>
          </div>
          <ol class="song-list encore-list" start="12">
            <li
              v-for="(song, index) in encore"
              :key="'encore-' + index"
              class="song-item song-item--encore"
            >
              <span class="song-number">{{
                String(index + 12).padStart(2, "0")
              }}</span>
              <span class="song-icon">{{ song.icon }}</span>
              <div class="song-info">
                <span class="song-title">{{ song.title }}</span>
                <span v-if="song.note" class="song-note">{{ song.note }}</span>
              </div>
              <span class="song-duration">{{ song.duration }}</span>
              <span class="song-play-icon" aria-hidden="true">▶</span>
            </li>
          </ol>
        </div>

        <!-- Note de bas de page -->
        <div class="setlist-footer">
          <p>
            🚨 Setlist sujette à modifications selon l'humeur de Phyla et les
            lag pics
          </p>
        </div>
      </div>
    </div>
  </section>
</template>

<script setup>
import { ref, computed } from "vue";

const hoveredIndex = ref(null);

const setlist = ref([
  {
    title: "Une glygly libre (Feat. La communauté des glyglys )",
    duration: "4:20",
    icon: "🪻",
    note: "feat. les poissons du salon",
  },
  {
    title: "Quand le Chat n'est pas là (Les Souris Dansent)",
    duration: "3:45",
    icon: "🐱",
  },
  {
    title: "AAAAAAHHH (Version Acoustique)",
    duration: "0:47",
    icon: "😱",
    note: "live reaction compilée",
  },
  {
    title: "Mon PC Lag Encore",
    duration: "6:66",
    icon: "💻",
    note: "durée approximative",
  },
  {
    title: "Drops de 3h du Matin",
    duration: "3:00",
    icon: "🌙",
    note: "exclusivement en nuit",
  },
  { title: "Le Sub Train Qui Déraille", duration: "2:30", icon: "🚂" },
  {
    title: "Je Serai à tes Cotés - Cover Remix De Anytime Anywhere - By Phyla",
    duration: "3:47",
    icon: "🔔",
    note: "Impactante",
  },
  {
    title: "Je Vais Aller Dormir (Faux Départ #1)",
    duration: "5:12",
    icon: "😴",
  },
  {
    title: "Saku dort, regardezzz comment elle est mignoooooooooooooooooone",
    duration: "3:33",
    icon: "🎬",
  },
  {
    title: "Winx Club Generique Cover By Phyla",
    duration: "1:28",
    icon: "📡",
    note: "durée indéterminée",
  },
  {
    title: "TW : Contenu Difficile",
    duration: "2:01",
    icon: "⚠️",
    note: "(c'est juste Phyla qui cuisine)",
  },
]);

const encore = ref([
  {
    title: "Je Vais Vraiment Aller Dormir (Faux Départ #2)",
    duration: "4:44",
    icon: "😴",
  },
  {
    title: "Glyctatrice (feat. Saku)",
    duration: "♥",
    icon: "🎊",
    note: "SURPRISE !",
  },
]);

// Calcul durée totale (factice mais fun)
const totalDuration = computed(() => "1h 42min (environ)");
</script>

<style scoped>
.setlist-section {
  padding: var(--spacing-section) 0;
  background: var(--color-bg);
}

/* ---- Card setlist ---- */
.setlist-card {
  background: var(--gradient-card);
  border: 1px solid var(--color-border);
  border-radius: var(--border-radius);
  overflow: hidden;
  box-shadow: var(--shadow-card);
}

/* ---- En-tête ---- */
.setlist-header {
  display: flex;
  gap: 0;
  background: rgba(123, 47, 190, 0.2);
  border-bottom: 1px solid var(--color-border);
  padding: 1.25rem 1.5rem;
  flex-wrap: wrap;
  gap: 1.5rem;
}

.setlist-header-info {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}

.header-label {
  font-size: 0.7rem;
  font-weight: 700;
  letter-spacing: 0.15em;
  text-transform: uppercase;
  color: var(--color-text-muted);
}

.header-value {
  font-size: 1rem;
  font-weight: 700;
  color: var(--color-text);
}

/* ---- Liste ---- */
.song-list {
  list-style: none;
  padding: 0.5rem 0;
}

.song-item {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 0.9rem 1.5rem;
  border-bottom: 1px solid rgba(157, 80, 224, 0.1);
  cursor: default;
  transition: background var(--transition-fast);
  position: relative;
}

.song-item:last-child {
  border-bottom: none;
}

.song-item:hover,
.song-item--active {
  background: rgba(123, 47, 190, 0.12);
}

.song-item:hover .song-play-icon,
.song-item--active .song-play-icon {
  opacity: 1;
  transform: translateX(0);
}

.song-item:hover .song-number,
.song-item--active .song-number {
  opacity: 0;
}

.song-number {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-text-muted);
  width: 28px;
  text-align: center;
  flex-shrink: 0;
  transition: opacity var(--transition-fast);
}

.song-play-icon {
  position: absolute;
  left: 1.5rem;
  width: 28px;
  text-align: center;
  font-size: 0.75rem;
  color: var(--color-accent);
  opacity: 0;
  transform: translateX(-4px);
  transition: all var(--transition-fast);
}

.song-icon {
  font-size: 1.3rem;
  flex-shrink: 0;
  width: 28px;
  text-align: center;
}

.song-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.1rem;
  min-width: 0;
}

.song-title {
  font-size: 0.95rem;
  font-weight: 600;
  color: var(--color-text);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.song-note {
  font-size: 0.75rem;
  color: var(--color-text-muted);
  font-style: italic;
}

.song-duration {
  font-size: 0.85rem;
  font-weight: 600;
  color: var(--color-text-muted);
  font-variant-numeric: tabular-nums;
  flex-shrink: 0;
}

/* ---- Rappel ---- */
.encore-section {
  border-top: 1px solid var(--color-border);
}

.encore-divider {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background: rgba(224, 64, 251, 0.08);
}

.encore-label {
  font-size: 0.8rem;
  font-weight: 800;
  letter-spacing: 0.2em;
  color: var(--color-accent);
  text-transform: uppercase;
}

.song-item--encore .song-title {
  color: var(--color-accent);
}

.encore-list {
  padding: 0.5rem 0;
}

/* ---- Footer setlist ---- */
.setlist-footer {
  padding: 1rem 1.5rem;
  background: rgba(0, 0, 0, 0.2);
  border-top: 1px solid var(--color-border);
}

.setlist-footer p {
  font-size: 0.8rem;
  color: var(--color-text-muted);
  font-style: italic;
  text-align: center;
}

/* ---- Inline disclaimer ---- */
.disclaimer-inline {
  font-size: 0.85em;
  color: var(--color-text-muted);
  font-style: italic;
}

/* ---- Responsive ---- */
@media (max-width: 600px) {
  .song-item {
    padding: 0.8rem 1rem;
    gap: 0.6rem;
  }

  .song-play-icon {
    left: 1rem;
  }

  .song-note {
    display: none;
  }
}
</style>
