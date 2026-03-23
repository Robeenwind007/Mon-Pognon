# AutoThunes — v2.1.7

Gestionnaire de finances personnelles sous forme de PWA single-file, développé par OBE.

---

## Présentation

AutoThunes est une application web progressive (PWA) autonome — un seul fichier `index.html` — conçue pour gérer comptes bancaires, transactions, charges récurrentes, prêts et placements (PEA). Les données sont synchronisées avec un backend Supabase et mises en cache localement via `localStorage`.

---

## Fonctionnalités principales

### Comptes
- Création de comptes avec icône personnalisée (PNG/JPEG/WEBP stocké en base64)
- Types : courant, épargne, PEA, crédit
- Solde initial paramétrable
- Inclusion/exclusion du solde global
- Solde réel affiché sur le dashboard
- Solde projeté fin de mois (récurrentes + opérations futures incluses)

### Transactions
- Saisie manuelle de dépenses, recettes et virements
- Catégorisation par icône et couleur
- Étiquettes libres (labels) avec auto-complétion
- Transactions futures affichées en italique avec badge « À VENIR »
- Groupement par mois dans l'onglet Opérations
- Modification et suppression

### Charges récurrentes
- Définition d'une charge, recette ou virement mensuel avec jour de prélèvement
- Application automatique au dépassement du jour prévu (`applyRecurringIfNeeded`)
- Activation/désactivation par toggle
- Date de démarrage optionnelle (`startMonthKey`)
- **⚡ Comptabilisation par anticipation** : bouton sur les récurrentes non encore appliquées ce mois, pour forcer la comptabilisation au jour actuel (cas d'un prélèvement en avance sur le calendrier)
- Badge `🔄 récurrent` sur les lignes de l'onglet Opérations

### Pointage bancaire
- Chaque transaction récurrente passée affiche une checkbox **Comptabilisé en banque** (cochée par défaut)
- Décocher exclut la transaction du solde réel (`accountBalance`) — utile quand l'écriture est dans l'app mais pas encore passée en banque
- L'état est persisté en local et synchronisé en Supabase (`is_unpointed`)
- Pour les virements récurrents, les deux jambes sont synchronisées ensemble

### Prêts
- Suivi d'un prêt avec capital, taux, durée
- Génération automatique d'une récurrente mensuelle associée
- Tableau d'amortissement

### PEA / Placements
- Suivi des lignes de portefeuille avec prix d'achat et quantité
- Cotation en temps réel via Finnhub (clé API configurable)
- Valorisation et plus/moins-value affichées

### Dashboard
- Solde réel et solde fin de mois par compte
- Prochaines opérations à venir (récurrentes actives du mois)
- Solde récurrent net (charges et produits restant ce mois)

### Stats
- Graphique donut par catégorie de dépenses
- Graphique barre mensuel par compte
- Navigation par mois, trimestre ou année

---

## Architecture technique

| Aspect | Détail |
|---|---|
| Format | Single-file HTML PWA |
| Stockage local | `localStorage` (cache) |
| Backend | Supabase (PostgreSQL) |
| Sync | Push dirty records + Pull complet au démarrage |
| UI Framework | Vanilla JS + CSS custom properties |
| Polices | DM Sans, DM Mono (Google Fonts) |

### Supabase

URL projet : `https://nqhcphhxdwqksgxcrafh.supabase.co`

Tables utilisées :

```
accounts
transactions
recurring_charges
recurring_applied
loans
categories
labels
holdings
```

#### Migration requise (v2.1.6+)

Ajouter la colonne de pointage bancaire sur les transactions :

```sql
ALTER TABLE transactions ADD COLUMN IF NOT EXISTS is_unpointed boolean DEFAULT false;
```

---

## Historique des versions

### v2.1.7
- Libellé du pointage renommé : **Comptabilisé en banque** / **Non comptabilisé en banque**

### v2.1.6
- **Pointage bancaire** : checkbox sur chaque transaction récurrente passée pour l'inclure/exclure du solde réel
- Ajout du champ `is_unpointed` dans la synchronisation Supabase
- Les virements récurrents synchronisent les deux jambes ensemble

### v2.1.5
- **Opérations récurrentes visibles** dans l'onglet Opérations (bug : filtre `!isRecurring` retiré)
- Badge `🔄 récurrent` sur les lignes concernées
- **Bouton ⚡ Comptabiliser maintenant** sur les récurrentes non appliquées ce mois (onglet Récurrentes)
- Statut basé sur `applied[]` plutôt que sur le seul jour calendaire

### v2.1.4 et antérieures
- Mise en place de l'architecture Supabase normalisée
- Gestion des prêts avec récurrente auto-générée
- PEA avec cotations Finnhub
- Icônes de compte en base64
- Transactions récurrentes inline avec badge 🔄 et style italique
- Onglet dédié Récurrentes dans la nav
- Solde projeté fin de mois

---

## Déploiement

L'application se déploie en copiant `index.html` sur n'importe quel hébergement statique (GitHub Pages, Netlify, etc.). Aucune dépendance serveur.

La clé Supabase anon est intégrée dans le fichier. La synchronisation démarre automatiquement au chargement si une session utilisateur est active.
