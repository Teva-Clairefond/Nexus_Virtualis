# JOB 01 – Introduction à la virtualisation

## Objectif

L’objectif de ce job est de comprendre les **principes fondamentaux de la virtualisation**.
Il permet d’identifier le rôle des machines virtuelles, des hyperviseurs et des ressources virtualisées, afin de poser les bases nécessaires à la mise en œuvre de la virtualisation imbriquée dans les jobs suivants.

---

## Contexte

La virtualisation est une technologie clé des infrastructures informatiques modernes.
Elle est utilisée dans les centres de données, le cloud computing, les environnements de test et de développement, ainsi que pour l’optimisation des ressources matérielles.

Avant de mettre en place des hyperviseurs et des machines virtuelles, il est indispensable de comprendre les concepts théoriques qui sous-tendent leur fonctionnement.

---

## Architecture / Principe

La virtualisation repose sur la création de **machines virtuelles (VM)** à partir d’un matériel physique unique.

Une machine physique peut héberger :

* un **hyperviseur**,
* plusieurs **machines virtuelles**,
* chacune disposant de son propre système d’exploitation et de ressources allouées.

### Principe général

* Le matériel physique fournit les ressources (CPU, mémoire, stockage, réseau).
* L’hyperviseur abstrait ces ressources.
* Les machines virtuelles utilisent ces ressources de manière isolée et contrôlée.

---

## Prérequis

Aucun prérequis technique n’est nécessaire pour ce job.
Il s’agit d’un job **théorique**, servant de base aux manipulations pratiques à venir.

---

## Étapes de réalisation

### 1. Comprendre la notion de machine virtuelle

Une machine virtuelle est un environnement logiciel qui simule un ordinateur complet.
Elle dispose :

* d’un processeur virtuel,
* de mémoire vive virtuelle,
* d’un disque virtuel,
* d’une ou plusieurs interfaces réseau virtuelles.

Chaque VM fonctionne de manière indépendante, comme si elle était installée sur un matériel dédié.

---

### 2. Comprendre le rôle de l’hyperviseur

L’hyperviseur est le composant central de la virtualisation.
Il permet :

* de créer et gérer des machines virtuelles,
* de répartir les ressources matérielles,
* d’assurer l’isolation entre les VM.

On distingue deux types d’hyperviseurs :

#### Hyperviseur de type 1 (bare-metal)

* S’exécute directement sur le matériel
* Offre de meilleures performances
* Principalement utilisé en production

Exemples : VMware ESXi, Hyper-V, Proxmox VE, XCP-ng

#### Hyperviseur de type 2 (hébergé)

* S’exécute au-dessus d’un système d’exploitation hôte
* Plus simple à installer
* Principalement utilisé pour les tests et l’apprentissage

Exemples : VMware Workstation, VirtualBox

---

### 3. Comprendre l’intérêt de la virtualisation

La virtualisation permet :

* une meilleure utilisation des ressources matérielles,
* une réduction des coûts matériels,
* une isolation des environnements,
* une facilité de déploiement et de sauvegarde,
* une grande flexibilité pour les tests et la formation.


