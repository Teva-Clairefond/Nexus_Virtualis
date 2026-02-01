# JOB 07 – Installation de XCP-ng en machine virtuelle

## Objectif

L’objectif de ce job est d’installer **XCP-ng** en tant qu’**hyperviseur de type 1 exécuté en environnement imbriqué** dans VMware Workstation Pro, puis d’y déployer une machine virtuelle **Debian** à l’aide d’un dépôt ISO configuré manuellement.

---

## Contexte

XCP-ng est un hyperviseur open source basé sur Xen, utilisé dans des infrastructures professionnelles.
Dans ce projet, XCP-ng est installé dans une machine virtuelle afin d’étudier son comportement et sa configuration en virtualisation imbriquée, notamment sans interface graphique complète (XO Lite).

---

## Architecture / Principe

* Machine physique hôte
* Hyperviseur de type 2 : VMware Workstation Pro
* Machine virtuelle : XCP-ng
* Hyperviseur imbriqué : XCP-ng
* Machine virtuelle invitée : Debian GNU/Linux

---

## Prérequis

* VMware Workstation Pro installé
* Image ISO de XCP-ng
* Image ISO Debian GNU/Linux
* Virtualisation matérielle activée sur la machine hôte
* Accès SSH à la machine XCP-ng

---

## Étapes de réalisation

### 1. Création de la machine virtuelle XCP-ng

1. Ouvrir **VMware Workstation Pro**.
2. Cliquer sur **Créer une nouvelle machine virtuelle**.
3. Sélectionner **Personnalisée (avancée)**, puis cliquer sur **Suivant**.
4. À l’étape **Compatibilité matérielle**, choisir **Workstation 25H2**, puis **Suivant**.
5. À l’étape **Installation du système d’exploitation invité** :

   * Sélectionner **Fichier image du disque d’installation (ISO)**
   * Choisir l’ISO de **XCP-ng**
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

1. Clic droit sur la machine virtuelle XCP-ng.
2. Sélectionner **Paramètres**.
3. Activer l’option :

   * **Virtualiser Intel VT-x/EPT ou AMD-V/RVI**
4. Valider les paramètres.

---

### 3. Installation de XCP-ng

1. Démarrer la machine virtuelle XCP-ng.
2. Suivre l’assistant d’installation :

   * **Select keymap** → AZERTY → OK
   * **Welcome to XCP-ng Setup** → OK
   * **End User Agreement** → Accept EULA
   * **System Hardware** → OK
   * **System Firmware** → OK
   * **Select Primary Disk** → OK
   * **Virtual Machine Storage** → OK
   * **Virtual Machine Storage Type** → EXT → OK
   * **Select Installation Source** → HTTP or FTP → OK
   * **Networking (IPv4)** → Automatic Configuration (DHCP) → OK
   * **Specify Repository** → OK
   * **Verify Installation Source** → Skip verification → OK
   * **Set password** → définir le mot de passe root
   * **Networking (IPv4)** → Automatic Configuration (DHCP) → OK
   * **Hostname and DNS Configuration** → OK
   * **Select Time Zone** → Europe → Paris → OK
   * **System Time** → Use default NTP servers → OK
   * **Confirm Installation** → Install XCP-ng
3. Attendre la fin de l’installation.
4. À l’écran **Installation Complete**, cliquer sur **OK**.

---

### 4. Ajout de l’ISO Debian via SSH (XO Lite)

XCP-ng étant utilisé avec XO Lite, l’ajout de l’ISO Debian se fait en ligne de commande via SSH.

1. Depuis la machine hôte, ouvrir un terminal.
2. Se connecter en SSH à la VM XCP-ng :

   ```bash
   ssh root@<IP_XCPNG>
   ```
3. Créer le répertoire destiné aux ISO :

   ```bash
   mkdir -p /var/opt/xen/iso
   ```
4. Créer un dépôt ISO :

   ```bash
   xe sr-create \
     name-label="ISO Linux" \
     type=iso \
     content-type=iso \
     device-config:location=/var/opt/xen/iso \
     device-config:legacy_mode=true
   ```
5. Quitter la session SSH :

   ```bash
   exit
   ```
6. Copier l’ISO Debian vers la VM XCP-ng :

   ```bash
   scp debian-12.x.x-amd64-netinst.iso root@<IP_XCPNG>:/var/opt/xen/iso/
   ```
7. Redémarrer la machine virtuelle XCP-ng dans VMware Workstation Pro.


![XCP-ng Installation](../../images/jobs/job07/JOB7.1-XCPng%20installation%20terminée.png)


---

### 5. Accès à l’interface web XCP-ng

1. Depuis la machine hôte, ouvrir un navigateur web.
2. Accéder à l’interface XCP-ng via l’adresse IP de la VM.
3. Se connecter avec l’identifiant **root** et le mot de passe défini précédemment.


![XCP-ng Web Interface](../../images/jobs/job07/JOB7.2-XCPng%20connection%20navigateur.png)


---

### 6. Création d’une machine virtuelle Debian

1. Dans l’interface XCP-ng, cliquer sur **Nouvelle VM**.
2. Choisir un modèle :

   * **Debian Bookworm 12**
3. Paramètres d’installation :

   * Source : **ISO/DVD**
   * Sélectionner l’ISO Debian ajouté précédemment
4. Firmware de démarrage :

   * **BIOS**
5. Configuration :

   * vCPUs : **2**
6. Cliquer sur **Créer**.

La machine virtuelle démarre et l’installation de Debian commence.


![XCP-ng Debian Installation](../../images/jobs/job07/JOB7.3-XCPng%20fonctionnel.png)