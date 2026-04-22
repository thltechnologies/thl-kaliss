.. _internal-accounts-operations:

Opérations sur Comptes Internes
===============================

Ce document détaille le fonctionnement technique et opérationnel des comptes internes au sein du système Core Banking.

Vue d'ensemble
--------------

Les comptes internes (``InternalAccount``) sont des entités comptables techniques utilisées par l'institution pour gérer ses propres fonds, contrairement aux comptes clients. Ils héritent de la structure de base des comptes (``Account``) mais incluent des fonctionnalités spécifiques de sécurité et de flux opérationnels.

Architecture des Comptes
------------------------

Hiérarchie et Classification
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Chaque compte interne est rattaché à un compte du **Plan Comptable (COA)**. Le système utilise une détection automatique intelligente pour assigner le compte COA le plus spécifique basé sur le numéro du compte interne.

Types de Comptes Internes
~~~~~~~~~~~~~~~~~~~~~~~~~

Les comptes internes sont classés par types (``InternalAccountType``) pour déterminer leurs comportements :

*   **BOX** : Caisse opérationnelle (guichet).
*   **BANK** : Compte bancaire de l'institution.
*   **MAIN_STRONGBOX** : Coffre-fort principal de l'agence.
*   **COLLECT_STRONG_BOX** : Coffre de collecte.
*   **SHARES** : Compte de capital/parts sociales (obligatoire pour les prélèvements de capitaux).
*   **OPENING_FEE** : Compte technique pour les frais d'ouverture.
*   **NONE** : Compte interne générique.

Flux d'Approvisionnement (Supply)
---------------------------------

L'approvisionnement est le processus de transfert de fonds vers un compte interne (généralement depuis la Banque vers le Coffre, ou du Coffre vers une Caisse).

Mécanisme Opérationnel
~~~~~~~~~~~~~~~~~~~~~~

1.  **Demande d'approvisionnement** : L'utilisateur initie une demande via le service ``InternalAccountService.supply()``.
2.  **Contrainte d'autorisation** : Le système vérifie que le compte source figure dans la liste ``suppliedBy`` du compte cible.
3.  **Création d'une requête** : Une ``TransactionRequest`` est créée avec le statut ``PENDING``.
4.  **Validation** : La transaction n'est effective (écritures comptables et mise à jour des soldes) qu'après validation de la requête par un responsable.

Types de transactions générés :
*   ``SUPPLY_BANK``
*   ``SUPPLY_MAIN_STRONGBOX``
*   ``INTERNAL_ACCOUNT_SUPPLY`` (Fallback)

Décaissement (Disbursement)
---------------------------

Le décaissement (``disburseInternalAccount``) permet de retirer des fonds d'un compte interne vers une destination externe ou un autre compte technique. Contrairement à l'approvisionnement, il ne nécessite pas de configuration ``suppliedBy`` préalable mais suit le même flux de validation via ``TransactionRequest``.

Sécurité et Restrictions
------------------------

Le système offre un contrôle granulaire sur l'utilisation des comptes internes.

Configuration par Rôle (Role-Based Access)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Il est possible de définir pour chaque compte interne quelles fonctions sont autorisées par rôle utilisateur :

*   **Can Debit** : Autorise le rôle à effectuer des retraits/débits sur ce compte.
*   **Can Credit** : Autorise le rôle à effectuer des dépôts/crédits sur ce compte.

Cette configuration est stockée dans l'entité ``InternalAccountRoleConfig``.

Transactions Exclues
~~~~~~~~~~~~~~~~~~~~

Pour garantir l'intégrité comptable, certains comptes internes peuvent interdire explicitement certains types de transactions bancaires via la liste ``transactionsExcluded``. 

Par exemple, un compte de "Frais d'ouverture" ne devrait pas pouvoir être utilisé pour une opération de "Dépôt d'épargne".

Prélèvement de Capitaux
------------------------

Le prélèvement de capitaux (``CAPITAL_WITHDRAWAL``) est une opération hybride qui transfère des fonds d'un compte client (Source) vers un compte interne de type ``SHARES`` (Cible).

Particularités Techniques :
~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **Validation stricte** : Le service ``TransactionService.capitalWithdrawal()`` vérifie que le compte cible possède impérativement le type ``InternalAccountType.SHARES``.
- **Écritures Directes** : Contrairement au flux *Supply/Disburse*, cette opération génère une transaction immédiate (si les contrôles de solde et d'exclusion sont validés).
- **Accounting Rule** : La règle comptable applique un Débit sur le compte source (Parts sociales du client) et un Crédit sur le compte cible (Capital institutionnel).

Récapitulatif Technique
-----------------------

+---------------------------+-------------------------------------------------------+
| Composant                 | Description                                           |
+===========================+=======================================================+
| **Modèle**                | ``InternalAccount.java``                              |
+---------------------------+-------------------------------------------------------+
| **Service**               | ``InternalAccountServiceImpl.java``                  |
+---------------------------+-------------------------------------------------------+
| **Contrôleur**            | ``InternalAccountController.java``                   |
+---------------------------+-------------------------------------------------------+
| **Workflow**              | Intégration avec ``TransactionRequestService``        |
+---------------------------+-------------------------------------------------------+
| **Sécurité**              | ``InternalAccountRoleConfig``                         |
+---------------------------+-------------------------------------------------------+
