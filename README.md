# AutoThunes — v2.2.0

Application web de gestion de finances personnelles. Single-file PWA, usage solo, pensée pour mobile.

---

## Architecture

- Un seul fichier : `index.html` — HTML, CSS et JavaScript embarqués
- Base de données : Supabase (source de vérité)
- Cache local : localStorage (navigation offline, résilience réseau)
- Stratégie de sync : dirty-tracking → upsert Supabase à chaque modification, pull complet au chargement

---

## Fonctionnalités

### 🏦 Tableau de bord
Soldes de tous les comptes, dernières opérations, prochaines charges récurrentes à venir.

### 💳 Saisie
Ajout rapide de dépenses, revenus et virements. Sélection du compte, catégorie, libellé (avec autocomplete sur l'historique). Support des opérations à une date passée ou future.

### 📊 Stats
Répartition des dépenses par catégorie (donut + détail), évolution mensuelle sur l'année, filtrage par compte. Navigation mois par mois.

### 🔍 Recherche dans les opérations
Filtrage en temps réel par libellé et par montant, en complément des filtres existants (compte, type, non pointés).

### ⚖️ Rapprochement bancaire
Outil de rapprochement par compte : saisie du solde relevé bancaire, comparaison avec le solde AutoThunes, affichage de l'écart et liste des opérations non pointées.

### 📥 Import CSV Société Générale
Import automatique des écritures depuis un export CSV SG (séparateur point-virgule). Détection des doublons, sélection ligne par ligne, choix de catégorie avant import. Les écritures importées apparaissent avec un fond rouge grisé jusqu'à leur pointage.

### ⚙️ Paramètres
- **Comptes** : création, édition, suppression, réordonnancement
- **Charges récurrentes** : loyer, abonnements, etc. — appliquées automatiquement le 1er du mois
- **Prêts** : suivi avec tableau d'amortissement (capital restant dû, mensualités, durée)
- **Thème** : sombre / clair / système
- **Sauvegarde / Restauration** : export JSON horodaté, import avec confirmation

---

## Supabase

**Projet :** `https://nqhcphhxdwqksgxcrafh.supabase.co`

| Table | Description |
|---|---|
| `accounts` | Comptes bancaires |
| `transactions` | Opérations (dépenses, revenus, virements) |
| `recurring_charges` | Charges récurrentes paramétrées |
| `recurring_applied` | Historique des applications mensuelles |
| `loans` | Prêts avec paramètres d'amortissement |
| `categories` | Catégories de dépenses |
| `labels` | Libellés fréquents (autocomplete) |

---

## Déploiement

L'app est un fichier HTML statique. Aucun serveur nécessaire.

Options d'hébergement :
- GitHub Pages
- Netlify Drop (glisser-déposer)
- Serveur local (`python3 -m http.server`)

**Installation PWA (mobile) :**
- iOS Safari : bouton Partager → "Sur l'écran d'accueil"
- Android Chrome : bannière automatique ou menu → "Ajouter à l'écran d'accueil"

---

## Versioning

| Version | Changement |
|---|---|
| 2.2.0 | Import CSV SG, rapprochement bancaire, recherche libellé/montant dans les opérations |
| 2.0.0 | Tagline OBE + numéro de version sur l'écran de démarrage, padding latéral des opérations |
| 1.9.6 | Masquage onglet PEA et section clé Finnhub dans les paramètres |
| 1.9.5 | Nettoyage navigation |
| 1.9.4 | Recherche ticker Finnhub dans le formulaire PEA |
| 1.9.3 | Toggle EUR/USD par position PEA, persistance `quote_currency` en DB |
| 1.9.2 | Détection automatique de devise par suffixe de ticker (`.PA`, `.AS`…) |
| 1.9.1 | Corrections UX formulaire PEA |
| 1.9.0 | Module PEA — cotations temps réel, plus/moins-value, export CSV |

---

## Notes

- Application à usage personnel et solo — pas de gestion multi-utilisateur
- Le localStorage sert de cache offline ; Supabase reste la source de vérité
- Les clés Supabase embarquées dans le HTML sont des clés publishable (lecture/écriture limitée aux règles RLS)
