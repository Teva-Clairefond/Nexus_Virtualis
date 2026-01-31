# Opération Nexus Virtualis

## Présentation

**Opération Nexus Virtualis** est un projet pédagogique consacré à la **virtualisation imbriquée (nested virtualization)**.
Il vise à concevoir, déployer et documenter des architectures de virtualisation complexes en utilisant plusieurs hyperviseurs, eux-mêmes exécutés dans des environnements virtualisés.

Le projet met l’accent sur la compréhension des mécanismes internes de la virtualisation, la configuration réseau, la gestion des ressources et le déploiement de machines Linux.

---

## Objectifs pédagogiques

* Comprendre les principes fondamentaux de la virtualisation
* Différencier les hyperviseurs de type 1 et de type 2
* Mettre en œuvre la virtualisation imbriquée
* Installer et configurer plusieurs hyperviseurs
* Déployer et administrer des machines virtuelles Linux
* Documenter une infrastructure technique de manière claire et exploitable

---

## Technologies utilisées

### Hyperviseurs

* VMware Workstation Pro
* Hyper-V (Windows Server 2022)
* VMware ESXi
* Proxmox VE
* XCP-ng

### Systèmes d’exploitation

* Windows Server 2022
* Debian GNU/Linux

---

## Architecture générale

Le projet repose sur une machine hôte exécutant un hyperviseur de type 2, au sein duquel sont déployés plusieurs hyperviseurs de type 1.
Chaque hyperviseur héberge ensuite des machines virtuelles Linux destinées aux tests et à la validation du fonctionnement de la virtualisation imbriquée.

---

## Structure du dépôt

```text
.
├── README.md
├── docs/
│   ├── Documentation.md
│   ├── JOB1.md
│   ├── JOB2.md
│   ├── JOB3.md
│   ├── JOB4.md
│   ├── JOB5.md
│   ├── JOB6.md
│   ├── JOB7.md
│   └── JOB8.md
├── images/
│   └── schémas et captures d’écran
```

---

## Documentation

La documentation complète du projet est disponible dans le dossier `docs/`.
Elle est organisée par **jobs**, chacun correspondant à une étape précise du projet :

* JOB 1 : Introduction à la virtualisation
* JOB 2 : Présentation des hyperviseurs et des architectures
* JOB 3 : Préparation de l’environnement hôte
* JOB 4 : Virtualisation imbriquée avec VMware Workstation et Hyper-V
* JOB 5 : Déploiement de VMware ESXi en environnement virtualisé
* JOB 6 : Installation et utilisation de Proxmox VE
* JOB 7 : Mise en place de XCP-ng
* JOB 8 : Tests, validation et analyse des environnements virtualisés

Chaque job décrit les objectifs, la configuration mise en place et les résultats obtenus.

---

## Compétences mises en œuvre

* Virtualisation et virtualisation imbriquée
* Administration systèmes
* Déploiement et configuration d’hyperviseurs
* Gestion des réseaux et du stockage virtualisés
* Installation et administration de systèmes Linux
* Rédaction de documentation technique

---

## Contexte

Ce projet a été réalisé dans un cadre pédagogique et a vocation à servir de **vitrine technique** démontrant des compétences en virtualisation et en administration de systèmes.
