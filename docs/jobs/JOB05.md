# JOB 05 – Installation de VMware ESXi en machine virtuelle

## Objectif

L’objectif de ce job est d’installer et de configurer **VMware ESXi 7** en tant qu’**hyperviseur de type 1 exécuté en environnement imbriqué** dans VMware Workstation Pro, puis d’y déployer une machine virtuelle Debian.

---

## Contexte

VMware ESXi est un hyperviseur bare-metal largement utilisé en production.
Dans le cadre de ce projet, ESXi est installé **à l’intérieur d’une machine virtuelle**, ce qui permet de tester son fonctionnement et ses limites en virtualisation imbriquée.

---

## Architecture / Principe

* Machine physique hôte
* Hyperviseur de type 2 : VMware Workstation Pro
* Machine virtuelle : VMware ESXi 7
* Hyperviseur imbriqué : ESXi
* Machine virtuelle invitée : Debian GNU/Linux

---

## Prérequis

* VMware Workstation Pro installé
* Image ISO de VMware ESXi 7
* Image ISO Debian GNU/Linux
* Virtualisation matérielle activée sur la machine hôte

---

## Étapes de réalisation

### 1. Création de la machine virtuelle ESXi

1. Ouvrir **VMware Workstation Pro**.
2. Cliquer sur **Créer une nouvelle machine virtuelle**.
3. Sélectionner **Personnalisé**, puis cliquer sur **Suivant**.
4. À l’étape **Compatibilité**, choisir **ESXi 7.0**, puis **Suivant**.
5. Sélectionner **Installer le système d’exploitation ultérieurement**, puis **Suivant**.
6. Choisir :

   * Système d’exploitation invité : **VMware ESX**
   * Version : **VMware ESXi 7**
7. Sélectionner l’emplacement de stockage de la machine virtuelle.
8. Type de microprogramme : **UEFI**.
9. Configuration matérielle :

   * Processeurs : **4**
   * Mémoire : **8000 Mo**
10. Réseau : **Utiliser la traduction d’adresses réseau (NAT)**.
11. Contrôleur disque :

* **SCSI paravirtualisé**

12. Type de disque : **SCSI**
13. Créer un nouveau disque virtuel.
14. Taille maximale du disque : **170 Go**.
15. Choisir **Stocker la machine virtuelle en tant que fichier unique (meilleures performances)**.
16. Finaliser la création de la machine virtuelle.

---

### 2. Configuration des paramètres de la machine virtuelle

1. Éteindre la machine virtuelle ESXi.
2. Clic droit sur la VM → **Paramètres**.

#### Carte réseau

* Type : **Personnalisé : réseau virtuel spécifique**
* Réseau : **VMnet1 (hôte uniquement)**
* Cocher **Se connecter lors de la mise en tension**

#### Lecteur CD/DVD (IDE)

* Sélectionner **Utiliser le fichier image ISO**
* Choisir l’ISO de **VMware ESXi**
* Cocher **Se connecter lors de la mise en tension**

#### Processeurs

* Activer **Virtualiser Intel VT-x/EPT ou AMD-V/RVI**

3. Valider les paramètres.

---

### 3. Installation de VMware ESXi

1. Démarrer la machine virtuelle ESXi.
2. À l’écran d’accueil :

   * *“VMware ESXi 7.0.3 installs on most systems…”* → **Continue**
3. Accepter le contrat de licence (**Accept and Continue**).
4. Attendre la phase de **scanning**.
5. Sélectionner le disque pour l’installation → **Continue**.
6. Choisir le clavier : **French** → **Continue**.
7. Définir un **mot de passe root** → **Continue**.
8. Confirmer l’installation → **Install**.
9. Attendre la fin de l’installation.
10. À l’écran *“Installation Complete”*, sélectionner **Reboot**.

La machine virtuelle redémarre avec ESXi installé.


![VMware ESXi Installation](C:\Users\P\Documents\lptf\Projets\20 - Opération Nexus Virtualis\Documentation_github\Nexus_Virtualis\images\jobs\job05\JOB5.1_ESXi fonctionnel 1.png)


---

### 4. Vérification de la configuration réseau

1. Vérifier que la machine virtuelle ESXi se trouve dans le **même sous-réseau que la machine hôte**.
2. Sur la machine hôte :

   * Ouvrir **PowerShell**
   * Exécuter la commande :

     ```
     ipconfig
     ```
3. Comparer l’adresse IP de l’hôte avec celle affichée sur la VM ESXi.
4. Si nécessaire, configurer l’adresse IP de la VM ESXi manuellement.

---

### 5. Accès à l’interface web ESXi

1. Ouvrir un navigateur web sur la machine hôte.
2. Saisir l’adresse IP de la machine virtuelle ESXi.
3. Se connecter avec :

   * Identifiant : **root**
   * Mot de passe : celui défini lors de l’installation


![VMware ESXi Web Interface](C:\Users\P\Documents\lptf\Projets\20 - Opération Nexus Virtualis\Documentation_github\Nexus_Virtualis\images\jobs\job05\JOB5.2_ESXi fonctionnel 2.png)


L’interface graphique web de VMware ESXi est accessible.

---

### 6. Création d’une machine virtuelle Debian sur ESXi

1. Dans l’interface ESXi, ouvrir **Machines virtuelles**.
2. Cliquer sur **Créer / Enregistrer une machine virtuelle**.
3. Choisir **Créer une machine virtuelle**, puis **Suivant**.
4. Renseigner :

   * Nom de la VM
   * Famille du système d’exploitation invité : **Linux**
   * Version du SE invité : **Debian GNU/Linux 11 (64 bits)**
5. Sélectionner un stockage, puis **Suivant**.
6. Personnaliser les paramètres :

   * Définir le nombre de CPU, la mémoire vive et la taille du disque
   * Contrôleur SCSI 0 : **LSI Logic Parallel**
   * Adaptateur réseau 1 :

     * Type : **E1000e**
   * Lecteur CD/DVD 1 :

     * Fichier ISO depuis la banque de données
     * Télécharger l’ISO Debian
     * Sélectionner l’image
     * État : **Connecter lors de la mise sous tension**
7. Cliquer sur **Suivant**, puis **Terminer**.

La machine virtuelle Debian est créée.


![VMware ESXi Debian VM](C:\Users\P\Documents\lptf\Projets\20 - Opération Nexus Virtualis\Documentation_github\Nexus_Virtualis\images\jobs\job05\JOB5.3_VM Debian crée.png)


---

### 7. Démarrage de la machine virtuelle Debian

1. Sélectionner la machine virtuelle Debian.
2. Cliquer sur **Mettre sous tension**.
3. La VM démarre et l’installation de Debian commence.


![VMware ESXi Debian Installation](C:\Users\P\Documents\lptf\Projets\20 - Opération Nexus Virtualis\Documentation_github\Nexus_Virtualis\images\jobs\job05\JOB5.4_ESXi complété Debian fonctionnel.png)