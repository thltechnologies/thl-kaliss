.. _collect-configuration:

===========================================
Configuration et Activation du Module Collecte
===========================================

Ce document détaille l'architecture, la procédure d'activation/désactivation et le paramétrage des produits pour le module Collecte au sein de THL Core Banking.

.. contents:: Table des matières
   :depth: 3
   :local:

Architecture du Contrôle d'Activation
-------------------------------------

Le système repose sur une **sécurité multicouche à deux niveaux** pour garantir un verrouillage absolu et une gouvernance stricte du module de collecte :

.. mermaid::

    flowchart TD
        subgraph Niveau 1 [Niveau 1 : Infrastructure & Déploiement]
            ENV[Variable d'Environnement COLLECT_ENABLED] --> YML[Fichier application-*.yml collect.enabled]
            YML --> CHECK_ENV{collect.enabled == true ?}
        end

        subgraph Niveau 2 [Niveau 2 : Entreprise & Métier Dynamique]
            CHECK_ENV -->|Non| KILL[Module Verrouillé Globalement : Accès Refusé]
            CHECK_ENV -->|Oui| DB_CONF[Configuration en Base de Données : Table configuration / enterprise]
            DB_CONF --> CHECK_DB{collect_enabled == true ?}
            CHECK_DB -->|Non| DESACT[Module Désactivé pour l'Institution : Opérations Bloquées]
            CHECK_DB -->|Oui| ALLOW[Opérations de Collecte Autorisées]
        end

        style KILL fill:#ef4444,color:#fff
        style DESACT fill:#f97316,color:#fff
        style ALLOW fill:#10b981,color:#fff
        style CHECK_ENV fill:#3b82f6,color:#fff
        style CHECK_DB fill:#8b5cf6,color:#fff

Niveau 1 : Configuration Serveur (Spring Boot)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Au niveau des fichiers de propriétés Spring Boot (``application-dev.yml``, ``application-prod.yml``, ``application-cid.yml``, etc.), une variable globale pilote la disponibilité du module sur l'instance :

.. code-block:: yaml

   collect:
     enabled: ${COLLECT_ENABLED:false}

- Si ``COLLECT_ENABLED`` est absent ou vaut ``false``, le module est totalement désactivé pour tout le serveur (kill-switch infrastructure).
- La méthode ``collectService.isEnvironmentCollectEnabled()`` retourne alors ``false``, coupant court à toute exécution avant même d'interroger la base de données.

Niveau 2 : Configuration Dynamique par Tenant (Base de Données)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Lorsque l'environnement serveur autorise la collecte, l'activation effective dépend des colonnes dédiées introduites par la migration Flyway ``V1_0_72__Add_collect_module_configuration.sql`` :

.. code-block:: sql

   ALTER TABLE enterprise ADD COLUMN IF NOT EXISTS collect_enabled BOOLEAN DEFAULT FALSE;
   ALTER TABLE configuration ADD COLUMN IF NOT EXISTS collect_enabled BOOLEAN DEFAULT FALSE;

   CREATE TABLE IF NOT EXISTS configuration_collect_categories (
       configuration_id BIGINT NOT NULL,
       category_id BIGINT NOT NULL,
       PRIMARY KEY (configuration_id, category_id),
       CONSTRAINT fk_config_collect_cat_config FOREIGN KEY (configuration_id) REFERENCES configuration (id) ON DELETE CASCADE,
       CONSTRAINT fk_config_collect_cat_cat FOREIGN KEY (category_id) REFERENCES product_category (id) ON DELETE CASCADE
   );

   CREATE TABLE IF NOT EXISTS configuration_collect_products (
       configuration_id BIGINT NOT NULL,
       product_id BIGINT NOT NULL,
       PRIMARY KEY (configuration_id, product_id),
       CONSTRAINT fk_config_collect_prod_config FOREIGN KEY (configuration_id) REFERENCES configuration (id) ON DELETE CASCADE,
       CONSTRAINT fk_config_collect_prod_prod FOREIGN KEY (product_id) REFERENCES product (id) ON DELETE CASCADE
   );

Gestion dans l'Interface Utilisateur (Frontend)
-----------------------------------------------

Accès à l'Écran de Configuration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Connectez-vous avec un compte disposant des privilèges **SUPER_ADMIN**.
2. Dans le menu de navigation de gauche, cliquez sur la rubrique **Configurations**.
3. Sélectionnez le sous-menu **Collecte** (URL : ``/configuration/collect``).

.. note::
   La modification du statut du module et des catégories de collecte est **strictement réservée au rôle SUPER_ADMIN**. 
   Si un utilisateur connecté ne possède pas ce rôle, les composants interactifs sont désactivés et un avertissement sécurisé avec une icône de cadenas s'affiche :
   
   *"Seul le rôle Super Admin peut activer ou désactiver ce module."*

Activer ou Désactiver le Module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

L'interface présente un bloc dédié à l'interrupteur principal :

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Élément Visuel
     - Description et Fonctionnement
   * - **Badge de Statut**
     - Affiche en temps réel :
       
       - 🟢 **Collecte Active** (badge vert avec icône de coche) lorsque le module est en service.
       - 🔴 **Collecte Inactive** (badge rouge avec icône d'interdiction) lorsque le module est coupé.
   * - **Interrupteur (InputSwitch)**
     - Permet de basculer l'état du module (``configuration.collectEnabled``).
   * - **Événement Déclenché**
     - Tout changement sur le switch appelle instantanément l'API backend ``PUT /configuration/collect-module``.
   * - **Notification Utilisateur**
     - Un toast de confirmation notifie l'utilisateur :
       
       - *"Le module collecte a été activé avec succès."* (succès)
       - *"Le module collecte a été désactivé avec succès."* (avertissement)

Impacts Fonctionnels de la Désactivation
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Dès que le module de collecte est désactivé (soit au niveau serveur, soit au niveau de la configuration) :

1. **Blocage des Opérations en Caisse et Approvisionnements** :
   - Toute tentative de valider une alimentation de compte collecteur (``supplyCollectorAccount``) est bloquée.
   - La méthode ``collectService.checkCollectAllowed()`` déclenche immédiatement une exception HTTP 400 :
     
     .. code-block:: text

        BadRequestException: "Impossible d'effectuer cette opération"

2. **Interdiction de Création de Comptes de Collecte** :
   - L'ouverture d'un nouveau compte client rattaché à un produit de collecte est rejetée par ``AccountServiceImp``.

3. **Gel des Flux de Transactions de Terrain** :
   - Les requêtes de versement et de retrait soumises depuis les terminaux collecteurs ne peuvent plus être exécutées.
   - Les consultations de statistiques et résumés de dépôts sont suspendues.

Paramétrage des Catégories de Produits de Collecte
---------------------------------------------------

Pour qu'un compte bancaire ou une transaction soit traité selon les règles spécifiques de la collecte, le produit associé doit être identifié comme un **produit de collecte**.

Sélection Multi-Catégories
~~~~~~~~~~~~~~~~~~~~~~~~~~

Plutôt que d'attacher individuellement chaque produit, THL Core Banking permet de sélectionner une ou plusieurs **catégories de produits** :

1. Dans la section **Catégories de Produits de Collecte**, utilisez le composant multisélection (``p-multiSelect``).
2. Choisissez les catégories souhaitées (ex: *Collecte Journalière*, *Tontine Mobile*, *Épargne Terrain*).
3. Cliquez sur le bouton **Enregistrer**.
4. L'appel API transmet les identifiants au contrôleur via ``PUT /configuration/collect-configuration`` avec le modèle :

.. code-block:: json

   {
     "collectEnabled": true,
     "categoryIds": [3, 7],
     "productIds": []
   }

Règle de Détection des Produits (isCollectProduct)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Le service backend applique la logique suivante dans ``CollectServiceImpl.java`` :

- Si une ou plusieurs catégories sont configurées dans ``config.getCollectCategories()``, tout produit appartenant à l'une de ces catégories est automatiquement qualifié de **produit de collecte** :

.. code-block:: java

   boolean matchesCategory = config.getCollectCategories().stream()
           .anyMatch(c -> product.getCategory().getId().equals(c.getId()));

Rétrocompatibilité (Fallback Historique)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Pour assurer la continuité de service des déploiements historiques où aucune catégorie personnalisée n'est encore paramétrée en base :

- Le système conserve automatiquement la règle par défaut : tout produit dont la catégorie porte le code historique **"04"** est reconnu comme un produit de collecte.

Consultation et Filtrage des Produits de Collecte
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Directement sous le formulaire de configuration, un tableau récapitulatif liste les produits impactés par les catégories sélectionnées :

- **Compteur dynamique** : Affiche le total des produits trouvés (ex: *Total : 12*).
- **Filtre par état** :
  - *Produits Actifs* (``ACTIVATED``)
  - *Produits Désactivés* (``DEACTIVATED``)
- **Bouton d'actualisation** : Recharge la liste depuis l'API ``GET /product/collect-products-by-state?state=...``.
- **Colonnes du tableau** : Code produit, Nom du produit, Catégorie rattachée (badge informatif), État opérationnel (tag couleur).

Spécifications des API REST Backend
-----------------------------------

Les points de terminaison suivants sont exposés par ``ConfigurationController`` et sécurisés par Spring Security :

1. Basculer l'état du module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. http:put:: /configuration/collect-module

   Active ou désactive le module de collecte.

   **Autorisation requise :** ``SUPER_ADMIN``

   **Corps de la requête :**

   .. code-block:: json

      {
        "collectEnabled": true
      }

   **Réponse (200 OK) :**

   .. code-block:: json

      {
        "id": 1,
        "collectEnabled": true,
        "collectCategories": [
          { "id": 3, "name": "Épargne Collecte", "code": "COL01" }
        ]
      }

2. Enregistrer la configuration complète
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. http:put:: /configuration/collect-configuration

   Met à jour conjointement le statut d'activation et la liste des catégories de collecte.

   **Autorisation requise :** ``SUPER_ADMIN``

   **Corps de la requête :**

   .. code-block:: json

      {
        "collectEnabled": true,
        "categoryIds": [3, 5],
        "productIds": []
      }

3. Consulter la configuration active
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. http:get:: /configuration/find-configuration

   Retourne la configuration complète incluant les paramètres du module de collecte.

   **Autorisation requise :** ``SUPER_ADMIN`` ou ``CONSULTANT``

4. Consulter les produits de collecte par état
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. http:get:: /product/collect-products-by-state?state=ACTIVATED

   Retourne tous les produits rattachés aux catégories de collecte actives.

   **Autorisation requise :** Utilisateur authentifié avec droits d'accès aux produits.
