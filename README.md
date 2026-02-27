# MonPognon 💰

**v1.4.9** — Application de gestion financière personnelle pour iPhone.  
Fichier HTML unique — aucune installation, aucun serveur, aucune dépendance.

---

## Installation sur iPhone

### Via GitHub Pages (recommandé)
1. Héberger `index.html` sur un dépôt GitHub public
2. Activer GitHub Pages : Settings → Pages → Branch: main → Save
3. Ouvrir l'URL générée dans **Safari** sur iPhone
4. Bouton **Partager** ↑ → **"Sur l'écran d'accueil"** → Ajouter

L'app s'ouvre en plein écran comme une app native, avec icône personnalisée.

### En local (Mac → iPhone via AirDrop)
1. Envoyer `index.html` par AirDrop
2. Ouvrir avec Safari → même procédure qu'au-dessus

---

## Navigation

4 onglets principaux + bouton FAB (➕ bas droite) pour saisir rapidement :

| Onglet | Contenu |
|---|---|
| 📊 Tableau | Soldes, dernières opérations, opérations à venir |
| 📋 Opérations | Historique complet avec filtres |
| 📈 Stats | Graphiques par période et catégorie |
| ⚙️ Paramètres | Comptes, récurrentes, catégories, sauvegarde |

---

## Fonctionnalités

### 📊 Tableau de bord

**Trois indicateurs globaux :**
- **Solde à date** — total des comptes inclus, toutes transactions passées
- **Fin de mois estimée** — solde à date + récurrentes actives non pointées restantes
- **Solde récurrent net** — charges/produits actifs, non pointés, à venir ce mois (comptes inclus uniquement)

**Par compte :**
- Solde réel à date
- Solde "en cours" (avec transactions futures saisies)
- Fin de mois estimée

**Dernières opérations** — 3 dernières transactions passées

**Opérations à venir** — 5 prochaines récurrentes actives ce mois, triées par jour, avec :
- Badge **J-X** (jours restants)
- Case **"Comptabilisé"** : cocher pour exclure du Solde récurrent net (pointage mensuel, se remet à zéro le mois suivant)

---

### ➕ Saisie rapide (FAB)

Accessible depuis tous les onglets via le bouton ➕ en bas à droite.

- **3 types** : Recette / Dépense / ⇄ Virement
- **Libellés intelligents** : autocomplétion sur l'historique, sauvegarde à la volée, catégorie auto-associée
- Date modifiable

---

### 📋 Opérations

Toutes les opérations saisies (non récurrentes), triées par date décroissante.

- Groupées par mois avec **solde net mensuel**
- **Filtre par compte** et par type (Recette / Dépense / Virement)
- Virements affichés comme ligne unique (jambe sortante)
- Suppression à la ligne (virements : suppression des deux jambes)

---

### 📈 Stats

- **Filtre par compte** ou situation globale
- **Périodes** : Mois / Trimestre / Année avec navigation ‹ ›
- Recettes et dépenses (**virements exclus** — mouvements internes)
- **Graphique donut** par catégorie de dépenses
- **Histogramme** des 6 derniers mois (recettes vs dépenses)
- Détail par catégorie avec barres de progression

---

### ⚙️ Paramètres

#### Comptes
- **Types** : Courant, PEL, LDD, Livret A, PEA, LLD, Compte Carte (icône auto)
- **Réordonner** avec ▲▼ — l'ordre est respecté dans le tableau de bord
- **Modifier** : nom, type, solde initial, icône (bouton ✏️)
- **Toggle "Inclus dans les totaux"** : exclure un compte du Solde à date, Fin de mois et Solde récurrent net
- **Supprimer** (supprime aussi les transactions et récurrentes associées)

#### Opérations récurrentes
- **3 types** : Charge / Produit / ⇄ Virement
- **Jour** de prélèvement/versement exact dans le mois
- **Toggle actif/inactif** — les inactives sont grisées et exclues des calculs
- **Modifier** : libellé, montant, compte, catégorie, jour (bouton ✏️)
- **Filtre par compte** (virements inclus si compte source ou destination)
- **Totaux** : charges actives du mois / produits actifs / solde récurrent net (non pointés, à venir)
- Application automatique au 1er du mois

#### Catégories
- 9 catégories système prédéfinies (non supprimables)
- Ajout de catégories personnalisées avec emoji et couleur
- Suppression des catégories personnalisées

#### Sauvegarde & Restauration
- **Export JSON** : fichier horodaté téléchargeable (copie dans le presse-papier sur iOS)
- **Import JSON** : restauration complète avec modal de confirmation
- Aucune donnée envoyée sur internet

---

## Données & confidentialité

Toutes les données sont stockées **localement** dans le `localStorage` de Safari.  
Clé de stockage : `mc_data_v2`

> ⚠️ **Important** : supprimer l'app de l'écran d'accueil **efface les données**. Exportez régulièrement via Paramètres → Sauvegarde.

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

---

## Modèle de données

```json
{
  "accounts": [
    {
      "id": "1700000000000",
      "name": "LCL Courant",
      "icon": "🏦",
      "type": "courant",
      "initialBalance": 1500,
      "includeInTotal": true
    }
  ],
  "transactions": [
    {
      "id": "1700000000001",
      "label": "Courses",
      "amount": 85.50,
      "accountId": "1700000000000",
      "category": "food",
      "date": "2026-02-15",
      "type": "expense",
      "isRecurring": false
    }
  ],
  "recurringCharges": [
    {
      "id": "1700000000002",
      "label": "Loyer",
      "amount": 900,
      "accountId": "1700000000000",
      "category": "housing",
      "day": 5,
      "type": "expense",
      "active": true,
      "pointed": { "2026-02": true },
      "applied": ["2026-02"]
    }
  ],
  "categories": [],
  "labels": []
}
```
