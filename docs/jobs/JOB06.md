# JOB 06 – Installation de Proxmox VE en machine virtuelle

## Objectif

L’objectif de ce job est d’installer **Proxmox Virtual Environment (PVE)** en tant qu’**hyperviseur de type 1 exécuté en environnement imbriqué** dans VMware Workstation Pro, puis d’y déployer une machine virtuelle Debian.

---

## Contexte

Proxmox VE est une solution de virtualisation open source basée sur Linux, largement utilisée dans les environnements professionnels et pédagogiques.
Dans ce projet, Proxmox VE est installé à l’intérieur d’une machine virtuelle afin d’étudier son fonctionnement en virtualisation imbriquée.

---

## Architecture / Principe

* Machine physique hôte
* Hyperviseur de type 2 : VMware Workstation Pro
* Machine virtuelle : Proxmox VE
* Hyperviseur imbriqué : Proxmox VE
* Machine virtuelle invitée : Debian GNU/Linux

---

## Prérequis

* VMware Workstation Pro installé
* Image ISO de Proxmox VE
* Image ISO Debian GNU/Linux
* Virtualisation matérielle activée sur la machine hôte

---

## Étapes de réalisation

### 1. Création de la machine virtuelle Proxmox VE

1. Ouvrir **VMware Workstation Pro**.
2. Cliquer sur **Créer une nouvelle machine virtuelle**.
3. Sélectionner **Personnalisée (avancée)**, puis cliquer sur **Suivant**.
4. À l’étape **Compatibilité matérielle**, choisir **Workstation 25H2**, puis **Suivant**.
5. À l’étape **Installation du système d’exploitation invité** :

   * Sélectionner **Fichier image du disque d’installation (ISO)**
   * Choisir l’image ISO de **Proxmox VE**
6. Sélectionner le système d’exploitation invité :

   * Système d’exploitation invité : **Autre**
   * Version : **Autre 64 bits**
7. Choisir l’emplacement de stockage de la machine virtuelle.
8. Configuration du processeur :

   * Nombre de processeurs : **4**
   * Nombre de cœurs par processeur : **2**
9. Mémoire :

   * **4096 Mo**
10. Type de réseau :

    * **Utiliser la traduction d’adresses réseau (NAT)**
11. Contrôleur SCSI :

    * **LSI Logic**
12. Type de disque virtuel :

    * **IDE**
13. Créer un disque virtuel.
14. Spécifier la capacité du disque :

    * Taille maximale : **30 Go**
    * **Stocker le disque virtuel en tant que fichier unique**
15. Spécifier l’emplacement du fichier disque.
16. Cliquer sur **Terminer** pour créer la machine virtuelle.

---

### 2. Activation de la virtualisation imbriquée

1. Clic droit sur la machine virtuelle Proxmox VE.
2. Sélectionner **Paramètres**.
3. Activer l’option :

   * **Virtualiser Intel VT-x/EPT ou AMD-V/RVI**
4. Valider les paramètres.

---

### 3. Installation de Proxmox VE

1. Démarrer la machine virtuelle Proxmox VE.
2. À l’écran de démarrage, sélectionner :

   * **Install Proxmox VE (Graphical)**
3. Suivre l’assistant d’installation :

   * **Proxmox Virtual Environment (PVE)** → **Next**
   * Sélectionner le pays → **Next**
   * Définir un mot de passe → **Next**
   * Configuration du réseau de gestion → **Next**
   * Récapitulatif → **Install**
4. Attendre la fin de l’installation.

La machine virtuelle redémarre avec Proxmox VE installé.


![Proxmox VE Installation](C:\Users\P\Documents\lptf\Projets\20 - Opération Nexus Virtualis\Documentation_github\Nexus_Virtualis\images\jobs\job06\JOB6.1_promox fonctionnel 1.png)


---

### 4. Accès à l’interface web Proxmox VE

1. Repérer l’adresse IP affichée sur la console Proxmox.
2. Depuis la machine hôte, ouvrir un navigateur web.
3. Accéder à l’interface web via l’adresse IP de la VM.
4. Se connecter avec les identifiants définis lors de l’installation.


![Proxmox VE Web Interface](C:\Users\P\Documents\lptf\Projets\20 - Opération Nexus Virtualis\Documentation_github\Nexus_Virtualis\images\jobs\job06\JOB6.2_Proxmox fonctionnel 2.png)


---

### 5. Ajout de l’ISO Debian dans Proxmox VE

1. Dans l’interface Proxmox VE, sélectionner **local (pve)**.
2. Ouvrir l’onglet **Images ISO**.
3. Cliquer sur **Téléverser**.
4. Sélectionner l’image ISO de **Debian** et attendre la fin du téléversement.

---

### 6. Création d’une machine virtuelle Debian

1. Cliquer sur **Créer une VM**.
2. Onglet **Général** :

   * Renseigner le nom de la machine virtuelle.
3. Onglet **Système d’exploitation** :

   * Sélectionner **Utiliser une image de média (ISO)**
   * Choisir l’ISO Debian téléversée précédemment.
4. Finaliser la création de la machine virtuelle.

---

### 7. Démarrage de la machine virtuelle Debian

1. Sélectionner la machine virtuelle Debian.
2. Cliquer sur **Démarrer**.
3. La machine virtuelle se lance et l’installation de Debian commence.


![Proxmox VE Debian Installation](C:\Users\P\Documents\lptf\Projets\20 - Opération Nexus Virtualis\Documentation_github\Nexus_Virtualis\images\jobs\job06\JOB6.3_promox fonctionnel 3.png)