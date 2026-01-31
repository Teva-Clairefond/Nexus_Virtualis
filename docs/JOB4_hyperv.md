# JOB 4 : Configuration Microsoft Hyper-V

## 📖 Présentation

Microsoft Hyper-V est un hyperviseur de type 1 (bare-metal) qui s'intègre directement au noyau Windows. Il est inclus dans les éditions Pro, Enterprise et Education de Windows 10/11, ainsi que dans Windows Server.

### Caractéristiques principales

- Hyperviseur Type 1 intégré à Windows
- Support de la virtualisation imbriquée (depuis Windows 10/Server 2016)
- Gestion via Hyper-V Manager et PowerShell
- Intégration native avec Windows
- Live Migration et Réplication (éditions Server)
- Checkpoints (snapshots)

## 🎯 Objectifs de ce JOB

- Activer Hyper-V sur Windows
- Configurer le Virtual Switch
- Créer une machine virtuelle Debian
- Configurer la virtualisation imbriquée
- Installer Integration Services

## 💻 Prérequis

### Matériel

- Processeur 64-bit avec SLAT (Second Level Address Translation)
- 4 GB RAM minimum (8 GB recommandé)
- 50 GB d'espace disque disponible
- Support de la virtualisation (Intel VT-x ou AMD-V)

### Logiciel

- Windows 10/11 Pro, Enterprise ou Education (64-bit)
- Ou Windows Server 2016/2019/2022
- Droits administrateur
- ISO Debian

### Vérification de la compatibilité

```powershell
# Vérifier le support Hyper-V
systeminfo | findstr /C:"Hyper-V"

# Ou utiliser
Get-ComputerInfo -Property "Hyper*"
```

**Résultat attendu** :
```
Hyper-V Requirements:      VM Monitor Mode Extensions: Yes
                          Virtualization Enabled In Firmware: Yes
                          Second Level Address Translation: Yes
                          Data Execution Prevention Available: Yes
```

## 📥 Installation et activation de Hyper-V

### Méthode 1 : Via l'interface graphique

1. **Ouvrir le Panneau de configuration**
   - Rechercher "Turn Windows features on or off"
   - Ou : Control Panel → Programs → Programs and Features → Turn Windows features on or off

2. **Activer Hyper-V**
   ```
   ✅ Hyper-V
       ✅ Hyper-V Management Tools
           ✅ Hyper-V GUI Management Tools
           ✅ Hyper-V Module for Windows PowerShell
       ✅ Hyper-V Platform
           ✅ Hyper-V Hypervisor
           ✅ Hyper-V Services
   ```

3. **Redémarrer le système**

### Méthode 2 : Via PowerShell (recommandé)

```powershell
# Ouvrir PowerShell en tant qu'administrateur

# Activer Hyper-V
Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All

# Ou pour Windows Server
Install-WindowsFeature -Name Hyper-V -IncludeManagementTools -Restart
```

### Méthode 3 : Via DISM

```cmd
# Invite de commandes en tant qu'administrateur
DISM /Online /Enable-Feature /All /FeatureName:Microsoft-Hyper-V
```

### Vérification de l'installation

```powershell
# Vérifier les services Hyper-V
Get-Service vmms,vmcompute

# Vérifier les modules PowerShell
Get-Module -ListAvailable -Name Hyper-V

# Lancer Hyper-V Manager
virtmgmt.msc
```

## ⚙️ Configuration initiale

### Lancer Hyper-V Manager

1. **Ouvrir Hyper-V Manager**
   - Rechercher "Hyper-V Manager" dans le menu Démarrer
   - Ou exécuter : `virtmgmt.msc`

2. **Se connecter au serveur local**
   - Dans le panneau de gauche, sélectionner votre ordinateur

### Configuration des paramètres Hyper-V

1. **Hyper-V Settings** (serveur global)
   - Clic droit sur le serveur → Hyper-V Settings

2. **Virtual Hard Disks**
   ```
   C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks\
   ```
   - Modifier selon vos besoins (ex: `D:\Hyper-V\VHDs\`)

3. **Virtual Machines**
   ```
   C:\ProgramData\Microsoft\Windows\Hyper-V\
   ```
   - Modifier selon vos besoins (ex: `D:\Hyper-V\VMs\`)

4. **NUMA Spanning**
   - ✅ Allow virtual machines to span physical NUMA nodes (si applicable)

5. **Live Migration** (Server uniquement)
   - Configurer selon besoins

## 🌐 Configuration du réseau virtuel

### Créer un Virtual Switch

#### Via Hyper-V Manager

1. **Virtual Switch Manager**
   - Hyper-V Manager → Actions → Virtual Switch Manager

2. **Créer un nouveau switch**
   - Sélectionner le type :

   **External** (NAT/Bridged) :
   ```
   Name : External-vSwitch
   Connection type : External network
   Network adapter : [Votre carte réseau physique]
   ✅ Allow management operating system to share this network adapter
   ```

   **Internal** :
   ```
   Name : Internal-vSwitch
   Connection type : Internal network
   ```

   **Private** :
   ```
   Name : Private-vSwitch
   Connection type : Private network
   ```

#### Via PowerShell

```powershell
# Lister les adaptateurs réseau
Get-NetAdapter

# Créer un External Switch
New-VMSwitch -Name "External-vSwitch" -NetAdapterName "Ethernet" -AllowManagementOS $true

# Créer un Internal Switch
New-VMSwitch -Name "Internal-vSwitch" -SwitchType Internal

# Créer un Private Switch
New-VMSwitch -Name "Private-vSwitch" -SwitchType Private

# Vérifier
Get-VMSwitch
```

### Configuration NAT (pour Internal Switch)

```powershell
# Créer un Internal Switch
New-VMSwitch -Name "NAT-vSwitch" -SwitchType Internal

# Obtenir l'interface index
Get-NetAdapter "vEthernet (NAT-vSwitch)"

# Configurer l'IP du switch
New-NetIPAddress -IPAddress 192.168.110.1 -PrefixLength 24 -InterfaceAlias "vEthernet (NAT-vSwitch)"

# Créer le NAT
New-NetNat -Name "NAT-Network" -InternalIPInterfaceAddressPrefix 192.168.110.0/24

# Vérifier
Get-NetNat
```

## 🖥️ Création de la machine virtuelle Debian

### Méthode 1 : Via Hyper-V Manager

1. **New Virtual Machine Wizard**
   - Actions → New → Virtual Machine

2. **Specify Name and Location**
   ```
   Name : Debian-HyperV-Lab
   ✅ Store the virtual machine in a different location
   Location : D:\Hyper-V\VMs\Debian-HyperV-Lab\
   ```

3. **Specify Generation**
   - ⚪ Generation 2 (recommandé pour Linux moderne)
   - Note : Generation 1 pour compatibilité legacy

4. **Assign Memory**
   ```
   Startup memory : 4096 MB
   ✅ Use Dynamic Memory (optionnel)
   ```

5. **Configure Networking**
   ```
   Connection : External-vSwitch (ou NAT-vSwitch)
   ```

6. **Connect Virtual Hard Disk**
   ```
   ⚪ Create a virtual hard disk
   Name : Debian-HyperV-Lab.vhdx
   Location : D:\Hyper-V\VHDs\
   Size : 20 GB
   ```

7. **Installation Options**
   ```
   ⚪ Install an operating system from a bootable image file
   Image file (.iso) : [Chemin vers debian-xx.x.x-amd64-netinst.iso]
   ```

8. **Finish**

### Méthode 2 : Via PowerShell

```powershell
# Définir les variables
$VMName = "Debian-HyperV-Lab"
$VMPath = "D:\Hyper-V\VMs"
$VHDPath = "D:\Hyper-V\VHDs\$VMName.vhdx"
$ISOPath = "D:\ISOs\debian-12.5.0-amd64-netinst.iso"
$SwitchName = "External-vSwitch"

# Créer la VM
New-VM -Name $VMName `
    -MemoryStartupBytes 4GB `
    -Generation 2 `
    -NewVHDPath $VHDPath `
    -NewVHDSizeBytes 20GB `
    -Path $VMPath `
    -SwitchName $SwitchName

# Configurer le processeur
Set-VMProcessor -VMName $VMName -Count 2

# Configurer la mémoire dynamique
Set-VMMemory -VMName $VMName -DynamicMemoryEnabled $true -MinimumBytes 2GB -MaximumBytes 4GB

# Ajouter le DVD avec l'ISO
Add-VMDvdDrive -VMName $VMName -Path $ISOPath

# Configurer l'ordre de boot (Generation 2)
$dvd = Get-VMDvdDrive -VMName $VMName
Set-VMFirmware -VMName $VMName -FirstBootDevice $dvd

# Désactiver Secure Boot (nécessaire pour certaines distributions Linux)
Set-VMFirmware -VMName $VMName -EnableSecureBoot Off

# Activer la virtualisation imbriquée
Set-VMProcessor -VMName $VMName -ExposeVirtualizationExtensions $true

# Démarrer la VM
Start-VM -VMName $VMName
```

### Configuration avancée de la VM

```powershell
# Désactiver les checkpoints automatiques
Set-VM -Name $VMName -AutomaticCheckpointsEnabled $false

# Configurer le Smart Paging (si mémoire dynamique)
Set-VM -Name $VMName -SmartPagingFilePath "D:\Hyper-V\SmartPaging"

# Activer la mémoire dynamique avec priorité
Set-VMMemory -VMName $VMName -Priority 80

# Configuration des snapshots
Set-VM -Name $VMName -SnapshotFileLocation "D:\Hyper-V\Snapshots"
```

## 🚀 Installation de Debian

### Démarrage de l'installation

1. **Connecter à la VM**
   - Double-clic sur la VM dans Hyper-V Manager
   - Ou PowerShell : `vmconnect localhost $VMName`

2. **Démarrer la VM**
   - Start button dans la fenêtre de connexion

3. **Boot Debian Installer**
   - Sélectionner "Graphical install" ou "Install"

### Configuration de l'installation

Les étapes sont similaires à JOB 3, avec quelques spécificités :

1. **Hostname**
   ```
   Hostname : debian-hyperv
   Domain : local
   ```

2. **Partitionnement**
   - Guided - use entire disk
   - Tout dans une partition (simple)

3. **Software selection**
   ```
   ✅ SSH server
   ✅ Standard system utilities
   ⚪ Desktop environment (optionnel - GUI)
   ```

4. **Installation de GRUB**
   - Yes
   - `/dev/sda` (Generation 1) ou `/dev/sda` (Generation 2)

### Post-installation pour Generation 2

Pour les VMs Generation 2, Debian peut avoir besoin d'ajustements :

```bash
# Se connecter en root

# Installer les paquets nécessaires
apt update
apt install -y linux-image-amd64 grub-efi-amd64

# Réinstaller GRUB (si nécessaire)
grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=debian

# Mettre à jour GRUB
update-grub

# Redémarrer
reboot
```

## 🛠️ Installation de Linux Integration Services

Les Integration Services modernes sont inclus dans le noyau Linux (à partir de Debian 8+).

### Vérification

```bash
# Vérifier les modules hv_*
lsmod | grep hv_

# Devrait afficher :
# hv_vmbus
# hv_storvsc
# hv_netvsc
# hv_utils
```

### Installation manuelle (si nécessaire)

```bash
# Installer les outils Hyper-V
sudo apt update
sudo apt install -y hyperv-daemons

# Activer et démarrer les services
sudo systemctl enable hv-kvp-daemon
sudo systemctl enable hv-vss-daemon
sudo systemctl start hv-kvp-daemon
sudo systemctl start hv-vss-daemon

# Vérifier
sudo systemctl status hv-*
```

## ⚡ Configuration de la virtualisation imbriquée

### Activation pour une VM

```powershell
# Arrêter la VM si elle est en cours d'exécution
Stop-VM -Name $VMName

# Activer la virtualisation imbriquée
Set-VMProcessor -VMName $VMName -ExposeVirtualizationExtensions $true

# Désactiver la mémoire dynamique (recommandé pour nested)
Set-VMMemory -VMName $VMName -DynamicMemoryEnabled $false

# Configurer MAC spoofing (si besoin de réseaux complexes)
Get-VMNetworkAdapter -VMName $VMName | Set-VMNetworkAdapter -MacAddressSpoofing On

# Redémarrer la VM
Start-VM -Name $VMName
```

### Vérification dans Debian

```bash
# Vérifier le support de virtualisation
egrep -o '(vmx|svm)' /proc/cpuinfo

# Si vmx ou svm apparaît, la virtualisation imbriquée est active

# Vérifier avec lscpu
lscpu | grep Virtualization
```

## 🌐 Configuration réseau avancée

### Configuration IP statique

```bash
# Identifier l'interface
ip addr show

# Éditer la configuration réseau (Debian 11+)
sudo nano /etc/network/interfaces

# Exemple :
auto eth0
iface eth0 inet static
    address 192.168.110.50
    netmask 255.255.255.0
    gateway 192.168.110.1
    dns-nameservers 8.8.8.8 8.8.4.4

# Redémarrer le réseau
sudo systemctl restart networking
```

### Avec netplan (Ubuntu-based)

```bash
# Éditer le fichier netplan
sudo nano /etc/netplan/00-installer-config.yaml

# Exemple :
network:
  version: 2
  ethernets:
    eth0:
      addresses:
        - 192.168.110.50/24
      gateway4: 192.168.110.1
      nameservers:
        addresses:
          - 8.8.8.8
          - 8.8.4.4

# Appliquer
sudo netplan apply
```

## 📸 Checkpoints (Snapshots)

### Via Hyper-V Manager

1. **Créer un checkpoint**
   - Clic droit sur la VM → Checkpoint

2. **Nommer le checkpoint**
   ```
   Name : Debian-Fresh-Install
   ```

### Via PowerShell

```powershell
# Créer un checkpoint
Checkpoint-VM -Name $VMName -SnapshotName "Debian-Fresh-Install"

# Lister les checkpoints
Get-VMSnapshot -VMName $VMName

# Restaurer un checkpoint
Restore-VMSnapshot -Name "Debian-Fresh-Install" -VMName $VMName -Confirm:$false

# Supprimer un checkpoint
Remove-VMSnapshot -VMName $VMName -Name "Debian-Fresh-Install"
```

## 🔧 Optimisations

### Améliorer les performances

```powershell
# Désactiver les checkpoints automatiques (pas utiles pour labs)
Set-VM -Name $VMName -AutomaticCheckpointsEnabled $false

# Configuration du NUMA (si applicable)
Set-VMMemory -VMName $VMName -MaximumAmountPerNumaNode 4GB

# Smart Paging
Set-VM -Name $VMName -SmartPagingFilePath "D:\Hyper-V\SmartPaging"

# Priorité du processeur
Set-VMProcessor -VMName $VMName -RelativeWeight 100
```

### Dans Debian

```bash
# Installer les outils de performance
sudo apt install -y linux-tools-$(uname -r)

# Désactiver les services inutiles
sudo systemctl disable bluetooth
```

## ✅ Tests de validation

### Checklist de vérification

- [ ] Hyper-V activé et fonctionnel
- [ ] Virtual Switch créé et configuré
- [ ] VM Debian créée (Generation 2, 2 vCPUs, 4 GB RAM)
- [ ] Virtualisation imbriquée activée
- [ ] Debian installé et opérationnel
- [ ] Integration Services fonctionnels
- [ ] Réseau configuré (ping vers Internet)
- [ ] Checkpoint initial créé

### Tests PowerShell

```powershell
# Vérifier l'état de la VM
Get-VM -Name $VMName | Select Name, State, CPUUsage, MemoryAssigned

# Vérifier la virtualisation imbriquée
Get-VMProcessor -VMName $VMName | Select VMName, ExposeVirtualizationExtensions

# Vérifier le réseau
Get-VMNetworkAdapter -VMName $VMName | Select VMName, SwitchName, IPAddresses

# Statistiques
Measure-VM -Name $VMName
```

## 🐛 Dépannage

### Problèmes courants

**Hyper-V ne s'active pas**
- Vérifier le support matériel : `systeminfo | findstr /C:"Hyper-V"`
- Désactiver Device Guard/Credential Guard si nécessaire
- Vérifier qu'aucun autre hyperviseur n'est installé (VirtualBox, VMware)

**VM Generation 2 ne boot pas**
- Désactiver Secure Boot : `Set-VMFirmware -VMName $VMName -EnableSecureBoot Off`
- Vérifier l'ordre de boot

**Pas de réseau**
- Vérifier le Virtual Switch
- S'assurer que le firewall n'bloque pas
- Pour NAT, vérifier `Get-NetNat`

**Performances médiocres**
- Désactiver la mémoire dynamique pour la virtualisation imbriquée
- Allouer plus de ressources CPU/RAM
- Utiliser des disques SSD

## 📚 Ressources supplémentaires

- [Hyper-V Documentation Microsoft](https://docs.microsoft.com/virtualization/hyper-v-on-windows/)
- [Nested Virtualization Guide](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization)
- [PowerShell Hyper-V Cmdlets](https://docs.microsoft.com/powershell/module/hyper-v/)

## 🚀 Prochaines étapes

Vous avez maintenant une VM Debian fonctionnelle sur Hyper-V ! Continuez avec :

- [JOB 5 : Configuration ESXi](./JOB5_esxi.md)

---

[← JOB 3 : VMware Workstation](./JOB3_vmware_workstation.md) | [Retour au README](../README.md) | [JOB 5 : ESXi →](./JOB5_esxi.md)
