# AutoThunes — v2.1.10

PWA de gestion financière personnelle. Application mono-fichier `index.html`, déployée sur GitHub Pages, avec synchronisation Supabase.

---

## Stack technique

| Élément | Détail |
|---|---|
| Format | Single-file HTML (CSS + JS inline) |
| Hébergement | GitHub Pages |
| Backend | Supabase (PostgreSQL + REST API) |
| Police | Sora / DM Mono (Google Fonts) |
| PWA | Manifest + icône Apple Touch, standalone |

---

## Structure Supabase

Cinq tables principales chargées en parallèle au démarrage :

| Table | Rôle |
|---|---|
| `accounts` | Comptes bancaires (solde initial, type, icône, ordre) |
| `transactions` | Opérations manuelles et récurrentes générées |
| `recurring_charges` | Définitions des charges/produits/virements récurrents |
| `categories` | Catégories custom (en plus des catégories système) |
| `labels` | Libellés sauvegardés avec compteur d'usage |

Relations : `transactions.accountId → accounts.id` (ON DELETE CASCADE), idem pour `recurring_charges`.

---

## Navigation (5 onglets)

| Onglet | Rôle |
|---|---|
| **Tableau** | Dashboard global + liste des comptes avec EN COURS / FIN DE MOIS |
| **Opérations** | Saisie et historique des transactions par compte et par mois |
| **Stats** | Graphiques par catégorie et période |
| **Récurrentes** | Gestion des charges/produits/virements récurrents |
| **Paramètres** | Comptes, thème, import/export, backup JSON |

---

## Types de comptes

`courant` 🏦 · `pel` 🏠 · `ldd` 🌱 · `livreta` 📗 · `pea` 📈 · `lld` 🚗 · `carte` 💳

---

## Logique des soldes

### Solde à date (`accountBalance`)
Solde initial + toutes les transactions dont `date ≤ aujourd'hui` et `unpointed = false`.

### Solde fin de mois (`accountMonthBalance`)
Solde à date + opérations manuelles futures du mois + récurrentes **en attente**.

**Définition "en attente"** (`isPending`) :
```js
r.day > todayDay
|| (r.day === todayDay && !(r.pointed && r.pointed[monthKey]))
```
Une récurrente du jour est en attente si elle n'est pas encore pointée (= pas encore en banque).

### EN COURS (différence dans Tableau > compte)
`FIN DE MOIS − SOLDE À DATE` = somme nette des récurrentes encore en attente ce mois.

---

## Récurrentes — cycle de vie

```
Créée → active, non pointée
   │
   ├─ Jour non encore arrivé     → dans les pending (compte dans FIN DE MOIS)
   │
   ├─ Jour = aujourd'hui
   │     ├─ Non pointée          → dans les pending (en suspens)
   │     └─ Pointée              → hors pending
   │
   └─ Jour passé
         ├─ applied = false      → applyRecurringIfNeeded() crée la transaction
         │                          + pointed[monthKey] = true
         └─ applied = true       → transaction déjà créée, pointed = true
```

### Pointage (`r.pointed[monthKey]`)
- `false` ou absent : récurrente **en attente**, comptée dans FIN DE MOIS
- `true` : récurrente **passée**, exclue des pending (la transaction est dans le solde réel)

### Mise à jour du pointage
| Action | Effet |
|---|---|
| `forceApplyRecurring()` — "Comptabiliser maintenant" | Crée la transaction + `pointed = true` + `applied.push()` |
| `applyRecurringIfNeeded()` — auto au démarrage | Crée la transaction + `pointed = true` + `applied.push()` |
| `togglePointed()` — case à cocher dans Récurrentes | Bascule `pointed` (bloqué si déjà `applied`) |
| `toggleTransactionPointing()` — "Comptabilisé en banque" dans Opérations | `unpointed` sur la transaction + remonte sur `r.pointed` via ID `rec-{id}-{monthKey}` |

---

## Format des IDs de transactions récurrentes

```
rec-{recurringId}-{YYYY-MM}          → charge / produit
rec-{recurringId}-{YYYY-MM}_out      → virement sortant
rec-{recurringId}-{YYYY-MM}_in       → virement entrant
```

Ce format permet à `toggleTransactionPointing` de retrouver la récurrente parente et de synchroniser `pointed`.

---

## Thèmes

Trois thèmes CSS via variables `:root` :

| Classe | Mode |
|---|---|
| *(défaut)* | Sombre |
| `.theme-light` | Clair |
| `.theme-system` | Suit `prefers-color-scheme` |

---

## Déploiement

1. Modifier `index.html`
2. Incrémenter `APP_VERSION` (constante JS)
3. Pousser sur la branche GitHub Pages
4. *(optionnel)* Mettre à jour `README.md`

---

## Historique des versions récentes

| Version | Changement |
|---|---|
| 2.1.10 | `toggleTransactionPointing` synchronise `r.pointed` sur la récurrente parente → supprime le double comptage lors du pointage depuis Opérations |
| 2.1.9 | `forceApplyRecurring` et `applyRecurringIfNeeded` posent `pointed = true` ; `togglePointed` bloqué si déjà `applied` |
| 2.1.8 | Correction `isPending` : récurrentes du jour non pointées incluses dans les calculs FIN DE MOIS et EN COURS |
