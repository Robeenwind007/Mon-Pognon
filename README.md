# AutoThunes

> Gestion du pognon et des comptes — par OBE

Application web de gestion financière personnelle, distribuée sous forme de **fichier HTML unique** (`index.html`). Fonctionne en mode PWA (Progressive Web App), installable sur iPhone et Android, sans serveur ni dépendance externe.

---

## Fonctionnalités principales

- **Tableau de bord** — soldes réels et théoriques par compte, flux mensuel entrant/sortant
- **Saisie d'opérations** — dépenses, recettes, virements entre comptes avec navigation mensuelle
- **Charges récurrentes** — prélèvements automatiques appliqués le 1er du mois
- **Prêts** — amortissement automatique avec génération des charges récurrentes associées
- **Statistiques** — graphiques camembert, courbe de solde, barres mensuelles
- **Paramètres** — gestion des comptes (avec icône personnalisée PNG), catégories, thème clair/sombre/système, export/import JSON
- **PEA** — suivi de portefeuille boursier avec actualisation des cours
- **Synchronisation Supabase** — sauvegarde cloud optionnelle (push/pull)
- **Notice d'aide intégrée** — PDF embarqué, accessible via le bouton `?` dans le header

---

## Démarrage rapide

1. Ouvrir `index.html` dans un navigateur (Chrome, Safari, Firefox)
2. Au premier lancement, créer au moins un compte (étape d'onboarding)
3. *(Optionnel)* Configurer Supabase dans **Paramètres → Synchronisation** pour la sauvegarde cloud

Aucune installation requise. Toutes les données sont stockées dans le **localStorage** du navigateur.

---

## Architecture

| Couche | Technologie |
|---|---|
| Interface | HTML/CSS/JS natif, single-file |
| Stockage local | `localStorage` |
| Stockage cloud | Supabase (PostgreSQL) — optionnel |
| Distribution | Fichier `.html` standalone, PWA installable |

### Schéma Supabase (tables relationnelles)

`accounts` · `transactions` · `recurring_charges` · `recurring_applied` · `loans` · `categories` · `labels`

La synchronisation fonctionne en **push/pull** : le localStorage est la source de vérité locale, Supabase est la source de vérité cloud.

---

## Utilisation

### Boutons du header

| Élément | Action |
|---|---|
| `↺` (pages) | Actualise la vue courante |
| `🔄` | Force une synchronisation Supabase |
| `?` | Ouvre la notice d'aide (PDF) dans un nouvel onglet |

### Navigation

4 pages accessibles via la barre de navigation en bas :

- **Tableau** — vue d'ensemble et soldes
- **Opérations** — liste et saisie par mois
- **Stats** — graphiques
- **Paramètres** — configuration

### Saisie d'une opération

- Appui long sur une opération → swipe ou appui long pour **supprimer**
- Type sélectionnable : **Dépense** (défaut), Recette, Virement
- Séparateurs décimaux acceptés : `.` et `,`

### Modification d'une opération

- Tap sur une opération → modal d'édition
- Possibilité de **changer le type** (Dépense ↔ Recette ↔ Virement) avec gestion automatique des conversions

---

## Changelog

### v2.1.4
- Icône de compte personnalisable : bouton 📁 pour importer un PNG/JPEG/WEBP depuis l'ordinateur
- L'image est stockée en base64 et affichée dans toute l'interface (dashboard, liste des comptes)
- Dans les menus déroulants, une image perso est représentée par 🖼

### v2.1.3
- Opérations récurrentes dans la liste : titre en grisé italique, montant en italique atténué

### v2.1.2
- Opérations récurrentes visibles dans la liste des opérations (mélangées aux autres)
- Badge `🔄 récurrent` dans le sous-titre pour les identifier
- Seules les récurrentes déjà générées sont affichées (pas les futures en attente)

### v2.1.1
- Notice d'aide PDF embarquée dans le fichier HTML
- Bouton `?` ajouté dans le header à côté de la synchronisation

### v2.1.0
- Formulaire de saisie : **Dépense** en premier et sélectionnée par défaut
- Modal d'édition : toggle Dépense / Recette / Virement avec gestion complète des conversions de type
- Modal d'édition : alignement CSS sur le style du sheet de saisie

### v2.0.0
- Refonte architecture sync (localStorage-first + Supabase comme source de vérité cloud)
- Migration vers schéma relationnel Supabase (7 tables)
- Gestion des prêts avec amortissement et génération automatique de charges récurrentes
- Correctifs : validation formulaires, séparateurs décimaux, race conditions sync, refresh DOM

---

## Fichiers

| Fichier | Description |
|---|---|
| `index.html` | Application complète (source unique) |
| `CHANGELOG.md` | Historique des versions |
| `notice_autothunes.pdf` | Notice d'utilisation (aussi embarquée dans l'app) |

---

## Notes techniques

- **PWA** : les `alert()` natifs ne fonctionnent pas dans certains contextes PWA — l'app utilise ses propres modals
- **Virements** : chaque virement crée deux opérations liées par un `transferId`
- **Charges récurrentes** : appliquées automatiquement à l'ouverture si le mois courant n'a pas encore été traité
- **Export/Import** : format JSON, accessible dans Paramètres — permet de migrer ou sauvegarder manuellement
