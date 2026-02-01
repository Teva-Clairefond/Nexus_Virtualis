# Opération Nexus Virtualis

## Présentation

**Opération Nexus Virtualis** est un projet pédagogique consacré à la **virtualisation imbriquée (nested virtualization)**.
Il a pour objectif de concevoir, déployer et analyser des architectures de virtualisation complexes, en mettant en œuvre plusieurs hyperviseurs exécutés au sein d’environnements virtualisés.

Le projet couvre l’ensemble du cycle : compréhension théorique, préparation de l’environnement, déploiement des hyperviseurs, création de machines virtuelles, puis validation et analyse des résultats.

---

## Objectifs pédagogiques

* Comprendre les principes fondamentaux de la virtualisation
* Différencier les hyperviseurs de type 1 et de type 2
* Mettre en œuvre la virtualisation imbriquée
* Installer et configurer plusieurs hyperviseurs
* Déployer et administrer des machines virtuelles Linux
* Tester, valider et analyser une infrastructure virtualisée
* Produire une documentation technique claire et structurée

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

Le projet repose sur une machine hôte exécutant un hyperviseur de type 2.
À l’intérieur de cet environnement sont déployés plusieurs hyperviseurs de type 1, chacun hébergeant des machines virtuelles Linux destinées aux tests et à la validation de la virtualisation imbriquée.

---

## Structure du dépôt

```text
operation-nexus-virtualis/
├── README.md
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
Elle est organisée par **jobs**, chacun correspondant à une étape précise du projet :

* JOB 01 : Introduction à la virtualisation
* JOB 02 : Présentation des hyperviseurs et des architectures
* JOB 03 : Préparation de l’environnement hôte
* JOB 04 : Virtualisation imbriquée avec VMware Workstation et Hyper-V
* JOB 05 : Déploiement de VMware ESXi en environnement virtualisé
* JOB 06 : Installation et utilisation de Proxmox VE
* JOB 07 : Mise en place de XCP-ng
* JOB 08 : Tests et validation des environnements virtualisés
* JOB 09 : Analyse, réponses aux questions et approfondissement
* JOB 10 : Conclusion technique et retour d’expérience

Chaque job détaille les objectifs, la procédure, les résultats attendus et les méthodes de validation.

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
