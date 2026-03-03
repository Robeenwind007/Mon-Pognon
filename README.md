# AutoThunes — v1.9.3

Application web de gestion de finances personnelles. Single-file PWA, usage solo, pensée pour mobile.

---

## Architecture

- **Un seul fichier** : `index_15.html` — HTML, CSS et JavaScript embarqués
- **Base de données** : Supabase (source de vérité)
- **Cache local** : `localStorage` (navigation offline, résilience réseau)
- **Stratégie de sync** : dirty-tracking → upsert Supabase à chaque modification, pull complet au chargement

---

## Fonctionnalités

### 🏦 Tableau de bord
Soldes de tous les comptes, dernières opérations, prochaines charges récurrentes à venir.

### 💳 Saisie
Ajout rapide de dépenses, revenus et virements. Sélection du compte, catégorie, libellé (avec autocomplete sur l'historique). Support des opérations à une date passée ou future.

### 📊 Stats
Répartition des dépenses par catégorie (donut + détail), évolution mensuelle sur l'année, filtrage par compte. Navigation mois par mois.

### ⚙️ Paramètres
- **Comptes** : création, édition, suppression, réordonnancement
- **Charges récurrentes** : loyer, abonnements, etc. — appliquées automatiquement le 1er du mois
- **Prêts** : suivi avec tableau d'amortissement (capital restant dû, mensualités, durée)
- **Thème** : sombre / clair / système
- **Sauvegarde / Restauration** : export JSON horodaté, import avec confirmation
- **Clé Finnhub** : saisie et test de la clé API

### 📈 PEA
Portefeuille boursier avec cotations en temps réel.
- Actions, ETF, FCP, Crypto
- Prix de revient, valeur actuelle, plus/moins-value (€ et %)
- Cotation en **EUR** (Euronext) ou **USD** (NYSE/Nasdaq) — choix par position
- Conversion USD→EUR automatique via [Frankfurter API](https://www.frankfurter.app)
- Export CSV du portefeuille

---

## APIs externes

| Service | Usage | Clé requise |
|---|---|---|
| [Finnhub](https://finnhub.io) | Cotations actions et ETF | ✅ Oui (gratuite) |
| [CoinGecko](https://coingecko.com) | Cotations crypto | Non |
| [Frankfurter](https://www.frankfurter.app) | Taux de change EUR/USD | Non |

La clé Finnhub se saisit dans **Paramètres → Clé API Finnhub**. Elle est stockée en `localStorage` uniquement.

### Formats de tickers Finnhub
- Euronext Paris : `AI.PA`, `TTE.PA`, `CW8.PA` → choisir **EUR** dans le formulaire
- NYSE / Nasdaq : `AAPL`, `NVDA`, `MSFT` → choisir **USD** (converti automatiquement en €)
- Crypto (CoinGecko ID) : `bitcoin`, `ethereum`, `solana` → toujours EUR

---

## Supabase

**Projet** : `https://nqhcphhxdwqksgxcrafh.supabase.co`

### Tables

| Table | Description |
|---|---|
| `accounts` | Comptes bancaires |
| `transactions` | Opérations (dépenses, revenus, virements) |
| `recurring_charges` | Charges récurrentes paramétrées |
| `recurring_applied` | Historique des applications mensuelles |
| `loans` | Prêts avec paramètres d'amortissement |
| `categories` | Catégories de dépenses |
| `labels` | Libellés fréquents (autocomplete) |
| `holdings` | Positions PEA |

### Schéma `holdings`

```sql
CREATE TABLE holdings (
  id             TEXT PRIMARY KEY,
  ticker         TEXT NOT NULL,
  name           TEXT,
  asset_type     TEXT DEFAULT 'stock',
  quantity       NUMERIC,
  avg_price      NUMERIC,
  currency       TEXT DEFAULT 'EUR',
  quote_currency TEXT NOT NULL DEFAULT 'EUR',  -- EUR ou USD
  last_price     NUMERIC,
  last_change    NUMERIC,
  last_update    TIMESTAMPTZ,
  exchange       TEXT,
  created_at     TIMESTAMPTZ DEFAULT NOW()
);
```

---

## Migrations

### v1.9.3 — Ajout `quote_currency`

```sql
ALTER TABLE holdings
  ADD COLUMN IF NOT EXISTS quote_currency TEXT NOT NULL DEFAULT 'EUR';
```

Fichier fourni : `migration_holdings_quote_currency.sql`

---

## Déploiement

L'app est un fichier HTML statique. Aucun serveur nécessaire.

**Options d'hébergement** :
- GitHub Pages
- Netlify Drop (glisser-déposer)
- Serveur local (`python3 -m http.server`)
- Directement depuis le système de fichiers (ouverture via `file://`)

**Installation PWA** (mobile) :
- iOS Safari : bouton Partager → "Sur l'écran d'accueil"
- Android Chrome : bannière automatique ou menu → "Ajouter à l'écran d'accueil"

---

## Versioning

| Version | Changement |
|---|---|
| 1.9.3 | Toggle EUR/USD par position PEA, persistance `quote_currency` en DB |
| 1.9.2 | Détection automatique de devise par suffixe de ticker (.PA, .AS…) |
| 1.9.1 | Corrections UX formulaire PEA |
| 1.9.0 | Module PEA — cotations temps réel, plus/moins-value, export CSV |

---

## Notes

- Application à usage **personnel et solo** — pas de gestion multi-utilisateur
- Le fichier `localStorage` sert de cache offline ; Supabase reste la source de vérité
- Les clés Supabase embarquées dans le HTML sont des clés **publishable** (lecture/écriture limitée aux règles RLS)
