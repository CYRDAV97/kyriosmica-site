# Changelog — kyriosmica-site

Toutes les modifications notables de la vitrine publique sont consignées ici.
Format inspiré de [Keep a Changelog](https://keepachangelog.com/).

---

## [3.0.0] — 2026-04-16

### Restructuration majeure de l'architecture

La vitrine passe d'un monolithe HTML opaque à une **architecture multipage
avec assets partagés**, sans régression de fonctionnalités.

### Identité visuelle

- **Nouveau brand kit v3** intégré (5 variantes logo, palette étendue) :
  - `logo_horizontal_EN.svg`/`.png` · `logo_horizontal_FR.svg`/`.png`
  - `logo_primary_EN.svg`/`.png`    · `logo_primary_FR.svg`/`.png`
  - `logo_icon.svg`/`.png`
  - `brand_kit_v3.png` (planche de livraison)
- Symbole en packing hexagonal : 1 centre terre (Bénin) + 6 nœuds cyan +
  6 sommets or (liaisons H) + 3 axes T₃ traversants
- Texte converti en tracés vectoriels dans tous les SVG (indépendance polices)
- Favicon.svg + favicon.ico générés

### Définition MICA — migration complète

Remplacement systématique de **« Mathematical Interface for Cellular
Architecture »** par la définition canonique bilingue :

- FR : **Matrice d'Intrication Coordonnée par les Amplitudes**
- EN : **Matrix of Intrication Coordinated by Amplitudes**

5 occurrences patchées :
- `index.html` lignes 2067, 2226, 2848, 3150
- `lab/qudits36.html` ligne 3062

Vérification : `grep -r "Mathematical Interface for Cellular Architecture" .`
retourne **0 occurrence**.

### Architecture

Avant :
```
un seul fichier index_site__.html (4070 lignes)
6 HTML autonomes orphelins (simulateur, t3net, qudits36, science4, analyzer, donnees)
```

Après :
```
kyriosmica-site/
├── index.html                  # SPA institutionnelle (Science, Défi, Revue, Documents, Consortium, Contact)
├── lab/                        # groupe Laboratoires interactifs
│   ├── simulateur.html         # démo Qudits-36 pédagogique
│   ├── t3net.html              # QNN T₃-Net playground
│   ├── qudits36.html           # lab Qudits-36 avancé
│   ├── science4.html           # Science 4.0
│   └── analyzer.html           # analyseur de séquences
├── data/donnees.html
├── assets/
│   ├── shared/ (brand.css, nav.css, nav.js, i18n.js, footer.js)
│   ├── logos/  (5 variantes SVG + 5 PNG + planche)
│   ├── favicon.svg, favicon.ico
├── docs/                       # PDFs déposés manuellement (structure prête)
├── robots.txt, sitemap.xml, README.md
```

### Nav et footer unifiés

- Nouveau composant `nav.js` : navbar unique injectée via `<div id="kyrios-nav"></div>`
  sur toutes les pages
- Groupe **« Laboratoires »** en dropdown (Simulateur, T₃-Net, Qudits-36, Analyzer)
- Toggle FR/EN intégré à la navbar (persistance `localStorage`)
- Menu hamburger mobile
- Fermeture dropdown au clic extérieur / Escape
- Composant `footer.js` : footer unique avec définition MICA bilingue

### Système i18n

- `i18n.js` parcourt tous les `[data-fr][data-en]` et applique la bonne langue
- Détection langue : `localStorage.kyrios_lang` → `navigator.language` → `fr`
- Événement `kmica:langchange` pour composants tiers
- API publique : `window.kyriosI18n.{setLang, getLang, apply}`
- Logos horizontaux (EN/FR) swappés automatiquement au changement de langue

### Design tokens (brand.css)

Palette CSS variables :
- `--marine #0A1628`, `--gold #C9972A`, `--cyan #22D3EE`
- `--earth #8B4513`, `--mahog #5C3310`, `--white #F1F5F9`, `--slate #A8BCDC`

Typographies via Google Fonts :
- `Playfair Display` (400/500/700, italic) → titres, wordmark
- `JetBrains Mono` (400/500/700) → labels techniques

Classes utilitaires : `.btn-gold`, `.btn-outline`, `.card-navy`, `.reveal`,
`.slogan-bicolor`, `.gold-rule`, etc.

### Documents (/docs/)

- Structure prête pour les PDFs réécrits avec identité v3
- Noms stables attendus (KMDOC001-Manifeste.pdf, etc.)
- Liens du site pointent déjà vers `/docs/KMDOC00X-...pdf`
- Dossier vide pour le moment, `README.md` explicatif

### Hygiène

- `README.md` complet (rôle, stack, structure, déploiement, brand kit, i18n)
- `robots.txt` + `sitemap.xml`
- `.gitignore` standard

---

## Migration depuis l'ancien dépôt

Pour remplacer le contenu existant :

1. Sur GitHub → `CYRDAV97/kyriosmica-site` → supprimer tous les fichiers existants
2. Uploader le contenu du zip `kyriosmica-site_v3.zip` décompressé
3. Commit → redéploiement automatique Vercel/Netlify

Voir `DEPLOY.md` pour la procédure détaillée.

---

*© 2026 kyriosMICA · Cyrille Egnon DAVOH*
