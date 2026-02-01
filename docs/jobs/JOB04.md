# JOB 04 – Virtualisation imbriquée avec VMware Workstation Pro et Hyper-V

## Objectif

L’objectif de ce job est de mettre en place une **architecture de virtualisation imbriquée**, dans laquelle **VMware Workstation Pro (hyperviseur de type 2)** héberge une machine virtuelle **Windows Server 2022**, elle-même utilisée pour installer et exécuter **Hyper-V (hyperviseur de type 1)**.

---

## Contexte

Dans ce projet, VMware Workstation Pro permet de virtualiser une machine physique afin d’y installer un hyperviseur de type 1 en environnement imbriqué.
Cette configuration permet ensuite de créer et gérer des machines virtuelles directement depuis Hyper-V.

---

## Architecture / Principe

* Machine physique hôte
* Hyperviseur de type 2 : VMware Workstation Pro
* Machine virtuelle : Windows Server 2022
* Hyperviseur imbriqué : Hyper-V
* Machine virtuelle invitée : Debian GNU/Linux

---

## Prérequis

* VMware Workstation Pro installé
* Image ISO de Windows Server 2022
* Image ISO Debian
* Virtualisation matérielle activée sur la machine hôte

---

## Étapes de réalisation

### 1. Création de la machine virtuelle Windows Server 2022

1. Ouvrir **VMware Workstation Pro**.
2. Cliquer sur **Créer une nouvelle machine virtuelle**.
3. Sélectionner **Personnalisé**, puis cliquer sur **Suivant**.
4. À l’étape **Compatibilité**, choisir **Workstation 25H2**, puis **Suivant**.
5. Sélectionner **Installer le système d’exploitation ultérieurement**, puis **Suivant**.
6. Choisir :

   * Système d’exploitation invité : **Microsoft Windows**
   * Version : **Windows Server 2022**
7. Sélectionner l’emplacement de stockage de la machine virtuelle.
8. Type de microprogramme : **UEFI**.
9. Configuration matérielle :

   * Processeurs : **4**
   * Mémoire : **6000 Mo**
10. Réseau : **Utiliser la traduction d’adresses réseau (NAT)**.
11. Contrôleur : **LSI Logic SAS**.
12. Type de disque : **NVMe**.
13. Créer un nouveau disque virtuel.
14. Choisir **Stocker la machine virtuelle en tant que fichier unique (meilleures performances)**.
15. Finaliser la création de la machine virtuelle.

---

### 2. Configuration avancée de la machine virtuelle

1. Éteindre la machine virtuelle nouvellement créée.
2. Clic droit sur la VM → **Paramètres**.
3. Dans **CD/DVD (SATA)** :

   * Sélectionner **Utiliser un fichier image ISO**
   * Choisir l’ISO de **Windows Server 2022**
   * Cocher **Se connecter au démarrage**
4. Dans **Processeur** :

   * Activer **Virtualiser Intel VT-x/EPT ou AMD-V/RVI**
5. Valider les paramètres.

---

### 3. Installation de Windows Server 2022

1. Démarrer la machine virtuelle.
2. Attendre l’apparition du menu UEFI.
3. Sélectionner **Boot from CD**.
4. Lorsque le message *“Press any key…”* apparaît, appuyer sur une touche.
5. L’installateur de Windows se lance.
6. Choisir :

   * **Windows Server 2022 Standard Evaluation (GUI)**
7. Type d’installation :

   * **Installation personnalisée : installer uniquement le système d’exploitation**
8. Sélectionner le disque de destination, puis cliquer sur **Suivant**.
9. Attendre la fin de l’installation du système.

---

### 4. Installation du rôle Hyper-V

1. Une fois Windows Server installé, ouvrir le **Gestionnaire de serveur**.
2. Cliquer sur **Ajouter des rôles et des fonctionnalités**.
3. Choisir **Installation basée sur un rôle ou une fonctionnalité**.
4. Sélectionner le serveur dans le pool de serveurs.
5. Cocher le rôle **Hyper-V**.
6. Ajouter les fonctionnalités demandées.
7. Sélectionner la carte réseau à utiliser.
8. Valider les étapes et lancer l’installation.
9. Redémarrer le serveur une fois l’installation terminée.

---

### 5. Création d’une machine virtuelle Debian dans Hyper-V

1. Dans le **Gestionnaire Hyper-V**, cliquer sur **Nouveau → Ordinateur virtuel**.
2. Choisir un nom pour la VM.
3. Sélectionner **Génération 1**.
4. Mémoire de démarrage : **1500 Mo**.
5. Options d’installation :

   * **Installer un système d’exploitation à partir d’un CD/DVD-ROM de démarrage**
   * Sélectionner l’ISO Debian téléchargée
6. Finaliser la création de la machine virtuelle.
7. Sélectionner la VM Debian et cliquer sur **Démarrer**.


![Hyper-V Debian VM](C:\Users\P\Documents\lptf\Projets\20 - Opération Nexus Virtualis\Documentation_github\Nexus_Virtualis\images\jobs\job04\job4.1_hyper-V.png)

*Interface web de VMware ESXi après installation.*

---

### 6. Exportation de la machine virtuelle

1. Dans le **Gestionnaire Hyper-V**, sélectionner la machine virtuelle Debian.
2. Cliquer sur **Exporter**.
3. Choisir l’emplacement de destination.
4. Lancer l’exportation de la machine virtuelle.