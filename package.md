## Index

[TOC]

- [Gestion des paquets debian](#gestion-des-paquets-debian)
- [Gestion des paquets fedora vs debian](#gestion-des-paquets-fedora-vs-debian)
- [Gestion des paquets fedora](#gestion-des-paquets-fedora)
- [Gestion des paquets archlinux](#gestion-des-paquets-archlinux)
- [Gestion des paquets openSUSE](#gestion-des-paquets-opensuse)
- [Gestion des paquets Solus](#gestion-des-paquets-solus)
- [MAJ debian](#maj-debian)
- [MAJ fedora](#maj-fedora)
- [fedora RPM fusion](#fedora-rpm-fusion)
- [debian depots](#debian-depots)
- [debian mise à niveau](#debian-mise-à-niveau)
- [debian passer en testing](#debian-passer-en-testing)
- [debian passer en sid](#debian-passer-en-sid)
- [backport un paquet Sid dans stable](#backport-un-paquet-sid-dans-stable)
- [dupliquer un systeme debian](#dupliquer-un-systeme-debian)
- [preconfiguration durant installation debian](#preconfiguration-durant-installation-debian)
- [creation d un mirroir local debian](#creation-d-un-mirroir-local-debian)
- [creation d un iso live perso](#creation-d-un-iso-live-perso)
- [compilation](#compilation)

_____________________________________________________________________________________
Gestion des paquets debian
-------------------------------------------------------------------------------------
```bash
apt update                                 #Mettre à jour la liste des paquets
apt upgrade                                #Installer les mises à jours
apt-get upgrade -s                         #passer en revue les paquets à mettre à jour
apt full-upgrade                           #Mettre à jour le système
apt install PACKAGE                        #Installer un paquet
apt install PACKAGE1 PACKAGE2              #Installer plusieurs paquets
apt install PACKAGE.deb                    #Installer un paquet .deb
apt install -t stretch-backports PACKAGE   #en modifiant les priorités
apt remove PACKAGE                         #Supprimer un paquet
apt autoremove                             #Supprimer les paquets inutilisés
apt purge PACKAGE                          #Supprimer les fichiers de configuration
apt edit-sources                           #Éditer le fichier sources.list
apt edit-sources deb-multimedia.list       #Éditer les fichiers dans sources.list.d
apt show PACKAGE                           #Trouver des informations sur un paquet
apt search TERME                           #Rechercher un paquet
apt list --installed                       #Lister les paquets installés
apt list --upgradeable                     #Lister les paquets prêts à être mis à jour
apt list --all-versions PACKAGE            #Lister toutes les versions d un paquet
apt -s install PACKAGE                     #Simuler l installation de paquets
apt -s full-upgrade                        #Simuler la mise à jour de paquets
apt-get source PACKAGE                     #telechargement des sources deb-src
apt-get autoremove                         #Nettoyage des paquets inutiles
dpkg-query -W | grep ~gpo                  #lister les blackports
checkrestart                               #redémarrer les services qui doivent l être
ar t PACKAGE.deb                           #lister le contenu d un paquet deb
ar x PACKAGE.deb                           #extraire le contenu d un paquet deb
dpkg -V                                    #vérifie tous les paquets installés
apt-listbugs list PACKAGE                  #obtenir la liste des bugs sur un paquet
reportbug                                  #report de bug
apt-cache showpkg PACKAGE                  #liste de toutes les versions disponibles
apt-mark hold PACKAGE                      #bloquer en version actuelle (pas d update)
apt-cache policy PACKAGE                   #montre le paquet et les versions disponibles
sudo aptitude install PACKAGE=VERSIONS     #Installer un paquet dans une version spécifique
apt-get install --no-install-recommends PACKAGE    #ne pas installer les paquets recommandés
```

_____________________________________________________________________________________
Gestion des paquets fedora vs debian
-------------------------------------------------------------------------------------
```bash
#Fedora                       #Debian                                           #Task

###>>>--------------------------------------------Adding, Removing and Upgrading Packages                                
auto                          apt-get update                                    #Refresh list of available packages
yum install PACKAGE           apt-get install PACKAGE                           #Install a package from a repository
yum install PACKAGE.rpm       dpkg --install PACKAGE.deb                        #Install a package file
rpm -i PACKAGE.rpm                
rpm -e PACKAGE                apt-get remove PACKAGE                            #Remove a package
yum remove PACKAGE            apt-get purge PACKAGE                             #Remove a package with configuration files
yum check-update              apt-get -s upgrade                                #Check for package upgrades
                              apt-get -s dist-upgrade
yum update                    apt-get upgrade                                   #Upgrade packages
rpm -Uvh
yum upgrade                   apt-get dist-upgrade                              #Upgrade the entire system
###>>>-------------------------------------------------------Package Information                                
yum search PACKAGE            apt-cache search PACKAGE                          #Get information about an available package
yum list available            apt-cache dumpavail                               #Show available packages
yum list installed            dpkg --list                                       #List all installed packages
rpm -qa
yum info PACKAGE              apt-cache show PACKAGE                            #Get information about a package
rpm -qi PACKAGE               dpkg --status PACKAGE                             #Get information about an installed package
rpm -ql PACKAGE               dpkg --listfiles PACKAGE                          #List files in an installed package
rpm -qd PACKAGE               -                                                 #List documentation files in an installed package
rpm -qc PACKAGE               dpkg-query --show -f '${Conffiles}\n' PACKAGE     #List configuration files in an installed package
rpm -qR PACKAGE               apt-cache depends                                 #Show the packages a given package depends on
rpm -q --whatrequires [args]  apt-cache rdepends                                #Show other packages that depend on a given package (reverse dependency)

###>>>--------------------------------------------------Package File Information
rpm -qpi PACKAGE.rpm          dpkg --info PACKAGE.deb                           #Get information about a package file
rpm -qpl PACKAGE.rpm          dpkg --contents PACKAGE.deb                       #List files in a package file
rpm -qpd PACKAGE.rpm          -                                                 #List documentation files in a package file
rpm -qpc PACKAGE.rpm          -                                                 #List configuration files in a package file
rpm2cpio FILE.rpm |cpio -vid  dpkg-deb --extract PACKAGE.deb DIRECTORY          #Extract files in a package
rpm -qf FILENAME              dpkg --search FILENAME                            #Find package that installed a file
yum provides FILENAME         apt-file search FILENAME                          #Find package that provides a particular file

###>>>----------------------------------------------Misc. Packaging System Tools
-                             apt-cache stats                                   #Show stats about the package cache
rpm -Va                       debsums                                           #Verify all installed packages
yum clean packages            apt-get clean                                     #Remove packages from the local cache directory
-                             apt-get autoclean                                 #Remove only obsolete packages from the local cache directory
yum clean headers             apt-file purge                                    #Remove header files from the local cache directory

###>>>--------------------------------------General Packaging System Information
*.rpm                         *.deb                                             #Package file extension
/etc/yum.conf                 /etc/apt/sources.list                             #Repository location configuration
```

_____________________________________________________________________________________
Gestion des paquets fedora
-------------------------------------------------------------------------------------
```bash
dnf repolist all                           #Lister les dépots disponibles
dnf install PACKAGE                        #Installer un paquet
dnf remove PACKAGE                         #Supprimer un paquet
dnf erase PACKAGE                          #Supprimer les fichiers de configuration
dnf update PACKAGE                         #Mettre à jour un paquet
dnf check-update                           #Verifier les mises à jour disponibles
dnf upgrade                                #Mettre à jour tous les paquets
dnf --exclude=PAQUET update                #exclure un paquet d une mise à jour
dnf downgrade PAQUET                       #Rétrograder un paquet
dnf grouplist                              #Liste les groupes de paquets disponibles
dnf groupinstall "GROUP NAME"              #Installer un groupe de paquets
dnf groupinfo "GROUP NAME"                 #Lister les paquets d un groupe
dnf groupremove "GROUP NAME"               #Supprimer un groupe de paquets
dnf search PACKAGE                         #Rechercher un paquet
dnf provides /usr/bin/FICHIER              #Rechercher un paquet contenant un fichier
dnf download PACKAGE                       #Telecharge sans installer
dnf list available | more                  #Liste les paquets disponibles
dnf list installed                         #Liste les paquets installés
dnf install --enablerepo=DEPOT PACKAGE     #Installer un paquet depuis un dépot
dnf --enablerepo=NOMDEPOT update           #Activer un dépôt pendant la mise à jour
dnf --disablerepo=NOMDEPOT update          #Désactiver un dépôt pendant la mise à jour
dnf provides COMMAND                       #Connaitre le paquet associé
dnf provides CHEMIN/FICHIER                #Connaitre le paquet associé
dnf info PACKAGE                           #Trouver des informations sur un paquet
time dnf makecache                         #Créer un cache manuel
dnf clean all                              #Supprimer le cache
dnf history                                #Historique des installations
dnf history info ID                        #Details sur une installation
dnf autoremove                             #Supprimer les paquets inutilisés
dnf distro-sync                            #Synchronise dans la dernière version stable
dnf reinstall PACKAGE                      #Reinstaller un paquet
dnf upgrade-to PACKAGE-VERSION.fcNUM       #Installer un paquet dans une version spécifique
dnf copr enable USER/PROJECT               #Installer un projet hors dépot
dnf help                                   #Aide

###>>>Limiter la bande passante de dnf
sudo nano /etc/dnf/dnf.conf
throttle=20k
max_connections=1

###>>>Désactiver les deltarpm
sudo nano /etc/dnf/dnf.conf
deltarpm=0

###>>>Utiliser le miroir le plus rapide
sudo nano /etc/dnf/dnf.conf
fastestmirror=true

###>>>Changement de version
sudo dnf upgrade --refresh
sudo dnf install dnf-plugin-system-upgrade
sudo dnf system-upgrade download --releasever=NEWNUM
sudo dnf system-upgrade reboot
```

_____________________________________________________________________________________
Gestion des paquets archlinux
-------------------------------------------------------------------------------------
```bash
pacman -Q                   #Affiche tous les paquets installés avec pacman
pacman -S PACKAGE           #Télécharge et installe
pacman -R PACKAGE           #Supprime le(s) paquet(s) spécifié(s) en argument
pacman -U PACKAGE           #Installe un paquet (préalablement téléchargé)
pacman -Sy                  #Actualise la liste en cache des dépôts
pacman -Syu                 #Met à jour tous les paquets
pacman -Sg                  #Affiche tous les groupes de paquets disponibles
pacman -Sc                  #Supprime tous les paquets non installés du cache
pacman -Si PACKAGE          #Donne les informations du paquet en argument
pacman -Ss PACKAGE          #Cherche les paquets correspondant au nom en argument
pacman -Rs PACKAGE          #Supprime paquet et dépendances inutiles
pacman -Rc PACKAGE          #Supprime paquet et dépendances

###>>>Acces aux paquets AUR
sudo vi /etc/pacman.conf
[archlinuxfr]
SigLevel = Never
Server = http://repo.archlinux.fr/$arch
pacman -S yaourt
```

_____________________________________________________________________________________
Gestion des paquets openSUSE
-------------------------------------------------------------------------------------
```bash
zypper install PACKAGE      #installe un paquet (et les dépendances, si nécessaire)
zypper remove PACKAGE       #désinstalle un paquet (et gère les dépendances liées)
zypper search PACKAGE       #recherche un paquet sans être sensible à la casse
zypper info PACKAGE         #donne les informations sur le paquet concerné
zypper list-updates         #liste les mises à jour disponibles
zypper update               #met à jour tous les programmes installés
zypper dist-upgrade         #mise à jour vers une version de distribution plus récente
zypper ps                   #lister les processus utilisant des fichiers supprimés
zypper refresh              #rafraichissement des méta-données de tous les dépôts
yast                        #outil magique
```

_____________________________________________________________________________________
Gestion des paquets Solus
-------------------------------------------------------------------------------------
```bash
sudo eopkg update-repo                      #mettre à jour les dépôts
sudo eopkg upgrade                          #mettre à jour les paquets
sudo eopkg upgrade PROGRAMME                #mettre à jour qu un paquet
sudo eopkg install PROGRAMME                #installer un paquet
sudo eopkg install --reinstall PROGRAMME    #reinstaller un paquet
sudo eopkg search PROGRAMME                 #rechercher un paquet
sudo eopkg info PROGRAMME                   #description d un paquet
sudo eopkg remove PROGRAMME                 #désinstaller un logiciel
```

_____________________________________________________________________________________
MAJ debian
-------------------------------------------------------------------------------------
```bash
sudo apt-get update -y && sudo apt-get upgrade -y && sudo apt-get dist-upgrade -y && sudo apt-get autoremove -y && sudo rpi-update && sudo reboot
```

_____________________________________________________________________________________
MAJ fedora
-------------------------------------------------------------------------------------
```bash
sudo dnf check-update && sudo dnf upgrade -y && sudo dnf clean packages -y && sudo reboot
```

_____________________________________________________________________________________
fedora RPM fusion
-------------------------------------------------------------------------------------
```bash
###>>>sudo ou su -lc  ...  en fonction de l utilisateur
﻿sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo rpm --import "http://rpmfusion.org/keys?action=AttachFile&do=get&target=RPM-GPG-KEY-rpmfusion-free-fedora-25"
sudo rpm --import "http://rpmfusion.org/keys?action=AttachFile&do=get&target=RPM-GPG-KEY-rpmfusion-nonfree-fedora-25"
```

_____________________________________________________________________________________
debian depots
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/apt/sources.list
# Debian stretch depot principal + paquets non libres
deb http://ftp.fr.debian.org/debian/ stretch main contrib non-free

# Debian stretch mises-a-jour de securite + paquets non libres
deb http://security.debian.org/debian-security/ stretch/updates main contrib non-free

# Debian stretch mises-a-jour volatiles + paquets non libres
deb http://ftp.fr.debian.org/debian/ stretch-updates main contrib non-free

# Backports repository
deb http://ftp.fr.debian.org/debian/ stretch-backports main contrib non-free
```

_____________________________________________________________________________________
debian mise à niveau
-------------------------------------------------------------------------------------
```bash
sed -i 's/jessie/stretch/g' /etc/apt/sources.list
sudo apt-get update -y && sudo apt-get dist-upgrade -y
```

_____________________________________________________________________________________
debian passer en testing
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/apt/sources.list
deb http://ftp.fr.debian.org/debian/ testing main contrib non-free 
deb http://security.debian.org/debian-security/ testing/updates main contrib non-free 
deb http://ftp.fr.debian.org/debian/ testing-updates main contrib non-free 

sudo apt-get update
sudo apt-get dist-upgrade
```
_____________________________________________________________________________________
debian passer en sid
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/apt/sources.list
deb http://ftp.fr.debian.org/debian/ sid main contrib non-free
deb-src http://ftp.fr.debian.org/debian/ sid main contrib non-free
deb http://www.deb-multimedia.org sid main non-free

sudo apt-get update
sudo apt-get dist-upgrade
sudo apt-get install apt-listbugs apt-listchanges deb-multimedia-keyring reportbug console-tools
```

_____________________________________________________________________________________
backport un paquet Sid dans stable
-------------------------------------------------------------------------------------
```bash
sudo apt install debhelper devscripts build-essential
sudo nano /etc/apt/sources.list
deb-src http://ftp.fr.debian.org/debian/ sid main contrib non-free

apt-get update
apt-get build-dep PACKAGE
apt-get -b source PACKAGE
dpkg -i PACKAGE.deb
```

_____________________________________________________________________________________
dupliquer un systeme debian
-------------------------------------------------------------------------------------
```bash
dpkg --get-selections > lstpkg.dpkg
dpkg --set-selections < lstpkg.dpkg
apt-get dselect-upgrade
```

_____________________________________________________________________________________
preconfiguration durant installation debian
-------------------------------------------------------------------------------------
```bash
sudo echo "#_preseed_V1" > preseed.cfg
sudo debconf-get-selections --installer >> preseed.cfg
sudo debconf-get-selections >> preseed.cfg
sudo debconf-set-selections -c preseed.cfg
###>>>a l installation choisir Advanced options / expert et pointer le cfg sur USB
```

_____________________________________________________________________________________
creation d un mirroir local debian
-------------------------------------------------------------------------------------
```bash
dpkg -l | awk '{print $2}' > list.sh
sed -i.BAK '1,4d' list.sh

nano list.sh
for package in '
...
...'
do
    fakeroot dpkg-repack $package
done

./list.sh
apt install dpkg-dev
cd packages/
dpkg-scanpackages ./ /dev/null | gzip -9c > Packages.gz (nom non modifiable)

###>>>client
sudo nano /etc/apt/sources.list
deb file:/media/$USER/doc  ./ ###>>>(uniquement)

apt update

###>>>ou simplement
sudo apt-get install aptoncd
```

_____________________________________________________________________________________
creation d un iso live perso
-------------------------------------------------------------------------------------
```bash
apt-get install live-manual live-build
mkdir Live_Build_Work && cd Live_Build_Work
mkdir auto
cp /usr/share/doc/live-build/examples/auto/* auto/

nano auto/config
#!/bin/sh
lb config noauto \
    --architectures amd64 \
    --linux-packages "linux-image" \
    --ignore-system-defaults \
    --bootappend-live "boot=live components autologin username=user"
    "${@}"

nano config/packages-list/live.list.chroot
linux-image-amd64
...
###>>>liste de tous les paquets souhaites

mkdir -p /config/includes.chroot/etc/skel
###>>>copier tous les fichiers de configurations

lb build
```

_____________________________________________________________________________________
compilation
-------------------------------------------------------------------------------------
```bash
sudo apt install build-essential
wget https://SOFT.tar.bz2
tar -xf SOFT.tar.bz2 
cd SOFT/
./configure 
make
sudo make install
```