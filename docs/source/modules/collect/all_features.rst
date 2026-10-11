.. _collect-all-features:

===========================================
Toutes les Fonctionnalités - Module Collecte
===========================================

Cette page documente l'ensemble des fonctionnalités opérationnelles du module **Collecte** disponibles pour les équipes en agence et les gestionnaires de collecte.

1. Supervision des Opérations de Collecte
==========================================

Accessible via ``/collects/branch-collects``, cette interface permet aux responsables d'agence et aux gestionnaires de suivre toutes les transactions de collecte en temps réel.

Fonctionnalités :
-----------------

- **Consultation chronologique des transactions** : Date et heure, référence de la transaction, compte client impacté, montant crédité/débité, collecteur assigné.
- **Filtrage multicritère** :
  - Par intervalle de dates (date de début et date de fin).
  - Par identifiant de collecteur ou employé.
  - Par type d'opération (Dépôt tontine, Retrait, Approvisionnement).
- **Rapprochement des soldes journaliers** : Calcul instantané du volume collecté sur la période sélectionnée.
- **Export des états** : Génération immédiate des bordereaux de collecte au format **PDF** et **Excel** pour archivage physique ou contrôle comptable.

2. Gestion des Requêtes de Transaction des Collecteurs
======================================================

Accessible via ``/collects/branch-collector-transaction-requests``, ce composant gère les demandes de flux financiers entre l'agence et les collecteurs itinérants.

Fonctionnalités :
-----------------

- **Alimentation de compte collecteur (Supply Request)** :
  - Demande soumise pour doter un collecteur d'un fond de roulement en espèces.
  - Contrôle automatique de la caisse source de l'agence.
  - Vérification de l'activation du module via ``collectService.checkCollectAllowed()``.
- **Circuit de validation à double signature** :
  - Notification au Responsable de Collecte ou Chef d'Agence.
  - Possibilité d'approbation ou de rejet motivé de la requête.
- **Rapprochement en fin de tournée** : Dépôt des fonds collectés vers la caisse principale de l'agence.

3. Gestion et Suivi des Collecteurs
===================================

Accessible via ``/collects/branch-collectors``, cette section administre les agents de terrain rattachés à l'institution.

Fonctionnalités :
-----------------

- **Fiche profil du collecteur** :
  - Informations civiles, contact téléphonique, agence de rattachement.
  - Caisse de collecte associée.
- **Portefeuille de comptes sous gestion** :
  - Attribution et réaffectation de comptes clients à un collecteur donné.
  - Visualisation des clients assignés à la zone du collecteur.
- **Résumé des Dépôts Périodiques (CollectorDepositSummary)** :
  - **Montant attendu** (dépôt minimum planifié par compte × nombre de comptes).
  - **Montant réellement collecté** sur la journée ou la période.
  - **Écart / Montant restant** à collecter.
- **Téléchargement du relevé individuel du collecteur** : Export du grand livre et de l'historique complet d'activité de l'agent.

4. Traitement des Demandes d'Enrôlement Client Terrain
======================================================

Accessible via ``/collects/branch-collector-client-requests``, ce module traite les nouveaux clients recrutés sur les marchés ou à domicile par les agents de terrain.

Fonctionnalités :
-----------------

- **Réception des dossiers d'adhésion mobiles** :
  - Données d'état civil, adresses, professions et contacts saisis depuis l'application mobile.
- **Contrôle KYC Approfondi** :
  - Visualisation des pièces d'identité scannées (recto/verso).
  - Vérification de la conformité du spécimen de signature et de la photo d'identité.
- **Création du Compte et Attribution du CIF** :
  - Dès validation par le pôle clientèle ou collecte, le système crée le client définitif dans la base centrale.
  - Génération automatique du code client (CIF) et ouverture du compte de collecte associé.
  - Notification SMS automatique de bienvenue au nouveau client.

5. Gestion des Caisses de Collecte (Collect Boxes)
==================================================

Accessible via ``/collects/branch-collect-boxes``, cette fonctionnalité assure la sécurité financière des flux d'espèces en agence.

Fonctionnalités :
-----------------

- **Comptes internes de type caisse de collecte** : Isolation comptable des flux collecteurs par rapport aux guichets physiques traditionnels.
- **Ouverture de session de collecte** : Attribution d'un fond de caisse initial et enregistrement de l'heure d'ouverture.
- **Clôture de caisse de collecte (Collect Close Box)** :
  - Saisie du billetage physique compté au retour de tournée.
  - Comparaison automatique entre le solde théorique système et le solde physique déclaré.
  - Détection et enregistrement immédiat des écarts de caisse (excédents ou déficits).
  - Génération de l'écriture comptable de régularisation.
