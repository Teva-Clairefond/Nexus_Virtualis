# JOB 03 – Téléchargement des images ISO nécessaires au projet

## Objectif

L’objectif de ce job est de **récupérer l’ensemble des images ISO** nécessaires à la réalisation du projet de virtualisation imbriquée.
Ces images serviront à l’installation des systèmes d’exploitation et des hyperviseurs utilisés dans les jobs suivants.

---

## Contexte

Avant de pouvoir créer des machines virtuelles et installer des hyperviseurs, il est indispensable de disposer des **images ISO officielles** des systèmes concernés.
Ce job consiste donc à identifier les sources fiables et à télécharger les versions appropriées des ISO utilisées tout au long du projet.

---

## Architecture / Principe

Chaque hyperviseur ou système d’exploitation est installé à partir d’une **image ISO**, qui représente le support d’installation virtuel.

Les ISO téléchargées dans ce job seront utilisées pour :

* créer des machines virtuelles,
* installer des hyperviseurs de type 1,
* déployer des systèmes invités Linux ou Windows.

Il est recommandé de stocker toutes les ISO dans un dossier dédié afin de faciliter leur gestion.

---

## Prérequis

* Accès à Internet
* Navigateur web
* Espace disque suffisant pour stocker les images ISO
* Compte Broadcom (pour VMware ESXi)

---

## Étapes de réalisation

### 1. Téléchargement de l’ISO Windows Server 2022

1. Ouvrir un navigateur web.
2. Dans la barre de recherche, saisir :
   **Windows Server 2022 iso**
3. Accéder au site officiel de Microsoft via la page suivante :
   [https://www.microsoft.com/fr-fr/evalcenter/download-windows-server-2022](https://www.microsoft.com/fr-fr/evalcenter/download-windows-server-2022)
4. Sélectionner l’édition souhaitée de **Windows Server 2022**.
5. Télécharger l’image ISO correspondante.

Cette image sera utilisée pour l’installation de Windows Server et du rôle Hyper-V.

---

### 2. Téléchargement de l’ISO VMware ESXi

1. Créer ou utiliser un **compte Broadcom** (anciennement VMware).
2. Se connecter à la plateforme de téléchargement Broadcom.
3. Accéder à la page des téléchargements gratuits :
   [https://support.broadcom.com/group/ecx/free-downloads](https://support.broadcom.com/group/ecx/free-downloads)
4. Rechercher **VMware vSphere Hypervisor**.
5. Télécharger l’image ISO de VMware ESXi.

Cette image permettra l’installation de l’hyperviseur ESXi dans une machine virtuelle.

---

### 3. Téléchargement de l’ISO Proxmox VE

1. Ouvrir un navigateur web.
2. Rechercher :
   **Proxmox ISO**
3. Accéder au site officiel de Proxmox :
   [https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso/proxmox-ve-8-4-iso-installer](https://www.proxmox.com/en/downloads/proxmox-virtual-environment/iso/proxmox-ve-8-4-iso-installer)
4. Télécharger l’ISO de **Proxmox VE 8.4**.

Cette image sera utilisée pour installer l’hyperviseur Proxmox VE.

---

### 4. Téléchargement de l’ISO XCP-ng

1. Ouvrir un navigateur web.
2. Rechercher :
   **xcp-ng iso**
3. Accéder au site officiel des images XCP-ng.
4. Télécharger l’ISO via le miroir officiel :
   [https://mirrors.xcp-ng.org/isos/8.3/xcp-ng-8.3.0-20250606.iso?https=1](https://mirrors.xcp-ng.org/isos/8.3/xcp-ng-8.3.0-20250606.iso?https=1)

Cette image permettra l’installation de l’hyperviseur XCP-ng.

---

### 5. Organisation des images ISO

Il est recommandé de :

* créer un dossier dédié aux ISO (par exemple `ISO/`),
* renommer les fichiers de manière explicite,
* conserver toutes les images ISO au même emplacement.

Exemple d’organisation :

```text
ISO/
├── Windows_Server_2022.iso
├── VMware_ESXi.iso
├── Proxmox_VE_8.4.iso
└── XCP-ng_8.3.iso
```

Cette organisation facilitera l’utilisation des ISO dans les jobs suivants.
