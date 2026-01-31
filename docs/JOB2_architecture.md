# JOB 2 : Architecture et environnement technique

## 🏗️ Vue d'ensemble de l'architecture

Ce document présente l'architecture globale du projet Nexus Virtualis et les spécifications techniques nécessaires pour sa mise en œuvre.

## 📐 Architecture générale

### Schéma d'architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Machine Physique (Hôte)                   │
│  • Processeur avec VT-x/AMD-V                                │
│  • 32 GB RAM recommandé                                      │
│  • 200+ GB stockage                                          │
│  • Virtualisation imbriquée activée                          │
└─────────────────────────────────────────────────────────────┘
                              │
        ┌─────────────────────┼─────────────────────┐
        │                     │                     │
        ▼                     ▼                     ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│  Type 2       │    │  Type 2       │    │  Type 1       │
│  VMware       │    │  Hyper-V      │    │  ESXi         │
│  Workstation  │    │               │    │  Proxmox      │
│               │    │               │    │  XCP-ng       │
└───────────────┘    └───────────────┘    └───────────────┘
        │                     │                     │
        ▼                     ▼                     ▼
┌───────────────┐    ┌───────────────┐    ┌───────────────┐
│   VM Debian   │    │   VM Debian   │    │   VM Debian   │
│   (Invitée)   │    │   (Invitée)   │    │   (Invitée)   │
└───────────────┘    └───────────────┘    └───────────────┘
```

### Niveaux de virtualisation

1. **Niveau 0 : Matériel physique**
   - Serveur ou station de travail physique
   - Processeur avec extensions de virtualisation
   - Ressources matérielles dédiées

2. **Niveau 1 : Hyperviseur hôte**
   - Hyperviseurs Type 1 ou Type 2
   - Gestion des ressources matérielles
   - Création de VMs de niveau 2

3. **Niveau 2 : Machines virtuelles invitées**
   - VMs Debian déployées
   - Peuvent héberger des hyperviseurs (nested)
   - Applications et services

## 💻 Spécifications matérielles

### Configuration minimale

| Composant | Spécification minimale |
|-----------|------------------------|
| **Processeur** | Intel VT-x ou AMD-V activé |
| **RAM** | 16 GB |
| **Stockage** | 100 GB disponible |
| **Réseau** | Connexion Ethernet 1 Gbps |

### Configuration recommandée

| Composant | Spécification recommandée |
|-----------|---------------------------|
| **Processeur** | Intel Core i7/i9 ou AMD Ryzen 7/9 (8+ cœurs) |
| **RAM** | 32 GB ou plus |
| **Stockage** | 200 GB+ SSD NVMe |
| **Réseau** | Connexion Ethernet 1 Gbps ou plus |

### Configuration optimale (Lab professionnel)

| Composant | Spécification optimale |
|-----------|------------------------|
| **Processeur** | Intel Xeon ou AMD EPYC (16+ cœurs) |
| **RAM** | 64 GB+ ECC |
| **Stockage** | 500 GB+ SSD NVMe en RAID |
| **Réseau** | Dual 10 Gbps |

## 🔧 Prérequis logiciels

### Système d'exploitation hôte

Pour les hyperviseurs Type 2 :
- **Windows 10/11 Pro ou Enterprise** (pour Hyper-V et VMware Workstation)
- **Windows Server 2019/2022** (pour Hyper-V en production)
- **Linux** (Ubuntu/Debian/CentOS) pour VMware Workstation sous Linux

Pour les hyperviseurs Type 1 :
- Démarrage direct sur le matériel (bare-metal)
- Pas de système d'exploitation hôte requis

### Activation de la virtualisation

#### Dans le BIOS/UEFI

1. **Intel VT-x** ou **AMD-V** : Activé
2. **Intel VT-d** ou **AMD-Vi** (IOMMU) : Activé (recommandé)
3. **Virtualisation imbriquée** : Activé si disponible

#### Dans Windows (pour Hyper-V)

```powershell
# Vérifier le support de virtualisation
systeminfo | findstr /C:"Hyper-V"

# Activer Hyper-V
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
```

## 🌐 Architecture réseau

### Topologie réseau

```
                    Internet
                        │
                   ┌────┴────┐
                   │ Router  │
                   └────┬────┘
                        │
            ┌───────────┴───────────┐
            │   Physical Switch     │
            └───────────┬───────────┘
                        │
        ┌───────────────┼───────────────┐
        │               │               │
┌───────▼──────┐ ┌──────▼──────┐ ┌─────▼──────┐
│ VMware       │ │ Hyper-V     │ │ ESXi/      │
│ vSwitch      │ │ vSwitch     │ │ Proxmox/   │
│              │ │             │ │ XCP-ng     │
└──────┬───────┘ └──────┬──────┘ └─────┬──────┘
       │                │               │
   VM Debian        VM Debian       VM Debian
```

### Types de réseaux virtuels

1. **NAT (Network Address Translation)**
   - Partage de la connexion Internet de l'hôte
   - VMs isolées du réseau physique
   - Idéal pour les tests

2. **Bridged (Pont)**
   - VM directement sur le réseau physique
   - Obtient une IP du réseau local
   - Communication avec autres machines physiques

3. **Host-only**
   - Réseau privé entre hôte et VMs
   - Pas d'accès Internet direct
   - Sécurisé pour les tests isolés

4. **Internal**
   - Communication entre VMs uniquement
   - Complètement isolé
   - Pour architectures complexes

### Plan d'adressage suggéré

| Réseau | Plage IP | Usage |
|--------|----------|-------|
| **Réseau physique** | 192.168.1.0/24 | LAN principal |
| **VMware NAT** | 192.168.100.0/24 | VMs VMware Workstation |
| **Hyper-V NAT** | 192.168.110.0/24 | VMs Hyper-V |
| **ESXi Management** | 192.168.120.0/24 | ESXi et VMs |
| **Proxmox Management** | 192.168.130.0/24 | Proxmox et VMs |
| **XCP-ng Management** | 192.168.140.0/24 | XCP-ng et VMs |

## 💾 Architecture de stockage

### Options de stockage

1. **Stockage local**
   - Disques SATA/SSD/NVMe
   - Performances élevées
   - Pas de redondance

2. **Stockage partagé (avancé)**
   - NFS
   - iSCSI
   - SMB/CIFS

### Organisation du stockage

```
/
├── Hyperviseurs/
│   ├── VMware_Workstation/
│   │   └── VMs/
│   ├── Hyper-V/
│   │   └── Virtual_Machines/
│   ├── ESXi/
│   │   └── Datastores/
│   ├── Proxmox/
│   │   └── images/
│   └── XCP-ng/
│       └── Storage_Repositories/
└── ISOs/
    └── debian-xx.x.x-amd64-netinst.iso
```

### Recommandations de stockage par VM

| Type de VM | Disque système | Swap/Page | Total |
|------------|----------------|-----------|-------|
| **Debian minimal** | 10 GB | 2 GB | 12 GB |
| **Debian standard** | 20 GB | 4 GB | 24 GB |
| **Hyperviseur imbriqué** | 50 GB | 8 GB | 58 GB |

## 🔒 Considérations de sécurité

### Isolation

- Chaque hyperviseur dans un environnement séparé
- VMs isolées par défaut
- Pare-feu configurés sur chaque niveau

### Accès et authentification

- Mots de passe forts pour tous les hyperviseurs
- Authentification SSH par clés (recommandé)
- Désactivation des comptes par défaut

### Mises à jour

- Maintenir les hyperviseurs à jour
- Patches de sécurité réguliers
- VMs invitées mises à jour

## 📊 Allocation des ressources

### Matrice de ressources suggérée

| Hyperviseur | vCPU | RAM | Stockage | VMs possibles |
|-------------|------|-----|----------|---------------|
| **VMware Workstation** | 2-4 | 4-8 GB | 50 GB | 1-2 Debian |
| **Hyper-V** | 2-4 | 4-8 GB | 50 GB | 1-2 Debian |
| **ESXi** | 4-8 | 8-16 GB | 100 GB | 2-4 Debian |
| **Proxmox VE** | 4-8 | 8-16 GB | 100 GB | 2-4 Debian |
| **XCP-ng** | 4-8 | 8-16 GB | 100 GB | 2-4 Debian |

### Formule de calcul

Pour un système avec 32 GB RAM et 8 cœurs :
- Réserver 25% pour l'hôte : 8 GB RAM, 2 cœurs
- Disponible pour VMs : 24 GB RAM, 6 cœurs
- Répartition entre hyperviseurs selon priorités

## 🛠️ Outils et utilitaires

### Outils de gestion

- **vCenter** (VMware)
- **Hyper-V Manager** (Microsoft)
- **Web UI** (ESXi, Proxmox, XCP-ng)
- **PowerShell/PowerCLI** (Scripting)

### Outils de monitoring

- **Top/htop** (Linux)
- **Performance Monitor** (Windows)
- **Prometheus + Grafana** (Avancé)

### Outils de backup

- **Veeam Backup**
- **Proxmox Backup Server**
- **XenServer Backup**

## 📋 Checklist de préparation

- [ ] Vérifier les spécifications matérielles
- [ ] Activer la virtualisation dans le BIOS
- [ ] Télécharger les ISOs des hyperviseurs
- [ ] Télécharger l'ISO Debian
- [ ] Planifier l'architecture réseau
- [ ] Préparer le stockage
- [ ] Documenter la configuration
- [ ] Créer des snapshots/backups de référence

## 🚀 Prochaines étapes

Maintenant que l'architecture est définie, vous pouvez procéder aux installations spécifiques :

- [JOB 3 : Configuration VMware Workstation](./JOB3_vmware_workstation.md)
- [JOB 4 : Configuration Hyper-V](./JOB4_hyperv.md)
- [JOB 5 : Configuration ESXi](./JOB5_esxi.md)
- [JOB 6 : Configuration Proxmox VE](./JOB6_proxmox.md)
- [JOB 7 : Configuration XCP-ng](./JOB7_xcpng.md)

---

[← JOB 1 : Introduction](./JOB1_introduction.md) | [Retour au README](../README.md) | [JOB 3 : VMware Workstation →](./JOB3_vmware_workstation.md)
