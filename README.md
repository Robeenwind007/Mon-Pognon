# MonPognon 💰

**v1.7.0** — Application de gestion financière personnelle pour iPhone et Mac.  
Fichier HTML unique — aucune installation, aucun serveur, aucune dépendance.  
Synchronisation automatique entre appareils via Supabase.

---

## Installation

### Sur iPhone (Safari)
1. Ouvrir l'URL GitHub Pages dans **Safari**
2. Bouton **Partager** ↑ → **"Sur l'écran d'accueil"** → Ajouter

### Sur Mac (Chrome ou Safari)
Ouvrir la même URL dans le navigateur.

---

## Synchronisation

Données synchronisées automatiquement via **Supabase** :
- Chaque modification → push Supabase (1.5s de délai)
- Au démarrage → pull Supabase (source de vérité)
- Hors ligne → localStorage en cache
- Bouton 🔄 en topbar pour forcer la sync

---

## Navigation

4 onglets + FAB (➕ bas droite) pour saisir :

| Onglet | Contenu |
|---|---|
| 📊 Tableau | Soldes, dernières opérations, opérations à venir |
| 📋 Opérations | Historique complet avec filtres |
| 📈 Stats | Graphiques par période et catégorie |
| ⚙️ Paramètres | Apparence, comptes, prêts, récurrentes, catégories, sauvegarde |

---

## Fonctionnalités

### 📊 Tableau de bord

- **Solde à date** — total des comptes inclus
- **Fin de mois estimée** — solde + récurrentes actives et démarrées restantes
- **Solde récurrent net** — charges/produits actifs, non pointés, à venir ce mois
- **Opérations à venir** — 5 prochaines récurrentes avec J-X et case "Comptabilisé"
- **3 dernières opérations** passées

### ➕ Saisie rapide
- Recette / Dépense / ⇄ Virement
- Libellés intelligents avec autocomplétion

### 📋 Opérations
- Historique trié par date, groupé par mois avec solde net
- Filtres compte et type, suppression avec confirmation

### 📈 Stats
- Périodes : Mois / Trimestre / Année
- Donut par catégorie + histogramme 6 mois
- Virements exclus

### ⚙️ Paramètres

#### Apparence
- 🌙 Sombre / ☀️ Clair / ⚙️ Système

#### Comptes
- Réordonner ▲▼, modifier ✏️, toggle "Inclus dans les totaux"

#### Prêts
- Saisie : libellé, compte, date début, 1ère échéance, jour, montant, durée, mensualité, assurance
- Affichage : capital restant, mensualités restantes, fin estimée, coût total, taux annuel, barre de progression
- Crée automatiquement une récurrente unique (mensualité + assurance) avec badge 🏦
- Respect de la 1ère échéance : inactif les mois précédents (badge 📅)
- Modifier ✏️ et supprimer 🗑 avec confirmation

#### Opérations récurrentes
- Charge / Produit / ⇄ Virement
- Toggle actif/inactif, modifier ✏️
- Badge 🏦 Prêt sur les récurrentes auto-générées
- Badge 📅 "démarre le..." si 1ère échéance future

#### Catégories
- 9 système + personnalisées (emoji + couleur)

#### Sauvegarde & Restauration
- Export JSON horodaté + Import avec confirmation

---

## Données

- Stockage : **Supabase** (cloud) + **localStorage** (cache offline)
- Clé localStorage : `mc_data_v2`
- Aucun tracking, aucune pub

> ⚠️ Exportez régulièrement via Paramètres → Sauvegarde.

---

## Structure

```
moncompte/
├── index.html       ← GitHub Pages
├── MonCompte.html   ← Source identique
├── README.md
├── CHANGELOG.md
└── BACKLOG.md
```
