## Index

[TOC]

- [wine](#wine)
- [back in time outil de sauvegarde graphique](#back-in-time-outil-de-sauvegarde-graphique)
- [installation de steam](#installation-de-steam)
- [chromium authoriser l installation d extensions](#chromium-authoriser-l-installation-d-extensions)
- [install de chrome](#install-de-chrome)
- [install de Brave](#install-de-brave)
- [install de firefox 57](#install-de-firefox-57)
- [install multimedia fedora](#install-multimedia-fedora)
- [minimal desktop](#minimal-desktop)
- [minimal install of centos](#minimal-install-of-centos)
- [proxmox interface web en local](#proxmox-interface-web-en-local)
- [proxmox connexions web](#proxmox-connexions-web)
- [proxmox retirer le popup](#proxmox-retirer-le-popup)
- [transfer.sh](#transfersh)
- [nikto scanner pour serveur web](#nikto-scanner-pour-serveur-web)
- [webmin administration](#webmin-administration)
- [cockpit surveillance](#cockpit-surveillance)
- [BoopSuite wifi](#boopsuite-wifi)

_____________________________________________________________________________________
wine
-------------------------------------------------------------------------------------
```bash
sudo dpkg --add-architecture i386
sudo apt install wine32 
wine PROGRAM.exe
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
back in time outil de sauvegarde graphique
-------------------------------------------------------------------------------------
```bash
###>>>Ubuntu PPA
sudo add-apt-repository ppa:bit-team/stable
sudo apt-get update
sudo apt-get install backintime-qt4

###>>>Debian/Ubuntu make packages
wget https://github.com/bit-team/backintime/releases/download/v1.1.20/backintime-1.1.20.tar.gz
./makedeb.sh
sudo dpkg -i ../backintime-common-1.1.20.deb
sudo dpkg -i ../backintime-qt-1.1.20.deb
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
installation de steam
-------------------------------------------------------------------------------------
```bash
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install steam
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
chromium authoriser l installation d extensions
-------------------------------------------------------------------------------------
```bash
apt-get update && apt-get install chromium chromium-l10n
cd /etc/chromium.d
sudo nano user-flags
export CHROMIUM_FLAGS="$CHROMIUM_FLAGS --enable-remote-extensions"

chromium --incognito
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
install de chrome et iridium (chrome sans google)
-------------------------------------------------------------------------------------
```bash
#pour fedora
sudo dnf install fedora-workstation-repositories
sudo dnf config-manager --set-enabled google-chrome
sudo dnf install google-chrome-stable

#pour debian
wget -q -O - https://dl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
echo "deb http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee /etc/apt/sources.list.d/google-chrome.list
sudo apt-get update
sudo apt install google-chrome-stable -y

dnf config-manager --add-repo https://dl.iridiumbrowser.de/fedora_28/iridium-browser.repo
dnf install iridium-browser
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
install de brave
-------------------------------------------------------------------------------------
```bash
sudo dnf config-manager --add-repo https://brave-browser-rpm-release.s3.brave.com/x86_64/
sudo rpm --import https://brave-browser-rpm-release.s3.brave.com/brave-core.asc
sudo dnf install brave-keyring brave-browser
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
install de firefox 57
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/apt/sources.list
deb http://ftp.fr.debian.org/debian/ unstable main contrib non-free

sudo apt update
sudo apt install -V -t unstable firefox firefox-l10n-fr

sudo nano /etc/apt/sources.list
#deb http://ftp.fr.debian.org/debian/ unstable main contrib non-free
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
install multimedia fedora
-------------------------------------------------------------------------------------
```bash
###>>>rip dvd
dnf config-manager --add-repo=https://negativo17.org/repos/fedora-multimedia.repo
dnf -y install ffmpeg HandBrake-{gui,cli}

###>>>lecture dvd
dnf config-manager --add-repo=https://negativo17.org/repos/fedora-multimedia.repo
dnf -y install libdvdcss libdvdread libdvdnav
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
minimal desktop
-------------------------------------------------------------------------------------
```bash
sudo apt-get install chromium-browser xinit openbox obconf obmenu xterm leafpad wicd pulseaudio tint2

feh --bg-scale /ADRESSE/VERS/VOTRE/IMAGE.jpg ###>>>(wallpaper)

nano ~/.config/openbox/autostart.sh
#! /bin/bash
numlockx on &
conky &
wicd &
wicd-client &
feh

###>>>OU dans fedora

dnf install i3 lightdm
dnf group install base-x
systemctl enable lightdm.service
systemctl set-default graphical.target
systemctl isolate graphical.target
echo "exec i3" >> ~/.xinitrc #si pas de lightdm-gtk
startx
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
minimal install of centos
-------------------------------------------------------------------------------------
```bash
#install centos from minimal cd
#use source
mirror.CentOS.org/CentOS/7.5.1804/os/x86_64/
#dont check use "This URL refers to a mirror list"

yum install epel-release
yum groupinstall "X Window System" "Desktop" "Desktop Platform"
yum install lightdm xorg-x11-xinit-session dejavu-sans-mono-fonts i3 i3status rxvt-unicode
systemctl set-default graphical.target
systemctl isolate graphical.target
#less than 700 packages
#it is really working (for once)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
proxmox interface web en local
-------------------------------------------------------------------------------------
```bash
apt install xorg i3 xinit chromium wicd-curses xterm
echo "exec --no-startup-id chromium --no-sandbox --start-fullscreen https://localhost:8006" >> .config/i3/config
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
proxmox connexions web
-------------------------------------------------------------------------------------
```bash
nano /etc/networks/interfaces
#configuration pour machines virtuelles
auto lo
iface lo inet loopback

iface enp3s0 inet manual

auto vmbr0
iface vmbr0 inet static
        address 192.168.1.58
        netmask 255.255.255.0
        gateway 192.168.1.255
        bridge_ports enp3s0
        bridge_stp off
        bridge_fd 0

#configuration pour hyperviseur
auto lo
iface lo inet loopback

auto enp3s0
iface enp3s0 inet dhcp
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
proxmox retirer le popup
-------------------------------------------------------------------------------------
```bash
vi /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js
if (data.status == 'Active') {
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
transfer.sh
-------------------------------------------------------------------------------------
```bash
wget --method PUT --body-file=/tmp/file.tar https://transfer.sh/file.tar -O - -nv

###>>>Encrypt files with password using gpg 
cat /tmp/TEXT.txt|gpg -ac -o-|curl -X PUT --upload-file "-" https://transfer.sh/TEXT.txt 
###>>>Download and decrypt 
curl https://transfer.sh/ID/TEXT.txt|gpg -o- > /tmp/TEXT.txt
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
nikto scanner pour serveur web
-------------------------------------------------------------------------------------
```bash
git clone https://github.com/sullo/nikto.git
cd nikto/program
perl nikto.pl -update
perl nikto.pl -h 192.168.0.1
nikto -host SITEWEB.COM -port 443 -ssl
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
webmin administration
-------------------------------------------------------------------------------------
```bash
su
echo "deb http://download.webmin.com/download/repository sarge contrib" >> /etc/apt/sources.list
cd /root
wget http://www.webmin.com/jcameron-key.asc
apt-key add jcameron-key.asc
apt-get update
apt-get install apt-transport-https
apt-get install webmin
/etc/init.d/webmin start
###>>>https://IP_OU_NOM_SERVEUR:10000/

###>>>ou
nano /etc/yum.repos.d/webmin.repo
[Webmin]
name=Webmin Distribution Neutral
#baseurl=http://download.webmin.com/download/yum
mirrorlist=http://download.webmin.com/download/yum/mirrorlist
enabled=1

wget http://www.webmin.com/jcameron-key.asc
rpm --import jcameron-key.asc
yum install webmin
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
cockpit surveillance
-------------------------------------------------------------------------------------
```bash
echo 'deb http://deb.debian.org/debian stretch-backports main' > /etc/apt/sources.list.d/backports.list
apt-get update
sudo apt-get install cockpit

###>>>https://IP_ADDRESS:9090
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
BoopSuite wifi
-------------------------------------------------------------------------------------
```bash
git clone https://github.com/M1ND-B3ND3R/BoopSuite.git
cd BoopSuite
sudo pip install -r requirements.txt
./install.py
boop -i wlan0
boopsniff -i wlan0mon
boopstrike -i wlan0mon -a MAC:AD:RE:SS:XX:XX
cd ~/pcap
aircrack-ng -a2 -b MAC:AD:RE:SS:XX:XX -w wordlist.txt fichier.pcap
```

**[`^        back to top        ^`](#)**
