# ` 🎟️ `︲Gestion Libre de Parc Informatique

<p align="center">
<img src="https://github.com/user-attachments/assets/4743b7b4-290f-4be7-a732-1439109cafff" width="100"/>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/GLPI-11.0.x-007ACC?style=for-the-badge">
  <img src="https://img.shields.io/badge/Type-Helpdesk%20%2F%20ITSM-28A745?style=for-the-badge">
  <img src="https://img.shields.io/badge/PHP-8.4-777BB4?logo=php&logoColor=white&style=for-the-badge">
  <img src="https://img.shields.io/badge/Database-MariaDB-003545?logo=mariadb&style=for-the-badge">
  <img src="https://img.shields.io/badge/OS-Debian%2013-A81D33?logo=debian&logoColor=white&style=for-the-badge">
  <img src="https://img.shields.io/badge/Guide-Installation%20%2B%20Configuration-007ACC?style=for-the-badge">
</p>

---

Ce dépôt présente un guide complet pour la mise en place et l'exploitation d'un service de gestion de parc informatique avec GLPI sous Debian 13.
Il couvre l'installation, la configuration et l'utilisation du serveur GLPI ainsi que de son agent de collecte, dans un contexte de laboratoire réseau.

Tu y apprendras à installer le serveur GLPI, à déployer l'agent sur les postes clients, à collecter et analyser l'inventaire matériel et logiciel, ainsi qu'à gérer les incidents utilisateurs via le Helpdesk, conformément aux bonnes pratiques ITIL.

---

## `📑`︲Sommaire (cliquez pour accéder directement à la section souhaitée)

1. [`📘`︲Introduction.](#introduction)

   * [`❔`︲Contexte et objectifs du TP.](#contexte-et-objectifs)
   * [`🧠`︲Notions de gestion de parc informatique.](#notions-gestion-parc)
   * [`📚`︲Référentiels et démarches qualité (ITIL, COBIT, ISO, CMMI).](#referentiels-qualite)

   ---

2. [`🧪`︲Présentation de l'environnement de travail.](#environnement-travail)

   * [`🖥️`︲Architecture du laboratoire.](#architecture-laboratoire)
   * [`📦`︲Machines utilisées et rôles.](#machines-roles)

   ---

3. [`🛠️`︲Installation du serveur GLPI.](#installation-glpi)

   * [`⚙️`︲Préparation du système Debian 13.](#preparation-systeme)
   * [`🗄️`︲Installation des services Apache, MariaDB et PHP.](#installation-services)
   * [`🔐`︲Sécurisation de MariaDB et création de la base de données.](#configuration-bdd)
   * [`⬇️`︲Téléchargement et installation du serveur GLPI.](#installation-serveur-glpi)
   * [`📁`︲Organisation des répertoires et permissions.](#organisation-repertoires)
   * [`🌐`︲Configuration du VirtualHost Apache.](#virtualhost-apache)
   * [`🖱️`︲Accès à l'interface web et configuration initiale.](#configuration-initiale-glpi)

   ---

4. [`🔑`︲Prise en main et gestion des comptes GLPI.](#gestion-comptes)

   * [`👑`︲Connexion au compte administrateur.](#compte-administrateur)
   * [`🧑‍💻`︲Présentation des profils utilisateurs.](#profils-utilisateurs)

   ---

5. [`🛰️`︲Installation et configuration de l'agent GLPI.](#installation-agent-glpi)

   * [`🐧`︲Installation de l'agent sur Debian.](#agent-debian)
   * [`🪟`︲Installation de l'agent sur Windows.](#agent-windows)

   ---

6. [`📦`︲Gestion de l'inventaire du parc.](#gestion-inventaire)

   * [`🔍`︲Visualisation des postes inventoriés.](#visualisation-postes)
   * [`📊`︲Analyse des informations matérielles et logicielles.](#analyse-inventaire)

   ---

7. [`🧑‍💼`︲Assistance aux utilisateurs (Helpdesk).](#assistance-utilisateurs)

   * [`👤`︲Création d'un utilisateur de type normal.](#creation-utilisateur-normal)
   * [`🧾`︲Création d'un ticket d'incident.](#creation-ticket)
   * [`🛠️`︲Attribution du ticket à un technicien.](#attribution-ticket)
   * [`📈`︲Suivi et résolution de l'incident.](#suivi-incident)
   * [`✅`︲Clôture du ticket.](#cloture-ticket)

   ---

8. [`✅`︲Validation du TP et vérifications finales.](#validation-tp)

   * [`🔎`︲Contrôles fonctionnels.](#controles-fonctionnels)
   * [`📝`︲Checklist de sécurité post-installation.](#checklist-securite)

   ---

9. [`📚`︲Outils et ressources utilisées.](#outils-ressources)
10. [`📌`︲Conclusion et perspectives.](#conclusion)

---

> [!TIP]
> **Pour naviguer rapidement dans le dépôt, vous pouvez également utiliser le sommaire généré automatiquement par GitHub, situé en haut à droite de ce cadre !**

---

<a id="introduction"></a>
# `📘`︲Introduction.

---

<a id="contexte-et-objectifs"></a>
### `❔`︲Contexte et objectifs du TP.

> [!NOTE]
> Tu vas apprendre à installer et configurer un serveur **GLPI 11** sur **Debian 13**, à déployer l'agent sur des postes clients Linux et Windows, à collecter l'inventaire matériel et logiciel du parc, et à gérer les incidents utilisateurs via le module Helpdesk.
> L'objectif est de te permettre de maîtriser les bases de la gestion de parc informatique et du service desk, conformément aux bonnes pratiques **ITIL**, dans un environnement de type **BTS SIO SISR**.

---

<a id="notions-gestion-parc"></a>
### `🧠`︲Notions de gestion de parc informatique.

> [!IMPORTANT]
> **Composition d'un parc informatique :**
> Le parc informatique d'une organisation est un assemblage, parfois hétéroclite, de matériels et de logiciels accumulés tout au long des années.

| Catégorie | Exemples |
|-----------|----------|
| **Matériel (hardware)** | Ordinateurs fixes/portables, serveurs, imprimantes, scanners, smartphones, switchs, routeurs, bornes Wi-Fi |
| **Logiciels (software)** | Systèmes d'exploitation, suites bureautiques, logiciels métiers, antivirus, outils de supervision, licences |
| **Réseau & télécoms** | Câblage, commutateurs, routeurs, DSLAM, systèmes téléphoniques VoIP |
| **Infrastructure physique** | Baies serveurs, onduleurs, climatisation salle serveurs, systèmes de sauvegarde |
| **Ressources documentaires** | Contrats de maintenance, garanties, documentations techniques, certificats de licences |

---

` 🔄 `︲**Périmètre de la gestion de parc :**

* ` 📦 ` ︲Inventaire complet du matériel et des logiciels installés sur chaque poste.
* ` 📄 ` ︲Suivi des licences logicielles et vérification de leur conformité.
* ` 🛡️ ` ︲Gestion des garanties, contrats de maintenance et dates d'expiration.
* ` 📍 ` ︲Suivi de la localisation physique de chaque équipement.
* ` 🔄 ` ︲Gestion du cycle de vie du matériel (achat → utilisation → renouvellement → rebut).
* ` 🔧 ` ︲Suivi des mises à jour système et des correctifs de sécurité (patching).
* ` 🔐 ` ︲Gestion des droits d'accès, des comptes utilisateurs et des habilitations.
* ` 📅 ` ︲Planification des renouvellements matériels à court et moyen terme.
* ` 💶 ` ︲Suivi financier et comptable (valeur d'achat, amortissement).
* ` 🎫 ` ︲Gestion des incidents, tickets d'assistance et demandes utilisateurs (helpdesk).
* ` 📈 ` ︲Analyse des besoins et planification des évolutions du système d'information.

> [!TIP]
> **Exemples de questions auxquelles répond la gestion de parc :**
> Quelles versions de Windows sont installées et sur quels postes ? Y a-t-il des disques proches de la saturation ? Quels postes sont encore sous garantie ? Combien de machines à renouveler dans 2 ans ?

---

<a id="referentiels-qualite"></a>
### `📚`︲Référentiels et démarches qualité (ITIL, COBIT, ISO, CMMI).

> [!NOTE]
> Une **démarche qualité** est un ensemble structuré de méthodes, processus et outils mis en place par une organisation pour garantir que ses produits, services et processus répondent à des exigences définies et s'améliorent continuellement.
> Elle repose sur le **cycle PDCA de Deming** : `Plan → Do → Check → Act`.

---

` 📘 `︲**ITIL — Information Technology Infrastructure Library.**

**ITIL** est le référentiel de bonnes pratiques IT le plus utilisé au monde. Il structure la gestion des services informatiques en processus clairs. GLPI implémente nativement les recommandations ITIL.

```
Détection → Enregistrement → Classification → Priorisation
    → Diagnostic → Résolution → Clôture
```

| ✅ Forces ITIL | 🔗 Lien avec GLPI |
|----------------|------------------|
| Très complet et reconnu mondialement | GLPI implémente les recommandations ITIL |
| Aligné sur les besoins métier | Tickets, SLA, CMDB intégrée |
| Processus structurés et mesurables | Helpdesk conforme aux bonnes pratiques |

---

` 📗 `︲**COBIT — Control Objectives for IT.**

**COBIT** est un cadre de gouvernance IT développé par l'ISACA. Il aligne la DSI sur les objectifs stratégiques de l'entreprise et fournit des métriques pour mesurer la performance des processus IT : traçabilité, conformité, gestion des risques.

---

` 📙 `︲**CMMI — Capability Maturity Model Integration.**

**CMMI** évalue et améliore la maturité des processus sur une échelle de **1 à 5** :

| Niveau | Nom | Description |
|--------|-----|-------------|
| 1 | Initial | Processus chaotiques, non définis |
| 2 | Géré | Processus planifiés et suivis |
| 3 | Défini | Processus standardisés à l'organisation |
| 4 | Géré quantitativement | Processus mesurés et contrôlés |
| 5 | En optimisation | Amélioration continue et innovation |

---

` 📕 `︲**ISO 27001 — Sécurité de l'information.**

La norme **ISO 27001** définit les exigences pour un Système de Management de la Sécurité de l'Information (SMSI). Elle impose de gérer les incidents de sécurité de façon structurée :

```
Identification → Classification → Réponse → Documentation → Retour d'expérience
```

> [!TIP]
> ISO 27001 est complémentaire à ITIL pour les incidents liés à la cybersécurité.

---

<a id="environnement-travail"></a>
# `🧪`︲Présentation de l'environnement de travail.

---

<a id="architecture-laboratoire"></a>
### `🖥️`︲Architecture du laboratoire.

> [!NOTE]
> Le laboratoire est entièrement virtualisé. Le serveur GLPI et les postes clients coexistent sur le même hyperviseur (VMware) avec une communication réseau en mode **Host-only** ou **LAN Segment**.

---

<a id="machines-roles"></a>
### `📦`︲Machines utilisées et rôles.

> [!IMPORTANT]
> **Prérequis système et outils :**
>
> * ` 🐧 `︲**Serveur GLPI :** *Debian 13 Trixie* **sans interface graphique** ︲[`🌐`](https://www.debian.org/)
>
> * ` 🐧 `︲**Client Debian :** *Debian 13 Trixie* (poste client avec agent GLPI) ︲[`🌐`](https://www.debian.org/)
>
> * ` 🟦 `︲**Client Windows :** *Windows 11* (poste client avec agent GLPI) ︲[`🌐`](https://www.microsoft.com/fr-fr/software-download/windows11)
>
> * ` 🎟️ `︲**Outil :** *GLPI (Gestion Libre de Parc Informatique)* `V.11.0.x` ︲[`🌐`](https://glpi-project.org/)
>
> * ` 📦 `︲**VMware :** ︲[`🌐`](https://www.vmware.com/)
>
> * ` ⚡ `︲**PuTTY :** ︲[`🌐`](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
>
> * ` 👤 `︲**Interface Chaise-Clavier fonctionnelle.** 🫵
>
> * ` ☕ `︲**Un peu de patience !**

---

` ⚙️ `︲**Configuration du serveur GLPI.**

* ` ❓ ` ︲**Hostname :** `srv-glpi`.
* ` 📡 ` ︲**Adressage IP :** statique recommandé sur le réseau de lab.
* ` 🖼️ ` ︲**Interface graphique :** **aucune** (installation en mode serveur / ligne de commande).
* ` 🧩 ` ︲**Services :** `apache2`, `mariadb`, `php8.4`, `ssh`.
* ` 📏 ` ︲**Mémoire :** `2048 Mo` minimum.
* ` 💾 ` ︲**Disque :** `20 Go` minimum *(allocation dynamique)*.

---

` 🚧 `︲**Couples d'identifiants (__NON NÉGOCIABLE__).**

```
ID : root    | MDP : btssio
ID : btssio  | MDP : btssio
```

* Créer les deux comptes avec ces mots de passe lors de l'installation / configuration.
* Vérifier les droits `sudo` si nécessaire pour l'utilisateur `btssio`.

---

<a id="installation-glpi"></a>
# ` 🛠️ `︲Installation du serveur GLPI.

---

> [!NOTE]
> Cette partie détaille l'installation complète du **serveur GLPI 11** sur **Debian 13 Trixie**.
> Le flux d'installation suit la séquence suivante :

```
[1] Mise à jour  →  [2] Socle LAMP  →  [3] Extensions PHP
     →  [4] Sécurisation BDD  →  [5] Téléchargement GLPI
          →  [6] Permissions & Répertoires  →  [7] VirtualHost Apache  →  [8] Config navigateur
```

---

<a id="preparation-systeme"></a>
## ` ⚙️ `︲Préparation du système Debian 13.

---

> [!NOTE]
> Toutes les commandes ci-dessous sont à exécuter en tant que **root** sur le serveur.

---

1️⃣︲**Mise à jour complète du système.**

```bash
apt update && apt upgrade -y
```

---

<a id="installation-services"></a>
## ` 🗄️ `︲Installation des services Apache, MariaDB et PHP.

---

1️⃣︲**Installation du socle LAMP.**

```bash
apt install apache2 mariadb-server php php-fpm -y
systemctl enable apache2 mariadb && systemctl start apache2 mariadb
```

---

2️⃣︲**Installation des extensions PHP 8.4 requises par GLPI.**

```bash
apt install php8.4-mysqli php8.4-mbstring php8.4-curl php8.4-gd \
        php8.4-simplexml php8.4-xml php8.4-intl php8.4-zip \
        php8.4-bz2 php8.4-ldap php8.4-apcu php8.4-xmlrpc -y
```

> [!TIP]
> **PHP 8.2 est le minimum requis, PHP 8.4 est recommandé pour GLPI 11.** Ces extensions couvrent la gestion BDD, l'internationalisation, la compression, l'annuaire LDAP et le cache.

---

3️⃣︲**Vérification des services.**

```bash
systemctl status apache2
systemctl status mariadb
```

> [!TIP]
> Les deux services doivent afficher `active (running)` avant de continuer.

---

<a id="configuration-bdd"></a>
## ` 🔐 `︲Sécurisation de MariaDB et création de la base de données.

---

1️⃣︲**Sécurisation de l'instance MariaDB.**

```bash
mariadb-secure-installation
```

> [!IMPORTANT]
> Répondre **Oui** à toutes les questions :
> - Définir un mot de passe root fort.
> - Supprimer les utilisateurs anonymes.
> - Désactiver l'accès root distant.
> - Supprimer la base de test.

---

2️⃣︲**Création de la base de données et de l'utilisateur GLPI.**

```sql
mariadb -u root -p

CREATE DATABASE glpi CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'glpiuser'@'localhost' IDENTIFIED BY 'mdpglpi';
GRANT ALL PRIVILEGES ON glpi.* TO 'glpiuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

> [!WARNING]
> **Remplace `mdpglpi` par un mot de passe robuste en production.** Ces identifiants seront saisis lors de la configuration web de GLPI.

---

<a id="installation-serveur-glpi"></a>
## ` ⬇️ `︲Téléchargement et installation du serveur GLPI.

---

1️⃣︲**Téléchargement de l'archive GLPI 11.**

```bash
cd /tmp
wget https://github.com/glpi-project/glpi/releases/download/11.0.4/glpi-11.0.4.tgz
tar xzf glpi-11.0.4.tgz -C /var/www/
```

> [!TIP]
> Vérifier la dernière version disponible sur : [`🌐`](https://github.com/glpi-project/glpi/releases)
> Remplacer `11.0.4` par la version la plus récente si nécessaire.

---

<a id="organisation-repertoires"></a>
## ` 📁 `︲Organisation des répertoires et permissions.

---

> [!NOTE]
> GLPI 11 impose de **déplacer les répertoires sensibles** (`config`, `files`, `logs`) hors de la racine web pour des raisons de sécurité. Seul le dossier `public/` doit être accessible depuis le navigateur.

---

1️⃣︲**Application des permissions de base.**

```bash
chown -R www-data:www-data /var/www/glpi
chmod -R 775 /var/www/glpi
```

---

2️⃣︲**Déplacement des répertoires sensibles hors du DocumentRoot.**

```bash
# Configuration
mkdir /etc/glpi && chown www-data /etc/glpi
mv /var/www/glpi/config /etc/glpi

# Fichiers (uploads, cache)
mkdir /var/lib/glpi && chown www-data /var/lib/glpi
mv /var/www/glpi/files /var/lib/glpi

# Logs
mkdir /var/log/glpi && chown www-data /var/log/glpi
```

> [!WARNING]
> **Ces déplacements sont obligatoires.** Sans eux, les répertoires de configuration et de données seraient accessibles directement depuis un navigateur, exposant des informations critiques.

---

<a id="virtualhost-apache"></a>
## ` 🌐 `︲Configuration du VirtualHost Apache.

---

1️⃣︲**Création du fichier de configuration Apache.**

```bash
nano /etc/apache2/sites-available/glpi.conf
```

Contenu du fichier :

```apache
<VirtualHost *:80>
    ServerName glpi.local
    DocumentRoot /var/www/glpi/public

    <Directory /var/www/glpi/public>
        Require all granted
        AllowOverride All
        RewriteEngine On
        RewriteCond %{REQUEST_FILENAME} !-f
        RewriteRule ^(.*)$ index.php [QSA,L]
    </Directory>
</VirtualHost>
```

---

2️⃣︲**Activation du site et du module rewrite.**

```bash
a2enmod rewrite
a2ensite glpi.conf
a2dissite 000-default.conf
systemctl restart apache2
```

> [!TIP]
> Le module `rewrite` est indispensable au routage interne de GLPI. Sans lui, la navigation dans l'interface renvoie des erreurs 404.

---

<a id="configuration-initiale-glpi"></a>
## ` 🖱️ `︲Accès à l'interface web et configuration initiale.

---

1️⃣︲**Ouverture de l'assistant d'installation dans le navigateur.**

```
http://@IpServeurGLPI
```

---

2️⃣︲**Déroulement de l'assistant d'installation.**

* Choisir la langue : **Français**
* Accepter la licence et cliquer sur **Installer**
* Renseigner les paramètres de base de données :

| Champ | Valeur |
|-------|--------|
| **Serveur SQL** | `localhost` |
| **Utilisateur SQL** | `glpiuser` |
| **Mot de passe SQL** | `mdpglpi` |
| **Base de données** | `glpi` |

* Finaliser et cliquer sur **Utiliser GLPI**

<details>
  <summary>📸︲Page de connexion GLPI.</summary>

  ![Page de connexion GLPI](screenshot_login.png)

  *Page de connexion GLPI 11 — saisir l'identifiant et le mot de passe pour accéder à l'interface.*

</details>

---

3️⃣︲**Action de sécurité immédiate post-installation.**

```bash
rm -rf /var/www/glpi/install
```

> [!WARNING]
> **Cette commande est OBLIGATOIRE immédiatement après la fin de l'assistant.** Le dossier `install/` laissé accessible permet à n'importe qui de réinstaller GLPI et d'écraser toute la configuration.

---

<a id="gestion-comptes"></a>
# ` 🔑 `︲Prise en main et gestion des comptes GLPI.

---

<a id="compte-administrateur"></a>
### ` 👑 `︲Connexion au compte administrateur.

---

> [!NOTE]
> GLPI crée automatiquement **4 comptes par défaut** lors de l'installation. Ces comptes doivent être sécurisés immédiatement.

---

<a id="profils-utilisateurs"></a>
### ` 🧑‍💻 `︲Présentation des profils utilisateurs.

---

| Identifiant | Mot de passe | Rôle |
|-------------|-------------|------|
| `glpi` | `glpi` | Administrateur — accès total |
| `tech` | `tech` | Technicien — gestion des tickets et du parc |
| `normal` | `normal` | Utilisateur normal — création de tickets uniquement |
| `post-only` | `postonly` | Post-only — suivi de tickets sans création |

> [!WARNING]
> **Changer immédiatement tous ces mots de passe par défaut après la première connexion.**
> Aller dans : **Administration > Utilisateurs** → sélectionner chaque compte → modifier le mot de passe.

---

<a id="installation-agent-glpi"></a>
# ` 🛰️ `︲Installation et configuration de l'agent GLPI.

---

> [!NOTE]
> L'agent GLPI est un outil de collecte installé sur chaque **poste client**. Il remonte automatiquement l'inventaire vers le serveur (CPU, RAM, disques, OS, logiciels, adresses MAC/IP).
> **Ces commandes sont à exécuter sur les postes CLIENTS, pas sur le serveur GLPI.**

---

<a id="agent-debian"></a>
## ` 🐧 `︲Installation de l'agent sur Debian.

---

1️⃣︲**Téléchargement de l'installeur.**

```bash
cd /tmp
wget https://github.com/glpi-project/glpi-agent/releases/latest/download/glpi-agent-linux-installer.pl
```

> [!TIP]
> Vérifier la dernière version disponible sur : [`🌐`](https://github.com/glpi-project/glpi-agent/releases)

---

2️⃣︲**Exécution de l'installeur.**

```bash
perl glpi-agent-linux-installer.pl \
  --type=typical \
  --server=http://@IpServeurGLPI \
  --no-ssl-check
```

> [!TIP]
> Remplacer `@IpServeurGLPI` par l'adresse IP réelle du serveur.
> Ajouter `--tag=NomPoste` pour identifier le poste dans l'inventaire GLPI.

---

3️⃣︲**Activation et démarrage du service.**

```bash
systemctl enable glpi-agent
systemctl start glpi-agent
systemctl status glpi-agent
```

---

4️⃣︲**Vérification de la configuration.**

```bash
cat /etc/glpi-agent/agent.cfg
```

> Vérifier que la ligne `server =` pointe bien vers l'adresse IP du serveur GLPI.

---

<a id="agent-windows"></a>
## ` 🪟 `︲Installation de l'agent sur Windows.

---

> [!IMPORTANT]
> L'installation requiert des **droits administrateur**. L'agent est disponible en 64 bits depuis la version 1.8.

---

1️⃣︲**Téléchargement.**

Se rendre sur [`🌐`](https://github.com/glpi-project/glpi-agent/releases) et télécharger le fichier `.msi` 64 bits.

Exemple : `GLPI-Agent-1.16-x64.msi`

---

2️⃣︲**Installation graphique (méthode simple).**

* Double-cliquer sur le fichier `.msi` et accepter l'élévation UAC.
* Choisir le type d'installation : **Typical**.
* Dans **Remote Targets (GLPI Server URL)**, saisir : `http://@IpServeurGLPI`
* Cliquer sur **Install** puis **Finish**.

---

3️⃣︲**Installation silencieuse (ligne de commande / GPO).**

```cmd
msiexec /i GLPI-Agent-1.16-x64.msi /quiet ^
  SERVER=http://@IpServeurGLPI ^
  RUNNOW=1
```

> [!TIP]
> `RUNNOW=1` force un inventaire immédiat après l'installation. Idéal pour un déploiement **GPO** sur un domaine Active Directory.

---

4️⃣︲**Installation via Winget.**

```powershell
winget install glpi-project.glpi-agent
```

---

5️⃣︲**Vérification du service Windows.**

Dans `services.msc`, vérifier que le service **GLPI Agent** est :

* ` ✅ ` ︲État : `Démarré`
* ` ✅ ` ︲Type de démarrage : `Automatique`

---

<a id="gestion-inventaire"></a>
# ` 📦 `︲Gestion de l'inventaire du parc.

---

<a id="visualisation-postes"></a>
### ` 🔍 `︲Visualisation des postes inventoriés.

---

1️⃣︲**Accès à la liste des ordinateurs.**

* Se connecter avec `glpi` / `glpi`.
* Aller dans **Parc > Ordinateurs**.
* Vérifier que les postes clients apparaissent *(attendre 1-2 min après un inventaire forcé)*.

<details>
  <summary>📸︲Tableau de bord GLPI.</summary>

  ![Tableau de bord GLPI](screenshot_dashboard.png)

  *Vue du tableau de bord administrateur : compteurs du parc (logiciels, ordinateurs, matériels réseau), statut des tickets et graphiques mensuels.*

</details>

<details>
  <summary>📸︲Liste des ordinateurs (Parc > Ordinateurs).</summary>

  ![Liste des ordinateurs dans GLPI](screenshot_computers.png)

  *Inventaire des postes remontés par les agents : nom, fabricant, numéro de série, type, système d'exploitation et date de dernière modification.*

</details>

---

<a id="analyse-inventaire"></a>
### ` 📊 `︲Analyse des informations matérielles et logicielles.

---

1️⃣︲**Forcer un inventaire immédiat depuis un poste Debian.**

```bash
# Remontée forcée vers le serveur
glpi-agent --force

# Consultation des logs
journalctl -u glpi-agent -n 50 --no-pager
```

> Chercher la ligne : `Inventory sent successfully` ou `New Inventory from [NomDuPoste]`.

---

2️⃣︲**Forcer un inventaire immédiat depuis un poste Windows.**

**Méthode 1 — Via le navigateur :**

```
http://localhost:62354/now
```

> La page affiche `OK` si l'inventaire a bien été envoyé.

**Méthode 2 — Via l'invite de commandes (administrateur) :**

```cmd
cd "C:\Program Files\GLPI-Agent"
glpi-agent.bat --force
```

**Méthode 3 — Statut de l'agent :**

```
http://localhost:62354
```

> Affiche le statut, la date du dernier inventaire et le serveur cible.

---

3️⃣︲**Activation de l'inventaire natif GLPI.**

* Aller dans **Administration > Inventaire**.
* Cocher **Activer l'inventaire** et sauvegarder.
* *(Optionnel)* Installer le plugin GLPI Inventory via **Configuration > Plugins**.

---

<a id="assistance-utilisateurs"></a>
# ` 🧑‍💼 `︲Assistance aux utilisateurs (Helpdesk).

---

> [!NOTE]
> Le module **Helpdesk** de GLPI permet de gérer les demandes d'assistance et les incidents selon les bonnes pratiques **ITIL**.
> Le cycle de traitement : `Détection → Enregistrement → Classification → Priorisation → Diagnostic → Résolution → Clôture`.

---

<a id="creation-utilisateur-normal"></a>
### ` 👤 `︲Création d'un utilisateur de type normal.

---

1️⃣︲**Création du compte utilisateur.**

* Aller dans **Administration > Utilisateurs > Ajouter un utilisateur**.
* Renseigner : Identifiant = `votre nom`, Profil = **Self-Service (normal)**.
* Affecter à cet utilisateur un élément matériel inventorié (un PC du parc).

---

<a id="creation-ticket"></a>
### ` 🧾 `︲Création d'un ticket d'incident.

---

1️⃣︲**Création du ticket depuis le compte utilisateur normal.**

Se connecter avec le compte `normal` et créer un ticket :

| Champ | Valeur exemple |
|-------|----------------|
| **Titre** | Poste de travail impossible à démarrer |
| **Description** | Le PC `DESKTOP-XXX` ne démarre plus depuis ce matin. Dernier usage hier soir. Écran noir au démarrage. |
| **Catégorie** | Matériel > Ordinateur |
| **Priorité** | Haute |
| **Élément associé** | Sélectionner le PC du parc affecté à cet utilisateur |

---

<a id="attribution-ticket"></a>
### ` 🛠️ `︲Attribution du ticket à un technicien.

---

1️⃣︲**Création d'un compte technicien.**

* Aller dans **Administration > Utilisateurs > Ajouter un utilisateur**.
* Renseigner : Identifiant = `technicien01`, Profil = **Technicien (admin)**.

---

2️⃣︲**Attribution du ticket.**

* Se connecter avec le compte administrateur (`glpi` / `glpi`).
* Aller dans **Assistance > Tickets**.
* Ouvrir le ticket créé et l'assigner au technicien `technicien01`.

---

<a id="suivi-incident"></a>
### ` 📈 `︲Suivi et résolution de l'incident.

---

1️⃣︲**Traitement du ticket par le technicien.**

Se connecter avec le compte `technicien01` et consulter le ticket assigné.

2️⃣︲**Ajout d'un suivi (diagnostic).**

> *« Diagnostic en cours — vérification de l'alimentation et de la RAM »*

3️⃣︲**Ajout d'une action (résolution).**

> *« Remplacement du module RAM défectueux — poste redémarré avec succès »*

4️⃣︲**Changement de statut.**

`En cours` → `Résolu`

<details>
  <summary>📸︲Détail d'un ticket Helpdesk.</summary>

  ![Détail d'un ticket GLPI](screenshot_ticket_detail.png)

  *Vue détaillée d'un ticket : fil de conversation entre l'utilisateur et le technicien, informations de résolution (date d'ouverture, date de résolution, statut, urgence).*

</details>

---

<a id="cloture-ticket"></a>
### ` ✅ `︲Clôture du ticket.

---

1️⃣︲**Fermeture du ticket.**

* Renseigner la solution dans le champ **Solution**.
* Changer le statut en **Fermé** et valider.

<details>
  <summary>📸︲Liste des tickets en cours.</summary>

  ![Liste des tickets GLPI](screenshot_tickets_list.png)

  *Vue de la file de tickets : filtrage par statut, tri par date de modification, priorité colorée, attribution au technicien responsable.*

</details>

> [!TIP]
> Le ticket clôturé est conservé dans l'historique GLPI avec l'intégralité du fil de traitement. Ces données alimentent les statistiques et le retour d'expérience ITIL.

---

<a id="validation-tp"></a>
# ` ✅ `︲Validation du TP et vérifications finales.

---

> [!NOTE]
> **Objectif :** valider que chaque composant fonctionne correctement avant de considérer le TP terminé.

```
[Test 1] Serveur web  →  Apache + MariaDB actifs
    ↓
[Test 2] Agent Debian  →  inventaire forcé + log OK
    ↓
[Test 3] Agent Windows  →  inventaire forcé + page 62354 OK
    ↓
[Test 4] Vérification GLPI  →  Parc > Ordinateurs peuplé
    ↓
[Test 5] Helpdesk  →  ticket créé, suivi, clôturé
    ↓
✅ Installation validée
```

---

<a id="controles-fonctionnels"></a>
### ` 🔎 `︲Contrôles fonctionnels.

---

1️⃣︲**Accessibilité du serveur GLPI.**

```bash
# Sur le serveur
systemctl status apache2
systemctl status mariadb

# Dans un navigateur
http://@IpServeurGLPI
```

> ✅ Les deux services doivent être en état `active (running)`.

---

2️⃣︲**Remontée d'inventaire Debian.**

```bash
glpi-agent --force
journalctl -u glpi-agent -n 50 --no-pager
```

> ✅ Chercher : `Inventory sent successfully`.

---

3️⃣︲**Remontée d'inventaire Windows.**

```
http://localhost:62354/now
```

> ✅ La page doit afficher `OK`.

---

4️⃣︲**Présence des postes dans GLPI.**

* **Parc > Ordinateurs** — les postes clients doivent apparaître avec leurs données matérielles et logicielles.

---

5️⃣︲**Cycle Helpdesk complet.**

* ` 🧾 ` ︲Ticket créé par l'utilisateur normal.
* ` 🛠️ ` ︲Ticket assigné et traité par le technicien.
* ` ✅ ` ︲Ticket clôturé avec solution renseignée.

---

<a id="checklist-securite"></a>
### ` 📝 `︲Checklist de sécurité post-installation.

---

> [!WARNING]
> **Ces actions sont obligatoires avant toute mise en production ou présentation du TP.**

---

- [ ] Supprimer le dossier d'installation : `rm -rf /var/www/glpi/install`
- [ ] Changer les mots de passe des 4 comptes par défaut (`glpi`, `tech`, `normal`, `post-only`)
- [ ] Vérifier que `/etc/glpi` et `/var/lib/glpi` ne sont pas accessibles depuis un navigateur
- [ ] Configurer un certificat HTTPS *(Let's Encrypt recommandé pour la production)*
- [ ] Vérifier que `server =` dans `/etc/glpi-agent/agent.cfg` pointe sur le bon serveur
- [ ] Vérifier le service GLPI Agent dans `services.msc` (Windows) — état `Démarré`, démarrage `Automatique`

> [!TIP]
> Si tous les tests passent : l'installation est opérationnelle. Les postes remontent leur inventaire automatiquement toutes les **24h**.

---

<a id="outils-ressources"></a>
# ` 🧰 `︲Outils et ressources utilisées.

---

> [!TIP]
> Vous trouverez ici la liste de tous les outils, ressources et services utilisés pour la création de cette documentation.
> Les liens correspondants sont accessibles en cliquant sur l'emoji : `  🌐  ` .

---

* `🗃️ ︲ 🌐` **︲Documents/Liens d'annexes fournis dans le TP :**
  * ` 📂 ` ︲`Gestion Libre de Parc Informatique — TP KATA`.`pdf`

  * ` 🌐 ` ︲`GLPI Project — Site officiel`︲[`🌐`](https://glpi-project.org/)

  * ` 🌐 ` ︲`GLPI — GitHub Releases`︲[`🌐`](https://github.com/glpi-project/glpi/releases)

  * ` 🌐 ` ︲`GLPI Agent — GitHub Releases`︲[`🌐`](https://github.com/glpi-project/glpi-agent/releases)

  * ` 🌐 ` ︲`Documentation GLPI officielle`︲[`🌐`](https://glpi-user-documentation.readthedocs.io/)

---

* ` 🤖 ` **︲`Claude`** ︲[`🌐`](https://claude.ai/)

* ` ❓ ` **︲`Markdownguide.org`** ︲[`🌐`](https://www.markdownguide.org/)

* ` 🤖 ` **︲`NoteBookLM`** ︲[`🌐`](https://notebooklm.google.com/)

* ` ✂️ ` **︲`Screenpresso`** ︲[`🌐`](https://www.screenpresso.com/fr/)

* ` 😀 ` **︲`Smiley.cool`** ︲[`🌐`](https://smiley.cool/emoji-list.php)

* ` ❓ ` **︲`Syntaxe de base pour l'écriture et la mise en forme`** ︲[`🌐`](https://docs.github.com/fr/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)

---

> * ` ☕ `︲De la patience.

---

<a id="conclusion"></a>
# ` 📌 `︲Conclusion et perspectives.

---

> [!NOTE]
> À l'issue de ce TP, tu maîtrises le cycle complet d'un service de gestion de parc informatique :
>
> * ` ✅ ` ︲Installation et configuration d'un serveur **GLPI 11** sur **Debian 13**.
> * ` ✅ ` ︲Déploiement de l'agent sur des postes **Linux** et **Windows**.
> * ` ✅ ` ︲Collecte et analyse de l'inventaire matériel et logiciel du parc.
> * ` ✅ ` ︲Gestion d'un cycle d'incident complet via le **Helpdesk ITIL**.
> * ` ✅ ` ︲Sécurisation post-installation conforme aux bonnes pratiques.

---
