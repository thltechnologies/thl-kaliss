.. _user-guide:

Guide Utilisateur Complet : Module Comptable
============================================

📖 Introduction
---------------

Qu'est-ce que le Module Comptable ?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Le module comptable est un système complet de gestion comptable qui permet de :

- Gérer les exercices et périodes comptables
- Maintenir le plan comptable
- Enregistrer les écritures comptables
- Générer le grand livre et la balance de vérification
- Effectuer la réconciliation bancaire
- Produire les états financiers

Prérequis
~~~~~~~~~

Pour utiliser le module comptable, vous devez avoir :

- ✅ Un compte utilisateur avec les droits **ADMIN** ou **SUPER_ADMIN**
- ✅ Une entreprise configurée
- ✅ Connexion au système Core Banking

🚪 Accès au Module
------------------

Navigation
~~~~~~~~~~

1. Connectez-vous au système Core Banking
2. Dans le menu principal, cliquez sur **Comptabilité**
3. Le menu comptable s'affiche avec toutes les options disponibles

Sections du Module Comptable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Le module comptable comprend les sections suivantes :

1. **Exercices Comptables** (``/accountant/fiscal-year``)
2. **Périodes Comptables** (``/accountant/accounting-period``)
3. **Plan Comptable** (``/accountant/chart-of-accounts``)
4. **Écritures Comptables** (``/accountant/journal-entry``)
5. **Grand Livre** (``/accountant/general-ledger``)
6. **Balance de Vérification** (``/accountant/trial-balance``)
7. **Réconciliation Bancaire** (``/accountant/bank-reconciliation``)
8. **Journaux Comptables** (``/accountant/accounting-journals``)
9. **États Financiers** (``/accountant/financial-statements``)
9. **Transferts Internes** (``/accountant/accountant``)

📅 Gestion des Exercices Comptables
-----------------------------------

Vue d'ensemble
~~~~~~~~~~~~~~

Les exercices comptables représentent les périodes fiscales (généralement annuelles) pendant lesquelles les opérations comptables sont enregistrées.

Accès
~~~~~

**Menu** : Comptabilité → Exercices Comptables
**URL** : ``/accountant/fiscal-year``

Fonctionnalités
~~~~~~~~~~~~~~~

1. Créer un nouvel exercice
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Cliquez sur le bouton **"Nouvel Exercice"** (icône ➕)
   - **Tooltip** : "Créer un nouvel exercice comptable"

2. Remplissez le formulaire :
   - **Nom** * (obligatoire) : Ex. "Exercice 2025"
   - **Date Début** * (obligatoire) : Date de début de l'exercice
   - **Date Fin** * (obligatoire) : Date de fin de l'exercice
   - **Notes** (optionnel) : Notes additionnelles

3. Cliquez sur **"Enregistrer"**
   - **Tooltip** : "Enregistrer l'exercice comptable"

**Ce qui se passe automatiquement :**

- ✅ Vérification qu'il n'y a pas de chevauchement de dates avec d'autres exercices
- ✅ Si aucun exercice n'est actif, le nouvel exercice est automatiquement activé
- ✅ Création automatique des 12 périodes mensuelles
- ✅ Message de confirmation : "Exercice créé avec succès. Les périodes mensuelles seront créées automatiquement."

**Important :**

- ❌ Impossible de créer un exercice avec des dates qui chevauchent un exercice existant
- ❌ Les dates doivent être valides (date début < date fin)

2. Modifier un exercice
^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Dans le tableau, cliquez sur l'icône **✏️ (crayon)**
   - **Tooltip** : "Modifier l'exercice comptable"
   - **Note** : Le bouton est désactivé si l'exercice est clôturé

2. Modifiez les champs souhaités dans le formulaire

3. Cliquez sur **"Enregistrer"**

**Restrictions :**

- ❌ Impossible de modifier un exercice **clôturé**
- ❌ Impossible de modifier les dates si des écritures ont déjà été enregistrées

3. Activer un exercice
^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Dans le tableau, cliquez sur l'icône **✓ (check)**
   - **Tooltip** : "Activer cet exercice comptable"
   - **Note** : Le bouton est désactivé si l'exercice est déjà actif ou clôturé

2. Confirmez l'activation si demandé

**Ce qui se passe :**

- ✅ L'exercice sélectionné devient actif
- ✅ Tous les autres exercices deviennent inactifs automatiquement
- ✅ Message de confirmation : "Exercice activé avec succès"

**Important :**

- ⚠️ Un seul exercice peut être actif à la fois
- ⚠️ L'exercice actif est utilisé par défaut pour les nouvelles opérations

4. Clôturer un exercice
^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Dans le tableau, cliquez sur l'icône **🔒 (lock)**
   - **Tooltip** : "Clôturer cet exercice comptable"
   - **Note** : Le bouton est désactivé si l'exercice est déjà clôturé

2. Confirmez la clôture dans la boîte de dialogue :
   - Message : "Êtes-vous sûr de vouloir clôturer l'exercice {nom} ?"
   - Cliquez sur **"Oui"** pour confirmer

**Conditions préalables :**

- ✅ Toutes les périodes de l'exercice doivent être **clôturées**
- ✅ Toutes les réconciliations bancaires doivent être **finalisées**
- ✅ Toutes les écritures doivent être **validées**

**Ce qui se passe :**

- ✅ L'exercice est marqué comme clôturé (``isClosed = true``)
- ✅ Date et utilisateur de clôture sont enregistrés
- ✅ Message de confirmation : "Exercice clôturé avec succès"

**Important :**

- ❌ **Irréversible** : Une fois clôturé, un exercice ne peut plus être modifié
- ❌ Impossible d'enregistrer de nouvelles écritures pour un exercice clôturé

5. Supprimer un exercice
^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Dans le tableau, cliquez sur l'icône **🗑️ (trash)**
   - **Tooltip** : "Supprimer cet exercice comptable"
   - **Note** : Le bouton est désactivé si l'exercice est clôturé

2. Confirmez la suppression dans la boîte de dialogue :
   - Message : "Êtes-vous sûr de vouloir supprimer l'exercice {nom} ?"
   - Cliquez sur **"Oui"** pour confirmer

**Restrictions :**

- ❌ Impossible de supprimer un exercice **clôturé**
- ❌ Impossible de supprimer un exercice avec des écritures enregistrées
- ℹ️ La suppression est un **soft delete** (état = DELETED)

Tableau des Exercices
~~~~~~~~~~~~~~~~~~~~~

Le tableau affiche les colonnes suivantes :

+-----------------+-----------------------------------------------------------+
| Colonne         | Description                                               |
+=================+===========================================================+
| **Nom**         | Nom de l'exercice (ex: "Exercice 2025")                   |
+-----------------+-----------------------------------------------------------+
| **Date Début**  | Date de début de l'exercice                               |
+-----------------+-----------------------------------------------------------+
| **Date Fin**    | Date de fin de l'exercice                                 |
+-----------------+-----------------------------------------------------------+
| **Statut**      | Actif / Inactif / Clôturé (avec code couleur)             |
+-----------------+-----------------------------------------------------------+
| **Actions**     | Boutons d'action (Modifier, Activer, Clôturer, Supprimer) |
+-----------------+-----------------------------------------------------------+

**Codes couleur des statuts :**

- 🟢 **Vert** : Actif
- 🟡 **Jaune** : Inactif
- ⚪ **Gris** : Clôturé

Actions du Dialog
~~~~~~~~~~~~~~~~~

- **Annuler** (icône ✖️) : Ferme le formulaire sans enregistrer
  - **Tooltip** : "Annuler et fermer le formulaire"
- **Enregistrer** (icône ✓) : Sauvegarde les modifications
  - **Tooltip** : "Enregistrer l'exercice comptable"
  - Affiche un indicateur de chargement pendant la sauvegarde

📆 Gestion des Périodes Comptables
----------------------------------

Vue d'ensemble
~~~~~~~~~~~~~~

Les périodes comptables sont les subdivisions d'un exercice (généralement mensuelles) qui permettent de suivre les opérations par période. Les périodes sont créées automatiquement lors de la création d'un exercice.

Gestion via les Exercices Comptables
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Les périodes comptables sont gérées automatiquement par le système et sont liées aux exercices comptables. Vous pouvez :

1. **Visualiser les périodes** : Lors de la création d'un exercice, 12 périodes mensuelles sont automatiquement créées (Janvier à Décembre)

2. **Sélectionner une période** : Les périodes sont disponibles dans les dropdowns lors de :
   - Création d'écritures comptables
   - Génération du Grand Livre
   - Calcul de la Balance de Vérification
   - Génération d'états financiers

3. **Clôturer une période** :
   - Les périodes doivent être clôturées via l'API backend ou l'interface d'administration
   - Avant de clôturer un exercice, toutes ses périodes doivent être clôturées
   - Une période clôturée ne peut plus recevoir de nouvelles écritures

**Important :**

- ✅ Les périodes sont créées automatiquement (pas besoin de les créer manuellement)
- ✅ Chaque période couvre un mois complet (ex: Janvier 2025, Février 2025, etc.)
- ✅ Toutes les périodes doivent être clôturées avant de clôturer l'exercice

📋 Plan Comptable
-----------------

Vue d'ensemble
~~~~~~~~~~~~~~

Le plan comptable (Chart of Accounts) est la liste structurée de tous les comptes utilisés dans la comptabilité.

Accès
~~~~~

**Menu** : Comptabilité → Plan Comptable
**URL** : ``/accountant/chart-of-accounts``

Mode Agence (Plan comptable par agence)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

En plus du mode comptable global, il existe un **mode Agence** qui affiche uniquement les comptes rattachés à l’agence de l’employé connecté.

**Menu (Agence)** : Agence → Plan comptable  
**URL** : ``/auth-branch/chart-of-accounts``

**Particularités :**

- Le contenu est **filtré côté backend** (pas uniquement côté interface).
- Seuls les comptes (et sous-comptes) de l’**agence** sont visibles.
- L’objectif est de permettre aux profils agence de consulter le plan comptable **sans accéder aux comptes des autres agences**.

**API (backend) :**

- Récupération d’un compte du plan comptable filtré par agence : ``GET /chart-of-account/{id}/state-activated/by-branch/{branchId}``

Fonctionnalités
~~~~~~~~~~~~~~~

1. Rechercher un compte
^^^^^^^^^^^^^^^^^^^^^^^

**Barre de recherche** :

- Recherche par **code** du compte
- Recherche par **libellé** du compte
- Recherche en temps réel (filtre automatique)

2. Créer un nouveau compte
^^^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Cliquez sur le bouton **"Nouveau Compte"** (icône ➕)

2. Remplissez le formulaire :
   - **Code** * (obligatoire) : Code unique du compte (ex: "101", "401")
   - **Libellé** * (obligatoire) : Nom du compte (ex: "Caisse", "Fournisseurs")
   - **Type** * (obligatoire) : Type de compte (Actif, Passif, Produits, Charges, etc.)
   - **Classe** (optionnel) : Classe comptable associée
   - **Compte Parent** (optionnel) : Pour créer une hiérarchie
   - **Actif** : Cocher si le compte est actif

3. Cliquez sur **"Enregistrer"**

**Types de comptes disponibles :**

- **Actif** : Actifs de l'entreprise
- **Passif** : Passifs de l'entreprise
- **Produits** : Revenus et produits
- **Charges** : Dépenses et charges
- **Capitaux** : Capitaux propres

3. Modifier un compte
^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Dans le tableau, cliquez sur l'icône **✏️ (crayon)**
2. Modifiez les champs souhaités
3. Cliquez sur **"Enregistrer"**

**Restrictions :**

- ❌ Impossible de modifier un compte système (comptes prédéfinis)
- ❌ Impossible de modifier le code d'un compte utilisé dans des écritures

4. Supprimer un compte
^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Dans le tableau, cliquez sur l'icône **🗑️ (trash)**
2. Confirmez la suppression

**Restrictions :**

- ❌ Impossible de supprimer un compte système
- ❌ Impossible de supprimer un compte utilisé dans des écritures
- ⚠️ La suppression est un **soft delete**

Tableau du Plan Comptable
~~~~~~~~~~~~~~~~~~~~~~~~~

+-------------+---------------------------------------+
| Colonne     | Description                           |
+=============+=======================================+
| **Code**    | Code unique du compte                 |
+-------------+---------------------------------------+
| **Libellé** | Nom du compte                         |
+-------------+---------------------------------------+
| **Type**    | Type de compte (Actif, Passif, etc.)  |
+-------------+---------------------------------------+
| **Classe**  | Classe comptable associée             |
+-------------+---------------------------------------+
| **Actif**   | Statut actif/inactif (vert/rouge)     |
+-------------+---------------------------------------+
| **Actions** | Boutons Modifier et Supprimer         |
+-------------+---------------------------------------+

💵 Workflow Caisse (Ouverture / Approvisionnement / Validation / Fermeture)
---------------------------------------------------------------------------

Vue d'ensemble
~~~~~~~~~~~~~~

Le workflow de caisse permet de gérer l’ouverture (approvisionnement) d’une caisse, sa validation par la hiérarchie, puis sa fermeture en fin de journée.

Rôles impliqués
~~~~~~~~~~~~~~~

- **CAISSIER** : initie la demande d’ouverture/approvisionnement de sa caisse.
- **CHEF D’AGENCE (BRANCH_MANAGER)** : confirme la demande.

1) Demande d’ouverture / approvisionnement de caisse (CAISSIER)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Objectif :** demander l’approvisionnement d’une caisse à partir du coffre-fort de l’agence.

**Ce que fait le caissier (UI) :**

- Sélectionne la caisse concernée
- Saisit le montant
- Enregistre la demande

**Ce que fait le système :**

- Crée une **TransactionRequest** au statut **PENDING**
- Associe automatiquement le **coffre-fort** de l’agence comme compte source
- Envoie une notification aux profils concernés

**API (backend) :**

- Création de la demande d’approvisionnement de caisse : ``POST /transaction-request/supply-box-request``

2) Confirmation de la demande (CHEF D’AGENCE)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Objectif :** valider la demande pour déclencher la transaction et la comptabilisation.

**Ce que fait le chef d’agence :**

- Ouvre la liste des demandes en attente
- Confirme la demande d’ouverture/approvisionnement

**Ce que fait le système :**

- Marque la demande **CONFIRMED** (avec la date et l’utilisateur de confirmation)
- Crée la **Transaction** correspondante
- Génère et poste les **écritures comptables** liées à la transaction
- Ouvre automatiquement le **billetage (Ticketing)** du caissier pour la caisse confirmée

**API (backend) :**

- Confirmation de la demande d’approvisionnement de caisse : ``PUT /transaction-request/confirm-box-request``

3) Fermeture / clôture de caisse (fin de journée)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**Objectif :** clôturer la caisse et consolider les opérations de la journée.

**Ce que fait le caissier (selon l’organisation) :**

- Lance la demande de fermeture / clôture avec le montant de clôture

**Ce que fait le système :**

- Marque les demandes d’ouverture de caisse concernées comme **CLOSED**
- Regroupe les éléments dans un objet de clôture (CloseBox) et notifie les profils concernés

📔 Journaux Comptables
---------------------

Vue d'ensemble
~~~~~~~~~~~~~~

Les journaux comptables sont des registres thématiques dans lesquels sont enregistrées les opérations. Chaque écriture comptable **doit** être rattachée à un journal.

Accès
~~~~~

**Menu** : Comptabilité → Journaux Comptables
**URL** : ``/accountant/accounting-journals``

Fonctionnalités
~~~~~~~~~~~~~~~

1. Créer un journal
^^^^^^^^^^^^^^^^^^^

**Étapes :**
- Cliquez sur **"Nouveau Journal"** (icône ➕)
- **Code** : Code court unique (ex: "OD", "BNK")
- **Libellé** : Nom complet (ex: "Opérations Diverses")
- **Type** : Catégorie (Caisse, Banque, Divers...)
- **Comptes Autorisés** : Si activé, seules les écritures sur ces comptes seront acceptées dans ce journal.

**Séquence de numérotation :**
Chaque journal gère sa propre séquence de numérotation. Le format par défaut est ``CODE/ANNÉE/MOIS/NUMÉRO`` (ex: ``OD/2026/04/0001``).

📝 Écritures Comptables
-----------------------

Vue d'ensemble
~~~~~~~~~~~~~~

Les écritures comptables (Journal Entries) enregistrent toutes les opérations comptables selon le principe de la comptabilité en partie double.

Accès
~~~~~

**Menu** : Comptabilité → Écritures Comptables
**URL** : ``/accountant/journal-entry``

Fonctionnalités
~~~~~~~~~~~~~~~

1. Rechercher des écritures
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Filtres disponibles :**

1. **Plage de dates** :
   - **Du** : Date de début
   - **Au** : Date de fin
   - Cliquez sur **"Chercher"** pour appliquer le filtre

2. **Recherche par période** :
   - Sélectionnez une période comptable dans le dropdown

2. Visualiser les écritures
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Le tableau affiche :

- **Référence** : Numéro de référence de l'écriture
- **Description** : Description de l'écriture
- **Montant** : Montant total de l'écriture
- **Nb. LIGNE** : Nombre de lignes d'écriture
- **AGENCE** : Nom de l’agence (affiché en priorité ; le code peut apparaître en complément)
- **Actions** : Voir détails, Modifier, Supprimer

**Expansion des lignes :**

- Cliquez sur l'icône **▶️** à gauche pour voir les détails
- Cliquez sur **▼** pour réduire les détails

3. Créer une nouvelle écriture
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Cliquez sur **"Nouvelle Écriture"** (icône ➕)

2. Remplissez le formulaire :
    - **Journal** * : Sélectionner le journal (ex: OD, BANQUE) - **Obligatoire**
    - **Référence** : Numéro de pièce (Généré automatiquement si laissé vide)
    - **Date** * : Date de l'écriture (détermine la période fiscale)
    - **Description** : Libellé global de l'opération
    - **Lignes d'écriture** :
      - **Compte** : Sélectionner un compte du plan comptable
      - **Débit** : Montant au débit (si applicable)
      - **Crédit** : Montant au crédit (si applicable)
      - **Description** : Description de la ligne

3. **Règle de la partie double** :
   - ✅ Total des débits = Total des crédits
   - ✅ Au moins 2 lignes sont requises
   - ✅ Le système valide automatiquement l'équilibre

4. Cliquez sur **"Enregistrer"**

**Validation automatique :**

- ✅ Vérification de l'équilibre (Débits = Crédits)
- ✅ Vérification que tous les comptes sont valides
- ✅ Vérification que le journal autorise ces comptes (si configuré)
- ✅ Vérification que la période est ouverte

**Statuts d'une écriture :**

- 🟡 **BROUILLON** : Saisie manuelle en cours, non encore comptabilisée.
- 🔵 **POSTEE** : Écriture enregistrée (génération automatique ou validation brouillon).
- 🟢 **VALIDEE** : Écriture contrôlée et verrouillée par un superviseur.

4. Modifier une écriture
^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Dans le tableau, cliquez sur l'icône **✏️ (crayon)**
2. Modifiez les champs souhaités
3. Cliquez sur **"Enregistrer"**

**Restrictions :**

- ❌ Impossible de modifier une écriture dans une période clôturée
- ❌ Impossible de modifier une écriture dans un exercice clôturé

5. Supprimer une écriture
^^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Dans le tableau, cliquez sur l'icône **🗑️ (trash)**
2. Confirmez la suppression

**Restrictions :**

- ❌ Impossible de supprimer une écriture dans une période clôturée
- ⚠️ La suppression est un **soft delete**

6. Exporter les écritures
^^^^^^^^^^^^^^^^^^^^^^^^^

**Envoi par email :**

- Cliquez sur le bouton **"Envoyer par mail"** (icône ✉️)
- **Tooltip** : "Envoyer par mail (Excel)"
- Les écritures sont exportées en format Excel et envoyées par email

📊 Grand Livre
--------------

Vue d'ensemble
~~~~~~~~~~~~~~

Le Grand Livre affiche toutes les écritures comptables organisées par compte, avec le solde de chaque compte.

Accès
~~~~~

**Menu** : Comptabilité → Grand Livre
**URL** : ``/accountant/general-ledger``

Fonctionnalités
~~~~~~~~~~~~~~~

1. Rechercher dans le Grand Livre
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Filtres disponibles :**

1. **Compte** : Sélectionner un compte spécifique (ou "Tous les comptes")
2. **Période** : Sélectionner une période comptable (ou "Toutes les périodes")
3. **Date Début** : Date de début de la recherche
4. **Date Fin** : Date de fin de la recherche

5. Cliquez sur **"Rechercher"** pour appliquer les filtres

2. Générer le Grand Livre (Mois ou Année)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Options de génération :**

1. **Génération pour la période (Mois)** :
   - Sélectionnez une **Période** dans le dropdown.
   - Cliquez sur **"Générer"** (icône 🔄) dans la barre d'outils.
   - **Note** : Le bouton est désactivé si aucune période n'est sélectionnée.

2. **Génération pour l'exercice (Année)** :
   - Cliquez sur **"Générer pour l'année"** (icône 📅).
   - Cette action déclenche un traitement en arrière-plan (Batch) qui régénère toutes les écritures du Grand Livre pour l'exercice fiscal actif.

**Ce qui se passe :**

- ✅ Calcul automatique des soldes par compte
- ✅ Traitement asynchrone pour les gros volumes de données
- ✅ Affichage des écritures chronologiques avec solde cumulé
- ✅ **Nouveau** : Affichage du **Journal Comptable** et de la **Référence Pièce** pour une meilleure traçabilité.

3. Exporter et Envoyer par e-mail
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Le bouton **"Exporter"** (SplitButton) propose deux options :

1. **Télécharger Excel (Direct)** :
   - Génère et télécharge immédiatement le fichier Excel du Grand Livre filtré.
   - Le fichier inclut les colonnes : Date, Journal, Compte, Référence, Description, Débit, Crédit et Solde.

2. **Envoyer par e-mail** :
   - Déclenche une génération asynchrone.
   - **Automatique** : Le système utilise l'adresse e-mail de l'utilisateur authentifié. Aucune saisie d'e-mail n'est requise.
   - Vous recevrez le fichier Excel en pièce jointe dès que le traitement est terminé.

3. Visualiser le Grand Livre
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Le tableau affiche les colonnes suivantes :

+-----------------+------------------------------------------------------------+
| Colonne         | Description                                                |
+=================+============================================================+
| **Date**        | Date de l'écriture                                         |
+-----------------+------------------------------------------------------------+
| **Journal**     | Nom du journal (BANQUE, CAISSE, OD, etc.)                  |
+-----------------+------------------------------------------------------------+
| **Compte**      | Numéro et libellé du compte                                |
+-----------------+------------------------------------------------------------+
| **Référence**   | Référence de la pièce comptable                            |
+-----------------+------------------------------------------------------------+
| **Description** | Libellé de l'opération                                     |
+-----------------+------------------------------------------------------------+
| **Débit**       | Montant au débit                                           |
+-----------------+------------------------------------------------------------+
| **Crédit**      | Montant au crédit                                          |
+-----------------+------------------------------------------------------------+
| **Solde**       | Solde cumulé du compte (positif ou négatif)                |
+-----------------+------------------------------------------------------------+

**Informations affichées :**

- ✅ Toutes les écritures du compte sélectionné
- ✅ Soldes débits et crédits
- ✅ Solde cumulé après chaque écriture
- ✅ Code couleur : Vert pour les soldes positifs, Rouge pour les soldes négatifs

⚖️ Balance de Vérification
--------------------------

Vue d'ensemble
~~~~~~~~~~~~~~

La Balance de Vérification (Trial Balance) est un rapport qui montre les soldes de tous les comptes à une date donnée, permettant de vérifier l'équilibre de la comptabilité.

Accès
~~~~~

**Menu** : Comptabilité → Balance de Vérification
**URL** : ``/accountant/trial-balance``

Fonctionnalités
~~~~~~~~~~~~~~~

1. Calculer la Balance de Vérification
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Sélectionnez une **Période** dans le dropdown
   - **Note** : Le bouton "Calculer" est désactivé si aucune période n'est sélectionnée

2. Cliquez sur **"Calculer"** (icône 🧮)
   - Un indicateur de chargement s'affiche pendant le calcul

**Ce qui se passe :**

- ✅ Calcul automatique des soldes de tous les comptes
- ✅ Calcul des totaux débits et crédits
- ✅ Vérification de l'équilibre de la balance

2. Vérifier l'équilibre
^^^^^^^^^^^^^^^^^^^^^^^

**Indicateur visuel :**

- 🟢 **"Balance équilibrée"** (fond vert) : Total débits = Total crédits
- 🔴 **"Balance non équilibrée"** (fond rouge) : Total débits ≠ Total crédits

**Si la balance n'est pas équilibrée :**

- ⚠️ Vérifiez les écritures comptables
- ⚠️ Vérifiez qu'il n'y a pas d'erreurs de saisie
- ⚠️ Vérifiez que toutes les écritures sont validées

3. Visualiser la Balance
^^^^^^^^^^^^^^^^^^^^^^^^

Le tableau affiche les colonnes suivantes :

+-----------------------+---------------------------------------------------------+
| Colonne               | Description                                             |
+=======================+=========================================================+
| **Code**              | Code du compte                                          |
+-----------------------+---------------------------------------------------------+
| **Libellé**           | Libellé du compte                                       |
+-----------------------+---------------------------------------------------------+
| **Débit Ouverture**   | Solde débiteur d'ouverture                              |
+-----------------------+---------------------------------------------------------+
| **Crédit Ouverture**  | Solde créditeur d'ouverture                             |
+-----------------------+---------------------------------------------------------+
| **Débit Période**     | Total des débits de la période                          |
+-----------------------+---------------------------------------------------------+
| **Crédit Période**    | Total des crédits de la période                         |
+-----------------------+---------------------------------------------------------+
| **Débit Clôture**     | Solde débiteur de clôture                               |
+-----------------------+---------------------------------------------------------+
| **Crédit Clôture**    | Solde créditeur de clôture                              |
+-----------------------+---------------------------------------------------------+
| **Solde Net**         | Solde net du compte (vert si positif, rouge si négatif) |
+-----------------------+---------------------------------------------------------+

**Totaux en bas du tableau :**

- **Total Débit** : Somme de tous les débits
- **Total Crédit** : Somme de tous les crédits
- **Différence** : Écart entre totaux (doit être 0 pour une balance équilibrée)

🏦 Réconciliation Bancaire
--------------------------

Vue d'ensemble
~~~~~~~~~~~~~~

La réconciliation bancaire permet de comparer les relevés bancaires avec les écritures comptables pour identifier et résoudre les différences.

Accès
~~~~~

**Menu** : Comptabilité → Réconciliation Bancaire
**URL** : ``/accountant/bank-reconciliation``

Guide complet
~~~~~~~~~~~~~

Consultez le **Guide Utilisateur de la Réconciliation Bancaire** pour une documentation détaillée.

Fonctionnalités principales
~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Créer une réconciliation
^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Cliquez sur **"Nouvelle Réconciliation"** (icône ➕)

2. Sélectionnez le **Compte Interne** :
   - Utilisez le dropdown filtré pour rechercher par numéro, clé ou libellé
   - L'affichage montre : **Numéro/Clé** (en gras) - **Libellé** (en gris)

3. Sélectionnez l'**Exercice Comptable** et la **Période Comptable**

4. Saisissez la **Date du Relevé** et le **Solde Bancaire**

5. Cliquez sur **"Enregistrer"**

2. Pointage automatique
^^^^^^^^^^^^^^^^^^^^^^^

- Cliquez sur l'icône **🔗 (lien)** pour matcher automatiquement les transactions
- Le système compare montants, dates et références

3. Finaliser la réconciliation
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

- Cliquez sur l'icône **✓ (check)** pour finaliser
- Tous les éléments doivent être pointés avant la finalisation

📈 États Financiers
-------------------

Vue d'ensemble
~~~~~~~~~~~~~~

Les états financiers sont des rapports comptables qui présentent la situation financière de l'entreprise.

Accès
~~~~~

**Menu** : Comptabilité → États Financiers
**URL** : ``/accountant/financial-statements``

Types d'États Financiers
~~~~~~~~~~~~~~~~~~~~~~~~

1. Bilan
^^^^^^^^

Le bilan présente la situation financière de l'entreprise à une date donnée (Actifs = Passifs + Capitaux).

**Génération :**

1. Sélectionnez un **Exercice Comptable** (obligatoire)
2. Optionnellement, sélectionnez une **Période**
3. Cliquez sur **"Générer Bilan"**

**Contenu du bilan :**

- **Actifs** : Actifs de l'entreprise
- **Passifs** : Passifs de l'entreprise
- **Capitaux Propres** : Capitaux propres

2. Compte de Résultat
^^^^^^^^^^^^^^^^^^^^^

Le compte de résultat présente les produits et charges de l'entreprise pour une période donnée.

**Génération :**

1. Sélectionnez un **Exercice Comptable** (obligatoire)
2. Optionnellement, sélectionnez une **Période**
3. Cliquez sur **"Générer Compte de Résultat"**

**Contenu du compte de résultat :**

- **Produits** : Revenus et produits
- **Charges** : Dépenses et charges
- **Résultat** : Bénéfice ou perte (Produits - Charges)

3. Flux de Trésorerie
^^^^^^^^^^^^^^^^^^^^^

Le flux de trésorerie présente les mouvements de trésorerie (encaissements et décaissements).

**Génération :**

1. Sélectionnez un **Exercice Comptable** (obligatoire)
2. Optionnellement, sélectionnez une **Période**
3. Cliquez sur **"Générer Flux de Trésorerie"**

Fonctionnalités
~~~~~~~~~~~~~~~

1. Visualiser un état
^^^^^^^^^^^^^^^^^^^^^

- Dans le tableau, cliquez sur l'icône **👁️ (œil)** pour voir les détails

2. Approuver un état
^^^^^^^^^^^^^^^^^^^^

- Cliquez sur l'icône **✓ (check)** pour approuver
- **Note** : Le bouton est désactivé si l'état est déjà approuvé

3. Finaliser un état
^^^^^^^^^^^^^^^^^^^^

- Cliquez sur l'icône **🔒 (lock)** pour finaliser
- Un état finalisé ne peut plus être modifié

Tableau des États Financiers
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+--------------+-------------------------------------------------+
| Colonne      | Description                                     |
+==============+=================================================+
| **Type**     | Bilan / Compte de Résultat / Flux de Trésorerie |
+--------------+-------------------------------------------------+
| **Date**     | Date de génération                              |
+--------------+-------------------------------------------------+
| **Période**  | Période ou exercice concerné                    |
+--------------+-------------------------------------------------+
| **Statut**   | Approuvé / Finalisé / Brouillon                 |
+--------------+-------------------------------------------------+
| **Actions**  | Voir, Approuver, Finaliser                      |
+--------------+-------------------------------------------------+

💰 Transferts Internes
----------------------

Vue d'ensemble
~~~~~~~~~~~~~~

Les transferts internes permettent de transférer des fonds entre comptes internes.

Accès
~~~~~

**Menu** : Comptabilité → Transferts Internes
**URL** : ``/accountant/accountant``

Fonctionnalités
~~~~~~~~~~~~~~~

1. Effectuer un transfert
^^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Remplissez le formulaire :
   - **Compte Source** * : Compte interne source
   - **Compte Cible** * : Compte interne cible
   - **Montant** * : Montant du transfert
   - **Date** * : Date du transfert
   - **Description** : Description du transfert

2. Vérifications automatiques :
   - ✅ Le compte source doit être différent du compte cible
   - ✅ Le compte source doit avoir un solde suffisant (si applicable)

3. Cliquez sur **"Transférer"** ou **"Enregistrer"**

3. Approvisionnement et Décaissement (Flux de Validation)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Contrairement aux transferts directs, l'approvisionnement des comptes de trésorerie (Banque vers Coffre) suit un flux de validation :

1. **Demande** : L'opérateur saisit la demande d'approvisionnement.
2. **Autorisation** : Le système vérifie que la source est autorisée pour cette cible (liste *Fourni par*).
3. **Approbation** : Un responsable valide la demande dans le menu **Opérations → Requêtes de Transaction**.
4. **Exécution** : Une fois approuvée, l'écriture comptable est générée et les soldes sont mis à jour.

4. Configuration de Sécurité des Comptes
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Vous pouvez restreindre l'utilisation des comptes internes par rôle :

1. Cliquez sur l'icône **🛡️ (bouclier)** sur la ligne du compte.
2. Pour chaque rôle (Caissier, Manager, etc.), cochez :
   - **Peut Débiter** : Autorise le rôle à retirer de l'argent.
   - **Peut Créditer** : Autorise le rôle à déposer de l'argent.
3. Cliquez sur **"Enregistrer la configuration"**.

5. Transactions Exclues
^^^^^^^^^^^^^^^^^^^^^^^

Vous pouvez interdire certains types de transactions pour un compte spécifique afin d'éviter les erreurs de saisie (ex: interdire les retraits d'épargne sur un compte de frais).

2. Visualiser les transferts
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Le tableau affiche :

- **Date** : Date du transfert
- **Compte Source** : Compte source
- **Compte Cible** : Compte cible
- **Montant** : Montant transféré
- **Description** : Description

✅ Bonnes Pratiques
-------------------

💼 Prélèvement de Capitaux
--------------------------

Vue d'ensemble
~~~~~~~~~~~~~~

Le prélèvement de capitaux permet de transférer des fonds depuis un compte de parts sociales (Client) vers un compte de capital interne (Institutional Shares). Cette opération est généralement utilisée lors de la sortie d'un membre ou pour des ajustements de fonds propres.

Accès
~~~~~

**Menu** : Comptabilité → Prélèvement de Capitaux
**URL** : ``/accountant/shares``

Fonctionnalités
~~~~~~~~~~~~~~~

1. Effectuer un prélèvement
^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Étapes :**

1. Saisissez le **Numéro de compte** du client (Source).
2. Sélectionnez le **Compte cible** (Compte interne de type SHARES).
3. Saisissez le **Montant** à prélever.
4. Ajoutez une **Description** explicative.
5. Cliquez sur **"Prélever"**.

2. Validation et Sécurité
^^^^^^^^^^^^^^^^^^^^^^^^^

- ✅ **Type de compte** : Le compte cible doit impérativement être un compte interne de type ``SHARES``.
- ✅ **Solde** : Le système vérifie que le compte client dispose d'un solde suffisant.
- ✅ **Composants** : Contrairement aux dépôts classiques, cette opération ne supporte généralement pas de frais ou de taxes supplémentaires (marge/TAF).

3. Historique des Prélèvements
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Le tableau en bas de page liste tous les prélèvements effectués, permettant un suivi rigoureux des mouvements de capitaux.

✅ Bonnes Pratiques
-------------------

Gestion des Exercices
~~~~~~~~~~~~~~~~~~~~~

1. **Créer l'exercice à l'avance** :
   - Créez l'exercice suivant avant la fin de l'exercice en cours
   - Vérifiez les dates pour éviter les chevauchements

2. **Clôturer dans l'ordre** :
   - Clôturez d'abord toutes les périodes
   - Puis clôturez l'exercice
   - Vérifiez que toutes les réconciliations sont finalisées

3. **Un seul exercice actif** :
   - Assurez-vous qu'un seul exercice est actif à tout moment
   - Désactivez l'ancien exercice lors de l'activation du nouveau

Écritures Comptables
~~~~~~~~~~~~~~~~~~~~

1. **Vérifier l'équilibre** :
   - Vérifiez toujours que Débits = Crédits avant d'enregistrer
   - Le système valide automatiquement, mais vérifiez manuellement

2. **Descriptions claires** :
   - Utilisez des descriptions explicites pour faciliter la recherche
   - Incluez des références (numéros de facture, etc.)

3. **Périodes appropriées** :
   - Enregistrez les écritures dans la période correcte
   - Ne modifiez pas les périodes après clôture

Réconciliation Bancaire
~~~~~~~~~~~~~~~~~~~~~~~

1. **Réconciliation régulière** :
   - Effectuez la réconciliation bancaire mensuellement
   - Utilisez le pointage automatique en premier
   - Vérifiez manuellement les éléments non pointés

2. **Documentation** :
   - Ajoutez des notes pour expliquer les écarts importants
   - Conservez une trace des réconciliations

Plan Comptable
~~~~~~~~~~~~~~

1. **Structure hiérarchique** :
   - Utilisez les comptes parents pour organiser le plan comptable
   - Suivez les normes comptables en vigueur

2. **Codes cohérents** :
   - Utilisez des codes cohérents et logiques
   - Évitez de modifier les codes une fois utilisés

❓ FAQ (Foire aux Questions)
----------------------------

Exercices Comptables
~~~~~~~~~~~~~~~~~~~~

**Q : Puis-je créer deux exercices qui se chevauchent ?**
R : Non, le système empêche la création d'exercices avec des dates qui se chevauchent.

**Q : Que se passe-t-il si j'active un exercice alors qu'un autre est actif ?**
R : L'ancien exercice actif est automatiquement désactivé. Un seul exercice peut être actif à la fois.

**Q : Puis-je modifier un exercice clôturé ?**
R : Non, les exercices clôturés sont verrouillés et ne peuvent plus être modifiés.

**Q : Comment savoir si je peux clôturer un exercice ?**
R : Le système vérifie automatiquement que toutes les périodes sont clôturées. Si ce n'est pas le cas, vous recevrez un message d'erreur.

Écritures Comptables
~~~~~~~~~~~~~~~~~~~~

**Q : Que se passe-t-il si Débits ≠ Crédits ?**
R : Le système refuse l'enregistrement et affiche un message d'erreur. Vous devez corriger l'écriture pour qu'elle soit équilibrée.

**Q : Puis-je modifier une écriture dans une période clôturée ?**
R : Non, les écritures dans les périodes clôturées sont verrouillées.

**Q : Combien de lignes puis-je ajouter à une écriture ?**
R : Aucune limite, mais vous devez respecter la partie double (au moins 2 lignes).

Balance de Vérification
~~~~~~~~~~~~~~~~~~~~~~~

**Q : Que faire si la balance n'est pas équilibrée ?**
R : Vérifiez les écritures comptables pour trouver l'erreur. Le système affiche la différence pour vous aider.

**Q : La balance est-elle calculée en temps réel ?**
R : Non, vous devez cliquer sur "Calculer" pour générer la balance. Cela permet d'optimiser les performances.

Réconciliation Bancaire
~~~~~~~~~~~~~~~~~~~~~~~

**Q : Pourquoi certains éléments ne sont pas pointés automatiquement ?**
R : Le pointage automatique fonctionne par montant, date et référence. Si aucun match n'est trouvé, vous devez pointer manuellement.

**Q : Puis-je finaliser une réconciliation avec des éléments non pointés ?**
R : Non, tous les éléments doivent être pointés avant la finalisation.

États Financiers
~~~~~~~~~~~~~~~~

**Q : Puis-je générer un état pour plusieurs exercices ?**
R : Non, chaque état est généré pour un exercice spécifique. Vous pouvez générer plusieurs états pour différents exercices.

**Q : Un état finalisé peut-il être modifié ?**
R : Non, les états finalisés sont verrouillés pour garantir l'intégrité.

🔐 Permissions et Sécurité
--------------------------

Droits d'accès
~~~~~~~~~~~~~~

Toutes les fonctionnalités du module comptable nécessitent les droits :

- **ADMIN** ou **SUPER_ADMIN**

Restrictions par fonctionnalité
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

+------------------+----------+--------------+-------------+--------------+
| Fonctionnalité   | Création | Modification | Suppression | Consultation |
+==================+==========+==============+=============+==============+
| Exercices        | ✅ ADMIN | ✅ ADMIN     | ✅ ADMIN    | ✅ Tous      |
+------------------+----------+--------------+-------------+--------------+
| Périodes         | ✅ ADMIN | ✅ ADMIN     | ✅ ADMIN    | ✅ Tous      |
+------------------+----------+--------------+-------------+--------------+
| Plan Comptable   | ✅ ADMIN | ✅ ADMIN     | ✅ ADMIN    | ✅ Tous      |
+------------------+----------+--------------+-------------+--------------+
| Écritures        | ✅ ADMIN | ✅ ADMIN     | ✅ ADMIN    | ✅ Tous      |
+------------------+----------+--------------+-------------+--------------+
| Grand Livre      | -        | -            | -           | ✅ Tous      |
+------------------+----------+--------------+-------------+--------------+
| Balance          | -        | -            | -           | ✅ Tous      |
+------------------+----------+--------------+-------------+--------------+
| Réconciliation   | ✅ ADMIN | ✅ ADMIN     | ✅ ADMIN    | ✅ Tous      |
+------------------+----------+--------------+-------------+--------------+
| États Financiers | ✅ ADMIN | ✅ ADMIN     | ✅ ADMIN    | ✅ Tous      |
+------------------+----------+--------------+-------------+--------------+

📞 Support et Assistance
------------------------

En cas de problème
~~~~~~~~~~~~~~~~~~

1. **Vérifiez les validations** :
   - Lisez attentivement les messages d'erreur
   - Vérifiez que tous les champs requis sont remplis
   - Vérifiez les permissions

2. **Consultez la documentation** :
   - Guide technique complet ou aide en ligne

3. **Contactez le support** :
   - Si le problème persiste, contactez l'administrateur système

🎯 Workflow Recommandé
----------------------

Pour une nouvelle période comptable
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **Créer l'exercice** (si nouveau) → Exercices Comptables
2. **Vérifier les périodes** → Périodes Comptables
3. **Activer l'exercice** → Exercices Comptables
4. **Enregistrer les écritures** → Écritures Comptables
5. **Vérifier le Grand Livre** → Grand Livre
6. **Vérifier la Balance** → Balance de Vérification
7. **Réconcilier les comptes bancaires** → Réconciliation Bancaire
8. **Clôturer les périodes** → Périodes Comptables
9. **Générer les états financiers** → États Financiers
10. **Clôturer l'exercice** → Exercices Comptables

Pour une réconciliation mensuelle
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **Accéder à Réconciliation Bancaire**
2. **Créer une nouvelle réconciliation** pour le mois
3. **Sélectionner le compte interne** (dropdown filtré)
4. **Saisir le solde bancaire** du relevé
5. **Utiliser le pointage automatique**
6. **Pointer manuellement** les éléments restants
7. **Vérifier l'équilibre**
8. **Finaliser la réconciliation**

🔄 Raccourcis et Astuces
------------------------

Recherche rapide
~~~~~~~~~~~~~~~~

- **Plan Comptable** : Utilisez la barre de recherche en temps réel
- **Grand Livre** : Utilisez les filtres par compte et période
- **Écritures** : Utilisez la plage de dates

Navigation
~~~~~~~~~~

- Tous les modules sont accessibles via le menu **Comptabilité**
- Utilisez le fil d'Ariane pour comprendre où vous vous trouvez
- Les tooltips vous aident à comprendre les actions

Export et Impression
~~~~~~~~~~~~~~~~~~~~

- **Écritures** : Export Excel par email
- **Balance** : Export disponible (vérifiez selon votre configuration)
- **États Financiers** : Export PDF/Excel (selon configuration)

📝 Notes Finales
----------------

Ce guide couvre toutes les fonctionnalités principales du module comptable. Pour des questions spécifiques ou des cas d'usage avancés, consultez la documentation technique ou contactez le support.

**Dernière mise à jour** : 2026
**Version** : 3.0
