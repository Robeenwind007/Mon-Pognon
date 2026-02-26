# MonPognon 💰
**v1.4.6** — Application de gestion financière personnelle pour iPhone.  
Fichier HTML unique — aucune installation, aucun serveur, aucune dépendance.

---

## Installation sur iPhone

### Via GitHub Pages (recommandé)
1. Héberger `index.html` sur GitHub Pages
2. Ouvrir l'URL dans **Safari** sur iPhone
3. Bouton **Partager** ↑ → **"Sur l'écran d'accueil"**
4. Nommer → **Ajouter**

L'app s'ouvre en plein écran comme une app native, avec icône personnalisée.

### En local (Mac → iPhone via AirDrop)
1. Envoyer `index.html` par AirDrop
2. Ouvrir avec Safari → même procédure

---

## Fonctionnalités

### 📊 Tableau de bord
- **Solde à date** — total des comptes inclus, transactions passées
- **Fin de mois estimée** — solde + récurrentes actives restantes
- **Solde récurrent net** — charges/produits actifs à venir ce mois
- Carte par compte : solde à date / en cours / fin de mois
- **3 dernières opérations** passées
- **5 prochaines opérations** à venir (récurrentes actives, avec J-X)

### ➕ Saisie rapide
- Bouton FAB (➕ bas droite) accessible depuis tous les onglets
- 3 types : Recette / Dépense / ⇄ Virement
- Libellés intelligents avec autocomplétion et sauvegarde

### 📋 Opérations
- Toutes les opérations non récurrentes, triées par date décroissante
- Groupées par mois avec solde net mensuel
- Filtre par compte et par type (Recette / Dépense / Virement)
- Suppression à la ligne

### 📈 Stats
- Filtrage par compte ou situation globale
- Périodes : Mois / Trimestre / Année avec navigation
- Recettes et dépenses (virements exclus — mouvements internes)
- Graphique donut par catégorie
- Histogramme des 6 derniers mois
- Détail par catégorie avec barres de progression

### ⚙️ Paramètres

#### Comptes
- Types : Courant, PEL, LDD, Livret A, PEA, LLD, Compte Carte
- Modification : nom, type, solde initial, icône
- Toggle "Inclus dans les totaux" : exclure un compte des soldes globaux
- Suppression

#### Opérations récurrentes
- Types : Charge / Produit / Virement
- Jour de prélèvement/versement exact
- Toggle actif/inactif par opération
- Modification complète
- Filtre par compte
- Totaux actifs + solde récurrent net (actifs à venir, comptes inclus)
- Application automatique au jour J

#### Catégories
- 9 catégories système (non supprimables)
- Ajout de catégories personnalisées avec emoji et couleur

#### Sauvegarde & Restauration
- Export JSON — fichier horodaté téléchargeable (ou copie sur iOS)
- Import JSON — restauration avec confirmation via modal

---

## Données & confidentialité

Toutes les données sont stockées **localement** dans le `localStorage` de Safari.  
Clé : `mc_data_v2` — aucune donnée n'est transmise sur internet.

> ⚠️ Supprimer l'icône de l'écran d'accueil **efface les données**. Exportez régulièrement.

---

## Structure

```
moncompte/
├── index.html / MonCompte.html   ← Application complète (source unique)
├── README.md                     ← Ce fichier
├── CHANGELOG.md                  ← Historique des versions
└── BACKLOG.md                    ← Idées & évolutions futures
```
