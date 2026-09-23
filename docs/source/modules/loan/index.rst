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
   - ✅ **Nouveau !** Calendrier de recouvrement interactif et système de relance client
   
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

   \e. **Remboursement partiel**

      - Si l'option *Autoriser le prélèvement du montant disponible dans le solde en cas de solde insuffisant* est activée globalement dans la configuration :
         - En cas de solde insuffisant du compte client pour régler la totalité de l'échéance lors d'un remboursement automatique ou manuel, le système prélève le solde disponible (qui devient alors nul).
         - Un enregistrement de remboursement partiel (`PartialLoanRepayment`) est créé pour suivre ce versement.
         - L'échéance principale voit son montant payé incrémenté, mais reste dans l'état non validée (`transactionValidated = false`) jusqu'à ce que la somme des remboursements partiels atteigne le montant total de l'échéance.
         - Les écritures comptables associées sont automatiquement recalculées et retraitées pour refléter le montant partiel effectivement perçu.

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
   - **Qard** : Financement sans intérêt (financement bienveillant)
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

8. Gestion des Pénalités de Retard
----------------------------------

Le système propose un cadre strict et automatisé pour l'application des pénalités en cas de retard de paiement :

* **Configuration Globale** : Un administrateur définit au niveau de la configuration générale du système si les pénalités sont activées (`penaltyEnabled`), le montant quotidien par jour de retard (`penaltyRate`), ainsi que le statut de conformité Shariah (`shariahCompliant`).
* **Calcul du Retard** : Le retard (nombre de jours) est calculé sur une base de **jours calendaires complets** (`LocalDate`), en ignorant les heures et minutes d'enregistrement. Par exemple, un remboursement dû le 30/05 et payé le 02/06 compte pour exactement 3 jours de retard.
* **Ordre de Prélèvement (Prioritaire)** : Les pénalités de retard sont prélevées en priorité absolue sur tout montant versé. Lors d'un remboursement, les fonds disponibles couvrent d'abord la pénalité accumulée due, puis le reliquat est affecté à l'échéance (marge puis principal).
* **Prévention de Découvert** : Le système ne prélève jamais plus que le solde disponible sur le compte du client pour éviter de le mettre en négatif, à moins qu'un découvert ne soit explicitement autorisé dans les paramètres.
* **Statut de Complétion** : Une échéance n'est marquée validée (`completed`) que lorsque la totalité de la mensualité due ET de la pénalité calculée ont été entièrement payées.
* **Garde-fous Métiers (Option A - Stricte)** :
    - L'activation des pénalités sur un dossier de financement individuel est bloquée si les pénalités sont désactivées globalement.
    - Lors de la mise en place d'un financement, si les pénalités globales sont actives, l'agent peut choisir d'activer les pénalités sur ce financement. Les champs **Montant par jour de retard** et **Conformité Shariah** sont en lecture seule et héritent automatiquement et strictement des valeurs de la configuration générale pour éviter toute altération.
* **Traitement Comptable (Conformité Shariah)** :
    - Si l'option **Conformité Shariah** est active, les pénalités perçues sont considérées comme un don obligatoire et reversées dans un compte de charité/Sadakah (`CHARITY_SADAKAH`).
    - Dans le cas contraire, elles sont créditées sur le compte de revenus de pénalités de l'institution (`PENALTY_INCOME`).

9. Calendrier de Recouvrement et Relance Client
-----------------------------------------------

Le **Calendrier de Recouvrement** centralise l'ensemble des échéances de remboursement du portefeuille de financements sur une interface dynamique et ergonomique basée sur FullCalendar. Il permet aux équipes de recouvrement et aux gestionnaires de suivre visuellement les flux financiers attendus, d'anticiper les risques d'impayés et de déclencher les relances nécessaires auprès des clients.

.. note::
   **Accès et habilitations** :
   Le calendrier est accessible depuis le menu **Financements** > **Calendrier Recouvrement** (route ``/loans/recovery``).
   Son accès est réservé aux profils autorisés : **SUPER_ADMIN**, **LOAN_MANAGER** et **LOAN_VALIDATOR**.

Fonctionnalités principales
~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **Vues temporelles flexibles** :
   - **Vue Mois (par défaut)** : Affiche les échéances planifiées sur le mois en cours.
   - **Vue Année / Multi-mois** : Permet une vision globale sur l'ensemble de l'exercice ou sur plusieurs mois consécutifs.
   - **Navigation rapide** : Boutons de navigation (*Aujourd'hui*, *Précédent*, *Suivant*) pour naviguer facilement d'une période à l'autre.

2. **Indicateurs visuels et statuts par couleur** :
   Chaque événement d'échéance indique le nom du client (ou la raison sociale) ainsi que le montant dû (ex: *Mamadou Diallo - 150 000 F*). Un code couleur intuitif permet d'identifier immédiatement la situation de chaque échéance :

   - 🔴 **En retard (Rouge / #ef4444)** : Échéances non réglées dont la date d'exigibilité est dépassée.
   - 🟡 **À venir (< 7 jours) (Orange / #f59e0b)** : Échéances exigibles dans les 7 prochains jours calendaires, idéales pour une relance préventive.
   - 🔵 **Futur (Bleu / #3b82f6)** : Échéances planifiées au-delà de 7 jours.
   - 🟢 **Payé (Vert / #22c55e)** : Échéances intégralement réglées et validées comptablement.

3. **Filtrage interactif par puces (Chips)** :
   Quatre puces cliquables en en-tête permettent de masquer ou d'afficher instantanément les statuts selon le besoin opérationnel :
   
   - Filtrer uniquement les **Retards** pour une campagne de recouvrement curatif.
   - Filtrer les échéances **À venir** pour des rappels de courtoisie préventifs.
   - Masquer les échéances déjà **Payées** pour désencombrer la grille.

4. **Détails de l'échéance (Fenêtre modale)** :
   Le clic sur un événement ouvre une boîte de dialogue présentant le récapitulatif complet :
   
   - **Identité du client** : Nom, prénom ou nom de l'entreprise cliente.
   - **Financement associé** : Intitulé du contrat de financement.
   - **Montant dû** : Montant exact formaté en monnaie locale (ex: FCFA / XOF).
   - **Date d'échéance** : Date contractuelle de paiement.
   - **Progression** : Numéro de l'échéance par rapport au nombre total de traites (ex: *Période 3 / 12*).
   - **Statut** : Badge coloré (*Payé*, *En retard*, *En attente*).
   - **Horodatage du paiement** : Date et heure réelles du prélèvement si l'échéance est validée.

5. **Actions opérationnelles et relance client** :
   - **Voir le dossier** : Redirige directement vers la fiche détaillée du financement du client (onglet de suivi des échéances et garanties).
   - **Relancer** : Pour toute échéance non soldée, le bouton **Relancer** permet de déclencher l'envoi d'un rappel ou d'une notification multicanale (WhatsApp / SMS / Notification) au client afin de l'inviter à approvisionner son compte.

Architecture et Intégration API
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **API REST Backend** : ``GET /repayments/calendar?start={ISO_DATE}&end={ISO_DATE}``
- **Composant Frontend** : ``LoanRecoveryComponent`` (Angular, FullCalendar v6, PrimeNG Dialog & Chips)
- **Chargement optimisé** : Les échéances sont chargées de façon paresseuse (*lazy loading*) en fonction des dates limites de la vue calendaire courante, garantissant fluidité et rapidité d'affichage.

Démo
~~~~

1. Le gestionnaire de financements veut créer un financement Murabaha de 500 000 FCFA pour le client John Doe.

2. Le validateur de financements veut approuver une demande de financement en attente.

3. Le gestionnaire veut confirmer un remboursement de 50 000 FCFA pour le financement #12345.

4. Le client veut effectuer un remboursement anticipé de son financement.

5. Le chargé de recouvrement ouvre le **Calendrier de Recouvrement**, filtre les échéances **À venir (< 7j)** et clique sur **Relancer** pour notifier les clients avant l'échéance.

.. toctree::
   :maxdepth: 1
   :caption: 📋 Documentation détaillée

   all_features
   new_features

