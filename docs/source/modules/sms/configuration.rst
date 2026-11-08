.. _sms-configuration:

==========================================================
Configuration et Contrôle d'Activation du Service SMS
==========================================================

Ce document détaille l'architecture à 3 niveaux, le modèle de base de données, l'interface d'administration Angular (``/configuration/sms``) et les API REST permettant de piloter le **Service SMS (SMS Banking)** au sein de THL Core Banking.

.. contents:: Table des matières
   :depth: 3
   :local:

Architecture du Contrôle d'Activation à 3 Niveaux
-------------------------------------------------

Le système repose sur une **architecture hiérarchique à 3 niveaux** associée à un **cache mémoire atomique** ($O(1)$) afin de garantir une gouvernance fine et une performance maximale :

.. mermaid::

    flowchart TD
        subgraph Niveau 1 [Niveau 1 : Coupe-Circuit Global Entreprise]
            ENT[Table enterprise / configuration : sms_banking_enabled] --> CHECK_GLOB{smsBankingEnabled == true ?}
        end

        subgraph Niveau 2 [Niveau 2 : Paramétrage par Fonctionnalité]
            CHECK_GLOB -->|Non| BLOCK_ALL[Envoi Global Interrompu : Tous les Modules Inactifs]
            CHECK_GLOB -->|Oui| CACHE[Cache Mémoire O&#40;1&#41; : EnumSet&lt;SmsFeature&gt;]
            CACHE --> CHECK_FEAT{Fonctionnalité présente dans enabledFeatures ?}
        end

        subgraph Niveau 3 [Niveau 3 : Abonnement Compte Client]
            CHECK_FEAT -->|Non| BLOCK_FEAT[SMS Désactivé pour cette Opération]
            CHECK_FEAT -->|Oui| ACC[Table account : sms_banking_enabled]
            ACC --> CHECK_ACC{account.smsBankingEnabled == true ?}
            CHECK_ACC -->|Non| BLOCK_ACC[Compte Non Abonné : Aucun SMS]
            CHECK_ACC -->|Oui| SEND[Envoi Asynchrone du SMS au Client]
        end

        style BLOCK_ALL fill:#ef4444,color:#fff
        style BLOCK_FEAT fill:#f97316,color:#fff
        style BLOCK_ACC fill:#eab308,color:#fff
        style SEND fill:#10b981,color:#fff
        style CHECK_GLOB fill:#3b82f6,color:#fff
        style CHECK_FEAT fill:#8b5cf6,color:#fff
        style CHECK_ACC fill:#0ea5e9,color:#fff

Niveau 1 : Coupe-Circuit Global Entreprise (Kill-Switch)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Piloté par l'attribut ``smsBankingEnabled`` sur les entités ``Enterprise`` et ``Configuration``.
- Lorsque ``smsBankingEnabled`` vaut ``false``, **aucun SMS n'est envoyé** pour l'ensemble de l'institution, quelle que soit l'opération ou le compte client.
- Dans l'interface d'administration, la désactivation de cet interrupteur global **passe automatiquement tous les modules particuliers à l'état Inactif et les grise**, empêchant toute incohérence visuelle.

Niveau 2 : Activation Granulaire par Fonctionnalité (SmsFeature)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Lorsque l'envoi global est actif, l'administrateur choisit précisément quelles opérations déclenchent un SMS grâce à l'énumération ``SmsFeature`` et aux 12 colonnes introduites par la migration Flyway ``V1_0_76__Add_sms_feature_settings_to_configuration.sql`` :

.. code-block:: sql

   ALTER TABLE configuration
       ADD COLUMN IF NOT EXISTS sms_deposit_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_collect_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_withdrawal_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_transfer_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_supply_account_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_loan_repayment_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_salary_payment_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_monthly_fee_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_loan_fee_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_previous_fees_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_capital_withdrawal_enabled BOOLEAN DEFAULT TRUE,
       ADD COLUMN IF NOT EXISTS sms_cancel_enabled BOOLEAN DEFAULT TRUE;

Niveau 3 : Abonnement Individuel par Compte Client
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Chaque compte client (``Account``) possède son propre indicateur ``smsBankingEnabled``.
- Même si l'entreprise et la fonctionnalité sont activées, l'administrateur ou le gestionnaire conserve la possibilité de couper ou de rétablir l'envoi des SMS sur un compte client spécifique.

Gestion dans l'Interface Utilisateur (Frontend Angular)
-------------------------------------------------------

Accès à la Section Dédiée « Service SMS »
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Toutes les configurations relatives aux SMS ont été regroupées dans une section dédiée du menu latéral de configuration :

1. Connectez-vous avec un compte disposant des privilèges **SUPER_ADMIN**.
2. Dans le menu latéral gauche, ouvrez la rubrique **Configurations**.
3. Cliquez sur le sous-menu **Service SMS** (icône ``pi-comments``, URL : ``/configuration/sms``).

.. note::
   La modification des paramètres SMS est **strictement réservée au rôle SUPER_ADMIN**.
   Les utilisateurs disposant du rôle ``CONSULTANT`` peuvent consulter l'état de la configuration en lecture seule ; un message d'avertissement avec cadenas s'affiche :

   *"Seul le rôle Super Admin peut activer ou interrompre l'envoi des SMS."*

Organisation de l'Écran de Configuration SMS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

L'écran ``SmsConfigurationComponent`` est structuré en 3 blocs complémentaires :

.. list-table::
   :widths: 28 72
   :header-rows: 1

   * - Bloc de Configuration
     - Description et Comportement Ergonomique
   * - **1. Envoi Global des SMS de l'Entreprise**
     - - Affiche un bandeau visuel dynamique (vert lorsque l'envoi est **actif**, rouge lorsqu'il est **interrompu**) ainsi qu'un badge d'état en haut à droite (``Envoi actif`` / ``Envoi interrompu``).
       - L'interrupteur principal appelle instantanément ``PUT /enterprise/sms-enabled``.
   * - **2. Nom de l'Expéditeur SMS Officiel (Sender ID)**
     - - Permet de définir l'identifiant alphanumérique affiché sur le téléphone des clients (ex. ``CID``, ``SAHIS``).
       - Le champ de saisie et le bouton **« Enregistrer l'expéditeur »** sont alignés sur la même ligne pour une ergonomie optimale.
       - Lorsque l'envoi global des SMS est désactivé, ce bloc est automatiquement grisé et verrouillé.
   * - **3. Activation des SMS par Fonctionnalité (12 Modules)**
     - - En-tête comprenant les boutons d'action groupée **« Tout activer »** et **« Tout désactiver »** alignés sur la même ligne, avec juste en dessous le badge récapitulatif du nombre de modules activés et désactivés : ``X / 12 modules activés (Y désactivés)``.
       - Grille de 12 cartes représentant chaque fonctionnalité bancaire avec son icône, son sens d'opération (``Crédit``, ``Débit``, ``Débit & Crédit``, ``Ajustement``), son badge d'état (``Actif`` / ``Inactif``) et son interrupteur individuel.

Comportement Intelligent lors de la Désactivation Globale
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Afin de garantir une cohérence totale de l'interface :

- Dès que l'option **Envoi Global des SMS de l'Entreprise** est désactivée :
  1. Tous les 12 modules particuliers passent immédiatement à l'état **Inactif** (interrupteurs basculés sur ``OFF`` et badge rouge ``Inactif``).
  2. Le compteur affiche ``0 / 12 modules activés (12 désactivés)``.
  3. Toutes les cartes des modules particuliers, les boutons *« Tout activer / Tout désactiver »* et le bloc *Sender ID* sont **grisés et verrouillés** (classe ``.sms-module-disabled``).
  4. Un bandeau d'information signale que les modules sont désactivés tant que l'envoi global est coupé.
- Dès que l'administrateur **réactive l'envoi global**, chaque module particulier retrouve automatiquement son état précédemment configuré en base de données.

Spécifications des API REST Backend
-----------------------------------

1. Activer ou interrompre l'envoi global des SMS
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. http:put:: /enterprise/sms-enabled

   Active ou interrompt globalement l'envoi des SMS pour l'entreprise et invalide le cache mémoire SMS.

   **Autorisation requise :** ``SUPER_ADMIN``

   **Paramètre de requête :** ``enabled`` (``boolean``)

2. Activer ou désactiver une fonctionnalité SMS spécifique
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. http:put:: /configuration/sms-feature/{feature}

   Met à jour l'état d'activation SMS d'une fonctionnalité bancaire donnée (ex: ``DEPOSIT``, ``WITHDRAWAL``, ``MONTHLY_FEE``) et rafraîchit immédiatement le cache mémoire.

   **Autorisation requise :** ``SUPER_ADMIN``

   **Paramètres :**
   - ``feature`` (Path Variable) : Valeur de l'énumération ``SmsFeature``.
   - ``enabled`` (Query Param) : ``true`` ou ``false``.

3. Activer ou désactiver toutes les fonctionnalités SMS en lot
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. http:put:: /configuration/sms-features

   Met à jour simultanément les 12 indicateurs de fonctionnalités SMS et invalide le cache mémoire.

   **Autorisation requise :** ``SUPER_ADMIN``

   **Corps de la requête :**

   .. code-block:: json

      {
        "smsDepositEnabled": true,
        "smsCollectEnabled": true,
        "smsWithdrawalEnabled": true,
        "smsTransferEnabled": true,
        "smsSupplyAccountEnabled": true,
        "smsLoanRepaymentEnabled": true,
        "smsSalaryPaymentEnabled": true,
        "smsMonthlyFeeEnabled": true,
        "smsLoanFeeEnabled": true,
        "smsPreviousFeesEnabled": true,
        "smsCapitalWithdrawalEnabled": true,
        "smsCancelEnabled": true
      }
