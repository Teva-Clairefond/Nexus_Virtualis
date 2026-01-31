# JOB 6 : Configuration Proxmox VE

## 📖 Présentation

Proxmox VE (Virtual Environment) est une plateforme open-source de virtualisation et de gestion d'infrastructure basée sur Debian. Elle combine KVM pour la virtualisation complète et LXC pour la conteneurisation légère.

### Caractéristiques principales

- Hyperviseur Type 1 (bare-metal) basé sur Debian
- KVM (Kernel-based Virtual Machine) + LXC (Linux Containers)
- Interface web complète (port 8006)
- Support de la virtualisation imbriquée
- Clustering et High Availability
- Backup et réplication intégrés
- API REST complète
- 100% open-source et gratuit

## 🎯 Objectifs de ce JOB

- Installer Proxmox VE
- Configurer l'interface web et le réseau
- Créer une machine virtuelle Debian (KVM)
- Activer la virtualisation imbriquée
- Explorer les fonctionnalités de base

## 💻 Prérequis

### Matériel

- Processeur 64-bit avec Intel VT-x/AMD-V
- 4 GB RAM minimum (8 GB recommandé)
- 32 GB d'espace disque pour Proxmox
- 100 GB supplémentaires pour les VMs
- Carte réseau (Gigabit recommandé)

### Logiciel

- ISO Proxmox VE (dernière version)
- Clé USB bootable ou support d'installation
- ISO Debian pour les VMs

### Téléchargements

- **Proxmox VE** : [proxmox.com](https://www.proxmox.com/en/downloads)
  - Télécharger l'ISO Proxmox VE 8.x (ou version disponible)
  - Pas de licence requise pour l'édition Community

## 📥 Installation de Proxmox VE

### Préparation du média d'installation

Utiliser Rufus, Etcher ou `dd` pour créer une clé USB bootable avec l'ISO Proxmox VE.

```bash
# Linux - avec dd (attention à bien identifier le périphérique)
sudo dd if=proxmox-ve_8.x-x.iso of=/dev/sdX bs=1M status=progress
sudo sync
```

### Installation

1. **Démarrer depuis la clé USB**
   - Boot sur la clé USB
   - Sélectionner "Install Proxmox VE (Graphical)"

2. **EULA**
   - Lire et accepter les termes : **I agree**

3. **Target Harddisk**
   ```
   Target Harddisk : /dev/sda (sélectionner le disque)
   Filesystem : ext4 (ou ZFS pour fonctionnalités avancées)
   ```
   
   **Options** (si nécessaire) :
   ```
   hdsize : [taille] (utiliser tout l'espace)
   swapsize : [auto ou personnalisé]
   maxroot : [taille partition root]
   ```

4. **Location and Time Zone**
   ```
   Country : France (ou votre pays)
   Time zone : Europe/Paris
   Keyboard Layout : French
   ```

5. **Administration Password and Email**
   ```
   Password : [Mot de passe root sécurisé]
   Confirm : [Répéter]
   Email : admin@example.com (pour notifications)
   ```

6. **Management Network Configuration**
   ```
   Management Interface : [Carte réseau principale, ex: enp0s3]
   Hostname (FQDN) : proxmox.local
   IP Address (CIDR) : 192.168.130.10/24
   Gateway : 192.168.130.1
   DNS Server : 8.8.8.8
   ```

7. **Summary**
   - Vérifier les paramètres
   - **Install** pour commencer l'installation

8. **Installation en cours**
   - Attendre 5-10 minutes

9. **Redémarrer**
   - Retirer la clé USB
   - **Reboot**

### Premier démarrage

Proxmox affiche l'écran de connexion :
```
Welcome to the Proxmox Virtual Environment. Please use your web browser to 
configure this server - connect to:

  https://192.168.130.10:8006/

Login as root with your root password
```

## 🌐 Accès à l'interface web

### Connexion

1. **Ouvrir un navigateur web**
   ```
   https://192.168.130.10:8006
   ```

2. **Ignorer l'avertissement de certificat** (self-signed)
   - Avancé → Continuer vers le site

3. **Se connecter**
   ```
   User name : root
   Password : [Votre mot de passe]
   Realm : Linux PAM standard authentication
   Language : French (ou English)
   ```

4. **Message de souscription**
   - Un message sur la souscription Enterprise peut apparaître
   - Cliquer sur **OK** (on utilisera les dépôts Community)

### Configuration des dépôts (Community)

Par défaut, Proxmox utilise les dépôts Enterprise (payants). Basculer vers Community :

#### Via l'interface web

1. **Se connecter en SSH**
   ```bash
   ssh root@192.168.130.10
   ```

2. **Désactiver le dépôt Enterprise**
   ```bash
   # Commenter la ligne enterprise
   nano /etc/apt/sources.list.d/pve-enterprise.list
   
   # Commenter (ajouter # au début) :
   # deb https://enterprise.proxmox.com/debian/pve bullseye pve-enterprise
   ```

3. **Ajouter le dépôt Community**
   ```bash
   # Ajouter le dépôt no-subscription
   echo "deb http://download.proxmox.com/debian/pve bullseye pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list
   ```

4. **Mettre à jour**
   ```bash
   apt update
   apt dist-upgrade -y
   ```

5. **Supprimer le message de souscription (optionnel)**
   ```bash
   # Éditer le fichier JavaScript
   sed -Ezi.bak "s/(Ext.Msg.show\(\{\s+title: gettext\('No valid sub)/void\(\{ \/\/\1/g" /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
   
   # Redémarrer le service web
   systemctl restart pveproxy
   ```

## ⚙️ Configuration initiale

### Configuration réseau

1. **Dans l'interface web**
   - Datacenter → [Nom du nœud] → System → Network

2. **Vérifier la configuration**
   - `vmbr0` : Bridge par défaut (connecté à l'interface physique)

3. **Créer un bridge supplémentaire** (optionnel)
   - Create → Linux Bridge
   ```
   Name : vmbr1
   IP Address : 192.168.131.1/24
   Autostart : Yes
   Comment : Internal network
   ```

### Stockage

1. **Datacenter → Storage**
   - Stockage par défaut : `local` et `local-lvm`
   - `local` : ISO, templates, backups
   - `local-lvm` : Disques VMs (LVM-thin)

2. **Uploader l'ISO Debian**
   - [Nom du nœud] → local → ISO Images
   - **Upload**
   - Sélectionner `debian-xx.x.x-amd64-netinst.iso`
   - Attendre la fin de l'upload

## 🖥️ Création de la machine virtuelle Debian

### Via l'interface web

1. **Créer une VM**
   - Clic sur **Create VM** (en haut à droite)

2. **General**
   ```
   Node : [Votre nœud Proxmox]
   VM ID : 100 (auto-incrémenté)
   Name : Debian-Proxmox-Lab
   Resource Pool : [Aucun ou créer un pool]
   ```

3. **OS**
   ```
   Use CD/DVD disc image file (iso)
   Storage : local
   ISO image : debian-xx.x.x-amd64-netinst.iso
   Guest OS Type : Linux
   Version : 6.x - 2.6 Kernel
   ```

4. **System**
   ```
   Graphic card : Default
   Machine : Default (i440fx)
   BIOS : Default (SeaBIOS)
   SCSI Controller : VirtIO SCSI single
   Qemu Agent : ✅ (à installer plus tard)
   ```

5. **Disks**
   ```
   Bus/Device : SCSI
   Storage : local-lvm
   Disk size (GiB) : 20
   Cache : Default (No cache)
   Discard : ✅ (si SSD)
   SSD emulation : ✅ (si stocké sur SSD)
   ```

6. **CPU**
   ```
   Sockets : 1
   Cores : 2
   Type : Default (kvm64) ou host (pour nested)
   ```
   **Important pour nested** : Choisir **Type : host**

7. **Memory**
   ```
   Memory (MiB) : 4096
   ```

8. **Network**
   ```
   Bridge : vmbr0
   Model : VirtIO (paravirtualized)
   Firewall : ✅ (optionnel)
   ```

9. **Confirm**
   - ✅ Start after created (optionnel)
   - **Finish**

### Activer la virtualisation imbriquée

**Important** : Configurer le CPU type = host et ajouter des flags

1. **Éditer la configuration de la VM**
   - Sélectionner la VM → Hardware → Processors
   - **Edit**
   - Type : **host**
   - Flags : **+pdpe1gb** (optionnel, pour grandes pages)

2. **Via CLI (SSH)**
   ```bash
   # Se connecter en SSH
   ssh root@192.168.130.10
   
   # Éditer la config de la VM (VM ID 100)
   nano /etc/pve/qemu-server/100.conf
   
   # S'assurer que la ligne CPU contient :
   cpu: host
   
   # Sauvegarder et quitter
   ```

3. **Vérifier le support du CPU hôte**
   ```bash
   # Sur Proxmox
   egrep -o '(vmx|svm)' /proc/cpuinfo
   
   # Vérifier que KVM nested est activé
   cat /sys/module/kvm_intel/parameters/nested  # Intel
   cat /sys/module/kvm_amd/parameters/nested    # AMD
   
   # Devrait afficher : Y ou 1
   ```

4. **Activer nested si nécessaire**
   ```bash
   # Pour Intel
   echo "options kvm_intel nested=1" > /etc/modprobe.d/kvm-nested.conf
   
   # Pour AMD
   echo "options kvm_amd nested=1" > /etc/modprobe.d/kvm-nested.conf
   
   # Recharger les modules (nécessite redémarrage ou)
   modprobe -r kvm_intel
   modprobe kvm_intel
   ```

## 🚀 Installation de Debian dans la VM

### Démarrer l'installation

1. **Démarrer la VM**
   - Sélectionner la VM → Start

2. **Ouvrir la console**
   - Console → noVNC ou xterm.js

3. **Installer Debian**
   - Procédure identique aux JOBs précédents
   ```
   Hostname : debian-proxmox
   Domain : local
   Root password : [Sécurisé]
   User : nexus / [mot de passe]
   Partitioning : Guided - use entire disk
   Software : SSH server, Standard utilities
   GRUB : /dev/sda
   ```

### Post-installation

```bash
# Se connecter en root
su -

# Mettre à jour
apt update && apt upgrade -y

# Installer qemu-guest-agent (important pour Proxmox)
apt install -y qemu-guest-agent

# Activer et démarrer le service
systemctl enable qemu-guest-agent
systemctl start qemu-guest-agent

# Installer des outils utiles
apt install -y vim curl wget net-tools sudo

# Redémarrer
reboot
```

### Vérification du QEMU Guest Agent

Dans l'interface Proxmox :
- VM → Summary
- Le champ "IPs" devrait afficher les adresses IP de la VM
- Indique que l'agent fonctionne correctement

## 🌐 Configuration réseau de la VM

### IP statique

```bash
# Éditer la configuration réseau
sudo nano /etc/network/interfaces

# Configuration
auto ens18
iface ens18 inet static
    address 192.168.130.50
    netmask 255.255.255.0
    gateway 192.168.130.1
    dns-nameservers 8.8.8.8 8.8.4.4

# Redémarrer le réseau
sudo systemctl restart networking

# Ou redémarrer la VM
sudo reboot
```

### Test de connectivité

```bash
# Ping gateway
ping -c 4 192.168.130.1

# Ping Proxmox host
ping -c 4 192.168.130.10

# Ping Internet
ping -c 4 8.8.8.8
ping -c 4 google.com
```

## 📸 Snapshots et backups

### Créer un snapshot

1. **Via l'interface web**
   - VM → Snapshots → Take Snapshot
   ```
   Snapshot name : debian-fresh-install
   Description : Clean Debian with QEMU agent
   ⚪ Include RAM (pour snapshot à chaud)
   ```

2. **Via CLI**
   ```bash
   # Créer un snapshot
   qm snapshot 100 debian-fresh-install --description "Clean install"
   
   # Lister les snapshots
   qm listsnapshot 100
   
   # Restaurer un snapshot
   qm rollback 100 debian-fresh-install
   
   # Supprimer un snapshot
   qm delsnapshot 100 debian-fresh-install
   ```

### Créer un backup

1. **Via l'interface web**
   - VM → Backup → Backup now
   ```
   Storage : local (ou autre destination)
   Mode : Snapshot (VMs en cours) ou Stop (arrêt temporaire)
   Compression : ZSTD (recommandé)
   ```

2. **Via CLI**
   ```bash
   # Backup complet
   vzdump 100 --storage local --mode snapshot --compress zstd
   
   # Restaurer un backup
   qmrestore /var/lib/vz/dump/vzdump-qemu-100-*.vma.zst 100
   ```

## 🔧 Gestion et optimisations

### Cloud-init (optionnel avancé)

Pour automatiser le déploiement de VMs :

1. **Télécharger une image Cloud**
   ```bash
   cd /var/lib/vz/template/iso
   wget https://cloud.debian.org/images/cloud/bullseye/latest/debian-11-generic-amd64.qcow2
   ```

2. **Créer une VM template avec cloud-init**
   - Utiliser les options Cloud-Init dans Proxmox

### Optimisation des performances

1. **CPU Pinning** (avancé)
   - VM → Hardware → Processors → Advanced
   - Affecter des cœurs spécifiques

2. **Huge Pages**
   ```bash
   # Sur Proxmox
   echo "vm.nr_hugepages = 512" >> /etc/sysctl.conf
   sysctl -p
   ```

3. **I/O Thread**
   - VM → Hardware → Hard Disk → Edit
   - ✅ IOThread

### Monitoring

1. **Via l'interface web**
   - VM → Summary : Graphiques temps réel
   - Datacenter → [Nœud] → Summary : Vue globale

2. **Via CLI**
   ```bash
   # État des VMs
   qm list
   
   # Statistiques d'une VM
   qm status 100
   
   # Top des ressources
   top
   htop
   ```

## ✅ Tests de validation

### Checklist de vérification

- [ ] Proxmox VE installé et accessible via web
- [ ] Dépôts Community configurés
- [ ] ISO Debian uploadée
- [ ] VM Debian créée (2 cores, 4 GB RAM, 20 GB disk)
- [ ] CPU type = host pour virtualisation imbriquée
- [ ] Nested virtualization activée sur Proxmox
- [ ] Debian installé et opérationnel
- [ ] QEMU Guest Agent installé et fonctionnel
- [ ] Réseau configuré et fonctionnel
- [ ] Snapshot initial créé

### Tests de virtualisation imbriquée

```bash
# Dans la VM Debian
# Vérifier le support
egrep -o '(vmx|svm)' /proc/cpuinfo

# Vérifier avec lscpu
lscpu | grep Virtualization

# Installer KVM pour tester (optionnel)
sudo apt install -y qemu-kvm libvirt-daemon-system
sudo systemctl status libvirtd
```

### Tests réseau et agent

```bash
# Test réseau
ping -c 4 192.168.130.1
ping -c 4 8.8.8.8

# Vérifier l'agent
sudo systemctl status qemu-guest-agent

# Dans Proxmox web : VM Summary devrait afficher l'IP
```

## 🐛 Dépannage

### Problèmes courants

**Pas d'accès à l'interface web**
- Vérifier que le service est actif : `systemctl status pveproxy`
- Vérifier l'IP : `ip addr show`
- Vérifier le firewall : `iptables -L`

**Message de souscription persistant**
- Revoir la section "Supprimer le message de souscription"
- Vider le cache du navigateur

**VM ne démarre pas**
- Vérifier les logs : `qm showcmd 100` puis `journalctl -xe`
- Vérifier l'espace disque : `df -h`

**QEMU Guest Agent ne fonctionne pas**
- Dans la VM : `sudo systemctl status qemu-guest-agent`
- Installer : `sudo apt install qemu-guest-agent`
- Dans Proxmox VM config : Hardware → Options → QEMU Guest Agent = Enabled

**Virtualisation imbriquée ne fonctionne pas**
- Vérifier CPU type = host
- Sur Proxmox : `cat /sys/module/kvm_*/parameters/nested`
- Doit afficher Y ou 1

### Commandes utiles

```bash
# Lister toutes les VMs
qm list

# Démarrer/arrêter une VM
qm start 100
qm stop 100
qm shutdown 100

# Redémarrer Proxmox
reboot

# Vérifier la version
pveversion -v

# Logs
journalctl -u pveproxy
journalctl -u pvedaemon
```

## 📚 Ressources supplémentaires

- [Proxmox VE Documentation](https://pve.proxmox.com/pve-docs/)
- [Proxmox VE Wiki](https://pve.proxmox.com/wiki/Main_Page)
- [Proxmox Forum](https://forum.proxmox.com/)
- [API Documentation](https://pve.proxmox.com/pve-docs/api-viewer/)

## 🚀 Prochaines étapes

Proxmox VE est maintenant opérationnel avec une VM Debian ! Continuez avec le dernier hyperviseur :

- [JOB 7 : Configuration XCP-ng](./JOB7_xcpng.md)

---

[← JOB 5 : ESXi](./JOB5_esxi.md) | [Retour au README](../README.md) | [JOB 7 : XCP-ng →](./JOB7_xcpng.md)
