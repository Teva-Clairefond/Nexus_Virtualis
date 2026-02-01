# Documentation – Opération Nexus Virtualis

## Introduction

Cette documentation accompagne le projet **Opération Nexus Virtualis**, consacré à la **virtualisation imbriquée (nested virtualization)**.
Elle décrit de manière progressive et structurée la mise en place d’environnements virtualisés complexes, depuis les concepts fondamentaux jusqu’à l’analyse finale et au retour d’expérience.

Le projet est découpé en **jobs**, chacun correspondant à une étape précise, avec des objectifs clairs, une procédure détaillée et des critères de validation.

---

## Présentation générale du projet

L’objectif principal du projet est de comprendre, déployer et analyser des architectures de virtualisation dans lesquelles des **hyperviseurs sont eux-mêmes exécutés dans des machines virtuelles**.

Le projet s’appuie sur :

* un hyperviseur de type 2 exécuté sur la machine hôte,
* plusieurs hyperviseurs de type 1 déployés de manière imbriquée,
* des machines virtuelles Linux utilisées pour tester et valider les environnements.

Cette approche permet d’explorer les limites, les contraintes et les usages réels de la virtualisation imbriquée.

---

## Architecture globale

L’architecture générale repose sur le principe suivant :

1. Une machine hôte physique avec la virtualisation matérielle activée (VT-x / AMD-V).
2. Un hyperviseur de type 2 (VMware Workstation).
3. Plusieurs hyperviseurs de type 1 exécutés dans des machines virtuelles :

   * Hyper-V
   * VMware ESXi
   * Proxmox VE
   * XCP-ng
4. Des machines virtuelles Debian déployées sur chaque hyperviseur afin de valider leur fonctionnement.

Cette architecture permet de comparer les solutions, d’observer leur comportement en environnement imbriqué et de valider leur exploitation.

---

## Organisation de la documentation

La documentation est organisée dans le dossier `docs/` et structurée de la manière suivante :

* `Documentation.md` : vue d’ensemble du projet (ce fichier)
* `jobs/` : documentation détaillée, job par job
* `annexes/` : ressources complémentaires (glossaire, dépannage)

Chaque job dispose de son propre fichier Markdown afin de faciliter la lecture, la maintenance et la validation.

---

## Sommaire des jobs

* [JOB 01 – Introduction à la virtualisation](jobs/JOB01.md)
* [JOB 02 – Présentation des hyperviseurs et des architectures](jobs/JOB02.md)
* [JOB 03 – Préparation de l’environnement hôte](jobs/JOB03.md)
* [JOB 04 – Virtualisation imbriquée avec VMware Workstation et Hyper-V](jobs/JOB04.md)
* [JOB 05 – Déploiement de VMware ESXi en environnement virtualisé](jobs/JOB05.md)
* [JOB 06 – Installation et utilisation de Proxmox VE](jobs/JOB06.md)
* [JOB 07 – Mise en place de XCP-ng](jobs/JOB07.md)
* [JOB 08 – Tests et validation des environnements virtualisés](jobs/JOB08.md)
* [JOB 09 – Analyse et réponses aux questions](jobs/JOB09.md)
* [JOB 10 – Conclusion technique et retour d’expérience](jobs/JOB10.md)

---

## Conventions utilisées

### Structure des jobs

Chaque fichier `JOBxx.md` suit strictement la structure suivante :

* Objectif
* Contexte
* Architecture / Principe
* Prérequis
* Étapes de réalisation
* Résultat attendu
* Validation

Cette structure garantit une lecture homogène et facilite la compréhension du projet.

---

### Conventions d’écriture

* Langage clair, technique et pédagogique
* Ton professionnel
* Étapes numérotées lorsque nécessaire
* Commandes intégrées dans des blocs de code
* Titres hiérarchisés en Markdown

---

### Images et schémas

* Les images sont stockées dans le dossier `images/`
* Les captures liées à un job sont placées dans `images/jobs/`
* Les schémas d’architecture sont placés dans `images/schemas/`
* Les images sont référencées par des chemins relatifs dans les fichiers Markdown

---

## Annexes

Les annexes complètent la documentation principale :

* `annexes/glossaire.md` : définitions des termes techniques utilisés
* `annexes/troubleshooting.md` : problèmes courants rencontrés et solutions associées

---

## Finalité de la documentation

Cette documentation a pour but :

* de démontrer la maîtrise des concepts de virtualisation,
* de servir de support pédagogique,
* de constituer une **vitrine technique** exploitable dans un contexte académique ou professionnel.
