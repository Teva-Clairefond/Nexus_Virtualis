# JOB 3 : Configuration VMware Workstation

## 📖 Présentation

VMware Workstation est un hyperviseur de type 2 (hosted) développé par VMware. C'est une solution puissante et mature pour la virtualisation sur poste de travail, idéale pour le développement, les tests et la formation.

### Caractéristiques principales

- Hyperviseur Type 2 (fonctionne sur Windows/Linux)
- Support de la virtualisation imbriquée
- Interface graphique intuitive
- Snapshots et clones
- Outils VMware Tools pour l'intégration hôte/invité
- Support de multiples systèmes d'exploitation invités

## 🎯 Objectifs de ce JOB

- Installer VMware Workstation
- Configurer la virtualisation imbriquée
- Créer une machine virtuelle Debian
- Configurer le réseau et les ressources
- Installer VMware Tools

## 💻 Prérequis

### Matériel

- Processeur 64-bit avec support VT-x/AMD-V
- 4 GB RAM minimum (8 GB recommandé)
- 50 GB d'espace disque disponible
- Windows 10/11 ou Linux

### Logiciel

- Windows 10/11 Pro/Enterprise ou distribution Linux compatible
- Droits administrateur
- ISO Debian (à télécharger)
- VMware Workstation Pro ou Player

### Téléchargements

- **VMware Workstation Pro** : [Site officiel VMware](https://www.vmware.com/products/workstation-pro.html)
- **VMware Workstation Player** : [Site officiel VMware](https://www.vmware.com/products/workstation-player.html) (gratuit pour usage personnel)
- **Debian ISO** : [debian.org](https://www.debian.org/distrib/netinst)

## 📥 Installation de VMware Workstation

### Installation sur Windows

1. **Télécharger l'installateur**
   ```
   VMware-workstation-full-xx.x.x-xxxxxx.exe
   ```

2. **Lancer l'installation**
   - Double-cliquer sur l'exécutable
   - Accepter les termes de la licence
   - Choisir le répertoire d'installation (par défaut : `C:\Program Files (x86)\VMware\VMware Workstation\`)

3. **Options d'installation**
   - ✅ Enhanced Keyboard Driver (recommandé)
   - ✅ Add VMware Workstation to System PATH
   - Créer des raccourcis selon préférence

4. **Entrer la clé de licence** (pour la version Pro)
   - Ou utiliser la version d'évaluation

5. **Redémarrer le système** si demandé

### Installation sur Linux

```bash
# Ubuntu/Debian
sudo apt update
sudo apt install build-essential linux-headers-$(uname -r)

# Rendre le fichier exécutable
chmod +x VMware-Workstation-Full-xx.x.x-xxxxxx.bundle

# Installer
sudo ./VMware-Workstation-Full-xx.x.x-xxxxxx.bundle

# Lancer VMware
vmware &
```

### Vérification de l'installation

```bash
# Windows (PowerShell)
& "C:\Program Files (x86)\VMware\VMware Workstation\vmware.exe" -v

# Linux
vmware --version
```

## ⚙️ Configuration initiale

### Paramètres de l'application

1. **Ouvrir VMware Workstation**

2. **Aller dans Edit → Preferences**

3. **Onglet Workspace**
   - Default location for virtual machines : Choisir un emplacement approprié
   - ✅ Keep VMs running after Workstation closes (optionnel)

4. **Onglet Memory**
   - Reserved memory : Réserver suffisamment pour l'hôte (recommandé : 4-8 GB)
   - Maximum recommended memory : Utiliser le curseur

5. **Onglet USB**
   - ✅ Show all USB input devices (si nécessaire)

### Configuration réseau virtuel

1. **Accéder à Virtual Network Editor**
   - Edit → Virtual Network Editor
   - Cliquer sur "Change Settings" (droits admin requis)

2. **Réseaux par défaut**
   - **VMnet0** : Bridged (Pont)
   - **VMnet1** : Host-only
   - **VMnet8** : NAT

3. **Configuration NAT (VMnet8)**
   ```
   Subnet IP : 192.168.100.0
   Subnet mask : 255.255.255.0
   Gateway : 192.168.100.2
   ```

4. **DHCP Settings** pour VMnet8
   ```
   Start IP : 192.168.100.128
   End IP : 192.168.100.254
   Lease time : 1800 seconds (30 min)
   ```

## 🖥️ Création de la machine virtuelle Debian

### Étape 1 : Assistant de création

1. **File → New Virtual Machine**
   - Choisir "Custom (advanced)" pour plus de contrôle

2. **Hardware Compatibility**
   - Sélectionner la version la plus récente (ex: Workstation 17.x)

3. **Guest Operating System Installation**
   - ⚪ Installer disc image file (iso)
   - Parcourir et sélectionner `debian-xx.x.x-amd64-netinst.iso`

4. **Guest Operating System**
   - ⚪ Linux
   - Version : Debian 11.x 64-bit (ou version appropriée)

5. **Name the Virtual Machine**
   ```
   Virtual machine name : Debian-VMware-Lab
   Location : C:\Virtual Machines\Debian-VMware-Lab
   ```

### Étape 2 : Configuration des ressources

1. **Processor Configuration**
   ```
   Number of processors : 1
   Number of cores per processor : 2
   Total : 2 cores
   ```
   - ✅ Virtualize Intel VT-x/EPT or AMD-V/RVI (important pour nested)

2. **Memory for the Virtual Machine**
   ```
   Memory : 4096 MB (4 GB)
   ```

3. **Network Type**
   - ⚪ Use network address translation (NAT)
   - Recommandé pour commencer

4. **I/O Controller Types**
   - ⚪ LSI Logic (recommended)

5. **Disk Type**
   - ⚪ SCSI (Recommended)

6. **Disk**
   - ⚪ Create a new virtual disk

7. **Disk Size**
   ```
   Maximum disk size : 20 GB
   ⚪ Store virtual disk as a single file
   ```

### Étape 3 : Options avancées

1. **Éditer les paramètres de la VM** (avant de démarrer)
   - Clic droit sur la VM → Settings

2. **Options → General**
   - Guest OS : Vérifier Linux / Debian 11.x 64-bit

3. **Options → Advanced**
   - ✅ Enable logging (pour le débogage)
   - UEFI ou Legacy BIOS selon préférence

4. **Hardware → Processors**
   - ✅ Virtualize Intel VT-x/EPT or AMD-V/RVI
   - ✅ Virtualize IOMMU (Intel VT-d/AMD-Vi) (si disponible)

5. **Hardware → Display**
   ```
   Specify monitor settings:
   - Use host settings for monitors
   Graphics memory : Automatic
   ✅ Accelerate 3D graphics (optionnel)
   ```

## 🚀 Installation de Debian

### Démarrage de l'installation

1. **Démarrer la VM**
   - Power On This Virtual Machine

2. **Boot menu de Debian**
   - Sélectionner "Graphical install" ou "Install"

### Configuration de l'installation

1. **Language**
   - Choisir votre langue (ex: French - Français)

2. **Location**
   - Choisir votre pays/région

3. **Keyboard**
   - Configuration du clavier (ex: French)

4. **Network Configuration**
   ```
   Hostname : debian-vmware
   Domain name : (laisser vide ou local)
   ```

5. **Users and Passwords**
   ```
   Root password : [mot_de_passe_sécurisé]
   Full name : Nexus Admin
   Username : nexus
   Password : [mot_de_passe_utilisateur]
   ```

6. **Partition Disks**
   - ⚪ Guided - use entire disk
   - Sélectionner le disque virtuel (20 GB)
   - All files in one partition (simple)
   - Finish partitioning → Yes

7. **Software Selection**
   ```
   ✅ Debian desktop environment (optionnel)
   ✅ GNOME (ou autre DE)
   ✅ SSH server (recommandé)
   ✅ Standard system utilities
   ```

8. **GRUB Boot Loader**
   - Yes, installer GRUB
   - Sélectionner `/dev/sda`

### Post-installation

1. **Premier démarrage**
   - Retirer l'ISO (VM → Settings → CD/DVD → Disconnect)
   - Redémarrer la VM

2. **Login**
   - Se connecter avec les identifiants créés

## 🛠️ Installation de VMware Tools

VMware Tools améliore les performances et l'intégration hôte-invité.

### Méthode 1 : Packages open-vm-tools (recommandé)

```bash
# Se connecter en tant que root ou utiliser sudo
su -

# Mettre à jour les dépôts
apt update

# Installer open-vm-tools
apt install -y open-vm-tools

# Avec environnement graphique
apt install -y open-vm-tools-desktop

# Redémarrer pour appliquer
reboot
```

### Méthode 2 : VMware Tools officiel

1. **Dans VMware Workstation**
   - VM → Install VMware Tools

2. **Dans Debian**
   ```bash
   # Monter le CD VMware Tools
   sudo mkdir /mnt/cdrom
   sudo mount /dev/cdrom /mnt/cdrom
   
   # Copier l'archive
   cd /tmp
   cp /mnt/cdrom/VMwareTools-*.tar.gz .
   
   # Extraire
   tar -xzf VMwareTools-*.tar.gz
   
   # Installer
   cd vmware-tools-distrib
   sudo ./vmware-install.pl
   
   # Suivre les instructions (généralement accepter les valeurs par défaut)
   ```

### Vérification

```bash
# Vérifier le service VMware Tools
systemctl status vmtoolsd

# Afficher les informations
vmware-toolbox-cmd -v
```

## 🌐 Configuration réseau

### Configuration réseau basique

```bash
# Vérifier les interfaces réseau
ip addr show

# Configuration IP (si DHCP ne fonctionne pas)
sudo nano /etc/network/interfaces

# Exemple de configuration statique
auto ens33
iface ens33 inet static
    address 192.168.100.50
    netmask 255.255.255.0
    gateway 192.168.100.2
    dns-nameservers 8.8.8.8 8.8.4.4

# Redémarrer le réseau
sudo systemctl restart networking
```

### Test de connectivité

```bash
# Ping gateway
ping -c 4 192.168.100.2

# Ping Internet
ping -c 4 8.8.8.8

# Test DNS
ping -c 4 google.com

# Vérifier les routes
ip route show
```

## 📸 Snapshots et gestion

### Créer un snapshot

1. **VM → Snapshot → Take Snapshot**
   ```
   Name : Debian-Fresh-Install
   Description : Clean Debian installation with VMware Tools
   ```

2. **Ou utiliser le Snapshot Manager**
   - VM → Snapshot → Snapshot Manager

### Cloner une VM

1. **VM → Manage → Clone**
2. **Choisir l'état** : Current state
3. **Type de clone** : Full clone ou Linked clone
4. **Nom** : Debian-VMware-Clone

## 🔧 Optimisations et ajustements

### Paramètres de performance

```bash
# Désactiver les services inutiles
sudo systemctl disable bluetooth
sudo systemctl disable cups

# Optimiser les mises à jour
sudo apt update && sudo apt upgrade -y
sudo apt autoremove -y
```

### Configuration avancée de la VM

**Fichier .vmx** (éditer avec précaution) :
```
# Activer la virtualisation imbriquée
vhv.enable = "TRUE"

# Améliorer les performances
mainMem.useNamedFile = "FALSE"
sched.mem.pshare.enable = "FALSE"

# Performance du disque
scsi0.virtualDev = "lsisas1068"
```

## ✅ Tests de validation

### Checklist de vérification

- [ ] VMware Workstation installé et fonctionnel
- [ ] VM Debian créée avec 2 vCPUs, 4 GB RAM
- [ ] Virtualisation imbriquée activée
- [ ] Debian installé et opérationnel
- [ ] VMware Tools installé et fonctionnel
- [ ] Réseau configuré (ping vers Internet)
- [ ] SSH fonctionnel (si installé)
- [ ] Snapshot initial créé

### Tests fonctionnels

```bash
# Vérifier le CPU et la virtualisation
lscpu | grep Virtualization

# Vérifier la RAM
free -h

# Vérifier le disque
df -h

# Vérifier le réseau
ip addr show
ping -c 4 google.com

# Vérifier VMware Tools
vmware-toolbox-cmd -v
```

## 🐛 Dépannage

### Problèmes courants

**La VM ne démarre pas**
- Vérifier que la virtualisation est activée dans le BIOS
- Vérifier les logs : `C:\Users\[user]\Documents\Virtual Machines\[VM]\vmware.log`

**Pas de réseau dans la VM**
- Vérifier VMnet8 dans Virtual Network Editor
- Redémarrer les services VMware :
  ```
  net stop VMwareHostd
  net start VMwareHostd
  ```

**Performances médiocres**
- Allouer plus de RAM
- Utiliser un SSD pour le stockage des VMs
- Désactiver les antivirus pour le dossier des VMs

**VMware Tools ne s'installe pas**
- Utiliser open-vm-tools à la place
- Vérifier les dépendances : `apt install build-essential linux-headers-$(uname -r)`

## 📚 Ressources supplémentaires

- [VMware Workstation Documentation](https://docs.vmware.com/en/VMware-Workstation-Pro/)
- [Debian Installation Guide](https://www.debian.org/releases/stable/amd64/)
- [VMware Tools Guide](https://docs.vmware.com/en/VMware-Tools/)

## 🚀 Prochaines étapes

Vous avez maintenant une VM Debian fonctionnelle sur VMware Workstation ! Continuez avec les autres hyperviseurs :

- [JOB 4 : Configuration Hyper-V](./JOB4_hyperv.md)

---

[← JOB 2 : Architecture](./JOB2_architecture.md) | [Retour au README](../README.md) | [JOB 4 : Hyper-V →](./JOB4_hyperv.md)
