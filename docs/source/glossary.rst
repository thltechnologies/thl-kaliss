.. _glossary:

Glossaire
=========

Ce glossaire définit les termes métier et techniques utilisés dans la documentation de **THL Core Banking**.

Termes Bancaires
----------------

**KYC (Know Your Customer)**
    Processus de vérification de l'identité des clients. Dans THL Core Banking, cela inclut la vérification des pièces d'identité et de la conformité documentaire avant l'approbation d'un compte ou d'un prêt.

**CIF (Customer Information File)**
    Identifiant unique d'un client dans le système (souvent appelé "Code Client"). Il regroupe toutes les informations statiques et documents liés à une personne physique ou morale.

**TAF (Taxe sur les Activités Financières)**
    Taxe appliquée sur certaines opérations financières et commissions. Le taux est paramétrable globalement dans le système.

**Billetage**
    Opération de comptage physique des coupures (billets et pièces) lors de l'ouverture ou de la fermeture d'une caisse pour s'assurer que le solde physique correspond au solde théorique.

**Grand Livre (General Ledger)**
    Document comptable regroupant tous les comptes de l'entreprise, où sont reportées les écritures comptables.

**Balance de Vérification**
    État récapitulatif de tous les comptes du Grand Livre montrant le total des débits et des crédits pour vérifier l'équilibre comptable.

**Réconciliation Bancaire**
    Procédure de comparaison entre le solde comptable du compte bancaire interne et le solde figurant sur le relevé de la banque externe.

Termes Techniques
-----------------

**Fetch Join**
    Technique d'optimisation de base de données utilisée pour charger des données liées en une seule requête SQL, évitant ainsi les erreurs de chargement tardif (*LazyInitializationException*).

**Soft Delete**
    Mécanisme de suppression où la donnée n'est pas réellement effacée de la base de données, mais marquée comme "supprimée" (State = DELETED) pour des raisons d'audit et de traçabilité.

**Batch Processing**
    Traitement automatisé de gros volumes de données en arrière-plan (ex: génération du Grand Livre pour l'année entière) sans bloquer l'interface utilisateur.

**Composant de Transaction**
    Élément granulaire d'une écriture comptable permettant de ventiler un montant global entre le principal, l'intérêt, la TAF et d'autres commissions.
