#  ` 🎟️ `︲Gestion Libre de Parc Informatique

---

Ce dépôt présente un guide complet pour la mise en place et l’exploitation d’un service de gestion de parc informatique avec GLPI sous Debian 12.
Il couvre l’installation, la configuration et l’utilisation du serveur GLPI ainsi que de son agent de collecte, dans un contexte de laboratoire réseau.

Tu y apprendras à installer le serveur GLPI, à déployer l’agent sur les postes clients, à collecter et analyser l’inventaire matériel et logiciel, ainsi qu’à gérer les incidents utilisateurs via le Helpdesk, conformément aux bonnes pratiques ITIL.

---


## `📑`︲Sommaire (cliquez pour accéder directement à la section souhaitée)

1. [`📘`︲Introduction.](#introduction)

   * [`❔`︲Contexte et objectifs du TP.](#contexte-et-objectifs)
   * [`🧠`︲Notions de gestion de parc informatique.](#notions-gestion-parc)
   * [`📚`︲Référentiels et démarches qualité (ITIL, COBIT, ISO, CMMI).](#referentiels-qualite)

   ---

2. [`🧪`︲Présentation de l’environnement de travail.](#environnement-travail)

   * [`🖥️`︲Architecture du laboratoire.](#architecture-laboratoire)
   * [`📦`︲Machines utilisées et rôles.](#machines-roles)

   ---

3. [`🛠️`︲Installation du serveur GLPI.](#installation-glpi)

   * [`⚙️`︲Préparation du système Debian 12.](#preparation-systeme)
   * [`🗄️`︲Installation des services Apache, MariaDB et PHP.](#installation-services)
   * [`🔐`︲Création et configuration de la base de données GLPI.](#configuration-bdd)
   * [`⬇️`︲Téléchargement et installation du serveur GLPI.](#installation-serveur-glpi)
   * [`🌐`︲Accès à l’interface web et configuration initiale.](#configuration-initiale-glpi)

   ---

4. [`🔑`︲Prise en main et gestion des comptes GLPI.](#gestion-comptes)

   * [`👑`︲Connexion au compte administrateur.](#compte-administrateur)
   * [`🧑‍💻`︲Présentation des profils utilisateurs.](#profils-utilisateurs)

   ---

5. [`🛰️`︲Installation et configuration de l’agent GLPI.](#installation-agent-glpi)

   * [`⬇️`︲Téléchargement de l’agent GLPI.](#telechargement-agent)
   * [`⚙️`︲Installation sur les postes clients.](#installation-clients)
   * [`📡`︲Remontée des informations vers le serveur.](#remontee-informations)

   ---

6. [`📦`︲Gestion de l’inventaire du parc.](#gestion-inventaire)

   * [`🔍`︲Visualisation des postes inventoriés.](#visualisation-postes)
   * [`📊`︲Analyse des informations matérielles et logicielles.](#analyse-inventaire)

   ---

7. [`🧑‍💼`︲Assistance aux utilisateurs (Helpdesk).](#assistance-utilisateurs)

   * [`👤`︲Création d’un utilisateur de type normal.](#creation-utilisateur-normal)
   * [`🧾`︲Création d’un ticket d’incident.](#creation-ticket)
   * [`🛠️`︲Attribution du ticket à un technicien.](#attribution-ticket)
   * [`📈`︲Suivi et résolution de l’incident.](#suivi-incident)
   * [`✅`︲Clôture du ticket.](#cloture-ticket)

   ---

8. [`✅`︲Validation du TP et vérifications finales.](#validation-tp)

   * [`🔎`︲Contrôles fonctionnels.](#controles-fonctionnels)
   * [`📝`︲Synthèse des résultats obtenus.](#synthese-resultats)

   ---

9. [`📚`︲Outils et ressources utilisées.](#outils-ressources)
10. [`📌`︲Conclusion et perspectives.](#conclusion)

---
