.. _chart-of-account-hierarchy:

Structure Hiérarchique du Plan Comptable SFD
============================================

📋 Vue d'ensemble
-----------------

Le plan comptable suit la structure hiérarchique définie dans le **Référentiel Comptable Spécifique des Systèmes Financiers Décentralisés de l'UMOA - Chapitre 1.4**.

🏗️ Structure Hiérarchique
-------------------------

Selon le référentiel, chaque compte est désigné par un numéro et un intitulé. Le premier chiffre représente la classe, et les chiffres suivants (de gauche à droite) décrivent de façon plus détaillée la nature des opérations.

Niveaux de la hiérarchie
~~~~~~~~~~~~~~~~~~~~~~~~

+--------+--------------------+-----------------------------+-----------------------+-------+
| Niveau | Type               | Description                 | Exemple               | Code  |
+========+====================+=============================+=======================+=======+
| 0      | **Classe**         | Premier chiffre du compte   | Classe 2              | `2`   |
+--------+--------------------+-----------------------------+-----------------------+-------+
| 1      | **Poste**          | Deux premiers chiffres      | Poste 20              | `20`  |
+--------+--------------------+-----------------------------+-----------------------+-------+
| 2      | **Compte général** | Trois premiers chiffres     | Compte général 202    | `202` |
+--------+--------------------+-----------------------------+-----------------------+-------+
| 3      | **Compte**         | Quatre premiers chiffres    | Compte 2022           | `2022`|
+--------+--------------------+-----------------------------+-----------------------+-------+
| 4+     | **Sous-compte**    | Cinq chiffres ou plus       | Sous-compte 20221     | `20221`|
+--------+--------------------+-----------------------------+-----------------------+-------+

Exemple de hiérarchie
~~~~~~~~~~~~~~~~~~~~~

.. code-block:: text

    Classe:      2
    Poste:       20
    Compte général: 202
    Compte:      2022
    Sous-compte: 20221

💻 Utilisation dans le code
---------------------------

Modèle ChartOfAccount
~~~~~~~~~~~~~~~~~~~~~

Le modèle ``ChartOfAccount`` inclut des méthodes pour accéder aux différents niveaux :

.. code-block:: java

    ChartOfAccount account = chartOfAccountRepository.findByCode("20221");

    // Obtenir le type de niveau
    ChartOfAccountLevelType levelType = account.getLevelType(); // SOUS_COMPTE

    // Obtenir les codes à chaque niveau
    String classe = account.getClasseCode();           // "2"
    String poste = account.getPosteCode();             // "20"
    String compteGeneral = account.getCompteGeneralCode(); // "202"
    String compte = account.getCompteCode();           // "2022"
    String sousCompte = account.getSousCompteCode();  // "20221"

    // Vérifier le type
    boolean isClasse = account.isClasse();             // false
    boolean isPoste = account.isPoste();               // false
    boolean isCompteGeneral = account.isCompteGeneral(); // false
    boolean isCompte = account.isCompte();             // false
    boolean isSousCompte = account.isSousCompte();     // true

    // Affichage hiérarchique
    String display = account.getHierarchicalDisplay(); 
    // "2 | 20 | 202 | 2022 | 20221"

Enum ChartOfAccountLevelType
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

    public enum ChartOfAccountLevelType {
        CLASSE(0, "Classe"),
        POSTE(1, "Poste"),
        COMPTE_GENERAL(2, "Compte général"),
        COMPTE(3, "Compte"),
        SOUS_COMPTE(4, "Sous-compte");
    }

DTO ChartOfAccountResponse
~~~~~~~~~~~~~~~~~~~~~~~~~~

Le DTO inclut toutes les informations de hiérarchie :

.. code-block:: json

    {
      "id": 123,
      "code": "20221",
      "label": "Sous-compte exemple",
      "level": 4,
      "levelType": "SOUS_COMPTE",
      "classeCode": "2",
      "posteCode": "20",
      "compteGeneralCode": "202",
      "compteCode": "2022",
      "sousCompteCode": "20221",
      "hierarchicalDisplay": "2 | 20 | 202 | 2022 | 20221"
    }

🔍 Requêtes utiles
------------------

Trouver tous les comptes d'une classe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

    List<ChartOfAccount> comptes = chartOfAccountRepository
        .findByCodeStartingWithAndState("2", State.ACTIVATED);

Trouver tous les postes d'une classe
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

    List<ChartOfAccount> postes = chartOfAccountRepository
        .findByLevelAndState(1, State.ACTIVATED)
        .stream()
        .filter(c -> c.getClasseCode().equals("2"))
        .collect(Collectors.toList());

Construire la hiérarchie complète
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. code-block:: java

    ChartOfAccount sousCompte = chartOfAccountRepository.findByCode("20221");

    // Remonter la hiérarchie
    ChartOfAccount compte = chartOfAccountRepository.findByCode(
        sousCompte.getCompteCode());
    ChartOfAccount compteGeneral = chartOfAccountRepository.findByCode(
        sousCompte.getCompteGeneralCode());
    ChartOfAccount poste = chartOfAccountRepository.findByCode(
        sousCompte.getPosteCode());
    ChartOfAccount classe = chartOfAccountRepository.findByCode(
        sousCompte.getClasseCode());

📊 Structure des classes du plan comptable SFD
----------------------------------------------

Selon le référentiel UMOA, le plan comptable comprend 9 classes principales :

1. **Classe 1**: Opérations de trésorerie et avec les institutions financières
2. **Classe 2**: Opérations avec les membres, bénéficiaires ou clients
3. **Classe 3**: Opérations sur titres et opérations diverses
4. **Classe 4**: Valeurs immobilisées
5. **Classe 5**: Provisions, fonds propres et assimilés
6. **Classe 6**: Comptes de charges
7. **Classe 7**: Comptes de produits
8. **Classe 8**: Comptes de résultat
9. **Classe 9**: Comptes d'engagements hors bilan

🎯 Avantages de cette structure
-------------------------------

1. **Traçabilité**: Chaque compte peut être identifié à tous les niveaux de la hiérarchie
2. **Agrégation**: Facilite les calculs de synthèse par niveau
3. **Navigation**: Permet de naviguer facilement dans la hiérarchie
4. **Conformité**: Respecte le référentiel comptable SFD UMOA
5. **Extensibilité**: Permet l'ajout de nouveaux comptes avec des suffixes

📝 Notes importantes
--------------------

- Les **sauts de plage** sont appliqués au niveau du plan de comptes pour permettre l'ajout ultérieur de comptes
- Les SFD peuvent créer en interne des comptes en ajoutant des **suffixes** aux numéros proposés
- Le premier chiffre du compte représente toujours le numéro de la classe
- Les autres chiffres décrivent de façon plus détaillée la nature des opérations

🔗 Références
-------------

- Référentiel Comptable Spécifique des Systèmes Financiers Décentralisés de l'UMOA
- Chapitre 1.4 - Plan comptable
