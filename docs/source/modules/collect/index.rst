.. _collect-index:

==============
Module Collecte
==============

Le module **Collecte** (ou collecte journalière / tontine mobile) est une composante clé du système THL Core Banking, dédiée aux opérations de terrain et à l'inclusion financière. Il permet aux institutions financières de déployer des agents collecteurs pour enregistrer des clients, collecter des dépôts journaliers, traiter des demandes de retrait et approvisionner leurs caisses mobiles de façon sécurisée et synchronisée avec le système central.

.. note::
   L'accès et le fonctionnement du module sont entièrement assujettis à son **activation dans la configuration générale du système**. En cas de désactivation, toutes les opérations de collecte sont immédiatement bloquées pour préserver l'intégrité comptable et financière.

.. toctree::
   :maxdepth: 2
   :caption: 📑 Sommaire du Module

   configuration
   all_features
   new_features

Vue d'ensemble
--------------

Le module Collecte offre un écosystème complet structuré autour de 5 axes majeurs :

1. **Administration & Paramétrage Centralisé** :
   - Activation / désactivation globale du module par le ``SUPER_ADMIN``.
   - Définition des catégories de produits réservées à la collecte (ex: Tontine, Épargne journalière).
   - Visualisation et contrôle en temps réel des produits de collecte actifs et désactivés.

2. **Supervision des Collectes en Agence** :
   - Tableau de bord des opérations de collecte avec filtres multi-critères (période, collecteur, agence).
   - Suivi des totaux attendus versus collectés (solde théorique vs solde réel).
   - Édition et téléchargement des bordereaux de collecte (PDF / Excel).

3. **Gestion des Collecteurs de Terrain** :
   - Répertoire des collecteurs rattachés à l'agence.
   - Affectation de portefeuilles de clients et de comptes à gérer.
   - Suivi des performances individuelles et récapitulatif des dépôts périodiques.

4. **Traitement des Requêtes de Transaction** :
   - Gestion des flux d'approvisionnement des comptes collecteurs.
   - Circuit d'approbation et validation par le Responsable de Collecte ou le Chef d'Agence.
   - Déversement automatique en comptabilité et mise à jour des soldes de caisse.

5. **Enrôlement Clientèle et Caisses Mobiles** :
   - Réception et validation des demandes d'adhésion client initiées sur le terrain.
   - Contrôle KYC : vérification des pièces d'identité et spécimens de signature.
   - Gestion des caisses de collecte (*Collect Boxes*) : ouverture, alimentation et clôture journalière.

Flux Opérationnel de Collecte
------------------------------

Le diagramme ci-dessous illustre le cycle complet d'une journée de collecte, depuis la vérification de la configuration jusqu'au versement final en caisse d'agence :

.. mermaid::

    graph TD
        A[Configuration Système : Collecte Active ?] -->|Non| B[Opérations Bloquées : BadRequestException]
        A -->|Oui| C[Attribution Portefeuille & Ouverture Caisse Collecte]
        C --> D[Collecteur sur le Terrain : Dépôts & Enrôlements]
        D --> E[Transmission des Demandes au Core Banking]
        E --> F{Validation Responsable Collecte / Agence}
        F -->|Rejet| G[Demande Rejetée avec Motif]
        F -->|Validation| H[Crédit des Comptes Clients & Mise à jour Soldes]
        H --> I[Clôture Caisse Collecteur & Rapprochement Espèces]
        I --> J[Intégration Comptable Finale]

        style A fill:#3b82f6,color:#fff
        style B fill:#ef4444,color:#fff
        style C fill:#0ea5e9,color:#fff
        style D fill:#f59e0b,color:#fff
        style F fill:#8b5cf6,color:#fff
        style H fill:#10b981,color:#fff
        style J fill:#10b981,color:#fff

Rôles et Habilitations
-----------------------

L'accès aux différentes fonctionnalités du module est strictement hiérarchisé :

.. list-table::
   :widths: 25 25 50
   :header-rows: 1

   * - Rôle
     - Niveau d'Accès
     - Périmètre de Responsabilité
   * - ``SUPER_ADMIN``
     - Système Global
     - Activation/Désactivation du module, sélection des catégories de produits de collecte, audit global.
   * - ``COLLECT_MANAGER``
     - Agence / Pôle Collecte
     - Supervision des collectes, validation des requêtes, gestion des collecteurs et clôture des caisses de collecte.
   * - ``BRANCH_MANAGER``
     - Agence
     - Validation finale des transactions d'approvisionnement et supervision du pôle collecte de son agence.
   * - ``CONSULTANT``
     - Lecture Seule
     - Consultation des bordereaux, des statistiques de collecte et des configurations.
   * - ``COLLECTOR`` (Mobile)
     - Application Terrain
     - Saisie des dépôts, création des demandes de clients et demande d'approvisionnement.
