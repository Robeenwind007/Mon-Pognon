# MonPognon 💰

**v1.5.8** — Application de gestion financière personnelle pour iPhone et Mac.  
Fichier HTML unique — aucune installation, aucun serveur, aucune dépendance.  
Synchronisation automatique entre appareils via Supabase.

---

## Installation

### Sur iPhone (Safari)
1. Ouvrir l'URL GitHub Pages dans **Safari**
2. Bouton **Partager** ↑ → **"Sur l'écran d'accueil"** → Ajouter
3. L'app s'ouvre en plein écran comme une app native

### Sur Mac (Chrome ou Safari)
1. Ouvrir la même URL dans le navigateur
2. Utiliser directement dans le navigateur

---

## Synchronisation entre appareils

Les données se synchronisent automatiquement via **Supabase** :
- À chaque modification → push vers Supabase (1.5s de délai)
- Au démarrage → pull depuis Supabase (source de vérité)
- Hors ligne → localStorage utilisé en cache

Un indicateur en haut à droite affiche l'état : `✓ synchronisé` / `↓ chargement…` / `⚠ hors ligne`

---

## Navigation

4 onglets + bouton FAB (➕ bas droite) pour saisir rapidement :

| Onglet | Contenu |
|---|---|
| 📊 Tableau | Soldes, dernières opérations, opérations à venir |
| 📋 Opérations | Historique complet avec filtres |
| 📈 Stats | Graphiques par période et catégorie |
| ⚙️ Paramètres | Comptes, récurrentes, apparence, sauvegarde |

---

## Fonctionnalités

### 📊 Tableau de bord

**Trois indicateurs globaux :**
- **Solde à date** — total des comptes inclus, transactions passées
- **Fin de mois estimée** — solde + récurrentes actives non pointées restantes
- **Solde récurrent net** — charges/produits actifs, non pointés, à venir ce mois

**Par compte :** solde réel / en cours / fin de mois

**Dernières opérations** — 3 dernières transactions passées

**Opérations à venir** — 5 prochaines récurrentes actives, avec :
- Badge **J-X** (jours restants)
- Case **"Comptabilisé"** — cocher pour exclure du Solde récurrent net (remise à zéro mensuelle automatique)

---

### ➕ Saisie rapide (FAB)
- 3 types : Recette / Dépense / ⇄ Virement
- Libellés intelligents avec autocomplétion et catégorie auto-associée

---

### 📋 Opérations
- Toutes les opérations saisies, triées par date décroissante
- Groupées par mois avec solde net mensuel
- Filtre par compte et par type
- Suppression avec confirmation

---

### 📈 Stats
- Filtre par compte ou global
- Périodes : Mois / Trimestre / Année avec navigation
- Virements exclus (mouvements internes)
- Donut par catégorie + histogramme 6 mois

---

### ⚙️ Paramètres

#### Comptes
- Types : Courant, PEL, LDD, Livret A, PEA, LLD, Compte Carte
- **Réordonner** avec ▲▼
- **Modifier** : nom, type, solde initial, icône (✏️)
- **Toggle "Inclus dans les totaux"** : exclure des soldes globaux
- Suppression avec confirmation

#### Opérations récurrentes
- Types : Charge / Produit / ⇄ Virement
- Toggle actif/inactif, modification complète
- Filtre par compte
- Totaux actifs + solde récurrent net

#### Apparence
- **🌙 Sombre** — thème bleu nuit (défaut)
- **☀️ Clair** — fond blanc/gris
- **⚙️ Système** — suit automatiquement le mode de l'appareil

#### Catégories
- 9 catégories système + catégories personnalisées (emoji + couleur)

#### Sauvegarde & Restauration
- Export JSON horodaté (backup manuel)
- Import JSON avec confirmation

---

## Données & confidentialité

- **Supabase** : données stockées dans un projet personnel, aucun tiers n'y a accès
- **localStorage** : cache local pour le mode hors ligne
- Aucune publicité, aucun tracking

> ⚠️ Exportez régulièrement via Paramètres → Sauvegarde en backup de sécurité.

---

## Structure du projet

```
moncompte/
├── index.html          ← Pour GitHub Pages (identique à MonCompte.html)
├── MonCompte.html      ← Application complète (source unique)
├── README.md           ← Ce fichier
├── CHANGELOG.md        ← Historique des versions
└── BACKLOG.md          ← Idées & évolutions futures
```
