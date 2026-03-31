# AutoThunes
## Documentation technique v2.1.14

---

## Présentation

AutoThunes est une Progressive Web App (PWA) de gestion financière personnelle, single-file `index.html`, déployée sur GitHub Pages avec Supabase comme backend. Pas de framework, pas de build.

---

## Architecture

| Élément | Technologie |
|---|---|
| Frontend | HTML/CSS/JS vanilla, single-file |
| Backend | Supabase (PostgreSQL) |
| Déploiement | GitHub Pages |
| Persistance locale | localStorage (cache offline) |

---

## Structure Supabase

### Tables

| Table | Rôle |
|---|---|
| `accounts` | Comptes bancaires (name, icon, type, initial_balance, include_in_total, sort_order, **closed**) |
| `transactions` | Opérations (label, amount, type, date, category, account_id, transfer_id…) |
| `recurring_charges` | Charges récurrentes (label, amount, day, type, account_id, loan_id, active…) |
| `recurring_applied` | Mois où une récurrente a été comptabilisée (recurring_charge_id, month_key) |
| `loans` | Prêts (label, amount, duration, monthly, insurance, start, first_payment…) |
| `categories` | Catégories custom |
| `labels` | Labels fréquents |
| `holdings` | PEA — positions (optionnel, table peut être absente) |

### Migrations nécessaires

```sql
-- Compte clos (ne pas supprimer, juste masquer du tableau de bord)
ALTER TABLE public.accounts ADD COLUMN IF NOT EXISTS closed boolean DEFAULT false;
```

### Notes FK importantes

La table `transactions` référence `accounts(id)` via plusieurs FK dont `transfer_from_fkey`. **Sans `ON DELETE CASCADE`**, la suppression d'un compte échoue si des transactions existent. Utiliser le bouton **"Clos"** plutôt que la suppression.

---

## Fonctionnalités

### Tableau de bord
- Solde réel et solde fin de mois par compte
- Total global (comptes inclus dans les totaux, non clos)
- Charges récurrentes restantes du mois
- Prochain prélèvement à venir
- Comptes **clos exclus** des totaux et des cartes

### Opérations
- Saisie dépense / revenu / virement
- Modification et suppression
- Pointage (comptabilisé en banque)
- Filtres : période, compte, type, catégorie

### Récurrentes
- Charges et revenus récurrents mensuels
- Application manuelle par mois avec pointage
- Lien prêt → récurrente automatique
- Onglet dédié "Récurrentes"

### Prêts
- Suivi capital restant basé sur le nombre de mensualités **réellement appliquées** (`rec.applied.length`) — pas sur le temps écoulé
- Calcul coût total (intérêts + assurance)
- Barre de progression

### Comptes
- Types : courant, épargne, PEA, livret, etc.
- Toggle "inclus dans les totaux"
- Réorganisation par flèches ▲▼
- **Bouton "Clos"** : masque le compte du tableau de bord et des selects sans le supprimer de Supabase (évite les erreurs FK)
- Comptes clos affichés en grisé/barré dans les réglages, réactivables à tout moment

### Synchronisation Supabase
- Push automatique 1,5s après modification
- Pull au démarrage
- **Suppressions pendantes persistées** dans `localStorage['mp_pending_deletes']` — survivent au rechargement de page
- Après `syncPull`, les comptes en attente de suppression sont filtrés localement
- Table `holdings` (PEA) **optionnelle** — erreur silencieuse si absente
- Erreur de sync affiche le message précis (PUSH: / PULL: + détail)

---

## Logique de synchronisation des suppressions

```
deleteAccount(id)
  → _dirty.deletedAccounts.add(id)
  → savePendingDeletes()          ← persiste dans localStorage
  → syncPush() immédiat

syncPull()
  → db.accounts filtrés des deletedAccounts pendants
  → db.transactions filtrées des accountId supprimés

syncPush() réussi
  → clearPendingDeletes()
  → _dirty reset
```

---

## Calcul capital restant prêt

```
paid = rec.applied.length   // mensualités réellement comptabilisées
capitalRemaining = amount - (amount / duration) × paid
```

Fallback sur calcul temporel si aucune charge récurrente liée.

---

## Déploiement

```bash
git clone https://github.com/Robeenwind007/autothunes.git
git add index.html
git commit -m "v2.1.14"
git push origin main
```

---

## Versioning

| Version | Date | Changements principaux |
|---|---|---|
| 2.1.14 | Mars 2026 | Compte "Clos" (masque sans supprimer), persistance suppressions pendantes localStorage, filtre syncPull sur comptes supprimés |
| 2.1.13 | Mars 2026 | Erreur sync détaillée (PUSH/PULL + message), holdings optionnel dans sync |
| 2.1.12 | Mars 2026 | Holdings optionnel, push immédiat sur suppression compte, persistance deletedAccounts |
| 2.1.11 | Mars 2026 | Correction loanStats : capital restant basé sur rec.applied.length |
| 2.1.10 | Mars 2026 | Corrections récurrentes et soldes |
| 2.0.x | Fév. 2026 | Migration Supabase multi-tables |
| 1.x | Jan. 2026 | Version initiale localStorage (MonPognon) |

---

*AutoThunes — usage personnel*
