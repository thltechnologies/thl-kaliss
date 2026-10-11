.. _collect-new-features:

===========================================
Nouvelles Fonctionnalités - Module Collecte
===========================================

Cette page résume les évolutions majeures, optimisations et renforcements de sécurité récemment intégrés au module **Collecte**.

1. Refonte Dynamique de la Configuration des Produits
=====================================================

Auparavant, le système reposait sur un code de catégorie fixe en dur (code ``"04"``) pour identifier les produits d'épargne journalière. Cette contrainte limitait les institutions ayant plusieurs gammes de produits ou des nomenclatures personnalisées.

**Améliorations apportées :**
- **Sélection multi-catégories** : Capacité d'associer un nombre illimité de catégories de produits au périmètre de la collecte via une table de jointure relationnelle (``configuration_collect_categories``).
- **Détection dynamique** : Tous les produits créés sous les catégories sélectionnées bénéficient automatiquement des règles de gestion propres à la collecte (gestion des dépôts minimums, commissions collecteurs, flux de tournée).
- **Rétrocompatibilité transparente** : Si une institution n'a pas encore configuré de catégories explicites, le système bascule automatiquement sur le code historique ``"04"`` sans rupture d'activité.

2. Double Sécurité d'Activation et Gouvernance RBAC
===================================================

Pour répondre aux exigences de conformité et d'audit des institutions financières :

- **Interrupteur d'infrastructure (Kill-Switch)** :
  - Introduction du paramètre ``collect.enabled: ${COLLECT_ENABLED:false}`` dans les profils Spring Boot (``application-dev.yml``, ``application-prod.yml``, etc.).
  - Permet à l'équipe DevOps de désactiver immédiatement la surface d'attaque ou l'accès au module sur un environnement donné sans redémarrage de la base.
- **Réservation exclusive au SUPER_ADMIN** :
  - Le verrouillage ou déverrouillage du module est strictement limité au rôle ``SUPER_ADMIN`` via les annotations Spring Security ``@PreAuthorize("hasAuthority('SUPER_ADMIN')")``.
  - Dans l'interface utilisateur, tout utilisateur non Super Admin visualise un cadenas de sécurité avec message d'interdiction et composants désactivés.
- **Verrouillage préventif strict** :
  - La méthode ``collectService.checkCollectAllowed()`` est appelée sur tous les points d'entrée critiques (création de compte de collecte, approvisionnement, versements).
  - Toute tentative non autorisée lève immédiatement une exception ``BadRequestException("Impossible d'effectuer cette opération")``.

3. Expérience Utilisateur Modernisée (Dashboard Angular)
========================================================

L'écran de configuration du module (``/configuration/collect``) a été entièrement conçu pour offrir clarté et contrôle :

- **Indicateur visuel d'état** :
  - Tag couleur PrimeNG haute visibilité : ``Collecte Active`` (vert avec icône de coche) ou ``Collecte Inactive`` (rouge avec icône d'interdiction).
- **Sélecteur de catégories en chips** :
  - Recherche et filtrage en temps réel dans le catalogue des catégories.
  - Affichage sous forme de badges amovibles (chips).
- **Tableau des produits associés avec filtre par état** :
  - Visualisation instantanée de tous les produits régis par les catégories choisies.
  - Bascule rapide entre produits actifs (``ACTIVATED``) et désactivés (``DEACTIVATED``).
  - Bouton de rafraîchissement à chaud sans rechargement de page.

4. Performance et Optimisation des Requêtes
===========================================

- **Élimination des requêtes N+1** :
  - Optimisation des requêtes de synthèse des dépôts collecteurs (``getDepositSummaryByCollectorAndDateBetween``) et des relevés périodiques.
  - Calcul direct des agrégats (somme des dépôts minimums, somme des dépôts réalisés) en base de données pour un traitement ultra-rapide des téléchargements de bordereaux.
