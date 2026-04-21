.. _accountant-index:

Comptabilité
============

Le module **Comptabilité** regroupe l'ensemble des outils nécessaires à la gestion financière, structurés selon un flux de travail logique : de la configuration initiale au contrôle final.

1. Configuration et Paramétrage
-------------------------------

Cette section constitue le socle du système comptable. Elle définit le cadre temporel et structurel des écritures.

*   **Exercices Comptables** : Gestion des exercices fiscaux avec création automatique des 12 périodes mensuelles.
*   **Plan Comptable** : Structure hiérarchique des comptes (Actifs, Passifs, Capitaux, Produits, Charges).
*   **Journaux Comptables** : Organisation des écritures par type (Banque, Caisse, OD, Standard) avec numérotation automatique des pièces.

.. toctree::
   :maxdepth: 1

   chart_of_account_hierarchy
   plan_comptable_sfd_umoa

2. Saisie et Opérations Quotidiennes
------------------------------------

Gestion des flux financiers et enregistrement des mouvements au jour le jour.

*   **Écritures Comptables** : Saisie manuelle ou génération automatique des écritures. Système optimisé par "Composants de Transaction" pour une traçabilité totale (TAF, Marge, Frais).
*   **Opérations sur comptes internes** : Gestion des transferts de fonds entre les comptes techniques et opérationnels de l'institution.
*   **Prélèvement des capitaux** : Administration des mouvements liés aux parts sociales et aux fonds propres.

3. États et Reporting Financier
-------------------------------

Outils de synthèse permettant de visualiser la situation financière en temps réel.

*   **Grand Livre** : Consultation détaillée de tous les mouvements par compte, avec génération mensuelle ou annuelle (mode Batch).
*   **Balance de Vérification** : État récapitulatif permettant de vérifier l'équilibre constant entre Débits et Crédits.
*   **États Financiers** : Génération semi-automatique du Bilan, du Compte de Résultat et du Tableau des Flux de Trésorerie.

4. Audit et Réconciliation
--------------------------

Procédures de contrôle pour garantir l'intégrité et la fiabilité des données.

*   **Réconciliation Bancaire** : Pointage des relevés externes avec les comptes internes de trésorerie.
*   **Réconciliation des Soldes** : Analyse des écarts et ajustements entre les soldes théoriques et physiques.

.. toctree::
   :maxdepth: 1

   bank_reconciliation
   verification_conformite

---

📚 Ressources Complémentaires
---------------------------

*   **Guide Utilisateur Complet (A-Z)** : :doc:`user_guide`
*   **Architecture Technique** : :doc:`accounting_module`
*   **Dernières Nouveautés** : :doc:`new_features`