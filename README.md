# 🎵 Concert de Phyla_Miila — Poisson d'Avril 2026

> **Site officiel** de l'événement musical le plus attendu de l'année.  
> *(Spoiler : il n'y a pas de concert.)*

---

## Aperçu

Site one-page fait en **Vue 3 + Vite**, déployé sur GitHub Pages, annonçant le "concert" de la streameuse **Phyla_Miila** pour le 1er Avril 2026.  
Thème violet, ambiance concert, et une bonne dose de poisson d'avril.

### Sections

| Section | Description |
| --- | --- |
| 🎤 Hero | Annonce du concert avec sparkles animées |
| ⏳ Compte à rebours | Timer live jusqu'à la date du concert |
| 🎵 Setlist | 11 chansons + 2 rappels d'une absurdité totale |
| 🎟️ Pass VIP | Générateur de pass VIP → révèle le poisson d'avril |
| ❓ FAQ | Questions/réponses hilarantes sur l'événement |
| 🐟 Footer | Avalanche de poissons au clic sur les mentions légales |

---

## Stack technique

- [Vue 3](https://vuejs.org/) (Composition API)
- [Vite 5](https://vitejs.dev/)
- CSS custom properties pour le thème (tout centralisé dans `src/assets/styles/theme.css`)
- GitHub Actions pour le déploiement automatique

---

## Lancer en local

```bash
npm install
npm run dev
```

Le site tourne sur `http://localhost:5173`.

---

## Déployer sur GitHub Pages

### Automatique (recommandé)

Chaque push sur `main` déclenche le workflow GitHub Actions qui build et déploie sur la branche `gh-pages`.

1. Push sur `main`
2. Aller dans **Settings → Pages → Source → `gh-pages`**
3. Le site est disponible sur `https://<pseudo>.github.io/Phyla_concert/`

### Manuel

```bash
npm run deploy
```

---

## Modifier le thème

Toutes les couleurs sont définies dans [`src/assets/styles/theme.css`](src/assets/styles/theme.css) :

```css
--color-primary:       #7B2FBE
--color-primary-light: #9D50E0
--color-primary-dark:  #5A1A9A
--color-accent:        #E040FB
--color-bg:            #0D0D1A
--color-bg-card:       #1A1A2E
--color-text:          #F0E6FF
--color-text-muted:    #A090C0
```

---

## Structure du projet

```text
Phyla_concert/
├── .github/workflows/deploy.yml   # CI/CD GitHub Pages
├── public/
│   └── favicon.svg
├── src/
│   ├── assets/styles/theme.css    # Variables CSS du thème
│   ├── components/
│   │   ├── HeroSection.vue
│   │   ├── CountdownSection.vue
│   │   ├── SetlistSection.vue
│   │   ├── VipSection.vue
│   │   ├── FaqSection.vue
│   │   └── FooterSection.vue
│   ├── App.vue
│   └── main.js
├── index.html
├── vite.config.js
└── package.json
```

---

*Fait avec ❤️ pour Phyla_Miila — Construit par Menth*  
*🐟 Aucun concert n'a été blessé lors de la création de ce site.*
