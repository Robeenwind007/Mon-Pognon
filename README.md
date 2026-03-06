# 💰 AutoThunes — Mon-Pognon

> Gestion du pognon et des comptes par OBE

Application web de gestion financière personnelle, hébergée en tant que PWA sur GitHub Pages.  
Architecture : fichier unique `index.html` · Base de données : Supabase · Version : **v2.1.5**

---

## 🚀 Accès

**URL** : [https://robeenwind007.github.io/Mon-Pognon/](https://robeenwind007.github.io/Mon-Pognon/)

---

## 📋 Fonctionnalités

### Tableau de bord
- Solde réel et théorique global ou par compte
- Carte synthèse des prêts actifs
- **5 dernières opérations** (transactions manuelles + récurrentes) avec logo du compte
- 5 prochaines opérations récurrentes à venir avec comptabilisation

### Opérations (Saisie)
- Ajout rapide : dépense / recette / virement
- **Type par défaut : Dépense** à chaque ouverture du formulaire
- Logo du compte dans le sous-texte de chaque ligne
- Groupement par mois avec total mensuel
- Filtres par compte et par type
- Badge `🔄 récurrent` pour les opérations issues des récurrentes
- Modification et suppression

### Statistiques
- Sélection de période : semaine / mois / trimestre / année
- Filtre par compte
- Graphique donut par catégorie
- Histogramme 6 mois revenus/dépenses
- **Détail par catégorie dépliable** : clic sur une catégorie pour voir chaque opération individuelle (label, date, compte, montant)

### Opérations récurrentes
- Charges, produits, virements récurrents
- Toggle actif/inactif
- Date de début paramétrable
- Logo du compte dans la liste
- Total charges actives, total produits actifs, solde récurrent net du mois

### Prêts
- Suivi capital restant, mensualités, assurance
- Barre de progression
- Taux annuel approximatif
- Fin estimée
- Logo du compte dans les cartes prêt

### Comptes
- Création avec emoji ou image personnalisée (base64)
- Types : courant, épargne, livret, investissement…
- Solde réel / théorique

### Réglages
- Thème **Sombre / Clair / Système** — appliqué à tous les onglets en temps réel
- Catégories personnalisables
- Synchronisation Supabase

---

## 🎨 Thèmes

| Mode | Description |
|------|-------------|
| 🌙 Sombre | Thème dark par défaut |
| ☀️ Clair | Fond clair, texte sombre |
| ⚙️ Système | Suit la préférence OS |

Le thème s'applique immédiatement à **tous les onglets** lors du changement.

---

## 🗂️ Historique des versions

| Version | Date | Changements |
|---------|------|-------------|
| **v2.1.5** | 2026-03-06 | Fix icônes base64 dans selects/formulaires édition · 5 dernières opérations sur tableau de bord incluant récurrentes · Détail par catégorie dépliable dans Stats · Type "Dépense" par défaut à l'ouverture du formulaire · Thème appliqué à tous les onglets · Fix bottom-nav en mode clair · Logo compte dans les opérations |
| v2.1.4 | 2026-03 | Logo compte dans les opérations (Tableau, Saisie, À venir) · Fix icônes base64 prêts et Tableau |
| v2.1.3 | 2026-03 | Fix icônes base64 dans la liste des récurrentes |
| v2.0.0 | 2026-03 | Suppression onglet PEA · Tagline splash screen · Nettoyage dépôt |
| v1.x | — | Versions initiales |

---

## 🛠️ Déploiement

1. Générer / modifier `index.html`
2. Renommer en `index.html`
3. Uploader via l'interface web GitHub dans le dépôt `Robeenwind007/Mon-Pognon`
4. GitHub Pages publie automatiquement

---

## 🗄️ Stack technique

| Composant | Technologie |
|-----------|-------------|
| Frontend | HTML5 · CSS3 · Vanilla JS (ES6+) |
| Police | Sora · DM Mono (Google Fonts) |
| Base de données | Supabase (PostgreSQL) |
| Hébergement | GitHub Pages |
| Mode PWA | `manifest.json` + Service Worker |
