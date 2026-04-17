# DEPLOY.md — Procédure de déploiement v3

## Objectif

Remplacer le contenu actuel de `github.com/CYRDAV97/kyriosmica-site` par la
version v3 complète, puis vérifier le rendu sur kyriosmica.com.

---

## Méthode 1 — Interface web GitHub (recommandée)

### 1. Supprimer l'ancien contenu

1. Se connecter à `https://github.com/CYRDAV97/kyriosmica-site`
2. Pour chaque fichier à la racine : cliquer sur le fichier → icône corbeille → *Commit*
3. Pour les dossiers : si l'ancien dépôt en a (src/, public/, etc.), les ouvrir
   et supprimer leur contenu jusqu'à ce que le dépôt soit **entièrement vide**
   (ne conserver que `.git/` invisible).

> Astuce : si le dépôt est trop chargé à nettoyer fichier par fichier, créer
> une nouvelle branche `v3` puis faire une PR de remplacement total.

### 2. Uploader la v3

1. Télécharger et décompresser `kyriosmica-site_v3.zip`
2. Sur GitHub → onglet **Add file** → **Upload files**
3. Glisser-déposer **tout le contenu** du zip décompressé (fichiers + dossiers)
4. Vérifier dans la preview :
   - `index.html` à la racine
   - dossier `assets/` contient `shared/` et `logos/`
   - dossier `lab/` contient les 5 HTML
   - dossier `data/` contient `donnees.html`
   - dossier `docs/` contient `README.md` et `.gitkeep`
   - fichiers racine : `README.md`, `CHANGELOG.md`, `DEPLOY.md`, `robots.txt`, `sitemap.xml`, `.gitignore`
5. Commit message : `v3.0.0 — Brand kit v3 + restructuration multipage`
6. **Commit directly to main**

### 3. Vérifier le déploiement

Vercel (ou Netlify) détecte automatiquement le push et redéploie.

- Ouvrir https://kyriosmica.com/ → hero doit afficher la nouvelle définition MICA
- Cliquer sur **Laboratoires** dans la nav → dropdown avec 4 entrées
- Tester chaque labo → chargement propre de la page avec nav/footer partagés
- Tester le toggle **FR / EN** → textes et logo changent

---

## Méthode 2 — Git CLI (si disponible)

```bash
# Cloner
git clone https://github.com/CYRDAV97/kyriosmica-site.git
cd kyriosmica-site

# Nettoyer (garde uniquement .git/)
find . -maxdepth 1 ! -name '.git' ! -name '.' -exec rm -rf {} +

# Copier le contenu du zip décompressé dans le dépôt vide
# (assumant le zip déjà décompressé dans ~/Downloads/kyriosmica-site_v3/)
cp -r ~/Downloads/kyriosmica-site_v3/* .
cp -r ~/Downloads/kyriosmica-site_v3/.gitignore .

# Commit + push
git add -A
git commit -m "v3.0.0 — Brand kit v3 + restructuration multipage"
git push origin main
```

---

## Checklist post-déploiement

Ouvrir https://kyriosmica.com/ et vérifier :

### Page d'accueil
- [ ] Header affiche le logo horizontal (FR ou EN selon langue)
- [ ] Hero affiche la nouvelle définition : "Matrice d'Intrication Coordonnée par les Amplitudes"
- [ ] Toggle FR ⇄ EN fonctionne sur tous les textes
- [ ] Scroll → les sections de l'institut s'affichent (Science, Défi, Revue, etc.)
- [ ] Footer affiche la déf MICA bilingue
- [ ] Copyright bar : nouvelle déf MICA (pas "Mathematical Interface...")

### Groupe Laboratoires
- [ ] Clic sur **Laboratoires** dans la nav → dropdown s'ouvre
- [ ] 4 entrées : Simulateur Qudits-36, T₃-Net QNN, Qudits-36 Lab, Analyzer
- [ ] Chaque lien charge la page avec header/footer partagés
- [ ] Données de la page (Simulateur, T₃-Net, etc.) fonctionnent comme avant
- [ ] Bouton retour (logo) ramène à l'accueil

### Mobile
- [ ] Menu hamburger s'ouvre
- [ ] Dropdown Labs expanded au tap
- [ ] Toggle FR/EN accessible

### Console navigateur
- [ ] Aucune erreur 404 (polices Google, images)
- [ ] Aucune erreur JS bloquante
- [ ] Page loads en < 2s sur connexion normale

### Documents
- [ ] Page Documents affiche la liste des KMDOC
- [ ] Les liens pointent vers `/docs/KMDOC00X-....pdf` — pour l'instant 404
  (attendu tant que les PDFs ne sont pas déposés)

---

## Rollback

En cas de régression bloquante :

```bash
# Si Git CLI
git revert <SHA_v3>
git push

# Via interface web
Settings → Branches → créer une branche depuis un commit antérieur
ou utiliser l'ancien zip stocké localement
```

Vercel conserve l'historique des déploiements : *Deployments* → *Promote to production*
sur le build précédent.

---

## Coordination avec les autres dépôts

- **lab.kyriosmica.com** (dépôt `CYRDAV97/lab.kyriosmica`) : inchangé
- **api.kyriosmica.com** (dépôt `kyriosMICA/MICA-KERNEL`) : inchangé
- **site principal** (ce dépôt) : v3 livrée

Les trois dépôts restent indépendants. Aucune coordination nécessaire pour
ce déploiement.

---

## Prochaines étapes

Après déploiement v3 validé :

1. **Réécriture des PDFs** (Manifeste, Protocole, Rapport…) avec identité v3
   → déposer dans `/docs/` un par un
2. **Mise à jour lab.kyriosmica.com** : appliquer le brand kit v3 au lab
   (futurs changements, pas dans ce déploiement)
3. **Mise à jour MICA-KERNEL** : aucun changement lié au brand kit nécessaire

---

*v3.0.0 · 2026-04-16*
