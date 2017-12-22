## Index

- [wine](#wine)
- [back in time outil de sauvegarde graphique](#back-in-time-outil-de-sauvegarde-graphique)
- [installation de steam](#installation-de-steam)
- [chromium authoriser l installation d extensions](#chromium-authoriser-l-installation-d-extensions)
- [install de chrome](#install-de-chrome)
- [install de firefox 57](#install-de-firefox-57)
- [install multimedia fedora](#install-multimedia-fedora)
- [minimal desktop](#minimal-desktop)
- [transfer.sh](#transfer.sh)
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

_____________________________________________________________________________________
installation de steam
-------------------------------------------------------------------------------------
```bash
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install steam
```

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

_____________________________________________________________________________________
install de chrome
-------------------------------------------------------------------------------------
```bash
apt-get install gconf-service libgconf-2-4 libnspr4 libnss3 libpango1.0-0 libappindicator1 libcurl3
dpkg -i google-chrome-stable_current_amd64.deb
```

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

_____________________________________________________________________________________
install multimedia fedora
-------------------------------------------------------------------------------------
```bash
###>>>rip dvd
dnf config-manager --add-repo=https://negativo17.org/repos/fedora-multimedia.repo
dnf -y install HandBrake-gui HandBrake-cli

###>>>lecture dvd
dnf config-manager --add-repo=https://negativo17.org/repos/fedora-multimedia.repo
dnf -y install libdvdcss libdvdread libdvdnav
```

_____________________________________________________________________________________
minimal desktop
-------------------------------------------------------------------------------------
```bash
sudo apt-get install chromium-browser xinit openbox obconf obmenu xterm leafpad wicd gmrun alsa-utils pulseaudio pcmanfm

feh --bg-scale /ADRESSE/VERS/VOTRE/IMAGE.jpg ###>>>(wallpaper)

nano ~/.config/openbox/autostart.sh
#! /bin/bash
numlockx on &
conky &
wicd &
wicd-client &
feh
```

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

_____________________________________________________________________________________
cockpit surveillance
-------------------------------------------------------------------------------------
```bash
echo 'deb http://deb.debian.org/debian stretch-backports main' > /etc/apt/sources.list.d/backports.list
apt-get update
sudo apt-get install cockpit

###>>>https://IP_ADDRESS:9090
```

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