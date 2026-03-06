# 📖 Guide utilisateur — AutoThunes v2.1.5

---

## 🏠 Tableau de bord

L'écran principal donne une vue synthétique de vos finances.

### Cartes de solde
- **Solde réel global** : somme de tous vos comptes actifs
- Appuyez sur une carte pour voir le détail réel / théorique d'un compte spécifique

### Prêts actifs
- Chaque prêt affiche le capital restant et la progression
- Le logo du compte débité est visible sous le nom du prêt

### Dernières opérations
- Affiche les **5 dernières opérations** passées (aujourd'hui inclus)
- Inclut les **opérations récurrentes** automatiques (marquées `🔄 récurrent`)
- Le logo de la banque concernée apparaît dans chaque ligne

### Opérations à venir
- Les 5 prochaines échéances récurrentes du mois en cours
- Compteur **J-X** pour savoir dans combien de jours
- Case **Comptabilisé** pour pointer une opération déjà passée en banque

---

## ➕ Saisir une opération

Appuyez sur le bouton **+** en bas de l'écran.

1. Le formulaire s'ouvre en mode **Dépense** par défaut
2. Choisissez le type : `− Dépense` / `+ Recette` / `⇄ Virement`
3. Renseignez : libellé, montant, compte, catégorie, date
4. Pour un virement : choisissez aussi le compte de destination
5. Appuyez sur **Enregistrer**

> 💡 Le type revient toujours à **Dépense** à chaque nouvelle ouverture du formulaire.

---

## 📊 Statistiques

### Filtres
- **Période** : Semaine · Mois · Trimestre · Année — naviguez avec `<` et `>`
- **Compte** : filtrez par compte ou affichez tout

### Graphiques
- **Donut** : répartition des dépenses par catégorie
- **Histogramme** : revenus vs dépenses sur les 6 derniers mois

### Détail par catégorie

Chaque ligne de catégorie est **cliquable** :

- Appuyez sur une catégorie pour **déplier** la liste des opérations qui la composent
- Chaque ligne détail affiche : libellé · date · logo du compte · montant
- Appuyez à nouveau pour **replier**
- La flèche `▼` indique l'état ouvert/fermé

---

## 🔄 Opérations récurrentes

Accessible depuis l'onglet **Réglages → Opérations récurrentes**.

### Ajouter une récurrente
1. Appuyez sur `+ Ajouter une opération récurrente`
2. Choisissez : Charge / Produit / Virement
3. Renseignez libellé, montant, compte, catégorie, jour du mois
4. Optionnel : définissez un **mois de début** si l'opération ne démarre pas ce mois-ci

### Gérer les récurrentes
- **Toggle** (interrupteur) : activer / suspendre sans supprimer
- **✏️** : modifier les détails
- **🗑** : supprimer définitivement

### Indicateurs
- `✓ passé(e)` : le jour est déjà passé ce mois
- `⏳ à venir` : le jour n'est pas encore arrivé
- `📅 démarre le…` : opération future non encore active
- `⏸ inactif` : opération suspendue

### Totaux affichés
- Total produits actifs du mois
- Total charges actives du mois
- **Solde récurrent net** : ce qu'il reste à passer d'ici la fin du mois

---

## 🏦 Prêts

Accessible depuis **Réglages → Prêts**.

Chaque prêt affiche :
- Capital restant
- Mensualités restantes / durée totale
- Mensualité + assurance
- Fin estimée
- Coût total du crédit
- Taux annuel approximatif

> Les prêts génèrent automatiquement une opération récurrente mensuelle liée.

---

## 🎨 Changer de thème

Dans **Réglages → Apparence** :

| Bouton | Effet |
|--------|-------|
| 🌙 Sombre | Fond sombre (défaut) |
| ☀️ Clair | Fond blanc, texte foncé |
| ⚙️ Système | Suit le mode de votre téléphone/ordinateur |

Le thème s'applique **immédiatement** à tous les onglets.

---

## 🖼️ Icônes de comptes

Lors de la création ou modification d'un compte, vous pouvez :
- Choisir un **emoji** dans la liste proposée
- Importer une **image** (logo de banque, etc.) depuis votre appareil

L'image est stockée en base64 et s'affiche partout dans l'app :
- Liste des comptes
- Listes déroulantes de sélection (affichée comme `🖼`)
- Sous-texte des opérations
- Détail des prêts et récurrentes

---

## 🔁 Synchronisation

L'application synchronise automatiquement avec **Supabase** à chaque modification.  
Appuyez sur **↺ Actualiser** (en haut à droite de chaque onglet) pour forcer un rechargement.

---

*AutoThunes v2.1.5 — par OBE*
