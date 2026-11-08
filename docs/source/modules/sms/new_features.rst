.. _sms-new-features:

==============================================
Nouvelles Fonctionnalités - Service SMS
==============================================

Cette page résume les évolutions majeures apportées au système de notifications **SMS Banking** tant au niveau de l'architecture backend que de l'interface d'administration Angular.

1. Contrôle Granulaire par Fonctionnalité (12 Modules)
======================================================

Auparavant, l'envoi des SMS transactionnels ne pouvait être piloté que globalement ou par compte client. Désormais, l'administrateur (``SUPER_ADMIN``) dispose d'une matrice complète de **12 fonctionnalités indépendantes** :

- Possibilité d'activer ou de désactiver l'envoi de SMS pour chaque type d'opération (dépôt, retrait, collecte, virement, frais mensuels, frais de dossier de financement, arriérés de frais, retrait de capital, annulation, etc.).
- Actions rapides **« Tout activer »** et **« Tout désactiver »** alignées sur la même ligne avec affichage immédiat du compteur ``X / 12 modules activés (Y désactivés)`` juste en dessous.

2. Extension des Notifications SMS à 4 Nouvelles Opérations
===========================================================

La couverture des notifications SMS après opération réussie a été étendue à **4 nouvelles catégories d'opérations** impactant directement le solde du compte client :

1. **Frais mensuels de tenue de compte & TAF** (``MONTHLY_FEE``).
2. **Frais de dossier de financement, assurances et dépôts de garantie** (``LOAN_FEE``).
3. **Régularisation d'arriérés de frais antérieurs** (``PREVIOUS_FEES``).
4. **Retrait de capital / parts sociales** (``CAPITAL_WITHDRAWAL``).

3. Nouvelle Section Dédiée « Service SMS » dans la Configuration
================================================================

- Création d'une entrée dédiée **Service SMS** (``/configuration/sms``) dans le menu latéral sous la rubrique **Configurations**.
- Regroupement dans un écran unique de tous les paramètres SMS :
  - **Coupe-circuit global de l'entreprise** (*Envoi Global des SMS*).
  - **Nom de l'expéditeur officiel (*Sender ID*)** avec champ de saisie et bouton **« Enregistrer l'expéditeur »** alignés sur la même ligne.
  - **Grille des 12 modules fonctionnels** avec badges de sens comptable et d'état.
- **Désactivation et grisage intelligents** : Lorsque l'envoi global des SMS est coupé dans les paramètres, tous les modules particuliers basculent automatiquement à l'état **Inactif** (interrupteurs à ``OFF``) et sont grisés/verrouillés afin d'éliminer toute incohérence visuelle.

4. Optimisation Haute Performance (Cache $O(1)$ & Asynchronisme)
================================================================

Pour garantir que ces vérifications multi-niveaux ne ralentissent jamais l'application :

- **Cache mémoire atomique (EnumSet<SmsFeature>)** :
  - ``SmsServiceImpl`` maintient en mémoire vive une structure immuable ``CachedSmsSettings`` contenant l'état global et un ``EnumSet<SmsFeature>`` des fonctionnalités actives.
  - Chaque vérification ``isSmsFeatureAllowed(feature)`` s'exécute en temps constant $O(1)$ en quelques nanosecondes, **sans aucune requête SQL** pendant le traitement des transactions.
  - Le cache est automatiquement invalidé (``evictSmsConfigCache()``) uniquement lorsqu'un Super Admin modifie la configuration SMS.
- **Exécution Asynchrone Post-Commit** :
  - L'événement ``TransactionSmsEvent`` est traité par ``TransactionSmsService`` via ``@Async`` et ``@TransactionalEventListener(phase = TransactionPhase.AFTER_COMMIT)``.
  - Le thread transactionnel du caissier ou du batch rend la main immédiatement dès la validation en base de données, sans attendre l'appel HTTP vers le fournisseur SMS.
