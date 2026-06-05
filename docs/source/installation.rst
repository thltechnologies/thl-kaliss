.. _installation:

Procédure d'Installation
========================

Cette page détaille la procédure pour installer et exécuter **THL Kaliss** (et son lanceur de bureau **THL-KALISS**) sur les différents systèmes d'exploitation (Windows, macOS et Linux).

.. contents:: Table des matières
   :local:
   :depth: 2

Environnement et Prérequis
--------------------------

Par défaut, l'application est conçue pour être autonome. Dans son profil d'évaluation/test (activé par défaut) :
* **Base de données** : Utilise une base H2 locale sous forme de fichier. Aucun serveur de base de données externe n'est requis.
* **Java** : Le lanceur d'application intègre ou recherche automatiquement un environnement d'exécution Java (JDK 17/21).

---

Installation sur Windows
------------------------

Le déploiement sur Windows s'effectue via un installateur NSIS exécutable :

1. Téléchargez la dernière version de l'installateur : `THL-KALISS Setup 1.0.0.exe`.
2. Double-cliquez sur l'exécutable pour lancer l'assistant d'installation.
3. Suivez les étapes de l'assistant pour finaliser l'installation.
4. Lancez l'application depuis le raccourci créé sur votre bureau ou le menu Démarrer.

---

Installation sur macOS
----------------------

Sur macOS, deux méthodes d'installation sont disponibles.

Méthode 1 : Via Homebrew Cask (Recommandé)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

C'est la méthode la plus rapide et propre. Ouvrez votre **Terminal** et exécutez les commandes suivantes :

.. code-block:: bash

   # 1. Ajouter le dépôt de formules THL
   $ brew tap thltechnologies/thl-kaliss

   # 2. Installer le Cask applicatif
   $ brew install --cask thl-kaliss

Méthode 2 : Installation Manuelle (.dmg)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Téléchargez le fichier image disque : `THL-KALISS-1.0.0.dmg`.
2. Double-cliquez sur le fichier `.dmg` pour l'ouvrir.
3. Glissez-déposez l'icône **THL-KALISS.app** dans votre dossier **Applications**.
4. **Important (Contournement de la quarantaine macOS / Gatekeeper)** :
   Comme l'application n'est pas signée numériquement auprès d'Apple, macOS affichera un message indiquant que le fichier est "endommagé". Pour résoudre cela, ouvrez votre **Terminal** et exécutez la commande suivante :

   .. code-block:: bash

      $ xattr -cr /Applications/THL-KALISS.app

5. Vous pouvez désormais ouvrir l'application normalement depuis votre Launchpad ou votre dossier Applications.

---

Installation sur Linux
----------------------

L'application est distribuée sous forme de package portable **AppImage** :

1. Téléchargez le fichier `THL-KALISS-1.0.0.AppImage`.
2. Ouvrez un terminal dans le dossier contenant le fichier téléchargé.
3. Rendez le fichier exécutable avec la commande suivante :

   .. code-block:: bash

      $ chmod +x THL-KALISS-1.0.0.AppImage

4. Lancez l'application en double-cliquant sur le fichier ou via le terminal :

   .. code-block:: bash

      $ ./THL-KALISS-1.0.0.AppImage

---

Configuration du Profil d'Exécution
-----------------------------------

L'application démarre par défaut en mode autonome. Si vous souhaitez modifier le profil ou vous connecter à une base de données PostgreSQL centralisée en production :

Lancement manuel en mode Test (H2)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Si vous exécutez le fichier JAR brut manuellement :

.. code-block:: bash

   $ java -Dspring.profiles.active=test -jar corebanking-testing.jar

Lancement manuel en mode Production (PostgreSQL)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Pour connecter l'application à un serveur PostgreSQL de production :

.. code-block:: bash

   $ java -Dspring.profiles.active=prod -jar corebanking-testing.jar
