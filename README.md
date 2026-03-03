# 💰 AutoThunes

Application de gestion de finances personnelles — PWA single-file, offline-first, synchronisée via Supabase.

---

## ✨ Fonctionnalités

- **Multi-comptes** — courant, PEL, LDD, Livret A, PEA, carte…
- **Transactions** — revenus, dépenses, virements entre comptes
- **Opérations récurrentes** — prélèvements, revenus, virements mensuels automatiques
- **Prêts** — suivi d'amortissement avec mensualité + assurance
- **Statistiques** — graphiques par catégorie et par période
- **Catégories** — système + custom (emoji + couleur)
- **Libellés** — autocomplétion intelligente avec fréquence d'utilisation
- **Thème clair/sombre**
- **PWA** — installable sur iPhone (Safari) et Android/Chrome
- **Offline-first** — fonctionne sans connexion, sync automatique dès le retour en ligne

---

## 🏗️ Architecture

```
AutoThunes
├── index.html          ← Application complète (HTML + CSS + JS)
├── migration.html      ← Outil de migration localStorage → Supabase (usage unique)
├── schema.sql          ← Script de création des tables Supabase
├── schema_update.sql   ← Script de mise à jour du schéma (colonnes additionnelles)
└── README.md
```

### Stockage des données

| Couche | Rôle |
|--------|------|
| **Supabase** | Source de vérité — BDD PostgreSQL cloud |
| **localStorage** | Cache offline — données disponibles sans connexion |
| **Mémoire (`db`)** | Cache runtime — toutes les opérations se font en mémoire |

### Flux de synchronisation

- **Au démarrage** : `syncPull()` charge toutes les tables Supabase en parallèle
- **À chaque modification** : `save()` → localStorage immédiat + `syncPush()` différé 1,5s
- **`syncPush()`** n'envoie que les entités réellement modifiées (système `_dirty`)
- **Hors ligne** : l'app fonctionne normalement, le statut affiche "⚠ hors ligne"

---

## 🚀 Déploiement

### Prérequis

- Un compte [Supabase](https://supabase.com) (gratuit)
- Un repo GitHub + GitHub Pages activé

### 1. Créer la base de données

Dans **Supabase > SQL Editor**, exécuter dans l'ordre :

```sql
-- Étape 1 : créer les tables
-- (contenu de schema.sql)

-- Étape 2 : ajouter les colonnes additionnelles
-- (contenu de schema_update.sql)
```

### 2. Configurer les credentials

Dans `index.html`, modifier les constantes en haut du bloc JavaScript :

```javascript
const SB_URL = 'https://VOTRE-PROJET.supabase.co';
const SB_KEY = 'votre-clé-anon-publique';
```

> Ces valeurs se trouvent dans **Supabase > Settings > API**

### 3. Déployer sur GitHub Pages

```bash
git add index.html
git commit -m "deploy"
git push
```

Activer GitHub Pages sur la branche `main` dans **Settings > Pages**.

### 4. Installer en PWA sur iPhone

1. Ouvrir l'URL GitHub Pages dans **Safari**
2. Tap **Partager** → **Sur l'écran d'accueil**
3. L'app s'installe comme une app native

---

## 🔄 Migration depuis localStorage

Si tu avais des données dans l'ancienne version (localStorage) :

1. Ouvrir `migration.html` dans le **même navigateur** que l'ancienne app
2. Vérifier le résumé des données détectées
3. Cliquer **Démarrer la migration**
4. Une fois terminé, utiliser `index.html` normalement

> ⚠️ La migration est non-destructive : le localStorage n'est pas supprimé.

---

## 📊 Schéma de la base de données

| Table | Description |
|-------|-------------|
| `accounts` | Comptes bancaires |
| `transactions` | Toutes les opérations (revenus, dépenses, virements) |
| `recurring_charges` | Opérations récurrentes mensuelles |
| `recurring_applied` | Suivi des mois où les récurrentes ont été appliquées |
| `loans` | Prêts avec amortissement |
| `categories` | Catégories système + custom |
| `labels` | Libellés sauvegardés pour l'autocomplétion |

---

## 🔧 Mise à jour de l'app

### Depuis Mac/PC

1. Remplacer `index.html` dans le repo
2. `git push`

### Sur iPhone

1. Fermer la PWA complètement (swipe up)
2. Rouvrir — elle rechargera automatiquement la nouvelle version

> Pas de service worker, la mise à jour est immédiate.

---

## 📝 Changelog

### v1.8.7 — Migration Supabase (tables normalisées)
- Remplacement du système de sync blob (`sync_data`) par des tables normalisées
- Chaque entité (compte, transaction, récurrente…) est stockée dans sa propre table
- Sync différentiel : seules les entités modifiées sont envoyées à Supabase
- Ajout des colonnes : `accounts.sort_order`, `recurring_charges.active`, `start_month_key`, `pointed`

### v1.8.6 et antérieures
- Gestion multi-comptes, prêts, récurrentes, stats, catégories custom
- PWA offline-first avec sync Supabase (ancien système blob)
