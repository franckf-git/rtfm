## Index

[TOC]

- [Gestion des paquets debian](#gestion-des-paquets-debian)
- [Gestion des paquets fedora vs debian](#gestion-des-paquets-fedora-vs-debian)
- [Gestion des paquets fedora](#gestion-des-paquets-fedora)
- [flatpak](#flatpak)
- [snap](#snap)
- [Gestion des paquets archlinux](#gestion-des-paquets-archlinux)
- [Gestion des paquets openSUSE](#gestion-des-paquets-opensuse)
- [Gestion des paquets Solus](#gestion-des-paquets-solus)
- [MAJ debian](#maj-debian)
- [MAJ fedora](#maj-fedora)
- [fedora RPM fusion](#fedora-rpm-fusion)
- [Dépots supplémentaires fedora](#dépots-supplémentaires-fedora)
- [fedora mise à niveau](#fedora-mise-à-niveau)
- [centos ajout de depots](#centos-ajout-de-depots)
- [centos priorite des depots](#centos-priorite-des-depots)
- [proxmox depot communautaire](#proxmox-depot-communautaire)
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
- [Vundle (package manager for vim)](#vundle--package-manager-for-vim-)

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

**[`^        back to top        ^`](#)**

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
alien --to-rpm PCK.deb        #conversion

#in bashrc
case $(lsb_release -i | awk '{ print $3 }') in
Ubuntu|Debian)
alias upgrade='sudo apt update && sudo apt -V upgrade && sudo apt-get autoremove -y && sudo apt-get clean'
;;
CentOS|Fedora)
alias upgrade='sudo dnf check-update ; sudo dnf upgrade ; sudo dnf clean packages -y'
;;
esac
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Gestion des paquets fedora
-------------------------------------------------------------------------------------
```bash
dnf repolist all                           #Lister les dépots disponibles
dnf install PACKAGE                        #Installer un paquet
dnf remove PACKAGE                         #Supprimer un paquet
dnf autoremove PACKAGE                     #Supprimer un paquet proprement
dnf erase PACKAGE                          #Supprimer les fichiers de configuration
dnf update PACKAGE                         #Mettre à jour un paquet
dnf check-update                           #Verifier les mises à jour disponibles
dnf check                                  #identifier les problèmes dans packagedb
dnf deplist                                #Liste les dépendances du paquet
dnf upgrade                                #Mettre à jour tous les paquets
dnf --exclude=PAQUET update                #exclure un paquet d une mise à jour
dnf downgrade PAQUET                       #Rétrograder un paquet
dnf grouplist                              #Liste les groupes de paquets disponibles
dnf grouplist hidden                       #tous les groupes
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
dnf makecache                              #Créer un cache manuel
dnf clean all                              #Supprimer le cache
dnf history                                #Historique des installations
dnf history info ID                        #Details sur une installation
dnf autoremove                             #Supprimer les paquets inutilisés
dnf distro-sync                            #Synchronise dans la dernière version stable
dnf reinstall PACKAGE                      #Reinstaller un paquet
dnf upgrade-to PACKAGE-VERSION.fcNUM       #Installer un paquet dans une version
dnf upgrade-minimal                        #MAJ les paquets qui affectent votre système
dnf copr enable USER/PROJECT               #Installer un projet hors dépot
dnf help                                   #Aide
dnf mark                                   #marquer paquets installés par utilisateur
dnf shell                                  #exécute un interpréteur DNF interactif
dnf updateinfo                             #affiche des avertissements sur les paquets
dnf builddep                               #Install build dependencies for package
dnf config-manager                         #Gestion de la config et des dépôts dnf
dnf debug-dump                             #déverse les infos des rpm dans le fichier
dnf debug-restore                          #restaure les paquets enreg dans débuggage
dnf debuginfo-install                      #installe les paquets debuginfo
dnf needs-restarting                       #liste les binaires qui besion redémarrage
dnf playground                             #Interagit avec le dépôt Playground
dnf repoclosure                            #Affiche liste de dépendances non résolues
dnf repograph                              #Sortie d un graphe de dépendance
dnf repomanage                             #Gère un dossier de paquets rpm
dnf reposync                               #télécharge tous les paquets depuis distant
dnf info-security                          #affiche le détails des modifications
dnf check-update --security                #affiche les paquets vitaux
dnf check-update --sec-severity {Critical,Important,Moderate,Low,None}
rpm -Va                                    #verification de l intégrité des paquets

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

###>>>Nombre de kernel disponibles au démarrage
sudo nano /etc/dnf/dnf.conf
installonly_limit=2

###>>>Changement de version
sudo dnf upgrade --refresh
sudo dnf install dnf-plugin-system-upgrade
sudo dnf system-upgrade download --releasever=NEWNUM
sudo dnf system-upgrade reboot

###>>>Détails des mirroirs
cat /etc/yum.repos.d/fedora.repo
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
flatpak
-------------------------------------------------------------------------------------
```bash
###>>>Activer flatpak
dnf install flatpak
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

flatpak remote-ls REPO --app             # list all availables apps
flatpak remote-ls flathub --app
flatpak search NAME                      # search for a package
flatpak info NAME                        # info on a package
flatpak install REPO NAME                # install an apps from a repo
flatpak install flathub com.valvesoftware.Steam
flatpak run NAME                         # run an apps
flatpak run --unshare=network --nosocket=ssh-auth --nodevice=all --nofilesystem=host --filesystem=home # some security
flatpak run --sandbox NAME               # full security
--socket=x11                             #Show windows using X11
--share=ipc                              #Share IPC namespace with the host
--device=dri                             #OpenGL rendering
--socket=wayland                         #Show windows using Wayland
--socket=pulseaudio                      #Play sounds using PulseAudio
flatpak list                             # list all installed apps
flatpak update NAME                      # update an apps
flatpak uninstall NAME                   # remove an apps
flatpak uninstall --unused               # delete orphans runtines
.local/share/flatpak/                    # configuration de flatpak
.var/app/                                # configuration des apps
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
snap
-------------------------------------------------------------------------------------
```bash
###>>>Activer snap
sudo dnf install snapd
sudo ln -s /var/lib/snapd/snap /snap
sudo systemctl start snapd.service # opt ??
sudo systemctl enable snapd.service

snap search NAME           # search an apps
snap find                  # search an apps in the repos
snap list NAME             # search an apps
sudo snap install NAME     # install an apps
snap info NAME
snap list                  # list all installed apps
sudo snap refresh NAME     # update an installed snap app
sudo snap remove NAME      # remove a snap
snap services              # list enabled services
```

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
MAJ debian
-------------------------------------------------------------------------------------
```bash
sudo apt-get update -y && sudo apt-get upgrade -y && sudo apt-get dist-upgrade -y && sudo apt-get autoremove -y && sudo rpi-update && sudo reboot
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
MAJ fedora
-------------------------------------------------------------------------------------
```bash
sudo dnf check-update && sudo dnf upgrade -y && sudo dnf clean packages -y && sudo reboot
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
fedora RPM fusion
-------------------------------------------------------------------------------------
```bash
###>>>sudo ou su -lc  ...  en fonction de l utilisateur
﻿sudo dnf install https://download1.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm https://download1.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
sudo rpm --import "http://rpmfusion.org/keys?action=AttachFile&do=get&target=RPM-GPG-KEY-rpmfusion-free-fedora-25"
sudo rpm --import "http://rpmfusion.org/keys?action=AttachFile&do=get&target=RPM-GPG-KEY-rpmfusion-nonfree-fedora-25"
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Dépots supplémentaires fedora
-------------------------------------------------------------------------------------
```bash
#Google Chrome
#PyCharm
#NVIDIA graphics drivers
#Steam client
sudo dnf install fedora-workstation-repositories
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
fedora mise à niveau
-------------------------------------------------------------------------------------
```bash
sudo dnf upgrade --refresh
sudo dnf install dnf-plugin-system-upgrade
sudo dnf system-upgrade download --releasever=28
sudo dnf system-upgrade reboot
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
centos ajout de depots
-------------------------------------------------------------------------------------

EPEL Repository
```bash
yum -y install deltarpm
yum-config-manager --enable extras
yum install epel-release
subscription-manager repos --enable "rhel-*-optional-rpms" --enable "rhel-*-extras-rpms"
```

REMI Repository - PHP
```bash
yum install epel-release
wget https://rpms.remirepo.net/enterprise/remi-release-7.rpm
rpm -Uvh remi-release-7.rpm
```

ELRepo Repository - hardware/drivers
```bash
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh https://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm
```

RPMFusion Repository - free and non-free add-on software
```bash
yum install epel-release
yum localinstall --nogpgcheck https://download1.rpmfusion.org/free/el/rpmfusion-free-release-7.noarch.rpm
yum localinstall --nogpgcheck https://download1.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-7.noarch.rpm
```

GhettoForge Repository - add softwares
```bash
rpm -Uvh http://mirror.ghettoforge.org/distributions/gf/gf-release-latest.gf.el7.noarch.rpm
```

Psychotic Ninja - add softwares
```bash
rpm --import http://wiki.psychotic.ninja/RPM-GPG-KEY-psychotic
rpm -ivh http://packages.psychotic.ninja/7/base/x86_64/RPMS/psychotic-release-1.0.0-1.el6.psychotic.noarch.rpm
```

NUX-dextop Repository - desktop and multimedia packages
```bash
yum -y install epel-release
rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
```

IUS Community Repository - latest versions of PHP, Python, MySQL
```bash
yum -y install epel-release
rpm -Uvh https://centos7.iuscommunity.org/ius-release.rpm
```

```bash
yum check-update
yum -y update
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
centos priorite des depots
-------------------------------------------------------------------------------------
```bash
yum install yum-plugin-priorities

vim /etc/yum.repos.d/CentOS-Base.repo
[base]
enabled=1
priority=1
name=CentOS-$releasever - Base
...
[updates]
enabled=1
priority=1
name=CentOS-$releasever - Updates
...
[extras]
enabled=1
priority=1
name=CentOS-$releasever - Extras
...

vim /etc/yum.repos.d/epel.repo
[epel]
enabled=1
priority=10
name=Extra Packages for Enterprise Linux 7 - $basearch

vim /etc/yum.repos.d/nux-dextop.repo
[nux-dextop]
name=Nux.Ro RPMs for general desktop use
baseurl=http://li.nux.ro/download/nux/dextop/el7/$basearch/
enabled=1
priority=10
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-nux.ro
protect=0

yum check-update
yum -y update
```

**[`^        back to top        ^`](#)**
_____________________________________________________________________________________
proxmox depot communautaire
-------------------------------------------------------------------------------------
```bash
nano /etc/apt/sources.list
deb http://ftp.debian.org/debian stretch main contrib
# security updates
deb http://security.debian.org stretch/updates main contrib
# PVE pve-no-subscription repository provided by proxmox.com,
# NOT recommended for production use
deb http://download.proxmox.com/debian/pve stretch pve-no-subscription

apt-get update
apt-get upgrade
```

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
debian mise à niveau
-------------------------------------------------------------------------------------
```bash
sed -i 's/jessie/stretch/g' /etc/apt/sources.list
sudo apt-get update -y && sudo apt-get dist-upgrade -y
```

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
dupliquer un systeme debian
-------------------------------------------------------------------------------------
```bash
dpkg --get-selections > lstpkg.dpkg
dpkg --set-selections < lstpkg.dpkg
apt-get dselect-upgrade
```

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

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

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Vundle (package manager for vim)
-------------------------------------------------------------------------------------
```
#install
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim

vim ~/.vimrc

set nocompatible
filetype off
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
Plugin 'VundleVim/Vundle.vim'

" Add plugins here
Plugin 'airblade/vim-gitgutter'
Plugin 'editorconfig/editorconfig-vim'
Plugin 'itchyny/lightline.vim'
Plugin 'junegunn/fzf.vim'
Plugin 'mattn/emmet-vim'
Plugin 'scrooloose/nerdtree'
Plugin 'terryma/vim-multiple-cursors'
Plugin 'tpope/vim-eunuch'
Plugin 'tpope/vim-surround'
Plugin 'w0rp/ale'

call vundle#end()
filetype plugin indent on

vim
:PluginInstall
```

**[`^        back to top        ^`](#)**

