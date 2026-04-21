.. _bank-reconciliation:

Guide d'utilisation : Réconciliation Bancaire
=============================================

📖 Vue d'ensemble
-----------------

Le module de réconciliation bancaire permet de comparer les relevés bancaires avec les écritures comptables internes pour identifier et résoudre les différences entre le solde bancaire et le solde comptable.

🚀 Démarrage rapide
-------------------

Accès au module
~~~~~~~~~~~~~~~
1. Connectez-vous au système Core Banking
2. Naviguez vers **Comptabilité** → **Réconciliation Bancaire**

Vue principale
~~~~~~~~~~~~~~
La page affiche un tableau listant toutes les réconciliations en attente avec les colonnes suivantes :
- **Compte Bancaire** : Numéro, clé ou libellé du compte interne
- **Période** : Période comptable
- **Date Relevé** : Date du relevé bancaire
- **Solde Bancaire** : Solde selon le relevé bancaire
- **Solde Comptable** : Solde calculé automatiquement depuis le grand livre
- **Écart** : Différence entre les deux soldes (vert si < 0.01, rouge sinon)
- **Statut** : Réconcilié ou En attente
- **Actions** : Voir détails, Pointage automatique, Finaliser

✨ Créer une nouvelle réconciliation
-----------------------------------

Étapes
~~~~~~

1. **Cliquez sur "Nouvelle Réconciliation"**

2. **Sélectionnez le compte interne** :
   - Un dropdown filtré s'affiche avec tous les comptes internes disponibles
   - Vous pouvez rechercher par :
     - Numéro du compte (ex: "1001-0001")
     - Clé du compte
     - Libellé du compte
   - L'affichage montre : **Numéro/Clé** (en gras) - **Libellé** (en gris)

3. **Sélectionnez l'exercice comptable** :
   - Choisissez l'exercice fiscal depuis le dropdown
   - L'exercice actif est sélectionné par défaut

4. **Sélectionnez la période comptable** :
   - Les périodes disponibles pour l'exercice sélectionné s'affichent
   - Choisissez la période correspondant au relevé bancaire

5. **Saisissez la date du relevé** :
   - Utilisez le calendrier pour sélectionner la date du relevé bancaire

6. **Saisissez le solde bancaire** :
   - Entrez le solde selon le relevé bancaire
   - Le solde comptable sera calculé automatiquement

7. **Ajoutez des notes** (optionnel) :
   - Vous pouvez ajouter des notes explicatives

8. **Cliquez sur "Enregistrer"**

Ce qui se passe automatiquement
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- ✅ Le système vérifie qu'aucune réconciliation n'existe déjà pour ce compte et cette période
- ✅ Le solde comptable est calculé automatiquement depuis le grand livre
- ✅ La différence entre les deux soldes est calculée
- ✅ La réconciliation est créée avec le statut "En attente"

🔍 Voir les détails d'une réconciliation
----------------------------------------

1. Cliquez sur l'icône **👁️ (œil)** dans la colonne Actions
2. Un dialog s'ouvre affichant :
   - Les soldes (bancaire, comptable, écart)
   - La liste complète des éléments de réconciliation
   - Le statut de chaque élément (pointé ou non)

🔗 Pointage automatique
-----------------------

Le pointage automatique permet de faire correspondre automatiquement les éléments du relevé avec les transactions internes.

Utilisation
~~~~~~~~~~~

1. Cliquez sur l'icône **🔗 (lien)** dans la colonne Actions
2. Le système compare automatiquement :
   - Les montants
   - Les dates
   - Les références (si disponibles)
3. Les éléments correspondants sont marqués comme "pointés"

Critères de matching
~~~~~~~~~~~~~~~~~~~~

- Montant identique (tolérance de 0.01)
- Date de transaction identique
- Référence correspondante (si disponible)

✅ Finaliser une réconciliation
-------------------------------

Une fois que tous les éléments sont pointés et que l'écart est résolu :

1. Cliquez sur l'icône **✓ (check)** dans la colonne Actions
2. Confirmez la finalisation
3. Le système :
   - Vérifie que tous les éléments sont pointés
   - Valide la cohérence des soldes
   - Marque la réconciliation comme "Réconciliée"
   - Enregistre qui et quand la réconciliation a été finalisée

Validations
~~~~~~~~~~~

- ❌ **Tous les éléments doivent être pointés** avant de finaliser
- ❌ **L'écart doit être résolu** (tolérance de 0.01)
- ❌ **Impossible de modifier** une réconciliation finalisée

✏️ Modifier une réconciliation
------------------------------

1. Cliquez sur l'icône **✏️ (crayon)** dans la colonne Actions (si disponible)
2. Modifiez les champs souhaités
3. Cliquez sur "Enregistrer"

**Note** : Seules les réconciliations non finalisées peuvent être modifiées.

🔎 Rechercher des réconciliations
---------------------------------

Par compte interne
~~~~~~~~~~~~~~~~~~
- Utilisez l'endpoint : ``GET /bank-reconciliation/account/{accountId}?state=ACTIVATED``

Par période
~~~~~~~~~~~
- Utilisez l'endpoint : ``GET /bank-reconciliation/period/{periodId}?state=ACTIVATED``

Par compte et période
~~~~~~~~~~~~~~~~~~~~~
- Utilisez l'endpoint : ``GET /bank-reconciliation/account/{accountId}/period/{periodId}?state=ACTIVATED``

Réconciliations en attente
~~~~~~~~~~~~~~~~~~~~~~~~~~
- Utilisez l'endpoint : ``GET /bank-reconciliation/pending``
- Ou consultez directement la vue principale qui affiche les réconciliations en attente

💡 Conseils et bonnes pratiques
-------------------------------

Avant de créer une réconciliation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
- ✅ Vérifiez que toutes les transactions de la période sont enregistrées
- ✅ Assurez-vous que le relevé bancaire est complet
- ✅ Vérifiez que la période comptable est correcte

Pendant la réconciliation
~~~~~~~~~~~~~~~~~~~~~~~~~
- ✅ Utilisez le pointage automatique en premier
- ✅ Vérifiez manuellement les éléments non pointés
- ✅ Ajoutez des notes pour expliquer les écarts importants

Après la réconciliation
~~~~~~~~~~~~~~~~~~~~~~~
- ✅ Vérifiez que l'écart est bien résolu
- ✅ Conservez une trace des réconciliations finalisées
- ✅ Vérifiez régulièrement les réconciliations en attente

❓ Questions fréquentes
-----------------------

Q: Pourquoi le solde comptable diffère-t-il du solde bancaire ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
**R:** Cela peut être dû à :
- Transactions en transit (chèques non encore débités/crédités)
- Frais bancaires non encore enregistrés
- Erreurs de saisie
- Transactions en attente de traitement

Q: Puis-je modifier une réconciliation finalisée ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
**R:** Non, les réconciliations finalisées ne peuvent pas être modifiées pour garantir l'intégrité des données.

Q: Que faire si un élément ne peut pas être pointé automatiquement ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
**R:** Vous pouvez :
1. Vérifier manuellement les transactions
2. Vérifier que les montants et dates correspondent
3. Ajouter une note explicative si nécessaire

Q: Comment le système calcule-t-il le solde comptable ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
**R:** Le système :
1. Récupère toutes les transactions du compte interne pour la période
2. Utilise le GeneralLedgerService si le compte a une correspondance dans le plan comptable
3. Calcule le solde en additionnant les crédits et soustrayant les débits

📚 Ressources supplémentaires
-----------------------------

- **Documentation technique** : Voir la synthèse technique
- **Code source** :
  - Backend : ``BankReconciliationController.java``, ``BankReconciliationServiceImp.java``
  - Frontend : ``bank-reconciliation.component.ts``
