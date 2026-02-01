# JOB 02 – Téléchargement et installation de VMware Workstation Pro

## Objectif

L’objectif de ce job est de **télécharger et installer VMware Workstation Pro**, qui sera utilisé comme hyperviseur de type 2 pour héberger les environnements de virtualisation imbriquée du projet.

---

## Contexte

VMware Workstation Pro est un hyperviseur hébergé permettant de créer et gérer des machines virtuelles sur un système d’exploitation hôte.
Il constitue la base de l’architecture du projet, car il permet d’exécuter des hyperviseurs de type 1 à l’intérieur de machines virtuelles.

---

## Architecture / Principe

Dans le cadre du projet :

* VMware Workstation Pro est installé sur la machine hôte,
* il héberge les machines virtuelles servant à installer Hyper-V, ESXi, Proxmox VE et XCP-ng,
* il doit donc être installé et fonctionnel avant toute autre manipulation.

---

## Prérequis

* Accès à Internet
* Droits administrateur sur la machine hôte
* Compte Broadcom

---

## Étapes de réalisation

### 1. Création d’un compte Broadcom

1. Ouvrir un navigateur web.
2. Se rendre sur le site officiel Broadcom.
3. Créer un compte utilisateur ou se connecter à un compte existant.

Ce compte est nécessaire pour accéder aux téléchargements VMware.

---

### 2. Téléchargement de VMware Workstation Pro

1. Accéder à la page officielle des téléchargements Broadcom :
   [https://support.broadcom.com/group/ecx/free-downloads](https://support.broadcom.com/group/ecx/free-downloads)
2. Rechercher **VMware Workstation Pro**.
3. Sélectionner la version adaptée à votre système d’exploitation.
4. Télécharger le programme d’installation.

---

### 3. Installation de VMware Workstation Pro

1. Lancer le fichier d’installation téléchargé.
2. Suivre l’assistant d’installation avec les options par défaut.
3. Accepter les conditions de licence.
4. Finaliser l’installation.
5. Redémarrer la machine si nécessaire.

---

Une fois l’installation terminée, VMware Workstation Pro est prêt à être utilisé pour la création des machines virtuelles du projet.

