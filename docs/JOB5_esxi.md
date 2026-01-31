# JOB 5 : Configuration VMware ESXi

## 📖 Présentation

VMware ESXi est un hyperviseur bare-metal (Type 1) de classe enterprise développé par VMware. Il s'installe directement sur le matériel physique et offre des performances optimales pour les environnements de production.

### Caractéristiques principales

- Hyperviseur Type 1 (bare-metal)
- Empreinte minimale (environ 150 MB)
- Gestion via interface web (vSphere Client)
- Support de la virtualisation imbriquée
- vMotion, HA, DRS (versions payantes)
- API complètes pour l'automatisation

## 🎯 Objectifs de ce JOB

- Installer VMware ESXi
- Configurer l'interface web et le réseau
- Créer une machine virtuelle Debian
- Activer la virtualisation imbriquée
- Gérer le stockage et les ressources

## 💻 Prérequis

### Matériel

- Processeur 64-bit avec support Intel VT-x ou AMD-V
- 8 GB RAM minimum (16 GB recommandé)
- 32 GB d'espace disque pour ESXi
- 100 GB supplémentaires pour les VMs
- Carte réseau compatible (consulter la HCL VMware)

### Logiciel

- ISO VMware ESXi (version gratuite disponible)
- Clé USB bootable (8 GB minimum) ou support d'installation
- Outil comme Rufus ou Etcher pour créer le média bootable
- ISO Debian pour les VMs

### Téléchargements

- **VMware ESXi** : [my.vmware.com](https://my.vmware.com/web/vmware/evalcenter?p=free-esxi8)
  - Créer un compte VMware
  - Télécharger ESXi 8.0 (ou version disponible)
  - Obtenir une licence gratuite

## 📥 Installation de VMware ESXi

### Préparation du média d'installation

#### Avec Rufus (Windows)

1. **Télécharger Rufus** : [rufus.ie](https://rufus.ie)

2. **Créer la clé USB bootable**
   ```
   Device : [Votre clé USB]
   Boot selection : VMware-VMvisor-Installer-xx.x.x-xxxxx.x86_64.iso
   Partition scheme : MBR
   Target system : BIOS or UEFI
   File system : FAT32
   ```

3. **Cliquer sur START**

#### Avec Etcher (Windows/Mac/Linux)

1. **Télécharger Etcher** : [balena.io/etcher](https://www.balena.io/etcher/)

2. **Créer la clé USB**
   - Select image → ESXi ISO
   - Select target → Clé USB
   - Flash!

### Installation

1. **Démarrer depuis la clé USB**
   - Entrer dans le BIOS/UEFI (F2, DEL, F12 selon fabricant)
   - Modifier l'ordre de boot
   - Sauvegarder et redémarrer

2. **Installer ESXi**
   - Écran de démarrage : Appuyer sur **Enter**
   - Accepter l'EULA : **F11**

3. **Sélectionner le disque d'installation**
   - Choisir le disque pour ESXi (32 GB minimum)
   - **Enter** pour confirmer

4. **Choisir le layout clavier**
   - French ou autre selon préférence
   - **Enter**

5. **Définir le mot de passe root**
   ```
   Root password : [Mot de passe sécurisé, min 8 caractères]
   ```
   - Respecter les exigences : majuscules, minuscules, chiffres, caractères spéciaux

6. **Confirmer l'installation**
   - **F11** pour installer
   - Attendre la fin de l'installation (quelques minutes)

7. **Redémarrer**
   - **Enter** pour redémarrer
   - Retirer la clé USB

### Premier démarrage

1. **Écran de démarrage ESXi**
   ```
   VMware ESXi 8.0.0
   
   Press F2 to Customize System/View Logs
   Press F12 for Shut Down/Restart
   ```

2. **Accéder à la configuration**
   - Appuyer sur **F2**
   - Entrer le mot de passe root

## ⚙️ Configuration initiale

### Configuration réseau

1. **Dans le menu de configuration** (F2)
   - **Configure Management Network**

2. **Network Adapters**
   - Sélectionner la ou les cartes réseau
   - **Enter** pour valider

3. **IPv4 Configuration**
   ```
   ⚪ Set static IPv4 address and network configuration
   
   IPv4 Address : 192.168.120.10
   Subnet Mask : 255.255.255.0
   Default Gateway : 192.168.120.1
   ```

4. **DNS Configuration**
   ```
   Primary DNS Server : 8.8.8.8
   Secondary DNS Server : 8.8.4.4
   Hostname : esxi-lab.local
   ```

5. **IPv6 Configuration**
   - Désactiver si non utilisé

6. **Valider et redémarrer les services**
   - **ESC** pour sortir
   - **Y** pour appliquer les changements et redémarrer le réseau

### Obtenir et appliquer la licence gratuite

1. **Sur my.vmware.com**
   - Se connecter
   - Products → All Products → VMware vSphere Hypervisor
   - Register → Obtenir la clé de licence

2. **Appliquer la licence via web**
   - Connecter au web client
   - Manage → Licensing → Assign License
   - Entrer la clé reçue

## 🌐 Accès à l'interface web (vSphere Client)

### Connexion

1. **Ouvrir un navigateur web**
   ```
   https://192.168.120.10
   ```

2. **Ignorer l'avertissement de certificat** (self-signed)

3. **Se connecter**
   ```
   User name : root
   Password : [Votre mot de passe]
   ```

4. **Interface vSphere Client**
   - Dashboard avec statistiques
   - Gestion des VMs
   - Configuration du stockage et réseau

### Configuration initiale via web

1. **Accepter la licence**
   - Premier accès : accepter le CLUF

2. **Activer SSH (optionnel)**
   - Host → Actions → Services → Enable Secure Shell (SSH)

3. **Activer ESXi Shell (console locale)**
   - Host → Actions → Services → Enable ESXi Shell

## 💾 Configuration du stockage

### Vérifier les datastores

1. **Storage → Datastores**
   - Un datastore par défaut est créé sur le disque d'installation

2. **Créer un nouveau datastore** (si disques supplémentaires)
   - Storage → Datastores → New datastore
   - Type : VMFS 6
   - Name : `datastore2`
   - Sélectionner le disque
   - Configuration : Use full disk

### Uploader l'ISO Debian

1. **Datastore browser**
   - Storage → Datastores → `datastore1` → Browse

2. **Créer un dossier ISOs**
   - Create directory → `ISOs`

3. **Upload**
   - Upload → Sélectionner `debian-xx.x.x-amd64-netinst.iso`
   - Attendre la fin de l'upload

## 🌐 Configuration du réseau virtuel

### vSwitch par défaut

ESXi crée automatiquement **vSwitch0** avec :
- VM Network (port group pour les VMs)
- Management Network (pour l'accès ESXi)

### Créer un port group supplémentaire

1. **Networking → Port groups**
   - Add port group

2. **Configuration**
   ```
   Name : VM-Network-Lab
   VLAN ID : 0 (pas de VLAN)
   vSwitch : vSwitch0
   Security :
       ✅ Promiscuous mode : Accept (pour virtualisation imbriquée)
       ✅ MAC address changes : Accept
       ✅ Forged transmits : Accept
   ```

### Créer un vSwitch supplémentaire (optionnel)

1. **Networking → Virtual switches**
   - Add standard virtual switch

2. **Configuration**
   ```
   vSwitch name : vSwitch1
   MTU : 1500
   Number of ports : 128
   ```

## 🖥️ Création de la machine virtuelle Debian

### Via vSphere Client

1. **Create / Register VM**
   - Virtual Machines → Create / Register VM

2. **Select creation type**
   - ⚪ Create a new virtual machine

3. **Select a name and guest OS**
   ```
   Name : Debian-ESXi-Lab
   Compatibility : ESXi 8.0 virtual machine
   Guest OS family : Linux
   Guest OS version : Debian GNU/Linux 11 (64-bit)
   ```

4. **Select storage**
   ```
   Datastore : datastore1
   ```

5. **Customize settings**

   **Virtual Hardware** :
   ```
   CPU : 2
   Memory : 4096 MB
   Hard disk 1 : 20 GB
   SCSI Controller : VMware Paravirtual
   Network Adapter 1 : VM Network (or VM-Network-Lab)
   CD/DVD Drive 1 : Datastore ISO file
       → Browse → ISOs/debian-xx.x.x-amd64-netinst.iso
       ✅ Connect at power on
   ```

   **VM Options** :
   - Boot Options → Firmware : BIOS (ou UEFI pour Debian récent)

6. **Ready to complete**
   - Review → **Finish**

### Activer la virtualisation imbriquée

**Important** : Faire avant de démarrer la VM

1. **Éditer les paramètres de la VM**
   - Clic droit sur la VM → Edit settings

2. **VM Options → Advanced**
   - Configuration Parameters → **Edit Configuration**

3. **Ajouter les paramètres**
   - Add Parameter :
   ```
   vhv.enable = TRUE
   ```

4. **Sauvegarder**

### Alternative : Éditer le fichier .vmx via SSH

```bash
# Se connecter en SSH
ssh root@192.168.120.10

# Naviguer vers le datastore
cd /vmfs/volumes/datastore1/Debian-ESXi-Lab/

# Éditer le fichier .vmx
vi Debian-ESXi-Lab.vmx

# Ajouter à la fin :
vhv.enable = "TRUE"

# Sauvegarder et quitter (:wq)

# Recharger la VM
vim-cmd vmsvc/getallvms  # Noter le vmid
vim-cmd vmsvc/reload [vmid]
```

## 🚀 Installation de Debian

### Démarrer l'installation

1. **Power on la VM**
   - Clic droit → Power → Power On

2. **Ouvrir la console**
   - Clic sur l'icône de console ou Launch Remote Console

3. **Boot Debian installer**
   - Sélectionner "Graphical install" ou "Install"

### Configuration

Les étapes sont similaires aux JOBs précédents :

```
Hostname : debian-esxi
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

# Installer les outils utiles
apt install -y open-vm-tools vim curl wget net-tools

# Redémarrer
reboot
```

## 🛠️ Installation de VMware Tools

### Méthode recommandée : open-vm-tools

```bash
# Installer open-vm-tools
sudo apt update
sudo apt install -y open-vm-tools

# Redémarrer
sudo reboot
```

### Vérification

```bash
# Vérifier le service
sudo systemctl status open-vm-tools

# Informations sur VMware Tools
vmware-toolbox-cmd -v
```

Dans vSphere Client, la colonne "VMware Tools" devrait afficher "Running (Guest managed)".

## 🌐 Configuration réseau de la VM

### IP statique

```bash
# Éditer la configuration
sudo nano /etc/network/interfaces

# Configuration
auto ens33
iface ens33 inet static
    address 192.168.120.50
    netmask 255.255.255.0
    gateway 192.168.120.1
    dns-nameservers 8.8.8.8 8.8.4.4

# Redémarrer le réseau
sudo systemctl restart networking

# Tester
ping -c 4 8.8.8.8
```

## 📸 Snapshots et gestion

### Créer un snapshot

1. **Via vSphere Client**
   - Clic droit sur la VM → Snapshots → Take snapshot
   ```
   Name : Debian-Fresh-Install
   Description : Clean Debian with VMware Tools
   ⚪ Snapshot the virtual machine's memory
   ```

2. **Via SSH/ESXi Shell**
   ```bash
   # Lister les VMs
   vim-cmd vmsvc/getallvms
   
   # Créer un snapshot (remplacer [vmid])
   vim-cmd vmsvc/snapshot.create [vmid] "Debian-Fresh-Install" "Clean install" 0 0
   
   # Lister les snapshots
   vim-cmd vmsvc/snapshot.get [vmid]
   ```

### Cloner une VM

1. **Via vSphere Client**
   - Clic droit sur la VM → Clone
   ```
   Name : Debian-ESXi-Clone
   Clone type : Create a full clone
   ```

## ⚡ Optimisations et ajustements

### Paramètres avancés de la VM

```
# Via Configuration Parameters
tools.syncTime = "FALSE"  # Désactiver sync temps (utiliser NTP)
isolation.tools.copy.disable = "FALSE"  # Activer copy/paste
isolation.tools.paste.disable = "FALSE"
```

### Performance

```bash
# Dans Debian
# Installer les outils de performance
sudo apt install -y sysstat htop iotop

# Vérifier les performances
vmstat 1
iostat -x 1
```

### Monitoring ESXi

1. **Via vSphere Client**
   - Monitor → Performance
   - Graphiques CPU, RAM, Network, Disk

2. **Via esxtop (SSH)**
   ```bash
   esxtop
   # c = CPU, m = Memory, n = Network, d = Disk
   # Appuyer sur la lettre pour changer de vue
   ```

## ✅ Tests de validation

### Checklist de vérification

- [ ] ESXi installé et accessible via web
- [ ] Licence gratuite appliquée
- [ ] Réseau configuré (IP statique)
- [ ] Datastore configuré et ISO uploadée
- [ ] VM Debian créée (2 vCPUs, 4 GB RAM, 20 GB disk)
- [ ] Virtualisation imbriquée activée (vhv.enable = TRUE)
- [ ] Debian installé et opérationnel
- [ ] VMware Tools (open-vm-tools) installé
- [ ] Réseau VM configuré et fonctionnel
- [ ] Snapshot initial créé

### Tests de virtualisation imbriquée

```bash
# Dans la VM Debian
# Vérifier le support
egrep -o '(vmx|svm)' /proc/cpuinfo

# Si vmx ou svm apparaît, nested est OK

# Vérifier avec lscpu
lscpu | grep Virtualization
```

### Tests réseau

```bash
# Ping ESXi host
ping -c 4 192.168.120.10

# Ping gateway
ping -c 4 192.168.120.1

# Ping Internet
ping -c 4 8.8.8.8
ping -c 4 google.com
```

## 🐛 Dépannage

### Problèmes courants

**ESXi ne démarre pas**
- Vérifier la compatibilité matérielle (HCL VMware)
- Vérifier que la virtualisation est activée dans le BIOS
- Essayer de désactiver Secure Boot

**Pas d'accès réseau à ESXi**
- Vérifier la configuration IP (F2 → Configure Management Network)
- Vérifier les câbles et switches
- Ping depuis ESXi (ALT + F1, puis `ping 192.168.120.1`)

**VM ne démarre pas**
- Vérifier les logs : `/vmfs/volumes/datastore1/[VM]/vmware.log`
- Vérifier les ressources disponibles (CPU, RAM)
- S'assurer que le datastore n'est pas plein

**VMware Tools ne fonctionnent pas**
- Utiliser open-vm-tools au lieu des tools officiels
- Vérifier : `sudo systemctl status open-vm-tools`

**Virtualisation imbriquée ne fonctionne pas**
- Vérifier que `vhv.enable = "TRUE"` est dans le .vmx
- La VM doit être éteinte lors de l'ajout du paramètre
- Recharger la config : `vim-cmd vmsvc/reload [vmid]`

### Commandes utiles

```bash
# Redémarrer les services ESXi
/etc/init.d/hostd restart
/etc/init.d/vpxa restart

# Voir les VMs
vim-cmd vmsvc/getallvms

# Power on/off une VM
vim-cmd vmsvc/power.on [vmid]
vim-cmd vmsvc/power.off [vmid]

# État d'une VM
vim-cmd vmsvc/get.summary [vmid]
```

## 📚 Ressources supplémentaires

- [VMware ESXi Documentation](https://docs.vmware.com/en/VMware-vSphere/8.0/vsphere-esxi-80-installation-setup-guide.pdf)
- [vSphere Command-Line Interface](https://developer.vmware.com/docs/11743/)
- [VMware Compatibility Guide (HCL)](https://www.vmware.com/resources/compatibility/search.php)
- [VMware Knowledge Base](https://kb.vmware.com/)

## 🚀 Prochaines étapes

ESXi est maintenant opérationnel avec une VM Debian ! Continuez avec :

- [JOB 6 : Configuration Proxmox VE](./JOB6_proxmox.md)

---

[← JOB 4 : Hyper-V](./JOB4_hyperv.md) | [Retour au README](../README.md) | [JOB 6 : Proxmox VE →](./JOB6_proxmox.md)
