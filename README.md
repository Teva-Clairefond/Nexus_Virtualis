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
├── LICENSE
├── .gitignore
├── docs/
│   ├── Documentation.md
│   ├── jobs/
│   │   ├── JOB01.md
│   │   ├── JOB02.md
│   │   ├── JOB03.md
│   │   ├── JOB04.md
│   │   ├── JOB05.md
│   │   ├── JOB06.md
│   │   ├── JOB07.md
│   │   ├── JOB08.md
│   │   ├── JOB09.md
│   │   └── JOB10.md
│   └── annexes/
│       ├── glossaire.md
│       └── troubleshooting.md
├── images/
│   ├── jobs/
│   └── schemas/
└── assets/
    └── exports/
```

---

## Documentation

La documentation complète du projet est disponible dans le dossier `docs/`.
Elle est organisée par **jobs** (JOB01 à JOB10), chacun correspondant à une étape précise du projet.
Les annexes dans `docs/annexes/` regroupent le glossaire et les informations de dépannage.

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
