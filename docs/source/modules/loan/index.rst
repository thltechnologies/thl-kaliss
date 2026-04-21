.. _loan-index:

Financements
============

Le module **Financements** est un système complet de gestion des financements qui permet la création, l'approbation, le suivi et le remboursement des financements accordés aux clients. Il intègre un système de tarification avancé, des échéanciers flexibles et une gestion automatisée des remboursements.

.. note::
   **Mises à jour importantes - Octobre 2025**
   
   Le module financements a bénéficié de corrections critiques et d'améliorations majeures :
   
   - ✅ Migration complète vers le système de composants de transaction
   - ✅ Correction des calculs financiers avec protection contre les divisions par zéro
   - ✅ Amélioration de la précision des échéanciers pour tous les types d'échéances
   - ✅ Renforcement des validations et de la sécurité des données
   - ✅ Correction des rapports et exports Excel
   - ✅ **Nouveau !** Système de conformité documentaire bancaire (KYC)
   - ✅ **Optimisation** : Centralisation du taux TAF au niveau de la configuration globale
   - ✅ **Avril 2026** : Export Excel et **Envoi E-mail automatique** (Profil connecté)
   - ✅ **Avril 2026** : Refonte de l'interface avec barre d'outils horizontale premium
   
   Ces améliorations garantissent une meilleure fiabilité et précision du système.

1. Création et gestion des financements
--------------------------------------

Cette section permet aux gestionnaires de financements de créer, modifier et gérer les financements accordés aux clients.

Procédure
~~~~~~~~~

1. Connectez-vous au logiciel.
2. Accédez au module **Financements**.
3. Cliquez sur **Nouveau financement** pour créer un financement.

   \a. **Création d'un financement**

      Dans le formulaire de création, renseignez :
         - Le type de financement (Murabaha, Ijara, Musharaka, Qard, etc.)
         - Le client bénéficiaire
         - Le montant du financement (prix d'achat + marge + TAF)
         - La date de livraison
         - La date du premier remboursement
         - Le type d'échéance (mensuel, trimestriel, etc.)
            - Une description du financement

   \b. **Modification d'un financement**

      - Sélectionnez le financement à modifier
      - Mettez à jour les informations nécessaires
      - Sauvegardez les modifications

   \c. **Mise à jour du produit de financement**

      - Modifiez le produit associé au financement
      - Ajustez les paramètres de tarification
      - Mettez à jour les conditions contractuelles

2. Approbation des financements
------------------------------

Cette fonctionnalité permet aux validateurs de financements d'approuver ou de rejeter les demandes de financement.

Procédure
~~~~~~~~~

1. Connectez-vous au logiciel avec les droits de **LOAN_VALIDATOR**.
2. Accédez au module **Financements**.
3. Consultez la liste des financements en attente d'approbation.

   \a. **Processus d'approbation**

      - Examinez les détails du financement
      - Vérifiez la solvabilité du client
      - Validez les conditions contractuelles
      - **Vérifiez la conformité du dossier documentaire**
      - Approuvez ou rejetez la demande

   \b. **Contrôles de sécurité**

      - Vérification des autorisations
      - Validation des montants
      - Contrôle de cohérence des données

3. Gestion des remboursements
------------------------------

Cette section permet de gérer les échéances et les remboursements des financements.

Procédure
~~~~~~~~~

1. Connectez-vous au logiciel.
2. Accédez au module **Financements**.
3. Sélectionnez le financement concerné.

   \a. **Création d'un remboursement**

      - Cliquez sur **Nouveau remboursement**
      - Renseignez le montant à rembourser
      - Définissez la période
      - Spécifiez la date de paiement

   \b. **Confirmation des remboursements**

      - Vérifiez les détails du remboursement
      - Confirmez le paiement
      - Le système génère automatiquement les transactions

   \c. **Remboursements personnalisés**

      - Créez des remboursements sur mesure
      - Ajustez les montants selon les besoins
      - Gérez les reports d'échéances

   \d. **Gestion des retards**

      - Identification automatique des échéances en retard
      - Calcul des pénalités
      - Suivi des remboursements en souffrance

4. Remboursement anticipé
--------------------------

Cette fonctionnalité permet aux clients de rembourser leurs financements de manière anticipée.

Procédure
~~~~~~~~~

1. Connectez-vous au logiciel.
2. Accédez au module **Financements**.
3. Sélectionnez le financement à rembourser.

   \a. **Calcul du remboursement anticipé**

      - Le système calcule automatiquement le montant dû
      - Prise en compte des intérêts et frais
      - Génération du solde de tout compte

   \b. **Validation du remboursement**

      - Vérification des fonds disponibles
      - Confirmation du paiement
      - Clôture du financement

5. Reports et exports
---------------------

Cette section permet de générer des rapports et d'exporter les données de financements.

Procédure
~~~~~~~~~

1. Connectez-vous au logiciel.
2. Accédez au module **Financements**.
3. Sélectionnez l'option **Rapports**.

   \a. **Export des financements**

      - Export de la liste complète des financements
      - Filtrage par période, client, ou statut
      - Export au format Excel et **Envoi E-mail automatique**
      - Disposition horizontale pour un accès rapide aux actions d'export

   \b. **Rapport des financements en retard**

      - Liste des financements en souffrance
      - Détails des échéances manquées
      - Export pour suivi de recouvrement

   \c. **Rapport de performance**

      - Statistiques de remboursement
      - Analyse des retards
      - Indicateurs de qualité du portefeuille

6. Système de tarification
---------------------------

Le système intègre un moteur de tarification sophistiqué basé sur la formule :

.. math::
   
   \text{Marge} &= \text{Prix de vente} - \text{Prix d'achat} \\
   \text{Pourcentage marge} &= \frac{\text{Marge}}{\text{Prix d'achat}} \times 100 \\
   \text{Valeur TAF} &= \text{Marge} \times \frac{\text{Pourcentage TAF}}{100} \\
   \text{Montant total} &= \text{Prix d'achat} + \text{Marge} + \text{Valeur TAF}

**Validations automatiques** :

- Prix d'achat doit être strictement supérieur à 0 (évite les divisions par zéro)
- Prix de vente doit être supérieur ou égal au prix d'achat
- Tous les calculs sont effectués avant l'enregistrement (évite les dépendances circulaires)
- Précision garantie sur tous les montants financiers

Procédure
~~~~~~~~~

1. **Configuration des paramètres**

   - Définition des pourcentages de marge
   - **Configuration globale du taux TAF** (via Menu Configuration)
   - Paramétrage des frais additionnels
   - Validation automatique de la cohérence des données

2. **Calcul automatique**

   - Le système calcule automatiquement le montant total
   - Application des règles de tarification islamique
   - Utilisation du taux TAF configuré dans le système
   - Génération de l'échéancier selon le type d'échéance
   - Vérification de la cohérence des montants

3. **Types de financements supportés**

   - **Murabaha** : Vente avec marge (financement islamique)
   - **Ijara** : Location-vente (leasing islamique)
   - **Musharaka** : Partenariat (participation islamique)
   - **Qard** : Financement sans intérêt (prêt bienveillant)
   - **Autres types** : Financements conventionnels

4. **Types d'échéances disponibles**

   - **MONTHLY** : Mensuel (30 jours)
   - **QUARTERLY** : Trimestriel (90 jours)
   - **FOURTH_MONTH** : Quatre mois (120 jours)
   - **FIFTH_MONTH** : Cinq mois (150 jours)
   - **SEMI_ANNUAL** : Semestriel (180 jours)
   - **ANNUAL** : Annuel (360 jours)

.. warning::
   Le calcul de l'échéancier est automatiquement mis à jour lorsque vous modifiez :
   
   - La date du premier remboursement
   - Le délai (nombre de périodes)
   - Le type d'échéance
   
   Cette mise à jour supprime les remboursements non validés et régénère l'échéancier.

7. Dossier de Financement et Conformité
---------------------------------------

Le système de conformité documentaire (KYC) permet de garantir que toutes les pièces justificatives sont présentes et validées avant l'approbation du financement.

Workflow de conformité :
~~~~~~~~~~~~~~~~~~~~~~~~

Chaque document requis passe par un workflow de statut rigoureux :

- **En attente (PENDING)** : Document non encore soumis (statut initial).
- **Soumis (SUBMITTED)** : Le document a été reçu mais n'est pas encore vérifié.
- **Validé (VALIDATED)** : Le document est conforme et certifié par un agent.
- **Rejeté (REJECTED)** : Le document est refusé (un motif de rejet est obligatoire).
- **Expiré (EXPIRED)** : Le document n'est plus valable (ex: CNI périmée).

Pièces Obligatoires :
~~~~~~~~~~~~~~~~~~~~~

Certaines documents peuvent être marqués comme **"Obligatoires"** au niveau du référentiel. 

.. important::
   **Blocage Système** : Le système bloque automatiquement l'approbation d'un financement si l'une de ses pièces obligatoires n'est pas au statut **Validé**.

Audit et Traçabilité :
~~~~~~~~~~~~~~~~~~~~~~

Pour chaque pièce du dossier, le système enregistre :
- Le dernier agent ayant modifié le statut.
- La date et l'heure exacte de la dernière mise à jour.
- Le motif de rejet en cas de refus.
- La date de validité pour les documents ayant une expiration.

Démo
~~~~

1. Le gestionnaire de financements veut créer un financement Murabaha de 500 000 FCFA pour le client John Doe.

2. Le validateur de financements veut approuver une demande de financement en attente.

3. Le gestionnaire veut confirmer un remboursement de 50 000 FCFA pour le financement #12345.

4. Le client veut effectuer un remboursement anticipé de son financement.
