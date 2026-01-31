# 🌐 Nexus Virtualis

> **Opération Nexus Virtualis** - Projet de virtualisation imbriquée multi-hyperviseurs

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Documentation](https://img.shields.io/badge/docs-latest-blue.svg)](./docs/)
[![Status](https://img.shields.io/badge/status-active-success.svg)]()

## 📋 Vue d'ensemble

**Nexus Virtualis** est un projet pédagogique avancé de virtualisation imbriquée (nested virtualization) qui explore les capacités et l'interopérabilité de plusieurs hyperviseurs de type 1 et type 2. Ce projet démontre la mise en œuvre complète d'environnements de virtualisation complexes et le déploiement de machines virtuelles Linux dans des architectures multi-niveaux.

### 🎯 Objectifs du projet

- Comprendre les concepts de virtualisation imbriquée
- Maîtriser plusieurs technologies d'hyperviseurs
- Déployer et configurer des environnements virtualisés complexes
- Comparer les performances et caractéristiques des différentes solutions
- Mettre en œuvre des scénarios réels d'infrastructure virtualisée

## 🏗️ Technologies utilisées

### Hyperviseurs Type 1 (Bare-metal)
- **VMware ESXi** - Solution enterprise de virtualisation
- **Proxmox VE** - Plateforme open-source de virtualisation et conteneurisation
- **XCP-ng** - Solution open-source basée sur Xen

### Hyperviseurs Type 2 (Hosted)
- **VMware Workstation** - Solution de virtualisation desktop
- **Microsoft Hyper-V** - Hyperviseur intégré à Windows

### Systèmes d'exploitation
- **Debian GNU/Linux** - Distribution Linux stable pour les machines virtuelles invitées

## 📚 Documentation

La documentation complète du projet est organisée par tâches (JOBs) dans le dossier [`docs/`](./docs/) :

| Documentation | Description |
|---------------|-------------|
| [JOB 1](./docs/JOB1_introduction.md) | Introduction et contexte du projet |
| [JOB 2](./docs/JOB2_architecture.md) | Architecture et environnement technique |
| [JOB 3](./docs/JOB3_vmware_workstation.md) | Configuration VMware Workstation |
| [JOB 4](./docs/JOB4_hyperv.md) | Configuration Hyper-V |
| [JOB 5](./docs/JOB5_esxi.md) | Configuration VMware ESXi |
| [JOB 6](./docs/JOB6_proxmox.md) | Configuration Proxmox VE |
| [JOB 7](./docs/JOB7_xcpng.md) | Configuration XCP-ng |

## 🚀 Démarrage rapide

### Prérequis

- Processeur avec support de virtualisation (Intel VT-x ou AMD-V)
- Minimum 16 GB de RAM (32 GB recommandé)
- 200 GB d'espace disque disponible
- Support de la virtualisation imbriquée activé dans le BIOS/UEFI

### Installation

Consultez la documentation spécifique à chaque hyperviseur dans le dossier [`docs/`](./docs/) pour des instructions détaillées d'installation et de configuration.

## 📁 Structure du projet

```
Nexus_Virtualis/
├── README.md                          # Ce fichier
├── docs/                              # Documentation complète
│   ├── JOB1_introduction.md          # Introduction et contexte
│   ├── JOB2_architecture.md          # Architecture technique
│   ├── JOB3_vmware_workstation.md    # VMware Workstation
│   ├── JOB4_hyperv.md                # Microsoft Hyper-V
│   ├── JOB5_esxi.md                  # VMware ESXi
│   ├── JOB6_proxmox.md               # Proxmox VE
│   └── JOB7_xcpng.md                 # XCP-ng
└── images/                            # Captures d'écran et diagrammes
    ├── architecture/                  # Diagrammes d'architecture
    ├── vmware/                        # Screenshots VMware
    ├── hyperv/                        # Screenshots Hyper-V
    ├── esxi/                          # Screenshots ESXi
    ├── proxmox/                       # Screenshots Proxmox
    └── xcpng/                         # Screenshots XCP-ng
```

## 🔑 Concepts clés

### Virtualisation imbriquée

La virtualisation imbriquée permet d'exécuter un hyperviseur à l'intérieur d'une machine virtuelle. Cette technique est essentielle pour :

- Les environnements de test et développement
- Les formations et démonstrations
- Les laboratoires d'apprentissage
- Les scénarios de Cloud Computing

### Architecture multi-hyperviseurs

Ce projet explore différentes approches de virtualisation :

- **Type 1 (Bare-metal)** : Accès direct au matériel, performances optimales
- **Type 2 (Hosted)** : Installation sur un OS existant, flexibilité maximale

## 🤝 Contribution

Ce projet est à vocation pédagogique. Les suggestions et améliorations sont les bienvenues via les issues et pull requests.

## 📝 Licence

Ce projet est distribué sous licence MIT. Voir le fichier `LICENSE` pour plus de détails.

## 👨‍💻 Auteur

**Teva Clairefond**

## 🙏 Remerciements

- Communautés open-source des projets Proxmox VE et XCP-ng
- Documentation officielle VMware et Microsoft
- Contributeurs et testeurs du projet

---

⭐ Si ce projet vous est utile, n'hésitez pas à lui donner une étoile !
