# PROJECT BORN2BEROOT FROM 42
By chdonnat (Christophe Donnat from 42 Perpignan, France)

![Born 2 be Root](https://m.media-amazon.com/images/I/51wwQPmkhCL._AC_UF1000,1000_QL80_.jpg)

There is quite a lot of documentation available online for this project. Therefore, I decided to create a guide with the following specifics:
- A guide in French
- That explains the fundamental concepts
- And that addresses the specifics of the project for the ARM 64 architecture (in my case, on a MacBook with an M1 chip)

## AIM OF THE PROJECT
e goal of this project is to create a virtual machine using VirtualBox (or UTM)
in order to install a Debian server while adhering to a set of specific rules related to the topic.

## BONUS
Bonus list:
- Properly set up partitions to achieve a structure similar to the example provided in the project description.
- Set up a functional WordPress website with services such as lighttpd, MariaDB, and PHP.
- Set up a service that you find useful (NGINX/Apache2 excluded!). During the defense, you will need to justify your choice.

**Note:** For this project, I only completed the first bonus.

# GUIDE POUR LE PROJET BORN TO BE ROOT SUR MACBOOK

Si vous essayez de réaliser le projet Born to be root sur un Macbook (ou tout appareil équipé de puce Apple Silicon de type M1, M2, etc) en suivant les tutos disponibles sur internet, vous allez vous rendre compte que vous ne pouvez pas appliquer leurs instructions telles quelles.

Après avoir passé quelques jours à chercher à comprendre ce qui m’empêchait de suivre aveuglément ces tutos, j’ai accumulé quelques connaissances bien utiles que je me propose de vous partager.

Les spécificités imposées par les puces M1 n’étant finalement pas très nombreuses, ce guide, et surtout les informations qu’il contient, pourra vous être utiles même si vous réalisez le projet à partir d’un ordinateur windows ou autre.

Le but de ce guide n’est pas tant de vous donner une marche à suivre précise que de vous apporter les connaissances qui vous permettront de réaliser le projet par vous même en vous exposant:

- les étapes pour réaliser le projet
- les particularités dues aux Macbook et assimilés
- les connaissances nécessaires pour réaliser le projet
- les commandes linux employées dans le projet
- le tout en français (avec toutefois certains terme techniques traduits en anglais)

<aside>

### Avertissement

Je suis moi-même au début de mon cursus à 42, j’ai pu commettre des erreurs ou des imprécisions, n’hésitez pas le cas échéant à me contacter afin que je puisse corriger ce qui doit l’être.

</aside>

> **Rendons à Cesar…**
Ce guide n’a pas été écrit à partir de rien. C’est une francisation, un développement (émaillé de nombreuses explications et savoirs glanés de ci de là) et une adaptation aux quelques spécificités imposées par les puces Apple Silicon du guide de **bepoisson (guide** lui-même basé sur le guide de **mcombeau).**
> 

### A quoi sert une VM?

Les bénéfices d'une machine virtuelle (VM) sont nombreux et offrent plusieurs avantages, notamment :

1. **Isolation** : Les machines virtuelles sont isolées les unes des autres, ce qui permet de séparer différentes applications ou systèmes d'exploitation sur une même machine physique. Cela évite les conflits et les risques de sécurité entre les environnements.
2. **Utilisation optimisée des ressources** : Une machine physique peut exécuter plusieurs VMs en même temps, ce qui permet d'exploiter pleinement les ressources matérielles, comme le processeur, la mémoire et le stockage, et d'optimiser l'utilisation du serveur.
3. **Flexibilité et portabilité** : Les VMs sont facilement transférables d'un hôte à un autre. Par exemple, une VM peut être copiée, déplacée ou sauvegardée, ce qui facilite la gestion et la migration des systèmes.
4. **Sécurité** : L'isolement des VMs permet de réduire l'impact des vulnérabilités. Si une VM est compromise, elle n'affecte généralement pas les autres machines ou l'hôte physique.
5. **Tests et développement** : Les VMs permettent de créer des environnements de test et de développement indépendants du système principal. Il est facile de tester des applications, des mises à jour ou des configurations sans risquer d'endommager l'environnement de production.
6. **Snapshots et restauration** : Les VMs peuvent être prises en "snapshot" (instantanés), permettant de sauvegarder l'état d'une machine à un moment donné. En cas de problème, il est possible de restaurer l'état antérieur de la VM rapidement.
7. **Consolidation des serveurs** : En consolidant plusieurs serveurs sur une même machine physique, une entreprise peut réduire les coûts liés à l'achat et à la gestion de matériel supplémentaire.
8. **Gestion simplifiée** : Les VMs peuvent être gérées et automatisées à l'aide d'outils de virtualisation, facilitant la création, le déploiement et la gestion des systèmes.

En résumé, les machines virtuelles offrent une grande flexibilité, une meilleure gestion des ressources et une isolation accrue, tout en facilitant les processus de sauvegarde, de test et de déploiement.

# Installer Virtualbox et Debian

## Un problème d’Architecture

**L'architecture** en informatique désigne la structure et le design d'un système, qu'il soit matériel (comme un processeur) ou logiciel. Elle définit la manière dont les composants (CPU, mémoire, bus, etc.) interagissent et les instructions que l'appareil peut comprendre.

**AMD64:** L’architecture dominante pour les PC Windows moderne est l’architecture **x86-64** (aussi appelée **AMD 64**). Elle prend en charge les applications 64 bits, tout en restant compatible avec les anciennes applications 32 bits (x86).

**ARM64**: L'architecture **ARM64** (ou **AArch64**) est une architecture de processeur 64 bits développée par ARM. Elle est couramment utilisée dans les smartphones, tablettes, appareils IoT, et plus récemment dans les ordinateurs (comme les Mac avec puces M1/M2).

Certains processeurs ARM64 incluent un mode appelé AArch32 qui permet d’exécuter des instructions 32 bits pour des applications conçues pour l’architecture ARM (ARMv7 par exemple), mais les processeurs **Apple Silicon** (M1, M2), basés sur ARM64, ne prennent **pas en charge les instructions ARM 32 bits (AArch32)**, marquant une transition vers un environnement purement 64 bits.
La plupart des logiciels populaires bénéficient d’une version pour chaque architecture, mais ce n’est pas toujours le cas.

## VirtualBox

Télécharger et installer VirtualBox est la première étape**.** C’est ce qu’on appelle un **hyperviseur** (*hypervisor*): 

**Hyperviseur:** La fonction des hyperviseurs est de **créer et gérer des machines virtuelles** en permettant à plusieurs systèmes d'exploitation de s'exécuter simultanément sur une même machine physique, en isolant leurs ressources.

**UTM**: L’environnement Mac dispose d’un hyperviseur dédié: **UTM.** J’ai toutefois décidé d’utiliser VirtualBox malgré tout afin de coller au plus près des outils employés par la majorité des étudiants.

<aside>

Sur le site https://www.virtualbox.org/wiki/Downloads téléchargez la version [**macOS / Apple Silicon hosts](https://download.virtualbox.org/virtualbox/7.1.4/VirtualBox-7.1.4-165100-macOSArm64.dmg)** puis installez la sur votre machine.

</aside>

## Debian

Télécharger l’image ISO de Debian est l’étape suivante.

**Debian** est une **distribution Linux** (ou "distro") très populaire, connue pour sa stabilité, sa flexibilité et son large éventail de logiciels. Elle est l'une des distributions les plus anciennes et sert de base pour de nombreuses autres distributions, dont **Ubuntu**, **Linux Mint**, et **Raspbian**.

### **Quelles différences entre Debian et Rocky?**

Debian et Rocky Linux sont deux distributions Linux avec des objectifs différents. **Debian** est communautaire, polyvalente, et axée sur la liberté logicielle, avec un large support d'architectures et un vaste choix de logiciels. Elle convient aux serveurs, postes de travail, et projets personnels. **Rocky Linux**, en revanche, est orientée entreprise, conçue comme une alternative gratuite et stable à Red Hat Enterprise Linux (RHEL). Elle se concentre sur les environnements professionnels avec un support long terme et une compatibilité RHEL. Debian est idéale pour les projets flexibles, tandis que Rocky est parfaite pour les systèmes critiques en entreprise.

**RHEL** (Red Hat Enterprise Linux) est une distribution Linux commerciale développée par **Red Hat**, conçue pour les entreprises et les environnements professionnels. Elle est réputée pour sa stabilité, son support technique, et ses mises à jour de sécurité à long terme, ce qui la rend idéale pour les systèmes critiques comme les serveurs, les bases de données ou les applications métier.

### Noyau Linux et distribution Linux :

**Noyau Linux** : C'est la partie centrale du système d'exploitation qui gère les interactions entre le matériel et le logiciel. Il peut fonctionner seul, mais il ne constitue pas un système complet utilisable.

**Distribution Linux** : Une distribution comme Debian regroupe le noyau Linux avec tous les outils nécessaires (gestion de paquets, applications, interface graphique) pour rendre l'ordinateur fonctionnel et utilisable par l'utilisateur.

**Une** **image ISO** (ou **fichier ISO**) est une copie exacte d'un **disque optique** (comme un CD, DVD ou Blu-ray) sous forme de fichier informatique unique. Le terme **ISO** vient du format standard de l'International Organization for Standardization (ISO 9660), qui est utilisé pour organiser les fichiers sur des disques optiques. Il peut être monté directement sur un ordinateur ou gravé sur un disque physique. Une image ISO permet de distribuer des systèmes d'exploitation, des logiciels, ou des sauvegardes de manière pratique sans nécessiter un support physique.

**CD ou DVD?** La principale différence entre une **image ISO CD** et une **image ISO DVD** de **Debian** réside dans la quantité de logiciels inclus. L'**ISO CD** contient une version minimale du système Debian, avec seulement les composants essentiels pour démarrer l'installation, tandis que l'**ISO DVD** inclut une installation plus complète, avec plusieurs environnements de bureau et une gamme étendue de logiciels. En résumé, l'ISO CD est idéale pour une installation de base nécessitant une connexion Internet pour les paquets supplémentaires, tandis que l'ISO DVD permet une installation complète sans connexion Internet. Pour notre projet, l’image CD est suffisante pour peux qu’on ait une connexion internet pour télécharger les paquets supplémentaires.

<aside>

Sur le site de Debian https://www.debian.org/ il ne faut pas télécharger le fichier depuis la page d’accueil (car c’est une image ISO pour architecture AMD 64); il faut aller dans A*utre* *téléchargement* puis *Téléchargement depuis les miroirs* et sélectionner un miroir situé proche de chez vous (en France pour moi), puis choisir une image ISO arm64 de la dernière version stable de Debian (Debian 12.8.0 à l’heure où j’écris). Le version cd est suffisante, comme celle que vous trouverez à la dernière ligne de cette page: https://debian.obspm.fr/debian-cd/current/arm64/iso-cd/

</aside>

## Bios et UEFI

Le **BIOS** (Basic Input/Output System) et **UEFI** (Unified Extensible Firmware Interface) sont deux types de **firmware** (logiciels intégrés dans le matériel informatique) qui sont responsables du démarrage et de l'initialisation du système d'exploitation sur un ordinateur. Bien qu'ils remplissent une fonction similaire, il existe des différences importantes entre eux.

- **Le BIOS** est un ancien firmware utilisé pour démarrer un ordinateur. Le BIOS utilise le **MBR** pour gérer la table des partitions d'un disque dur. Il a une interface texte et est plus lent. Il est encore présent sur beaucoup de systèmes aujourd'hui, mais il est limité par son ancienneté et ses capacités.
- **Le UEFI** est un firmware moderne qui remplace le BIOS. L'UEFI utilise la **GPT** (GUID Partition Table) pour gérer les partitions d'un disque. L’UEFI permet d’avoir une interface graphique et un démarrage plus rapide, et inclut des fonctionnalités de sécurité avancées comme le **Secure Boot**. L'UEFI est compatible avec des systèmes modernes comme ceux à base de **processeurs ARM**, **MacBook avec processeurs M1**, et les architectures **64 bits**.

**Les Macbook M1** et suivants prennent en charge uniquement le UEFI contrairement aux PC Windows modernes qui sont compatibles avec BIOS et UEFI. 

Lorsque vous créez une machine virtuelle (VM) sur un MacBook M1, même pour une distribution Linux comme **Debian ARM64**, il est nécessaire de cocher l'option **"Enable EFI"** (activer l'EFI) dans les paramètres de la VM. Cela permet de simuler le démarrage d'un système compatible **UEFI**, ce qui est indispensable pour que la VM fonctionne correctement sur l'architecture ARM du MacBook M1.

Sans l'option **"Enable EFI"**, vous pourriez rencontrer des problèmes de démarrage, car la configuration **BIOS** (le mode de démarrage traditionnel) n'est pas compatible avec l'architecture du MacBook M1.

## Le partitionnement

Le **partitionnement** consiste à diviser un disque dur ou un SSD en plusieurs **partitions** distinctes, chacune agissant comme un disque virtuel. Cela permet d’organiser les données, d'installer plusieurs systèmes d'exploitation ou de mieux gérer l’espace de stockage en séparant les fichiers système, les données personnelles et les sauvegardes, par exemple. Chaque partition peut être formatée avec un système de fichiers différent et avoir sa propre table de partition (comme MBR ou GPT).

**Une table de partition** est une structure de données utilisée pour définir et organiser les partitions d'un disque dur ou d'un SSD. Elle contient des informations sur la manière dont un disque est divisé en sections appelées **partitions**, et chaque partition peut être utilisée pour un système de fichiers ou un autre type de stockage. Il existe principalement deux types de **tables de partitionnement** utilisées dans les systèmes informatiques :

- **MBR (Master Boot Record)** est un **ancien format** utilisé depuis les années 1980. Il est limité à **4 partitions primaires** (ou 3 primaires + 1 étendue). Il Supporte des **disques jusqu'à 2 To maximum.** Il est utilisé principalement avec le **BIOS** pour démarrer le système. MBR est principalement associé au BIOS pour des raisons historiques (bien qu’il puisse parfois être utilisé avec UEFI).
- **GPT (GUID Partition Table)** est un **format moderne** qui remplace le MBR. Il supporte **un nombre illimité de partitions** (généralement jusqu'à 128). Il prend en charge des **disques de plus de 2 To.** Il est utilisé avec **UEFI**, offrant des fonctionnalités avancées comme le **Secure Boot.** Il est plus fiable et sécurisé, notamment avec des **contrôles de redondance**.

### Partition, Chiffrement et Partition logique:

**Une partition** est une division logique d'un disque dur ou d'un SSD en plusieurs **segments**. Chaque partition peut être traitée comme un disque séparé, avec son propre système de fichiers (comme NTFS, ext4, etc.).

**Le chiffrement** est une méthode de sécurisation des données en les transformant en une forme illisible sans une **clé de déchiffrement**. Le chiffrement peut être appliqué à une partition, un disque entier, ou même à des fichiers individuels.

**Une partition logique** est une **partition interne** à une **partition étendue**. Dans le système de partitionnement **MBR**, une partition étendue est utilisée pour contourner la limitation de quatre partitions primaires. À l'intérieur de cette partition étendue, on peut créer plusieurs **partitions logiques**. Avec une table de partitionnement GPT, les partitions logiques sont inutiles puisqu’il n’y a pas de limite au nombre de partitions.

### LVM

**LVM (Logical Volume Manager)** est un outil utilisé principalement sous Linux pour gérer les disques et les volumes de manière plus flexible que les méthodes de partitionnement traditionnelles.

**Volumes logiques:** LVM permet de gérer des **volumes logiques** (à ne pas confondre avec les partitions logiques) qui sont des abstractions des disques physiques. LVM fonctionne au-dessus des **disques physiques** (ou des partitions physiques) et permet de créer des volumes plus flexibles, appelés **volumes logiques**, qui peuvent être redimensionnés et ajustés dynamiquement: Avec LVM, vous pouvez facilement **ajuster la taille** des volumes logiques, ajouter de nouveaux disques, ou en retirer sans perturber les données ou redémarrer le système. Un volume logique peut combiner plusieurs disque physiques ou partitions.

**Snapshots:** LVM permet aussi de créer des **snapshots**, c'est-à-dire des copies exactes de l'état d'un volume à un instant donné. Cela peut être très utile pour des sauvegardes ou des tests, car les snapshots permettent de revenir à un état précédent du volume.

### Les systèmes de fichier (*file* *system*)

Lorsque vous partitionnez un disque, vous devez choisir un **système de fichiers** pour chaque partition. Le système de fichiers détermine comment les données sont organisées, stockées et récupérées sur cette partition. Voici les principaux types de systèmes de fichiers :

- **FAT 32 (File Allocation Table 32):** Compatible avec presque tous les OS. Limitation : Taille maximale des fichiers = 4 Go, taille maximale de partition = 8 To. Usage courant : Clés USB, disques externes pour compatibilité multiplateforme.
- **NTFS (New Technology File System):** Utilisé par défaut sur Windows. Supporte de grands fichiers (> 4 Go) et des fonctionnalités avancées comme le chiffrement et les autorisations. **Usage courant** : Partitions pour Windows ou disques internes.
- **HFS+ / APFS (Apple File Systems):** Utilisés par macOS (APFS est plus récent, optimisé pour SSD). Non compatible nativement avec Windows. **Usage courant** : Partitions pour macOS.
- **ext4 (Fourth Extended File System):** Système de fichiers par défaut pour Linux. Fiable, rapide, et supporte de grands fichiers. **Usage courant** : Partitions pour les systèmes Linux.
- **Swap (Linux Swap Partition):** Utilisé par Linux pour étendre la mémoire vive (RAM) lorsqu'elle est insuffisante. Pas utilisé pour stocker des fichiers normaux. **Usage courant** : Partitions réservées au swap sous Linux.

Seuls les deux derniers types de système de fichiers (ceux pour Linux) seront utilisés dans le cadre du projet (en réalité le FAT 32 sera aussi utilisé par la partition EFS mais le type de système de fichier pour la partition EFS sera appliqué sans intervention de votre part).

## Création de la machine virtuelle

**Pour créer un nouvelle machine virtuelle** (*Virtual* *Machine* abrégé *VM*), il suffit de cliquer sur le bouton “Nouvelle”. Vous aurez alors accès à 4 onglets.

- **Name and operating system:** Ici vous devez nommer votre VM (obligatoire), puis vous pouvez changer le répertoire où elle sera enregistrée (*folder*) et enfin vous devez sélectionner l’image ISO (*ISO* *Image*) de Debian que vous avez téléchargé précédemment. Les champs “Type”, “Subtype” et “Version” seront alors remplis automatiquement. **Vous devez par contre cocher la case “Skip Unattended Installation”** afin d’empêcher l’installation automatisée.
- **Unattended Install:** Cet onglet sera grisé et inaccessible si vous avez bien coché la case idoine.
- **Hardware:** Vous devez sélectionner ici la mémoire vive (*RAM*) que vous voulez allouer à votre VM (attention cela réduira la RAM disponible pour la machine hôte); 1024 MB semble  suffisant pour le projet. Ensuite le nombre de coeurs CPU (*processors*) que vous voulez allouer à votre VM (ça marche très bien avec un seul). Enfin sur Macbook vous devez ensuite cocher la case “Enable EFI” pour les raisons vues plus haut.
- **Hard Disk:** Sélectionnez “Create a Virtual Hard Disk Now”; vous pouvez ensuite changer le répertoire de destination, puis vous devez choisir la taille du “disque dur” que vous souhaitez créer (15 Go semble largement suffisant, mais si vous voulez avoir des tailles de partitions semblables à celle montrées dans le sujet, prenez 30Go). Enfin vous devez sélectionner VDI (*Virtualbox Disk Image*) comme type de fichier pour disque dur virtuel (*Hard Disk File Type And Variant*). Ne cochez pas l’option “preallocate full size” afin d’avoir une allocation dynamique de la mémoire.

**VDI:** Dans VirtualBox, lors de la création ou de l'ajout d'un disque virtuel à une machine virtuelle (VM), vous devez choisir un **type de fichier de disque dur virtuel**. Ces fichiers représentent l'image du disque dur pour la VM, ils émulent un disque dur physique pour une VM. VDI est le format natif de Virtualbox.

**Allocation dynamique:** Sans l’option “pre allocate full size”, le fichier disque virtuel ne grandit que lorsque la VM y écrit des données. La partition interne dans la VM fonctionne de la même manière, mais moins d'espace est immédiatement consommé sur l'hôte. Avec l'option cochée, l'espace total est alloué tout de suite, mais cela ne change rien à la gestion des partitions à l'intérieur de la VM (cela signifie qu'un fichier disque virtuel de par exemple 30 Go sera immédiatement créé sur le disque dur de votre **ordinateur hôte**).

Vous pouvez ensuite cliquer sur “suivant” pour valider la création de la VM.

## Installation de Debian

**Pour lancer l’installation de Debian** sélectionnez votre VM dans Virtualbox et cliquez sur “démarrer”. Au lancement, dans le menu “view” de Virtualbox, sélectionnez “scale mode” puis cliquez sur “switch” afin de régler la fenêtre de l’affichage.

Dans la VM, sélectionner ensuite “installation” pour commencer l’installation de Debian.

Sélectionnez la langue, le fuseau horaire, puis la disposition du clavier (”français” pour tout dans mon cas).

**Nom de la machine:** Entrez le nom souhaité, c’est le *hostname* dont parle le sujet (votre identifiant suivi de “42”).

**Domaine:** Vous pouvez laisser ce champ vierge.

**Qu’est-ce qu’un "domaine" ?**

Un domaine est une partie de l'adresse réseau d'un ordinateur, généralement utilisée dans les environnements où plusieurs machines communiquent (comme des réseaux locaux ou internet). Par exemple : **Nom d’hôte (*hostame*)**: `monserveur` **Nom de domaine**: `exemple.com`**Nom complet (FQDN)**: `monserveur.exemple.com`

**Quand est-ce utile ?**

- **Réseaux locaux ou d'entreprise** : Si votre système fait partie d'un réseau plus grand (comme un réseau d’entreprise), le domaine aide à localiser votre machine. Par exemple, dans une entreprise, votre PC pourrait être : `pc-123.bureau.local`.
- **Serveurs accessibles publiquement** : Si vous voulez que votre serveur soit identifiable sur internet avec un nom de domaine comme `www.monsite.com`.

 Si vous ne spécifiez pas de domaine, Debian utilisera simplement le **nom de l’hôte** pour identifier la machine. Cela fonctionne parfaitement pour un usage domestique ou hors réseau.

Choisissez ensuite un mot de passe pour le superutilisateur “root”.

### **C’est quoi le superutilisateur root?**

Le **superutilisateur `root`** est le compte avec les privilèges les plus élevés sur un système d'exploitation basé sur Linux ou Unix (comme Debian). Il dispose d'un accès total au système, ce qui lui permet de lire, écrire, exécuter et modifier tous les fichiers, y compris ceux protégés. Ce compte est utilisé pour administrer et gérer le système, installer des logiciels, configurer les périphériques et modifier les paramètres système. Le compte root est généralement désigné par l'ID utilisateur (UID) 0, qui est unique et réservé pour ce rôle.

Le superutilisateur est responsable de la gestion des utilisateurs, notamment la création, modification ou suppression de comptes. Il s'occupe aussi de la maintenance du système, comme la réparation ou la reconfiguration en cas de problème. L'installation ou la mise à jour de logiciels, ainsi que la modification de fichiers systèmes critiques, fait aussi partie de ses tâches. Enfin, il gère la sauvegarde et la restauration des données ou des partitions.

Lorsqu'on est connecté en tant qu'utilisateur standard, il est possible de se connecter en tant que root en utilisant la commande `su root` suivie du mot de passe du superutilisateur.

**Nouvel utilisateur:** Vous devez ensuite choisir un nom complet pour la création du premier utilisateur comme défini dans le sujet (votre login de l’intra42), puis un identifiant pour cette utilisateur (vous pouvez laisser la même chose) et enfin choisir un mot de passe.

### Partition

Sélectionner le mode de partition “manuel” pour éviter que l’installateur ne crée automatiquement les partitions. Sélectionnez ensuite le disque sur lequel faire les partitions (VBOX HARDISK) pour créer une nouvelle table des partitions. Une ligne affichant l’espace libre sur le disque va alors apparaître. Il faut cliquer sur cette ligne pour créer la première partition. 

**Partition système EFI:** Créez une première partition (taille 512 MB suffisante), placez-la au début et  dans “utiliser comme” sélectionnez “Partition système EFI”. Cette partition n’apparaît pas dans les exemples donnés dans le sujet, mais elle est nécessaire puisqu’on utilise le mode UEFI pour démarrer le système d’exploitation. Il suffit ensuite de cliquer sur “fin du paramétrage de cette partition”.

### **Pourquoi j’ai une partition EFI en plus par rapport au sujet?**

Si vous avez bien suivi jusque là, vous aurez compris que les spécificité du Macbook et sa non-prise en charge du BIOS (ainsi peut-être que l’image ISO ARM64 de Debian) vous obligent à utiliser UEFI qui ne peut pas fonctionner sans cette partition EFI.

**Partition /boot:** Créez une seconde partition (taille 512 MB suffisante), dans “utiliser comme” sélectionnez “système de fichiers journalisés ext4” (puisque c’est le système de fichiers par défaut pour Linux), puis dans “point de montage” sélectionnez “/boot”, puis “fin du paramétrage de cette partition”.

### **Pourquoi j’ai des espaces libres de 1 MB ou similaires qui se créent ?**

Les espaces libres de **1 MB** (ou similaires) qui apparaissent lors de la création ou de la modification de partitions ne sont pas des erreurs. Ils sont intentionnels et remplissent des fonctions importantes liées à la compatibilité, l’alignement et l’efficacité.

Tout d'abord, ces espaces permettent d’aligner les partitions sur les blocs physiques des disques modernes, comme les SSD, qui utilisent des blocs de 4 KB ou plus. Cet alignement optimise les performances de lecture et d’écriture. Ensuite, dans le cadre de la table de partitionnement GPT, des espaces libres sont réservés au début et à la fin du disque pour stocker des métadonnées critiques et réduire les risques de conflits avec certains outils.

Ils servent également à assurer la compatibilité entre les systèmes BIOS et UEFI, en réservant un espace pour les en-têtes nécessaires à l’amorçage ou pour une partition bootable potentielle. Par ailleurs, les gestionnaires de partitions, comme GParted ou ceux intégrés à Windows et Linux, laissent parfois ces espaces libres pour anticiper des modifications futures, comme l’expansion ou la fusion de partitions. Enfin, certains systèmes de fichiers ou configurations spécifiques, comme LVM ou RAID, nécessitent ces marges pour leur bon fonctionnement.

**Configurer les volumes chiffrés:** Avant de créer les volumes logiques, il faut effectuer le chiffrement comme demandé dans le sujet. Pour cela il faut aller dans “Configurer les volumes chiffrés”, sélectionner l’espace libre restant sur le disque puis “fin du paramétrage de cette partition”, puis “terminer” et choisir une phrase de passe.

Un **volume chiffré** est une unité de stockage (comme une partition, un disque entier ou un fichier qui agit comme un disque virtuel) dont le contenu est protégé par un algorithme de **chiffrement**. Cela signifie que les données stockées dans ce volume ne peuvent être lues ou utilisées qu'avec une **clé de déchiffrement**, comme un mot de passe, une clé cryptographique, ou un fichier clé. Le **chiffrement AES** (Advanced Encryption Standard) est un algorithme de cryptographie symétrique utilisé pour sécuriser les données. C'est l'un des algorithmes de chiffrement les plus couramment utilisés aujourd'hui, car il est rapide, sûr et standardisé.

**Configurer le gestionnaire des volumes logiques:** Cliquer ensuite sur “Configurer le gestionnaire des volumes logiques”, puis “Créer un groupe de volume” (nommez-le “LVMGroup si vous voulez le nommer comme dans le sujet) et sélectionnez le volume crypté précédemment (qui doit s’appeler “sda3_crypt”). A l’intérieur de ce ce groupe, nous allons ensuite créer plusieurs volumes logiques.

**Pourquoi sur le sujet les partitions SDA3 et SDA4 n’existent pas alors qu’il y a SDA5?**

**Sous un système utilisant le partitionnement MBR (Master Boot Record)**, qui est souvent associé au BIOS, les partitions primaires sont limitées à **quatre** par disque. **SDA1 à SDA4** (ou leurs équivalents sur d'autres disques) sont réservées pour les **partitions primaires**. Ce sont les premières partitions créées sur le disque. Les partitions logiques sont numérotées à partir de **SDA5** et au-delà.

Avec **GPT (GUID Partition Table)**, il n'y a **plus de limitation stricte** sur les partitions primaires, contrairement à MBR. Toutes les partitions créées sur un disque GPT sont équivalentes. Il n'y a pas de notion de "partition primaire" ou "partition logique". Les partitions sur un disque GPT sont numérotées comme suit : **sda1, sda2, sda3**, etc., pour chaque partition créée. Il n'y a pas de limitation à **sda1-sda4** comme avec MBR.

**Créer les volumes logique** comme sur le sujet en cliquant sur “Créer un volume logique”, sélectionnez le groupe de volume “LVMGroup” précédemment créé, nommez le premier volume ainsi créé “root”, choisissez sa taille (par exemple 10G comme dans le sujet). Répétez l’opération pour les volumes logiques “swap”, “home, “var”, “srv”, “tmp”, “var-log” pour avoir les mêmes que la partie bonus du sujet. Cliquez sur “terminer” pour revenir à la table des partitions.

**Affecter les différents volumes:** Cliquez ensuite sur la ligne du premier volume (la ligne où apparait sa taille en MB ou GB) qui doit être le “home” (si vous les avez créé dans le même ordre que moi) et dans “utiliser comme” sélectionnez “système de fichiers journalisés ext4”, puis dans “point de montage” sélectionnez “home”, puis “fin du paramétrage de cette partition”.

Répétez l’opération pour chacun des volumes du groupe (”var” doit être monté sur “var”, “root” sur “/”, “srv”, sur “srv” etc) avec deux exceptions:

- Le volume swap: dans “utiliser comme” il faut choisir “espace d’échange (”swap”)” et vous n’aurez donc pas à choisir de point de montage.
- Le volume var-log: Après avoir choisi sont système de fichiers, il faut lui sélectionner le point de montage “autre choix” et saisir manuellement “/var/log”.

Une fois l’opération terminée, cliquez sur “Terminer les partitionements et appliquer les changements” et acceptez. Puis pour “Analyser les systèmes d’information” faites “non”

### **C’est quoi un point de montage?**

Un **point de montage** est un répertoire dans un système de fichiers où un périphérique de stockage (comme un disque dur, une clé USB, ou une partition) est "connecté" pour être accessible et utilisable par le système d'exploitation. Une fois qu'un périphérique est monté, ses fichiers peuvent être consultés comme s'ils faisaient partie du système de fichiers principal. Lorsqu'un périphérique est monté, son contenu devient accessible à un emplacement précis dans l'arborescence des fichiers du système (exemple : `/home`).

### Configurer l’outil de gestion des paquets

Un **outil de gestion des paquets** est un logiciel ou un ensemble de programmes utilisé pour installer, mettre à jour, configurer, et gérer les logiciels sur un système d'exploitation. Il facilite le processus de gestion des dépendances et de mise à jour des applications.

Choisissez le pays du miroir proche de vous (”France” pour moi), puis sélectionnez un miroir (le premier *deb.debian.org* est très bien) et vous pouvez laisser “Mandataire http” vide.

Pour “popularity contest” à vous de voir si vous voulez participer à l’étude ou non.

Dans “sélection des logiciels” décochez tout afin de n’avoir aucun environnement de bureau et de n’installer aucun utilitaire supplémentaire (les utilitaires nécessaires seront installés par la suite manuellement).

Enfin acceptez l’installation de GRUB.

**GRUB** (GNU GRand Unified Bootloader) est un chargeur de démarrage utilisé pour lancer un système d'exploitation lors du démarrage de l'ordinateur. Il permet de gérer plusieurs systèmes d'exploitation, d'offrir un menu interactif pour choisir quoi démarrer, et de passer des paramètres au noyau. Compatible avec MBR et GPT, GRUB supporte de nombreux systèmes de fichiers et offre des outils de dépannage en cas de problème. Il est très personnalisable et constitue une pièce essentielle des systèmes Linux et UNIX.

# Configurer Debian

Dans cette seconde partie, je vais vous exposer les outils et commandes à connaître pour réaliser la configuration de Debian telle que demandée dans le sujet (hors bonus).

## Configurer sudo

**Sudo (superuser do)** est une commande qui permet à un utilisateur d'exécuter des actions avec les privilèges d'un autre utilisateur, généralement **root**, sans se connecter directement en tant que ce dernier. Il assure un contrôle précis des permissions et améliore la sécurité en limitant l'accès administrateur. Pour configurer sudo, vous devez vous logger en tant que superutilisateur root.

### Ajouter un utilisateur <username> au groupe sudo

**`su root`**  permet de se logger en tant que root

**`apt update`**  met à jour la liste des paquets disponibles dans les dépôts configurés sur le système, sans installer ni mettre à jour aucun paquet ****

**`apt upgrade`**  met à jour tous les paquets installés sur le système vers leurs dernières versions disponibles, en utilisant les informations de la liste mise à jour par `apt update`

**`apt install sudo`**  Installe sudo

**`sudo usermod -aG sudo <username>`**  ajoute l’utilisateur <username> au groupe sudo sans l’enlever de ses autres groupes

**`exit`**  permet de quitter un terminal ou de fermer une session shell (permet par exemple de fermer la session root)

**`groups <username>`**  affiche tous les groupes auxquels appartient l’utilisateur

### Méthode alternative: modifier le fichier sudoers

Pour ajouter un utilisateur au groupe **`sudo`** via le fichier **`sudoers`**, ouvrez d'abord le fichier en utilisant la commande **`visudo`**, car cette commande permet de vérifier les erreurs de syntaxe. Tapez **`sudo visudo`** dans le terminal. Ensuite, ajoutez la ligne suivante à la fin du fichier pour accorder des privilèges **sudo** à l'utilisateur souhaité : **`<username> ALL=(ALL:ALL) ALL`**, en remplaçant **`<username>`** par le nom de l'utilisateur. Cette ligne permet à l'utilisateur d'exécuter n'importe quelle commande avec **sudo**. Enfin, sauvegardez et fermez le fichier. Avec **`visudo`**, la syntaxe sera automatiquement vérifiée pour éviter toute erreur. Cela donne à l'utilisateur les droits d'administrateur et lui permet d'utiliser **sudo** pour exécuter des commandes en tant que superutilisateur.

**La ligne `ALL=(ALL:ALL) ALL` dans le fichier `sudoers` permet à un utilisateur de:**

**`ALL`** (avant le signe égal) : Accéder à toutes les machines, ce qui signifie que cette règle s'applique à tous les hôtes.

**`(ALL:ALL)`** : Exécuter des commandes en tant que n'importe quel utilisateur (le premier **`ALL`**) et en tant que n'importe quel groupe (le deuxième **`ALL`**).

**`ALL`** (après le signe égal) : L'utilisateur peut exécuter toutes les commandes avec **sudo**.

En résumé, cela donne à l'utilisateur tous les droits d'exécution avec **sudo** sur toutes les commandes, quel que soit l'utilisateur ou le groupe.

### Apt et aptitude

**`apt`** (Advanced Package Tool) est un système de gestion de paquets utilisé sur les distributions basées sur Debian (comme Ubuntu). Il permet d'installer, de mettre à jour, de supprimer et de gérer les logiciels sur un système Linux en utilisant des dépôts de logiciels. `apt` simplifie l'installation et la gestion des paquets, en résolvant automatiquement les dépendances nécessaires pour faire fonctionner un logiciel.

**`apt`** est un outil moderne et simple pour gérer les paquets via la ligne de commande, principalement utilisé pour des tâches comme l'installation et la mise à jour de paquets. **`aptitude`**, quant à lui, offre une interface plus interactive et propose une gestion plus avancée des dépendances, souvent capable de résoudre des conflits de manière plus flexible. En général, **`apt`** est préféré pour sa simplicité, tandis que **`aptitude`** est utilisé pour des tâches plus complexes.

### Configurer le groupe sudo

Il faut pour cela éditer le fichier sudoers (ce fichier définit les permissions d'utilisation de `sudo` pour les utilisateurs et groupes) et y intégrer les paramètres que l’on souhaite. Le fichier `sudoers` se trouve généralement dans le répertoire `/etc/`  mais il vaut mieux l’ouvrir de manière sécurisée avec la commande suivante:

**`sudo visudo`**  permet d'éditer le fichier `sudoers` de manière sécurisée: `visudo` vérifie la syntaxe avant de sauvegarder les modifications pour éviter les erreurs qui pourraient rendre le système inaccessible.

Les paramètres à écrire dans le fichier pour respecter le sujet sont:

```bash
Defaults     passwd_tries=3                    
# Limite le nombre de tentatives de mot de passe à 3 avant d'échouer.
Defaults     badpass_message="Wrong password. Try again!" 
 # Message affiché après une tentative infructueuse de saisie du mot de passe.
Defaults     logfile="/var/log/sudo/sudo.log"  
# Spécifie l'emplacement du fichier journal où les commandes sudo sont enregistrées.
Defaults     log_input                         
# Active l'enregistrement des entrées (les commandes exécutées avec sudo).
Defaults     log_output                        
# Active l'enregistrement des sorties des commandes exécutées avec sudo.
Defaults     requiretty                        
# Exige que sudo soit exécuté dans un terminal pour une sécurité accrue (désactive les appels sans terminal).
```

## Configurer le pare-feu UFW

**UFW (Uncomplicated Firewall)** est un outil de gestion de pare-feu pour les systèmes Linux. Il simplifie la configuration des règles de filtrage réseau, permettant aux utilisateurs de définir facilement quelles connexions réseau sont autorisées ou bloquées sur un système. Il est conçu pour être facile à utiliser tout en étant puissant, et est souvent utilisé sur des distributions comme Ubuntu.

### C’est quoi AppArmor?

AppArmor (**Application Armor**) est un module de sécurité pour Linux qui applique des politiques de contrôle d'accès obligatoires (MAC). Il limite les actions que les applications peuvent effectuer en définissant des profils qui spécifient les fichiers, réseaux et ressources auxquels elles peuvent accéder. Cela permet de réduire les risques liés à des comportements malveillants ou à des vulnérabilités.

### Quelle différence entre AppArmor et UFW?

Ce sont deux outils de sécurité distincts, mais ils complètent la protection globale d'un système Linux.

- **UFW** se concentre sur la gestion des connexions réseau en configurant des règles de pare-feu pour autoriser ou bloquer le trafic entrant et sortant sur des ports spécifiques.
- **AppArmor** protège les applications en restreignant les ressources système qu'elles peuvent utiliser, comme les fichiers, la mémoire, ou les périphériques.

Bien qu'il n'y ait pas de lien direct entre eux, ils peuvent être utilisés ensemble pour renforcer la sécurité. Par exemple, UFW contrôle l'accès réseau tandis qu'AppArmor restreint les comportements des applications exposées au réseau.

**`sudo apt install ufw`**  installe ufw

**`sudo ufw enable`**  active ufw

**`sudo ufw status`**  vérifie le statut pour s’assurer qu’il est activé

**`sudo systemctl enable ufw`**  configure ufw pour qu’il se lance automatiquement au démarrage du système

### C’est quoi systemctl?

La commande `systemctl` est un outil de gestion du système et des services sous Linux. Elle permet de démarrer, arrêter, redémarrer, activer, désactiver, et vérifier le statut des services (ou unités) système. Elle est utilisée pour interagir avec le système d'initialisation **systemd**, qui gère les processus système et les services.

Quelques exemples :

- `systemctl start <service>` : Démarre un service.
- `systemctl stop <service>` : Arrête un service.
- `systemctl status <service>` : Affiche l'état actuel d'un service.
- `systemctl enable <service>` : Active un service pour qu'il se lance au démarrage.
- `systemctl disable <service>` : Désactive le lancement automatique d'un service au démarrage.

**`sudo ufw status verbose`**  permet d'afficher l'état actuel du pare-feu UFW avec des informations détaillées. Lorsqu'elle est exécutée, elle montre la liste des règles actives, les interfaces réseau concernées, et d'autres informations sur le statut du pare-feu. Le mot-clé "verbose" ajoute des détails supplémentaires, comme les ports ouverts, les actions par défaut (accepté, rejeté, etc.) et la configuration générale du pare-feu.

### C’est quoi un port?

Un **port** en informatique fait référence à un **point de communication** dans un système qui permet à des programmes ou services d'échanger des données avec d'autres ordinateurs ou systèmes. Il est associé à un protocole réseau, comme TCP ou UDP, et identifie un service spécifique sur une machine. Les ports servent de points d'accès pour que des applications ou services puissent être accessibles via un réseau.

Les ports sont identifiés par des numéros compris entre 0 et 65535. On les divise généralement en trois catégories :

- **Ports bien connus** (0-1023) : Ces ports sont réservés pour des services standards, comme le port 80 pour HTTP, le port 443 pour HTTPS, ou le port 22 pour SSH.
- **Ports enregistrés** (1024-49151) : Ces ports peuvent être utilisés par des applications non standards, mais qui sont enregistrées auprès de l'IANA (Internet Assigned Numbers Authority) pour éviter les conflits.
- **Ports dynamiques ou privés** (49152-65535) : Ces ports sont généralement utilisés par des applications pour des connexions temporaires ou pour des échanges de données avec des serveurs.

En résumé, un port est un identifiant utilisé pour diriger le trafic réseau vers le bon service sur un appareil donné. Par exemple, un serveur web écoute sur le port 80 pour recevoir les requêtes HTTP des clients.

### Pourquoi les ports apparaissent 2 fois quand j’examine le status de ufw?

Les ports apparaissent deux fois dans la sortie de `ufw status verbose` parce que le pare-feu UFW gère à la fois les connexions IPv4 et IPv6.

**IPv4** (Internet Protocol version 4) et **IPv6** (Internet Protocol version 6) sont deux versions du protocole Internet utilisées pour adresser et acheminer les données sur Internet. IPv6 offre une capacité d'adressage beaucoup plus grande qu'IPv4, ce qui permet de connecter beaucoup plus d'appareils à Internet. Les adresses IPv4 sont plus courtes et sont écrites en décimal, tandis que les adresses IPv6 sont plus longues et sont écrites en hexadécimal. IPv6 dispose d'un mécanisme de configuration automatique des adresses, ce qui facilite la gestion des réseaux. IPv6 a été conçu avec des améliorations de sécurité par rapport à IPv4, notamment avec le chiffrement des données. Bien que IPv6 soit plus moderne et offre des avantages pour l'avenir d'Internet, IPv4 reste encore largement utilisé, et la transition entre les deux protocoles est en cours. Les deux peuvent coexister sur le même réseau.

Commandes pour gérer les ports:

**`sudo ufw allow <port>`**  autorise le traffic sur le port <port>

**`sudo ufw deny <port>`**  bloque le traffic sur le port <port>

**`sudo ufw delete allow <port>`**  retire l’autorisation

**`sudo ufw delete deny <port>`**  retire le blocage

## Configurer SSH

La première étape est d’installer OpenSSH.

**OpenSSH (Open Secure Shell)** est un ensemble d'outils et de protocoles permettant de sécuriser les communications réseau, principalement via la commande **SSH (Secure Shell)**. Il est utilisé pour établir des connexions à distance sécurisées entre ordinateurs, ce qui permet de se connecter à un serveur à travers un réseau tout en chiffrant les échanges pour garantir la confidentialité et l'intégrité des données. Voici quelques points clés concernant OpenSSH :

- **SSH** : OpenSSH permet d'utiliser le protocole SSH, qui est une méthode sécurisée pour se connecter à un autre ordinateur (généralement un serveur) via une interface de ligne de commande. SSH remplace les protocoles non sécurisés comme Telnet et rlogin.
- **Chiffrement** : SSH chiffre toutes les données échangées entre l'utilisateur et le serveur, garantissant ainsi que même si la communication est interceptée, les informations restent protégées.
- **Authentification** : OpenSSH permet une authentification forte via des mots de passe ou des clés cryptographiques (par exemple, clés publiques et privées) pour garantir que seules les personnes autorisées puissent se connecter.
- **SFTP et SCP** : OpenSSH inclut des outils pour transférer des fichiers de manière sécurisée. **SFTP (Secure File Transfer Protocol)** et **SCP (Secure Copy Protocol)** permettent de copier des fichiers entre ordinateurs de manière chiffrée.

En résumé, OpenSSH est une solution fiable et largement utilisée pour gérer des connexions à distance sécurisées entre ordinateurs sur un réseau, offrant une authentification forte et un chiffrement des données.

**`sudo apt install openssh-server`**  installe OpenSSH

**`sudo systemctl status ssh`**  vérifie le statut de SSH

Le port par défaut pour SSH est le port 22, toutefois il est possible de le changer (et c’est demandé dans le sujet). Pour changer le port écouté, il faut modifier le fichier de configuration de SSH situé à `/etc/ssh/sshd_config` 

**`sudo nano /etc/ssh/sshd_config`** ouvre le fichier en question avec nano
Pour changer le port écouté il faut chercher la ligne `#Port 22` , la décommenter et remplacer le numéro de port par le numéro désiré (4242 en l’occurence)
**`sudo systemctl restart ssh`**  permet de redémarrer le service (nécéssaire après avoir changé le port) 

**`sudo ufw allow 4242`**  autorise le traffic sur le port 4242

### Configurer la redirection de port dans Virtualbox

**La redirection de port** dans **VirtualBox** permet de faire communiquer l'**hôte** (votre machine physique) avec la **machine virtuelle** (VM). Elle est particulièrement utile lorsque vous voulez accéder à des services (comme un serveur web, un serveur SSH, etc.) qui sont en cours d'exécution dans la VM, mais vous ne souhaitez pas qu'ils soient directement exposés à internet. En d'autres termes, la redirection de port permet à la machine virtuelle d'écouter sur un port spécifique tout en faisant en sorte que ce port soit accessible depuis l'extérieur de la VM, à travers un port sur l'hôte.

Dans Virtualbox il faut sélectionner sa VM, cliquer sur “Configuration” >> “Réseau” >> “Redirection de port” et “ajouter une nouvelle règle de redirection” (le petit + à droite). remplir les champs de la façon suivante: Nom: Port 4242, Protocole: TCP, Port hôte: 4242, Port invité: 4242. Puis dans la VM redémarrer à nouveau SSH.

### C’est quoi TCP?

Le **protocole TCP** (Transmission Control Protocol) est un protocole de communication utilisé dans les réseaux informatiques pour garantir la transmission fiable et ordonnée des données entre deux ordinateurs. Il est souvent utilisé en complément du protocole IP (Internet Protocol), formant ainsi le couple TCP/IP.

TCP est utilisé par des applications nécessitant une transmission fiable des données, comme le web (HTTP), l'envoi d'emails (SMTP), ou le transfert de fichiers (FTP).

### Connexion depuis le terminal hôte

Pour se connecter à sa VM depuis le terminal de l’ordinateur hôte, il faut ouvrir un terminal et taper la commande:

**`ssh <username>@localhost -p 4242`**  permet de se connecter à un serveur via SSH (Secure Shell) en utilisant l'utilisateur spécifié (`<username>`) sur la machine locale (`localhost`). Le `-p 4242` indique que la connexion SSH doit se faire via le port `4242` au lieu du port SSH par défaut (qui est le port `22`).

**Bloquer les connexions ssh en tant que superutilisateur root:** Il faut modifier le fichier de configuration SSH (`sshd_config`) pour respecter cette consigne de l’énoncé.

**`sudo nano /etc/ssh/sshd_config`**  ouvre le fichier de configuration SSH

Il faut ensuite chercher la ligne `PermitRootLogin`  la décommenter, et définir sa valeur à “no”

```bash
PermitRootLogin no
```

## Politique de mot de passe

Vous pouvez configurer certaines règles de politique de mot de passe directement via le fichier **`/etc/login.defs`**. Ce fichier contient des paramètres globaux pour l'authentification des utilisateurs, y compris la gestion des mots de passe.

Sous “password aging control” modifier comme suit:

```bash
# Le mot de passe doit être changé tous les 30 jours
PASS_MAX_DAYS 30

# Il doit y avoir un minimum de 2 jours entre chaque changement de mot de passe
PASS_MIN_DAYS 2

# L'utilisateur sera averti 7 jours avant l'expiration du mot de passe
PASS_WARN_AGE 7
```

**Les changements ne s’appliquent pas automatiquement aux utilisateurs existants,** il faut donc utiliser la commande **`chage`** pour faire les changements sur les utilisateurs existants (avec sudo).

**`sudo chage -M 30 <username>`**  le mot de passe doit être changé tous les 30 jours

**`sudo chage -m 2 <username>`**  il doit y avoir 2 jours minimum entre chaque changement de mot de passe

**`sudo chage -W 7 <username>`**  l’utilisateur sera averti 7 jours avant expiration

**`chage -l <username>`**  voir les paramètres de mot de passe appliqués à l’utilisateur <username>

### La bibliothèque libpwquality

Le fichier **`pwquality.conf`** est un fichier de configuration utilisé par la bibliothèque **libpwquality**, qui est responsable de la gestion des politiques de qualité des mots de passe sous Linux. Ce fichier permet d'appliquer des règles de sécurité aux mots de passe afin d'assurer qu'ils répondent à certains critères de complexité.

Lorsqu'un utilisateur tente de changer son mot de passe, la bibliothèque **libpwquality** vérifie que le nouveau mot de passe respecte les règles définies dans **`pwquality.conf`**. Si le mot de passe ne respecte pas les critères, il sera rejeté et l'utilisateur sera invité à en choisir un autre.

Sur une machine Debian ou similaire, le fichier **`pwquality.conf`** se trouve généralement à cet emplacement : **`/etc/security/pwquality.conf`**

Ouvrez le fichier avec nano et éditez le pour qu’il ressemble à ça:

```bash
# Number of characters in the new password that must not be present in the 
# old password.
difok = 7
# The minimum acceptable size for the new password (plus one if 
# credits are not disabled which is the default)
minlen = 10
# The maximum credit for having digits in the new password. If less than 0 
# it is the minimun number of digits in the new password.
dcredit = -1
# The maximum credit for having uppercase characters in the new password. 
# If less than 0 it is the minimun number of uppercase characters in the new 
# password.
ucredit = -1
# ...
# The maximum number of allowed consecutive same characters in the new password.
# The check is disabled if the value is 0.
maxrepeat = 3
# ...
# Whether to check it it contains the user name in some form.
# The check is disabled if the value is 0.
usercheck = 1
# ...
# Prompt user at most N times before returning with error. The default is 1.
retry = 3
# Enforces pwquality checks on the root user password.
# Enabled if the option is present.
enforce_for_root
# ...

```

### Modifier les règles pour le superutilisateur root

Le superutilisateur root doit être soumis aux même règles de mot de passe sauf en ce qui concerne le “difok” (le nombre de caractères dans le nouveau mot de passe qui ne doivent pas être présent dans l’ancien).

**Vous devez donc créer un fichier spécifique pour root:**

 **`/etc/security/pwquality-root.conf`**

et l’éditer avec nano pour qu’il soit identique au **`pwquality.conf`**  à l’exception de la ligne concernant “difok” puisque nous ne voulons pas imposer de règle à ce paramètre.

```bash
# Number of characters in the new password that must not be present in the 
# old password.
# difok = 0
# The minimum acceptable size for the new password (plus one if 
# credits are not disabled which is the default)
minlen = 10
# The maximum credit for having digits in the new password. If less than 0 
# it is the minimun number of digits in the new password.
dcredit = -1
# The maximum credit for having uppercase characters in the new password. 
# If less than 0 it is the minimun number of uppercase characters in the new 
# password.
ucredit = -1
# ...
# The maximum number of allowed consecutive same characters in the new password.
# The check is disabled if the value is 0.
maxrepeat = 3
# ...
# Whether to check it it contains the user name in some form.
# The check is disabled if the value is 0.
usercheck = 1
# ...
# Prompt user at most N times before returning with error. The default is 1.
retry = 3
# Enforces pwquality checks on the root user password.
# Enabled if the option is present.
enforce_for_root
# ...

```

**Modifier le fichier PAM pour distinguer root des autres utilisateurs:**

Éditez le fichier PAM `/etc/pam.d/common-password` (avec **`sudo`**  pour pouvoir écrire) et juste **au-dessus** de la ligne:

```bash
password	requisite			pam_pwquality.so retry=3
```

ajoutez les lignes suivantes:

```bash
# verify if user is root:
auth [success=1 default=ignore] pam_succeed_if.so uid=0
# if user is root:
password	requisite			pam_pwquality.so retry=3 conf=/etc/security/pwquality-root.conf
```

Ainsi si l'utilisateur est **root**, le système va utiliser les règles définies dans le fichier **`/etc/security/pwquality-root.conf`**  au lieu de **`/etc/security/pwquality.conf`** 

La configuration de la politique de mot de passe est terminée, vous pouvez modifier les mots de passe existants pour qu’ils soient conforme à la nouvelle politique de mot de passe avec la commande: **`sudo passwd <username>`**  

### C’est quoi PAM?

PAM (**Pluggable Authentication Modules**) est un système modulaire utilisé sur les systèmes Unix/Linux pour gérer l'authentification. Il permet d'intégrer différentes méthodes d'authentification (mots de passe, biométrie, clés SSH, etc.) et de configurer des règles de sécurité de manière centralisée pour les applications et services.

## Autres commandes à connaître

Les commandes pour gérer les utilisateurs nécessitent l’emploie de **`sudo`  dans la plupart des cas (à partir du moment où la commande modifie quelque chose).**

### Nom d’hôte

**`sudo hostnamectl set-hostname <new_hostname>`** changer le nom d’hôte en <new_hostname>

**`hostnamectl status`** affiche les informations du système

La commande **`hostnamectl`** permet de gérer le **nom d'hôte** (hostname) d'une machine sous Linux. Elle permet de **voir, définir ou modifier** le nom d'hôte, ainsi que d'autres informations liées au système, comme le type de matériel ou la version du système d'exploitation.

### Ajouter des utilisateurs

Les commandes pour gérer les utilisateurs (ajouter, supprimer, etc) nécessitent d’être exécutées avec **`sudo` :**

`adduser <username>` : Crée un nouvel utilisateur avec des interface interactives (création du répertoire personnel, du mot de passe, possibilité de renseigner des informations comme le numéro de téléphone de l’utilisateur, son numéro de chambre, etc)

`useradd <username>` : Crée un utilisateur avec des options personnalisées (moins interactif).

Contrairement à adduser, useradd crée uniquement l'utilisateur avec des paramètres de base et n'initialise pas automatiquement des configurations comme le dossier personnel, les scripts de démarrage, etc., à moins d'utiliser des options spécifiques. Nécessite d'ajouter des options ou de modifier les fichiers système manuellement pour des configurations spécifiques. Exemples :

- `m` : Crée le répertoire personnel.
- `s` : Définit le shell par défaut.
- `G` : Ajoute l'utilisateur à un groupe.

### Supprimer des utilisateurs

`deluser <username>` : Supprime un utilisateur tout en conservant son répertoire personnel (optionnellement).

`userdel -r <username>` : Supprime un utilisateur et son répertoire personnel.

`userdel` est une commande basique pour supprimer un utilisateur, mais elle ne supprime pas automatiquement son répertoire personnel et ses fichiers associés, sauf avec l'option `-r`. En revanche, `deluser` est un outil plus complet et convivial, souvent fourni par le paquet `adduser`, qui supprime non seulement l'utilisateur, mais aussi ses fichiers et répertoires associés de manière plus propre et sécurisée.

### Modifier et lister les utilisateurs

`id <username>` : Affiche l'UID, GID et les groupes de l'utilisateur.

`id -g <username>` : Affiche le GID principal de l’utilisateur.

Le **GID principal** (Group ID) d'un utilisateur est l'identifiant du groupe principal auquel cet utilisateur appartient. Lorsqu'un utilisateur est créé, un groupe portant le même nom est souvent créé automatiquement, et ce groupe devient le groupe principal de l'utilisateur. Le GID principal est utilisé pour définir les permissions d'accès aux fichiers et répertoires, et détermine les autorisations par défaut de l'utilisateur pour les fichiers qu'il crée.

`usermod -L <username>` : Verrouille un compte (désactive le mot de passe) sans le supprimer (*lock*).

`usermod -U <username>` : Déverrouille un compte (*unlock*).

`usermod` est une commande utilisée pour modifier un utilisateur existant sur un système Linux. Elle permet d'ajuster les paramètres d'un compte utilisateur, comme changer le groupe principal ou ajouter des groupes secondaires, modifier le shell par défaut, renommer un utilisateur, verrouiller ou déverrouiller un compte, et changer le répertoire personnel ou déplacer son contenu.

`passwd <username>` : Modifie le mot de passe d’un utilisateur.

`cat /etc/passwd` : Affiche tous les utilisateurs du système.

`awk -F: '$3 >= 1000 {print $1}' /etc/passwd` : Affiche uniquement les utilisateurs “humains”

### C’est quoi tous ces utilisateurs?

Les utilisateurs que vous voyez dans le fichier `/etc/passwd` ne sont pas uniquement les utilisateurs que vous avez créés. En effet, ce fichier contient également des **utilisateurs système** créés par le système ou par les logiciels pour des tâches spécifiques. Ces utilisateurs système n'ont souvent pas de répertoire personnel ou de shell et sont utilisés par des services et des processus pour fonctionner de manière sécurisée, sans avoir besoin d'un accès complet à l'environnement utilisateur. Par exemple: daemon, bin, sys, mail sont des utilisateurs associés à des services système, tels que des processus de gestion des mails ou des services de base.

La commande `awk -F: '$3 >= 1000 {print $1}' /etc/passwd` filtre les utilisateurs dont l’UID (troisième champ dans le fichier /etc/passwd)  est supérieur à 1000 et n’affiche que le nom (premier champ) car les utilisateurs système ont généralement des UID inférieurs à 1000 (une exception par exemple est l’utilisateur système nobody)

`who` : Liste les utilisateurs connectés.

`w` : Donne des informations détaillées sur les sessions des utilisateurs.

### Gérer les groupes

`groupadd <group>`  **créer un nouveau groupe** 

`groupdel <group>`   supprime un groupe

`getent group <group>`  affiche la liste des membres d’un groupe

`groups <username>` liste les groupes auxquels appartient l'utilisateur.

`usermod -aG <group> <username>` : Ajoute un utilisateur à un groupe.

## Script Monitoring.sh

Le script doit être réalisé en tant qu’utilisateur root et enregistré dans le dossier `/root` .

Pour réaliser ce script, il suffit de déclarer des variables et d’y stocker les résultats des commandes détaillées plus bas, puis d’afficher le tout à l’aide de `echo` et de `| wall`  pour que le message soit envoyé à tous les utilisateurs connectés.

### Quelques bases de script bash

**Un script bash commence par une ligne appelée shebang** (`#!/bin/bash`) qui spécifie l'interpréteur à utiliser pour exécuter le script. Dans cet exemple, elle indique que le script doit être exécuté avec l'interpréteur **Bash**. Cela permet au système d'utiliser automatiquement le bon programme pour exécuter le script, même si celui-ci est lancé directement.

**En bash on peut affecter le résultat d’une commande à une variable** comme ceci:

```bash
variable=$(commande)
```

**Pour accéder à la valeur d’une variable** on utilise la syntaxe suivante:

```bash
$variable
```

`echo`  permet d’afficher une chaîne de caractères ou la valeur d’une variable dans le terminal.

`wall`  envoie un message à tous les utilisateurs connectés à la machine.

`|` permet de rediriger la sortie d’une commande vers l’entrée d’une autre commande.

### Exemple avec la commande `date` qui retourne la date et l’heure:

```bash
#!/bin/bash

message=$(date)
echo $message | wall
# Ce script enverra la date et l'heure retournées par la commande date
# à tous les utilisateurs connectés
```

### Le script commenté

```bash
#!/bin/bash

# Récupère l'architecture du système, la version du noyau, et le type de système (ex: x86_64, Linux, GNU)
arch=$(uname -srmo)

# Récupère la version du noyau du système
karnel=$(uname -v)

# Nombre de CPU physiques détectés sur le système
# Cette commande lit le fichier /proc/cpuinfo, filtre les lignes contenant "physical id", 
# supprime les doublons avec sort -u et compte le nombre de lignes avec wc -l
pcpu=$(cat /proc/cpuinfo | grep "physical id" | sort -u | wc -l)

# Nombre de CPU virtuels détectés sur le système
# Cette commande lit le fichier /proc/cpuinfo, filtre les lignes contenant "processor", 
# puis compte le nombre de processeurs avec wc -l
vcpu=$(cat /proc/cpuinfo | grep "processor" | sort -u | wc -l)

# Calcul de l'utilisation du CPU en pourcentage
# Utilise la commande top pour afficher les statistiques des processus, filtre les lignes contenant 'Cpu', 
# puis calcule l'utilisation du CPU avec awk en additionnant l'usage des processus utilisateur et système
cpu_used=$(top -bn1 | grep 'Cpu' | xargs | awk '{printf("%.1f%%"), $2 + $4}')

# Récupère la mémoire totale (RAM) disponible sur le système
mem_total=$(free -h | grep Mem | awk '{print $2}')

# Récupère la mémoire (RAM) utilisée sur le système
mem_used=$(free -h | grep Mem | awk '{print $3}')

# Calcule le pourcentage de la mémoire utilisée
mem_perc=$(free -k | grep Mem | awk '{printf("%.2f%%"), $3 / $2 * 100}')

# Récupère l'espace disque total disponible sur le système
hdd_total=$(df -h --total | grep "total" | awk '{print $2}')

# Récupère l'espace disque utilisé sur le système
hdd_used=$(df -h --total | grep "total" | awk '{print $3}')

# Récupère l'utilisation de l'espace disque en pourcentage
hdd_perc=$(df -h --total | grep "total" | awk '{print $5}')

# Récupère la dernière date et heure du redémarrage du système
reboot=$(who -b | awk '{print($3 " " $4)}')

# Récupère la date et l'heure actuelles au format YYYY-MM-DD et HH:MM:SS
actual_time=$(date +%Y-%m-%d && date +%H:%M:%S)

# Nombre de connexions TCP actives
# Cette commande récupère les informations des connexions TCP dans /proc/net/sockstat 
# et extrait le nombre de connexions actives
tcp=$(grep TCP /proc/net/sockstat | awk '{print $3}')

# Nombre d'utilisateurs actuellement connectés au système
# Cette commande utilise "who" pour lister les utilisateurs connectés et "wc -l" pour compter le nombre de lignes
user_conn=$(who | wc -l)

# Adresse IPv4 de la machine
ipv4=$(hostname -I)

# Adresse MAC de la machine
mac=$(ip link show | grep ether | cut -c 16-32)

# Nombre de commandes exécutées avec sudo
# La commande recherche les lignes contenant "COMMAND=" dans le fichier de log sudo et compte le nombre d'occurrences
sudo=$(sudo grep "COMMAND=" /var/log/sudo/sudo.log | wc -l)

# Vérifie si LVM (Logical Volume Manager) est activé sur le système
# Cette commande utilise lsblk pour lister les périphériques et vérifier s'il existe des volumes LVM
lvm=$(if [ $(lsblk | grep lvm | wc -l) -eq 0 ]; then
                 echo "Disable"
             else
                 echo "Enable"
             fi)

# Affiche les informations système collectées et les envoie à tous les utilisateurs connectés via la commande wall
echo "
	________________________________________________________________________
	MONITORING		
	________________________________________________________________________
	Architecture            	: $arch
	Kernel				            : $karnel
	Number of physical CPU		: $pcpu
	Number of virtual CPU		  : $vcpu
	CPU load			            : $cpu_used
	RAM usage			            : $mem_used/$mem_total ($mem_perc)
	Disk usage		          	: $hdd_used/$hdd_total ($hdd_perc)
	Last boot			            : $reboot
	LVM status		          	: $lvm
	Active TCP connexions		  : $tcp
	Users currently connected	: $user_conn
	IPv4 address			        : $ipv4
	MAC address			          : $mac
	Commands run with sudo		: $sudo
	________________________________________________________________________
" | wall

```

N’oubliez pas de donner les permissions d’exécution avec la commande `chmod 755 monitoring.sh` .

## Exécuter une tache de manière automatisée

Pour automatiser l'exécution de tâches à des horaires spécifiques, il faut un **planificateur de tâches** ou **démon de planification** comme **cron.**

**Cron** est un utilitaire sous Linux permettant d'exécuter des tâches automatisées à des moments spécifiques. Ces tâches, appelées **jobs**, peuvent être programmées pour s'exécuter à intervalles réguliers (comme toutes les heures, chaque jour, etc.).
Il faut d’abord activer **cron** pour qu’il démarre automatiquement à chaque démarrage du système avec la commande suivante: **`systemctl enable cron`** 

### La crontab

La crontab est le fichier qui contient les taches à exécuter et les dates et heures des exécutions programmées. Voici comment elle fonctionne:

1. **`crontab -e`** : Cette commande permet de modifier le fichier crontab de l'utilisateur actuel (elle ouvre un éditeur de texte pour modifier le fichier idoine).
2. **Format** : Un fichier crontab contient des lignes avec cinq champs horaires (minute, heure, jour du mois, mois, jour de la semaine) suivis de la commande à exécuter.
3. Exemple : `30 14 * * 1 /home/user/backup.sh` exécutera le script `backup.sh` chaque lundi à 14h30.
4. On peut définir plusieurs valeurs par champs (par exemple `30,40 14 * * 1`   pour que l’exécution se fasse à 14h30 et 14h40 chaque lundi) ou des plages de valeurs (comme par exemple **`1-5`**  qui signifie 1, 2, 3 4 et 5).
5. Une autre façon d définir plusieurs valeurs est par exemple: `*/10 14 * * 1`   qui signifie à chaque fois que les minutes forment un multiple de dix (c’est à dire toutes les dix minutes), à 14h, les lundis.
6. Les champs `*` signifient “n’importe quel” ou “tous” (par exemple * sur le champ des moi signifie que la commande sera exécutée quel que soit le mois).

Vous pouvez aussi voir les jobs planifiés avec **`crontab -l`** et supprimer un job avec **`crontab -r`**.

### Créer la crontab pour Born2beroot

Toujours en tant qu’utilisateur root, il faut créer la crontab en ajoutant la ligne suivante dans le fichier ouvert par **`crontab -e` :**

**`*/10 * * * * bash /root/monitoring.sh`** 

Le mot “bash” dans la ligne signifie que le fichier doit être exécuté en tant que script bash, mais sa présence n’est normalement pas nécessaire si vous avez bien mis le **shebang** au début de votre fichier.

A partir de là, le scipt monitoring.h va être exécuté toutes les dix minutes (par exemple à 14h10, puis 14h20, puis 14h30, etc). Or le sujet spécifie qu’il doit être exécuté toutes les dix minutes à partir du démarrage du système. Nous allons devoir créer un petit script pour calculer le délai entre l’heure de démarrage du système et la plus proche dizaine de minute pour appliquer ce délai à l’exécution de notre script.

### Créer le script sleep.sh

Toujours dans le dossier root et en tant qu’utilisateur root, créez un fichier sleep.h contenant le script suivant qui utilise la commande sleep.

La commande **`sleep` suspend l’exécution d’un script pendant un temps spécifié.**

```bash
#!bin/bash

# Recueration des minutes de boot et sec
boot_min=$(uptime -s | cut -c 15-16)
boot_sec=$(uptime -s | cut -c 18-19)

# On calcul le nombre de minutes qui nous separt de la 10ene la plus proche
# Pour 18:42:23
# 42%10 = 2 donc 2 minutes de l'ecart avec la 10ene la plus proche qui est 40
# 2*60 = 120 pour convertir en seconde
# 120+23 = 143 seconde entre la 10ene la plus proche
delay=$(bc <<< $boot_min%10*60+$boot_sec)

#on passe les seconde au sleep pour avoir notre delay
sleep $delay
```

Modifiez ensuite la ligne précédemment créée dans la crontab comme suit:

**`*/10 * * * * bash /root/sleep.sh && bash /root/monitoring.sh`** 

L’opérateur logique AND **`&&`** permet d'exécuter la commande suivante **uniquement si la commande précédente a réussi** (c'est-à-dire que la commande précédente s'est terminée avec un code de sortie de 0, indiquant un succès).

Cette ligne dans le fichier crontab va exécuter deux scripts à intervalles réguliers. Voici ce qu'elle fait en détail :

- **`/10 * * * *`** : Cela signifie que la commande doit être exécutée toutes les 10 minutes (chaque minute qui est un multiple de 10).
- **`bash /root/sleep.sh`** : Le script `/root/sleep.sh` sera exécuté en premier.
- **`&&`** : Si le premier script (`sleep.sh`) s'exécute correctement (sans erreur), alors le deuxième script sera exécuté.
- **`bash /root/monitoring.sh`** : Le script `/root/monitoring.sh` sera exécuté **uniquement si** le premier script s'est terminé avec succès.

En résumé, cette ligne va lancer le script `sleep.sh` toutes les 10 minutes, et si ce script réussit (après avoir appliqué le délai imposé par la commande sleep), il lancera  `monitoring.sh` .

## Le fichier signature.txt

**Pour créer le fichier** il suffit d’aller dans le répertoire où est stockées la VM et d’exécuter **`shasum` sur le fichier .vdi de votre VM.**

### C’est quoi shasum?

**Shasum** est une commande utilisée pour calculer et vérifier les empreintes cryptographiques (ou "hashes") de fichiers, en utilisant des algorithmes SHA (Secure Hash Algorithm) comme SHA-1, SHA-256, etc. Elle sert principalement à vérifier l'intégrité et l'authenticité des fichiers. Elle crée un hash, qui est une empreinte numérique unique générée à partir de données, utilisée pour vérifier l'intégrité ou l'authenticité.

C’est quoi un fichier VDI?

**U**n fichier **VDI** (Virtual Disk Image) est un format de fichier utilisé par **VirtualBox** pour représenter un disque dur virtuel. Ce fichier contient toutes les données du système invité (comme un disque dur réel) et permet à la machine virtuelle d’accéder à un espace de stockage pour son système d'exploitation et ses fichiers.

**`~/Virtualbox VMS`**  est le répertoire de destination par défaut des VM sur Mac. Votre VM sera donc stockée dans le répertoire **`~/Virtualbox VMS/NomDeLaVM`** 

**`shasum NomDeLaVm.vdi > signature.txt`**  est la commande à exécuter pour créer l’empreinte de votre fichier et la stocker dans le fichier signature.txt (que sera créé en exécutant cette commande). La sortie de shasum stockée dans signature.txt sera le hash (une chaîne alphanumérique) suivi du nom du fichier analysé (si l’exécution de la commande prend un peu de temps, pas d’affolement, c’est normal). Par exemple:

a933ad77674444661a25fea08af33154550ddef7  Born2beroot.vdi

**Pour comparer le fichier** avec sa signature, il suffira d’exécuter  **`shasum`** avec ****l’option **`-c`** suivi du nom du fichier signature (**`signature.txt`** en l’occurence).

La commande **`shasum -c signature.txt`** permet ainsi  de vérifier l'intégrité d'un fichier en comparant son hash avec celui stocké dans un fichier de signature. Lors de la vérification, si le hash correspond, la commande affichera **`fichier.txt: OK`**, sinon une erreur indiquera que le fichier a été modifié.
