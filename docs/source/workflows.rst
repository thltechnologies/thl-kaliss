.. _workflows:

Diagrammes de Workflow
======================

Cette section présente les processus métiers clés du système sous forme de diagrammes visuels pour faciliter la compréhension des flux opérationnels.

Cycle de Vie d'un Financement
-----------------------------

Ce diagramme illustre les étapes successives par lesquelles passe un dossier de financement, de la demande initiale jusqu'au remboursement final ou à l'annulation.

.. mermaid::

    graph TD
        A[Demande de Financement] --> B{Analyse de Risque}
        B -->|Refut| C[Dossier Rejeté]
        B -->|Accepté| D[Approbation / Signature]
        D --> E[Décaissement / Livraison]
        E --> F[Paiements des Traites]
        F --> G{Solde Restant = 0 ?}
        G -->|Non| F
        G -->|Oui| H[Financement Terminé]
        
        style A fill:#f9f,stroke:#333
        style H fill:#9f9,stroke:#333
        style C fill:#f99,stroke:#333

Workflow d'Ouverture et Approvisionnement de Caisse
---------------------------------------------------

Ce flux décrit l'interaction entre un caissier et son chef d'agence pour le démarrage d'une session de travail.

.. mermaid::

    sequenceDiagram
        participant C as Caissier
        participant S as Système (THL Core Banking)
        participant CA as Chef d'Agence
        
        C->>S: Demande d'approvisionnement (Montant X)
        S->>S: Création TransactionRequest (PENDING)
        S->>CA: Notification de demande
        CA->>S: Validation / Confirmation
        S->>S: Génération Écritures Comptables
        S->>S: Changement Statut (CONFIRMED)
        S->>C: Caisse activée / Billetage ouvert
        
        Note over C,CA: Le coffre-fort de l'agence est débité au profit de la caisse.

Processus de Clôture d'Exercice Comptable
-----------------------------------------

Étapes obligatoires pour verrouiller une année fiscale et passer à la suivante.

.. mermaid::

    graph LR
        Step1[Clôturer toutes les Périodes] --> Step2[Réconciliation Bancaire Totale]
        Step2 --> Step3[Validation des Écritures]
        Step3 --> Step4[Calcul de la Balance Finale]
        Step4 --> Step5[Clôture de l'Exercice]
        Step5 --> Step6[Ouverture Nouvel Exercice]

        style Step5 fill:#f66,stroke:#333
        style Step6 fill:#66f,stroke:#333,color:#fff
