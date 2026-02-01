# JOB 10 – Étude des solutions de sauvegarde de machines virtuelles

## Objectif

L’objectif de ce job est de **se renseigner sur les principales solutions de sauvegarde de machines virtuelles**, d’en comprendre le fonctionnement et d’identifier leurs avantages, leurs limites et leurs cas d’usage.

Ce travail permet de replacer la solution mise en œuvre au **JOB 09** dans un contexte plus global et professionnel.

---

## Contexte

La sauvegarde des machines virtuelles est un élément fondamental de la continuité de service dans une infrastructure informatique.
En cas de panne matérielle, d’erreur humaine, d’attaque ou de corruption des données, la sauvegarde permet de **restaurer rapidement les systèmes**.

Il existe plusieurs approches et solutions pour sauvegarder des machines virtuelles, selon :

* l’hyperviseur utilisé,
* l’environnement (test, production, cloud),
* les contraintes de stockage et de fréquence,
* les exigences de restauration.

---

## Principes généraux de sauvegarde des machines virtuelles

### Sauvegarde au niveau de l’hyperviseur

Cette méthode consiste à sauvegarder la machine virtuelle **sans dépendre du système d’exploitation invité**.

Caractéristiques :

* sauvegarde du disque virtuel (VMDK, QCOW2, etc.),
* possibilité de sauvegarde à chaud (VM allumée),
* restauration rapide de la VM complète.

C’est la méthode la plus utilisée en environnement professionnel.

---

### Sauvegarde au niveau du système invité

La sauvegarde est réalisée **depuis l’intérieur de la machine virtuelle**, à l’aide d’outils classiques (rsync, tar, logiciels de sauvegarde).

Caractéristiques :

* sauvegarde des fichiers uniquement,
* restauration partielle possible,
* dépendance au système d’exploitation invité.

Cette méthode est souvent utilisée en complément.

---

## Présentation des principales solutions de sauvegarde

---

### Proxmox Backup Server

**Proxmox Backup Server (PBS)** est une solution dédiée à la sauvegarde des machines virtuelles et des conteneurs Proxmox VE.

Fonctionnalités :

* sauvegardes incrémentales
* déduplication et compression
* planification avancée
* politiques de rétention
* intégration native avec Proxmox VE

Avantages :

* solution open source
* excellente intégration
* performances élevées

Limites :

* principalement adaptée à Proxmox VE

---

### Veeam Backup & Replication

Veeam est une solution très répandue dans les environnements professionnels.

Fonctionnalités :

* sauvegarde des VM VMware et Hyper-V
* restauration granulaire
* réplication de machines virtuelles
* gestion centralisée

Avantages :

* solution robuste et mature
* très utilisée en entreprise

Limites :

* solution propriétaire
* coût de licence élevé

---

### VMware vSphere Data Protection / Solutions VMware

VMware propose ses propres solutions de sauvegarde intégrées ou compatibles.

Fonctionnalités :

* sauvegarde native des VM ESXi
* intégration avec l’écosystème VMware
* support des snapshots

Avantages :

* compatibilité native
* simplicité de mise en œuvre

Limites :

* fonctionnalités parfois limitées
* dépendance à l’écosystème VMware

---

### Sauvegardes basées sur les snapshots

Les snapshots permettent de figer l’état d’une machine virtuelle à un instant donné.

Avantages :

* très rapide
* utile avant une modification critique

Limites :

* ne constitue pas une véritable sauvegarde
* dégradation des performances si conservé trop longtemps
* dépendance au stockage principal

Les snapshots doivent être utilisés **uniquement comme solution temporaire**.

---

### Solutions open source complémentaires

Certaines solutions open source peuvent être utilisées :

* Bacula
* Bareos
* Restic
* Rsync

Ces outils permettent de sauvegarder les données depuis l’intérieur des VM ou via des scripts automatisés.

---

## Comparaison des solutions

| Solution              | Niveau      | Automatisation | Restauration rapide | Licence      |
| --------------------- | ----------- | -------------- | ------------------- | ------------ |
| Proxmox Backup Server | Hyperviseur | Oui            | Oui                 | Open source  |
| Veeam                 | Hyperviseur | Oui            | Oui                 | Propriétaire |
| Solutions VMware      | Hyperviseur | Oui            | Oui                 | Propriétaire |
| Snapshots             | Hyperviseur | Limité         | Oui                 | Inclus       |
| Sauvegarde OS invité  | VM          | Variable       | Partielle           | Variable     |

---

## Bonnes pratiques de sauvegarde

* Mettre en place une planification régulière
* Définir une politique de rétention claire
* Tester régulièrement les restaurations
* Stocker les sauvegardes sur un système distinct
* Ne pas confondre snapshot et sauvegarde

---

## Conclusion

La sauvegarde des machines virtuelles est un **élément critique de toute infrastructure virtualisée**.
Il existe de nombreuses solutions, chacune répondant à des besoins spécifiques.

Dans le cadre de ce projet :

* **Proxmox Backup Server** a été retenu pour sa simplicité et son intégration native,
