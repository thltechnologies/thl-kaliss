.. _sms-index:

==================================
Module Service SMS (SMS Banking)
==================================

Le module **Service SMS (SMS Banking)** de THL Core Banking assure l'envoi automatisé, sécurisé et en temps réel de notifications SMS aux clients lors de l'exécution d'opérations bancaires sur leurs comptes. Il intègre un **système de gouvernance à 3 niveaux** (Entreprise → Fonctionnalité → Compte Client) conçu pour offrir aux administrateurs un contrôle granulaire sans aucun impact sur les performances transactionnelles.

.. note::
   L'envoi effectif d'un SMS est conditionné par l'activation simultanée des **3 niveaux de contrôle** : l'interrupteur global de l'entreprise, l'activation de la fonctionnalité bancaire concernée et l'abonnement SMS actif sur le compte du client.

.. toctree::
   :maxdepth: 2
   :caption: 📑 Sommaire du Module

   configuration
   all_features
   new_features

Vue d'ensemble
--------------

Le module Service SMS repose sur 4 piliers majeurs :

1. **Gouvernance et Contrôle à 3 Niveaux** :
   - **Niveau 1 — Coupe-circuit Global Entreprise (Kill-Switch)** : Permet au ``SUPER_ADMIN`` d'activer ou d'interrompre instantanément tous les envois de SMS de l'institution.
   - **Niveau 2 — Paramétrage Granulaire par Fonctionnalité** : Permet de choisir individuellement parmi les **12 modules d'opérations bancaires** ceux qui déclenchent l'envoi d'un SMS.
   - **Niveau 3 — Abonnement par Compte Client** : Respecte le choix d'abonnement (``smsBankingEnabled``) propre à chaque compte client (activable ou désactivable à l'ouverture du compte ou depuis la fiche compte).

2. **Couverture Complète des Opérations Clientèle (12 Fonctionnalités)** :
   - Opérations de caisse et de terrain : **Dépôts en agence**, **Collecte journalière / Tontine**, **Retraits en agence**.
   - Opérations de virement et d'alimentation : **Virements de compte à compte** (SMS débit émetteur + SMS crédit bénéficiaire), **Approvisionnements de compte** (banque / remise de chèque), **Virements de salaires**.
   - Crédits et financements : **Remboursements d'échéances de financement** (manuels, partiels ou automatiques), **Frais de dossier de financement et garanties**.
   - Frais et opérations sur capital : **Frais mensuels de tenue de compte & TAF**, **Régularisation d'arriérés de frais**, **Retrait de capital / parts sociales**.
   - Régularisations : **Annulations & Extournes de transactions**.

3. **Exclusions Métier Intelligentes** :
   - **Décaissement de financement (DISBURSEMENT)** : Impactant les comptes internes de l'institution, cette opération ne déclenche pas de SMS client inutile.
   - **Frais d'ouverture de compte (OPENING_FEE)** : Lors de la création du compte, un SMS dédié de bienvenue contenant le numéro du compte est déjà envoyé au client selon l'option choisie à l'ouverture.

4. **Architecture Haute Performance (Zéro Latence)** :
   - **Cache mémoire atomique en** $O(1)$ : Évaluation instantanée via un ``EnumSet<SmsFeature>`` mis en cache en mémoire vive (``volatile``), évitant toute requête SQL supplémentaire lors des transactions.
   - **Traitement asynchrone post-validation** : Les SMS sont publiés via ``@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)`` et exécutés dans un pool de threads dédié (``@Async``), garantissant qu'aucune lenteur réseau de l'opérateur télécom ne ralentit les guichets.

Flux Décisionnel d'Envoi d'un SMS
---------------------------------

Le diagramme ci-dessous illustre la chaîne de vérification ultra-rapide appliquée à chaque opération bancaire :

.. mermaid::

    graph TD
        A[Transaction Bancaire Validée - AFTER_COMMIT] --> B{Niveau 1 : Envoi Global Entreprise Actif ?}
        B -->|Non| X[Aucun SMS Envoyé - Arrêt Immédiat O&#40;1&#41;]
        B -->|Oui| C{Niveau 2 : Fonctionnalité SMS Activée ?}
        C -->|Non| X
        C -->|Oui| D{Niveau 3 : Compte Client Abonné au SMS ?}
        D -->|Non| X
        D -->|Oui| E[Formatage du Message & Envoi Asynchrone @Async]
        E --> F[Réception du SMS par le Client avec Sender ID Officiel]

        style A fill:#3b82f6,color:#fff
        style B fill:#8b5cf6,color:#fff
        style C fill:#8b5cf6,color:#fff
        style D fill:#0ea5e9,color:#fff
        style E fill:#f59e0b,color:#fff
        style F fill:#10b981,color:#fff
        style X fill:#ef4444,color:#fff

Rôles et Habilitations
----------------------

.. list-table::
   :widths: 25 25 50
   :header-rows: 1

   * - Rôle
     - Niveau d'Accès
     - Périmètre de Responsabilité
   * - ``SUPER_ADMIN``
     - Système Global
     - Activation/interruption de l'envoi global des SMS, modification du nom d'expéditeur officiel (*Sender ID*), activation/désactivation individuelle ou globale des 12 fonctionnalités SMS.
   * - ``BRANCH_MANAGER`` / ``CUSTOMER_SERVICE``
     - Agence / Clientèle
     - Activation ou suspension de l'abonnement SMS Banking sur un compte client spécifique lors de l'ouverture ou de la gestion du compte.
   * - ``CONSULTANT``
     - Lecture Seule
     - Consultation de l'état du service SMS et des fonctionnalités actives dans l'écran de configuration (composants en lecture seule avec cadenas de sécurité).
