# JOB 7 : Configuration XCP-ng

## 📖 Présentation

XCP-ng (Xen Cloud Platform - next generation) est une plateforme de virtualisation open-source basée sur Xen, le célèbre hyperviseur. C'est un fork de XenServer de Citrix, totalement gratuit et sans restrictions.

### Caractéristiques principales

- Hyperviseur Type 1 (bare-metal) basé sur Xen
- 100% open-source et gratuit (basé sur XenServer)
- Interface web Xen Orchestra
- Support de la virtualisation imbriquée
- Live Migration gratuite
- Backup et réplication avancés
- Excellentes performances
- Communauté active

## 🎯 Objectifs de ce JOB

- Installer XCP-ng
- Configurer Xen Orchestra (interface web)
- Créer une machine virtuelle Debian
- Activer la virtualisation imbriquée
- Explorer les fonctionnalités de base

## 💻 Prérequis

### Matériel

- Processeur 64-bit avec Intel VT-x/AMD-V
- 4 GB RAM minimum (8 GB recommandé)
- 46 GB minimum d'espace disque pour XCP-ng
- 100 GB supplémentaires pour les VMs
- Carte réseau (Gigabit recommandé)

### Logiciel

- ISO XCP-ng (dernière version)
- Clé USB bootable ou support d'installation
- ISO Debian pour les VMs
- Machine pour héberger Xen Orchestra (VM ou serveur séparé)

### Téléchargements

- **XCP-ng** : [xcp-ng.org](https://xcp-ng.org/download/)
  - Télécharger l'ISO XCP-ng 8.x (ou version disponible)
  - Totalement gratuit, aucune licence requise

## 📥 Installation de XCP-ng

### Préparation du média d'installation

Créer une clé USB bootable avec l'ISO XCP-ng.

```bash
# Linux - avec dd
sudo dd if=XCP-ng_8.x.x.iso of=/dev/sdX bs=1M status=progress
sudo sync
```

### Installation

1. **Démarrer depuis la clé USB**
   - Boot sur la clé USB
   - Écran de démarrage XCP-ng

2. **Welcome to XCP-ng**
   - Appuyer sur **Enter** pour continuer

3. **Keyboard**
   - Sélectionner le layout clavier (French ou autre)
   - **OK**

4. **EULA**
   - Lire et **Accept EULA**

5. **Disks**
   ```
   ⚪ Enable thin provisioning (SR) (optionnel)
   Sélectionner le disque d'installation
   ```
   - **OK**

6. **Installation Source**
   - ⚪ Local media (l'ISO)
   - **OK**

7. **Verify Installation Source**
   - ✅ Verify installation source (recommandé)
   - Ou Skip pour accélérer

8. **Set Password**
   ```
   Password : [Mot de passe root sécurisé]
   Re-enter : [Répéter]
   ```
   - **OK**

9. **Network Configuration**
   - **Configure Management Interface**
   ```
   Interface : [Sélectionner la carte réseau]
   Configuration : 
     ⚪ Static configuration
     IP : 192.168.140.10
     Netmask : 255.255.255.0
     Gateway : 192.168.140.1
   ```
   - **OK**

10. **Hostname and DNS**
    ```
    Hostname : xcpng
    DNS servers : 8.8.8.8
    ```
    - **OK**

11. **Time zone**
    - Sélectionner votre fuseau horaire
    - ⚪ NTP : Manual time entry (ou NTP server si disponible)
    - **OK**

12. **Install XCP-ng**
    - Confirmer l'installation
    - **Install XCP-ng**

13. **Installation en cours**
    - Attendre 5-10 minutes

14. **Installation Complete**
    - Retirer la clé USB
    - **OK** pour redémarrer

### Premier démarrage

Écran de XCP-ng :
```
XCP-ng Console

IPv4 Management: 192.168.140.10
IPv6 Management: [Désactivé]

Press <F8> for Network and Management Interface
```

- Appuyer sur **F8** pour accéder aux paramètres (si nécessaire)
- Appuyer sur **Enter** pour afficher les options avancées

## 🌐 Gestion de XCP-ng

XCP-ng peut être géré de plusieurs façons :

### 1. Via XCP-ng Center (Windows uniquement)

- Télécharger depuis [xcp-ng.org](https://xcp-ng.org/)
- Application Windows pour gérer XCP-ng
- Interface similaire à XenCenter

### 2. Via Xen Orchestra (recommandé)

**Xen Orchestra** est l'interface web moderne pour gérer XCP-ng. Deux options :

#### Option A : XOA (Xen Orchestra Appliance) - Payant

- Appliance pré-configurée
- Support officiel
- [xen-orchestra.com](https://xen-orchestra.com/)

#### Option B : XO from Sources - Gratuit (Community)

Installation manuelle sur une VM Linux

### 3. Via CLI (SSH)

```bash
# Se connecter en SSH
ssh root@192.168.140.10

# Commandes de base
xe vm-list
xe host-list
xe sr-list
```

## 🖥️ Installation de Xen Orchestra from Sources

Nous allons installer Xen Orchestra (XO) sur une VM Debian. Cette VM peut être créée sur XCP-ng lui-même ou sur un autre hyperviseur.

### Prérequis pour XO

- VM Debian 11/12 (2 vCPU, 2-4 GB RAM, 20 GB disk)
- Node.js et npm
- Git

### Installation de Xen Orchestra

```bash
# Se connecter à une VM Debian (SSH ou console)
# Mettre à jour le système
sudo apt update && sudo apt upgrade -y

# Installer les dépendances
sudo apt install -y \
    git \
    curl \
    build-essential \
    redis-server \
    libpng-dev \
    python3-minimal \
    python-is-python3 \
    nfs-common \
    cifs-utils \
    lvm2

# Installer Node.js (version 18 LTS)
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs

# Vérifier
node --version  # v18.x.x
npm --version   # 9.x.x

# Installer yarn
sudo npm install -g yarn

# Créer un utilisateur pour XO
sudo useradd -m -s /bin/bash xo

# Cloner le dépôt Xen Orchestra
sudo su - xo
git clone -b master https://github.com/vatesfr/xen-orchestra.git

# Aller dans le dossier
cd xen-orchestra

# Installer les dépendances et compiler (prend du temps)
yarn
yarn build

# Revenir à root
exit

# Créer le fichier de configuration
sudo cp /home/xo/xen-orchestra/packages/xo-server/sample.config.toml /home/xo/xen-orchestra/packages/xo-server/.xo-server.toml
sudo chown xo:xo /home/xo/xen-orchestra/packages/xo-server/.xo-server.toml

# Éditer la configuration (optionnel)
sudo nano /home/xo/xen-orchestra/packages/xo-server/.xo-server.toml
# Modifier port, bind address, etc. si nécessaire

# Créer un service systemd
sudo nano /etc/systemd/system/xo-server.service
```

**Contenu de xo-server.service** :
```ini
[Unit]
Description=Xen Orchestra Server
After=network.target

[Service]
Type=simple
User=xo
WorkingDirectory=/home/xo/xen-orchestra/packages/xo-server
ExecStart=/usr/bin/yarn start
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

```bash
# Activer et démarrer le service
sudo systemctl daemon-reload
sudo systemctl enable xo-server
sudo systemctl start xo-server

# Vérifier le statut
sudo systemctl status xo-server

# Vérifier les logs
sudo journalctl -u xo-server -f
```

### Accéder à Xen Orchestra

1. **Ouvrir un navigateur**
   ```
   http://[IP_VM_XO]:80
   ```
   Par défaut, XO écoute sur le port 80

2. **Premier accès**
   ```
   Email : admin@admin.net
   Password : admin
   ```
   **Important** : Changer le mot de passe après connexion !

3. **Changer le mot de passe**
   - Settings (icône roue dentée) → Users
   - Modifier l'utilisateur admin

### Ajouter XCP-ng à Xen Orchestra

1. **Dans Xen Orchestra**
   - Settings → Servers → Add Server

2. **Configuration**
   ```
   Label : XCP-ng-Lab
   Host : 192.168.140.10
   Username : root
   Password : [Mot de passe root XCP-ng]
   ✅ Unauthorized certificates
   ```

3. **Save**
   - XCP-ng devrait apparaître connecté (vert)

## 💾 Configuration du stockage

### Vérifier le Storage Repository par défaut

1. **Via Xen Orchestra**
   - Home → Storage
   - Un SR par défaut existe (Local storage)

2. **Via CLI**
   ```bash
   # Sur XCP-ng (SSH)
   xe sr-list
   
   # Informations détaillées
   xe sr-param-list uuid=[SR-UUID]
   ```

### Uploader l'ISO Debian

1. **Via Xen Orchestra**
   - New → Import → Disk
   - Ou aller dans Home → ISOs

2. **Via CLI**
   ```bash
   # Créer un Storage Repository ISO
   xe sr-create name-label="ISO-Library" type=iso device-config:location=/var/opt/xen/iso_import device-config:legacy_mode=true content-type=iso
   
   # Uploader via SCP
   scp debian-12.5.0-amd64-netinst.iso root@192.168.140.10:/var/opt/xen/iso_import/
   
   # Rescan le SR
   xe sr-scan uuid=[SR-UUID]
   ```

## 🖥️ Création de la machine virtuelle Debian

### Via Xen Orchestra

1. **Créer une VM**
   - New → VM

2. **Select Template**
   ```
   Template : Debian Bullseye 11 (ou Other install media)
   ```

3. **Info**
   ```
   Name : Debian-XCPng-Lab
   Description : Debian VM for nested virtualization testing
   ```

4. **CPU**
   ```
   vCPUs : 2
   Topology : 2 cores, 1 socket
   ```

5. **Memory**
   ```
   Memory : 4 GB (4096 MB)
   ```

6. **Disk**
   ```
   Storage : [SR par défaut]
   Name : Debian-XCPng-Lab
   Size : 20 GB
   ```

7. **Network**
   ```
   Network : Pool-wide network (ou autre)
   MAC address : Auto
   ```

8. **Boot**
   ```
   Boot : CD
   ISO/DVD : debian-xx.x.x-amd64-netinst.iso
   ```

9. **Advanced**
   - Cloud config : None
   - Resource set : None

10. **Summary**
    - **Create**

### Via CLI

```bash
# Se connecter en SSH à XCP-ng
ssh root@192.168.140.10

# Obtenir l'UUID du template Debian
xe template-list name-label="Debian Bullseye 11"

# Créer la VM à partir du template
xe vm-install template=[TEMPLATE-UUID] new-name-label="Debian-XCPng-Lab"

# Obtenir l'UUID de la VM créée
xe vm-list name-label="Debian-XCPng-Lab"

# Configurer la VM
VM_UUID=[UUID de la VM]

# CPU
xe vm-param-set uuid=$VM_UUID VCPUs-max=2
xe vm-param-set uuid=$VM_UUID VCPUs-at-startup=2

# RAM (en bytes : 4GB = 4294967296)
xe vm-param-set uuid=$VM_UUID memory-static-max=4294967296
xe vm-param-set uuid=$VM_UUID memory-dynamic-max=4294967296
xe vm-param-set uuid=$VM_UUID memory-dynamic-min=4294967296
xe vm-param-set uuid=$VM_UUID memory-static-min=4294967296

# Disque (créé automatiquement avec le template)
# Modifier la taille si nécessaire
VDI_UUID=$(xe vm-disk-list vm=$VM_UUID --minimal)
xe vdi-resize uuid=$VDI_UUID disk-size=21474836480  # 20GB

# Attacher l'ISO
ISO_UUID=$(xe cd-list name-label="debian-xx.x.x-amd64-netinst.iso" --minimal)
xe vm-cd-add uuid=$VM_UUID cd-name="debian-xx.x.x-amd64-netinst.iso" device=3

# Démarrer la VM
xe vm-start uuid=$VM_UUID
```

## ⚡ Configuration de la virtualisation imbriquée

### Activer nested virtualization

```bash
# Sur XCP-ng (SSH)
# Pour une VM spécifique (avant de démarrer la VM)
VM_UUID=[UUID de la VM Debian]

xe vm-param-set uuid=$VM_UUID platform:exp-nested-hvm=true

# Pour toutes les nouvelles VMs (optionnel)
xe pool-param-set uuid=[POOL-UUID] other-config:default-nested-hvm=true

# Vérifier
xe vm-param-get uuid=$VM_UUID param-name=platform param-key=exp-nested-hvm
# Devrait afficher : true
```

### Via Xen Orchestra

1. **Sélectionner la VM**
   - Home → VMs → Debian-XCPng-Lab

2. **Advanced**
   - Onglet "Advanced"
   - Chercher "Nested virtualization"
   - ✅ Activer

3. **Redémarrer la VM** si elle était démarrée

## 🚀 Installation de Debian dans la VM

### Démarrer l'installation

1. **Démarrer la VM**
   - Xen Orchestra : Start button
   - CLI : `xe vm-start uuid=$VM_UUID`

2. **Ouvrir la console**
   - Xen Orchestra : Console (onglet)
   - XCP-ng Center : Console

3. **Installer Debian**
   - Procédure standard
   ```
   Hostname : debian-xcpng
   Domain : local
   Root password : [Sécurisé]
   User : nexus / [mot de passe]
   Partitioning : Guided - use entire disk
   Software : SSH server, Standard utilities
   GRUB : /dev/xvda (ou /dev/sda)
   ```

### Post-installation

```bash
# Se connecter en root
su -

# Mettre à jour
apt update && apt upgrade -y

# Installer les Xen Guest Utilities
# Montrer le CD guest-tools
# Via Xen Orchestra : VM → Advanced → Install guest tools

# Ou manuellement :
mount /dev/cdrom /mnt
cd /mnt/Linux
./install.sh

# Si le script install n'est pas disponible, installer via APT
apt install -y xe-guest-utilities

# Activer et démarrer
systemctl enable xe-linux-distribution
systemctl start xe-linux-distribution

# Redémarrer
reboot
```

### Vérification des Guest Tools

```bash
# Dans la VM
systemctl status xe-linux-distribution

# Dans XCP-ng
xe vm-list params=name-label,os-version uuid=$VM_UUID
# Les informations OS doivent apparaître
```

## 🌐 Configuration réseau de la VM

### IP statique

```bash
# Identifier l'interface (généralement eth0 ou ens3)
ip addr show

# Éditer la configuration
sudo nano /etc/network/interfaces

# Configuration
auto eth0
iface eth0 inet static
    address 192.168.140.50
    netmask 255.255.255.0
    gateway 192.168.140.1
    dns-nameservers 8.8.8.8 8.8.4.4

# Redémarrer le réseau
sudo systemctl restart networking

# Ou redémarrer la VM
sudo reboot
```

## 📸 Snapshots et gestion

### Créer un snapshot

1. **Via Xen Orchestra**
   - VM → Snapshots → New snapshot
   ```
   Snapshot name : debian-fresh-install
   ```

2. **Via CLI**
   ```bash
   # Créer un snapshot
   xe vm-snapshot uuid=$VM_UUID new-name-label="debian-fresh-install"
   
   # Lister les snapshots
   xe snapshot-list
   
   # Restaurer un snapshot
   xe snapshot-revert uuid=[SNAPSHOT-UUID]
   ```

### Cloner une VM

1. **Via Xen Orchestra**
   - VM → General → Clone
   ```
   Name : Debian-XCPng-Clone
   Full copy : Yes
   ```

2. **Via CLI**
   ```bash
   # Clone complet
   xe vm-copy vm=$VM_UUID new-name-label="Debian-XCPng-Clone"
   ```

## 🔧 Optimisations

### Paramètres avancés

```bash
# Configurer le nombre de vCPUs
xe vm-param-set uuid=$VM_UUID VCPUs-max=4
xe vm-param-set uuid=$VM_UUID VCPUs-at-startup=4

# HA (High Availability) - nécessite pool
xe vm-param-set uuid=$VM_UUID ha-restart-priority=restart

# Description
xe vm-param-set uuid=$VM_UUID name-description="Debian VM for testing"
```

### Monitoring

1. **Via Xen Orchestra**
   - Dashboard : Vue d'ensemble des ressources
   - VM → Stats : Graphiques détaillés

2. **Via CLI**
   ```bash
   # Statistiques d'une VM
   xe vm-param-list uuid=$VM_UUID
   
   # CPU usage
   xentop
   ```

## ✅ Tests de validation

### Checklist de vérification

- [ ] XCP-ng installé et accessible
- [ ] Xen Orchestra installé et fonctionnel
- [ ] XCP-ng ajouté à Xen Orchestra
- [ ] ISO Debian uploadée
- [ ] VM Debian créée (2 vCPUs, 4 GB RAM, 20 GB disk)
- [ ] Virtualisation imbriquée activée
- [ ] Debian installé et opérationnel
- [ ] Xen Guest Utilities installées
- [ ] Réseau configuré et fonctionnel
- [ ] Snapshot initial créé

### Tests de virtualisation imbriquée

```bash
# Dans la VM Debian
# Vérifier le support
egrep -o '(vmx|svm)' /proc/cpuinfo

# Vérifier avec lscpu
lscpu | grep Virtualization

# Doit afficher "Virtualization: VT-x" ou "AMD-V"
```

### Tests réseau

```bash
# Ping XCP-ng host
ping -c 4 192.168.140.10

# Ping gateway
ping -c 4 192.168.140.1

# Ping Internet
ping -c 4 8.8.8.8
ping -c 4 google.com
```

## 🐛 Dépannage

### Problèmes courants

**Pas d'accès à XCP-ng**
- Vérifier l'IP : Sur la console XCP-ng, appuyer sur Enter
- Vérifier le réseau : `xe pif-list`

**Xen Orchestra ne se connecte pas**
- Vérifier que le service est démarré : `sudo systemctl status xo-server`
- Vérifier les logs : `sudo journalctl -u xo-server`
- Réinstaller si nécessaire

**VM ne démarre pas**
- Vérifier les logs : `xe vm-param-get uuid=$VM_UUID param-name=start-time`
- Logs XCP-ng : `tail -f /var/log/xensource.log`

**Guest Tools ne s'installent pas**
- Utiliser `apt install xe-guest-utilities` au lieu du CD
- Vérifier : `systemctl status xe-linux-distribution`

**Virtualisation imbriquée ne fonctionne pas**
- Vérifier : `xe vm-param-get uuid=$VM_UUID param-name=platform param-key=exp-nested-hvm`
- Doit être "true"
- La VM doit être arrêtée lors de la configuration

### Commandes utiles

```bash
# Lister toutes les VMs
xe vm-list

# Démarrer/arrêter une VM
xe vm-start uuid=$VM_UUID
xe vm-shutdown uuid=$VM_UUID
xe vm-reboot uuid=$VM_UUID

# Informations système
xsconsole  # Interface TUI de configuration

# Redémarrer XCP-ng
reboot

# Version de XCP-ng
cat /etc/xcp/version
```

## 📚 Ressources supplémentaires

- [XCP-ng Documentation](https://docs.xcp-ng.org/)
- [Xen Orchestra Documentation](https://xen-orchestra.com/docs/)
- [XCP-ng Forum](https://xcp-ng.org/forum/)
- [XCP-ng GitHub](https://github.com/xcp-ng)
- [Xen Project](https://xenproject.org/)

## 🎉 Conclusion du projet

**Félicitations !** Vous avez maintenant complété l'**Opération Nexus Virtualis** avec succès !

Vous avez installé et configuré :
- ✅ VMware Workstation (Type 2)
- ✅ Microsoft Hyper-V (Type 1)
- ✅ VMware ESXi (Type 1)
- ✅ Proxmox VE (Type 1)
- ✅ XCP-ng (Type 1)

Vous maîtrisez maintenant :
- Les concepts de virtualisation imbriquée
- L'installation et la configuration de multiples hyperviseurs
- Le déploiement de machines virtuelles Linux
- La gestion des ressources et du réseau
- Les snapshots et backups

### Prochaines explorations possibles

- Configurer un cluster Proxmox ou XCP-ng
- Mettre en place des migrations à chaud (Live Migration)
- Automatiser le déploiement avec Terraform/Ansible
- Explorer la conteneurisation (Docker, LXC)
- Approfondir les performances et le tuning

---

[← JOB 6 : Proxmox VE](./JOB6_proxmox.md) | [Retour au README](../README.md)
