.. _sms-all-features:

============================================
Toutes les Fonctionnalités - Service SMS
============================================

Cette page détaille l'ensemble des **12 fonctionnalités transactionnelles** couvertes par le système d'envoi de SMS ainsi que les règles métier appliquées pour chaque type d'opération.

Catalogue des 12 Modules d'Envoi de SMS
=======================================

.. list-table::
   :widths: 18 22 15 45
   :header-rows: 1

   * - Code ``SmsFeature``
     - Libellé dans l'Interface
     - Sens Comptable
     - Déclencheur et Contenu de la Notification
   * - ``DEPOSIT``
     - **Dépôts en agence**
     - Crédit
     - Envoyé au titulaire lors d'un versement d'espèces validé au guichet (``TransactionServiceImp.saveDeposit``). Indique le montant crédité et le nouveau solde disponible.
   * - ``COLLECT``
     - **Collecte journalière / Tontine**
     - Crédit
     - Envoyé au client lors de la validation d'une opération de collecte effectuée par un agent de terrain.
   * - ``WITHDRAWAL``
     - **Retraits en agence**
     - Débit
     - Envoyé au titulaire lors d'un retrait d'espèces au guichet ou de la validation d'une requête de retrait. Indique le montant débité et le solde restant.
   * - ``TRANSFER``
     - **Virements de compte à compte**
     - Débit & Crédit
     - Déclenche deux notifications distinctes : un SMS d'avis de débit pour le compte donneur d'ordre et un SMS d'avis de crédit pour le compte bénéficiaire.
   * - ``SUPPLY_ACCOUNT``
     - **Approvisionnement de compte**
     - Crédit
     - Envoyé lors d'un approvisionnement de compte client depuis un compte bancaire (``SUPPLY_ACCOUNT``) ou lors de la confirmation d'une remise de chèque (``SUPPLY_CHECK_DELIVERY``).
   * - ``LOAN_REPAYMENT``
     - **Remboursements d'échéances de financement**
     - Débit
     - Envoyé lors du remboursement manuel, partiel ou par prélèvement automatique d'une échéance de financement.
   * - ``SALARY_PAYMENT``
     - **Virements de salaires**
     - Crédit
     - Envoyé individuellement à chaque employé/client bénéficiaire lors de l'exécution d'un lot de paiement de salaires.
   * - ``MONTHLY_FEE``
     - **Frais mensuels de tenue de compte & TAF**
     - Débit
     - Envoyé lors du prélèvement périodique des frais mensuels de gestion de compte et de la Taxe sur les Activités Financières (TAF).
   * - ``LOAN_FEE``
     - **Frais de dossier de crédit & garanties**
     - Débit
     - Envoyé lors du prélèvement sur le compte client des frais liés à un financement : frais de dossier (``APPLICATION_FEE``), assurance (``INSURANCE_FEE``), notaire (``NOTARY_FEE``), enregistrement (``REGISTRATION_FEE``), expertise (``SURVEYOR_FEE``), visite/évaluation (``VISIT_AND_EVALUATION_FEE``), frais administratifs/commerciaux/transport/communication, dépôt de garantie (``GUARANTEE_DEPOSIT``) ou dépôt de bonne foi (``GOOD_FAITH_DEPOSIT``).
   * - ``PREVIOUS_FEES``
     - **Régularisation d'arriérés de frais**
     - Débit
     - Envoyé lors de la récupération automatique ou manuelle d'arriérés de frais antérieurs impayés (``PREVIOUS_FEES``) sur le compte client.
   * - ``CAPITAL_WITHDRAWAL``
     - **Retrait de capital / parts sociales**
     - Débit
     - Envoyé lors d'une opération de retrait de capital ou de remboursement de parts sociales sur le compte du membre/client.
   * - ``CANCEL``
     - **Annulations & Extournes de transactions**
     - Ajustement
     - Envoyé lors de la confirmation d'une demande d'annulation de transaction (``TransactionCancelRequest``) ou de l'annulation d'une remise de chèque, informant le client de la régularisation et du solde rectifié.

Opérations Non Soumises aux Notifications Transactionnelles
===========================================================

Certaines opérations techniques ou spécifiques sont volontairement exclues du déclenchement transactionnel standard :

1. **Décaissement de financement (DISBURSEMENT)** :
   - Cette écriture mouvemente les comptes internes de l'institution financière et n'impacte pas directement le solde disponible du compte client de la même manière qu'un dépôt ou retrait client. Aucun SMS n'est donc envoyé pour éviter toute confusion.

2. **Frais d'ouverture de compte (OPENING_FEE)** :
   - Lors de l'ouverture d'un compte client, le système envoie déjà un **SMS de bienvenue dédié** contenant les informations d'identification du nouveau compte si l'option SMS Banking est activée sur ce compte.

Gestion de l'Abonnement SMS au Niveau du Compte Client
======================================================

En complément de la configuration générale de l'entreprise et par fonctionnalité, chaque compte client dispose d'un contrôle individuel :

- **À la création du compte** : L'agent peut activer ou désactiver l'option ``SMS Banking`` selon la souscription du client.
- **En cours de vie du compte** : L'abonnement SMS peut être suspendu ou rétabli individuellement sur n'importe quel compte client sans affecter les autres comptes de l'institution.
