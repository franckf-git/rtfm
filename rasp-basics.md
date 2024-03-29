## Index

- [sauvegarde media vers image](#sauvegarde-media-vers-image)
- [coucher une image sur un media](#coucher-une-image-sur-un-media)
- [installation archlinux](#installation-archlinux)
- [installation fedora](#installation-fedora)
- [stopper le resize automatique](#stopper-le-resize-automatique)
- [utiliser les cartes SD en lecture seule](#utiliser-les-cartes-sd-en-lecture-seule)
- [overclock raspberry pi 3](#overclock-raspberry-pi-3)
- [boot sur usb](#boot-sur-usb)
- [pi zero USB OTG device](#pi-zero-usb-otg-device)
- [Expansion GPIO](#expansion-gpio)
- [gestion LED au demarrage](#gestion-led-au-demarrage)
- [desactiver USB](#desactiver-usb)
- [gestion des logs pour card sd](#gestion-des-logs-pour-card-sd)
- [ecran waveshare](#ecran-waveshare)
- [screensaver CLI](#screensaver-cli)
- [bloquer wifi et bluetooth](#bloquer-wifi-et-bluetooth)
- [jumellage bluetooth](#jumellage-bluetooth)
- [activation des licences mpeg 2 (domaine public)](#activation-des-licences-mpeg-2-domaine-public)
- [picamera](#picamera)
- [nouvel utilisateur](#nouvel-utilisateur)
- [capture video pour 30 min (-t 0 pour infini)](#capture-video-pour-30-min--t-0-pour-infini)
- [slowmotion](#slowmotion)
- [surveillance motion](#surveillance-motion)
- [serveur streaming vlc](#serveur-streaming-vlc)
- [ip statique](#ip-statique)
- [wifi config](#wifi-config)
- [hotspot](#hotspot)
- [VNC](#vnc)
- [RPI monitor](#rpi-monitor)
- [pihole bloqueur pub](#pihole-bloqueur-pub)
- [mode kiosque](#mode-kiosque)
- [compilation opencv (3h sur raspberry 3)](#compilation-opencv-3h-sur-raspberry-3)

_____________________________________________________________________________________
sauvegarde media vers image
-------------------------------------------------------------------------------------
```bash
sudo dd if=/dev/mmcblk0 of=~/Desktop/RASPBERRYPI.img
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
coucher une image sur un media
-------------------------------------------------------------------------------------
```bash
sudo dd bs=1024 if=/raspbian-jessie.img of=/dev/SDCARD
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
installation archlinux
-------------------------------------------------------------------------------------
```bash
###>>>Start fdisk to partition the SD card:
fdisk /dev/sdX

###>>>At the fdisk prompt, delete old partitions and create a new one:
###>>>Type o. This will clear out any partitions on the drive.
###>>>Type p to list partitions. There should be no partitions left.
###>>>Type n, then p for primary, 1 for the first partition on the drive, press ENTER to accept the default first sector, then type +100M for the last sector.
###>>>Type t, then c to set the first partition to type W95 FAT32 (LBA).
###>>>Type n, then p for primary, 2 for the second partition on the drive, and then press ENTER twice to accept the default first and last sector.
###>>>rite the partition table and exit by typing w.

###>>>Create and mount the FAT filesystem:
mkfs.vfat /dev/sdX1
mkdir boot
mount /dev/sdX1 boot
###>>>Create and mount the ext4 filesystem:
mkfs.ext4 /dev/sdX2
mkdir root
mount /dev/sdX2 root
###>>>Download and extract the root filesystem (as root, not via sudo):
wget http://os.archlinuxarm.org/os/ArchLinuxARM-rpi-2-latest.tar.gz
bsdtar -xpf ArchLinuxARM-rpi-2-latest.tar.gz -C root
sync
###>>>Move boot files to the first partition:
mv root/boot/* boot
###>>>Unmount the two partitions:
umount boot root
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
installation fedora
-------------------------------------------------------------------------------------
```bash
wget http://mirrors.ircam.fr/pub/fedora/linux/releases/26/Spins/armhfp/images/Fedora-LXQt-armhfp-26-1.5-sda.raw.xz
xzcat Fedora-IMAGE-NAME.raw.xz | sudo dd status=progress bs=4M of=/dev/XXX
gparted /dev/XXX
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
stopper le resize automatique
-------------------------------------------------------------------------------------
```bash
sudo nano /boot/cmdline.txt
###>>>retirer init=/usr/lib/raspi-config/init_resize.sh
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
utiliser les cartes SD en lecture seule
-------------------------------------------------------------------------------------
```bash
###>>>modifications dans le micrologiciel donc irreversible !
###>>>seulement sur la version lite de raspbian
wget https://raw.githubusercontent.com/adafruit/Raspberry-Pi-Installer-Scripts/master/read-only-fs.sh
sudo bash read-only-fs.sh
#jumper yes on GPIO21
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
overclock raspberry pi 3
-------------------------------------------------------------------------------------
```bash
cat /sys/class/thermal/thermal_zone0/temp
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
lscpu | grep max

echo "performance" | sudo tee /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor

sudo nano /boot/config.txt
gpu_freq=450            #Frequence du GPU
arm_freq=(1200-1400)    #Frequence du CPU
over_voltage=(3-6)      #Puissance electrique envoyee au CPU/GPU (4 = 1.3V)
disable_splash=1        #Desactive l affichage de l ecran d alerte electrique
sdram_freq=(400-475)

###>>>hold down the shift key during the boot to temporarily disable all overclocking
cat /opt/vc/bin/vcgencmd measure_temp # Afficher la température du processeur
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
boot sur usb
-------------------------------------------------------------------------------------
```bash
###>>>coucher une raspbian lite (2017_04_10 ou plus) sur carte sd
echo program_usb_boot_mode=1 | sudo tee -a /boot/config.txt
sudo reboot
vcgencmd otp_dump | grep 17:
#17:3020000a
###>>>le OTP (One Time Programmable) memory est reprogramme pour l usb
###>>>coucher une raspbian (2017_04_10 ou plus) sur cle usb
###>>>retirer la carte sd brancher usb et redemmarer
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
pi zero USB OTG device
-------------------------------------------------------------------------------------
```bash
sudo nano /boot/config.txt
###>>>rajout a la fin
dtoverlay=dwc2

> /boot/ssh #active le ssh sur raspian

wpa_supplicant.conf #configuration du wifi

country=gb
update_config=1
ctrl_interface=/var/run/wpa_supplicant
network={
scan_ssid=1
ssid="Your network name"
psk="Your network password"
}

sudo nano /boot/cmdline.txt
modules-load=dwc2,g_ether ###>>>entre rootwait et quiet

###>>>connect via USB
ssh pi@raspberrypi.local

###>>>windows>network>properties>sharing>allow USB/RNDIS ethernet reboot
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Expansion GPIO
-------------------------------------------------------------------------------------
```bash
###>>>utiliser la version x86 de Raspbian pour piloter un Raspberry 0 en USB
sudo apt install usbbootgui

###>>>brancher le Raspberry 0 en USB sans carte SD
###>>>choisir GPIO expansion board
ifconfig
###>>>usb0

export GPIOZERO_PIN_FACTORY=pigpio
export PIGPIO_ADDR=fe80::1%usb0
python3
>>> from gpiozero import LED
>>> led = LED(17)
>>> led.blink()
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gestion LED au demarrage
-------------------------------------------------------------------------------------
```
PWR (rouge) - la LED d alimentation
eteinte  : verifier l alimentation du Pi
clignote : regulateur 3.3 volts n a plus assez de tension

ACT (verte) - la LED d activite du Pi
eteinte  : probleme avec la carte SD, formatage, corruption, version
3 eclats : start.elf introuvable
4 eclats : start.elf pas lance
7 eclats : kernel.img introuvable

echo 0 >/sys/class/leds/led0/brightness      # Éteindre la LED verte (ACT)
echo 1 >/sys/class/leds/led0/brightness      # la rallumer
echo 0 >/sys/class/leds/led1/brightness      # Éteindre la LED rouge (PWR)
echo 1 >/sys/class/leds/led1/brightness      # la rallumer
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
desactiver USB
-------------------------------------------------------------------------------------
```bash
sudo sh -c "echo 0 > /sys/devices/platform/soc/3f980000.usb/buspower"
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gestion des logs pour card sd
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/fstab
###>>>rajouter a la fin
tmpfs /tmp tmpfs defaults,noatime,nosuid,size=100m 0 0
tmpfs /var/tmp tmpfs defaults,noatime,nosuid,size=30m 0 0
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
ecran waveshare
-------------------------------------------------------------------------------------
```bash
###>>>Turn on the "backlight" switch then connect the LCD to your Pi (HDMI Port of LCD -> HDMI Port of Pi; USB Port of LCD -> USB Port of Pi; 5V~2A power supply). Download the Raspbian image from Raspberry Pi web site. Write the image to a TF card and append the following lines to the config.txt file which is located in the root of your TF card:
sudo nano /boot/config.txt
max_usb_current=1
hdmi_group=2
hdmi_mode=87
hdmi_cvt 1024 600 60 6 0 0 0
hdmi_drive=1
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
screensaver CLI
-------------------------------------------------------------------------------------
```bash
###>>>set the console blanking in seconds
cat /sys/module/kernel/parameters/consoleblank

###>>>to turn screen blanking off or set the number of seconds of inactivity
sudo nano /boot/cmdline.txt
consoleblank=0
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
bloquer wifi et bluetooth
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/modprobe.d/raspi-blacklist.conf
#wifi
blacklist brcmfmac
blacklist brcmutil
#bluetooth
blacklist btbcm
blacklist hci_uart
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
jumellage bluetooth
-------------------------------------------------------------------------------------
```bash
###>>>CLI
sudo bluetoothctl
agent on
default-agent
scan on
pair 01:02:03:04:05:06:07:08
connect 01:02:03:04:05:06:07:08

sudo nano /etc/bluetooth/main.conf
[Policy]
AutoEnable=true

###>>>GUI
sudo apt-get install bluetooth bluez blueman
###>>>Bluetooth Manager
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
activation des licences mpeg 2 (domaine public)
-------------------------------------------------------------------------------------
```bash
cd /boot
sudo perl -i.bak -pe 's/\x47\xE9362H\x3C\x18/\x47\xE9362H\x3C\x1F/g' start.elf
vcgencmd codec_enabled MPG2/WVC1
#doit afficher enabled
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
picamera
-------------------------------------------------------------------------------------
```bash
sudo apt-get install python-picamera
sudo apt-get install python3-picamera
sudo apt-get install python-picamera-docs
```
```python
import picamera
camera = picamera.PiCamera()

camera.capture('IMAGE.jpg')

camera.hflip = True
camera.vflip = True

camera.start_preview()
camera.stop_preview()

camera.brightness = 70
camera.sharpness = 0
camera.contrast = 0
camera.brightness = 50
camera.saturation = 0
camera.ISO = 0
camera.video_stabilization = False
camera.exposure_compensation = 0
camera.exposure_mode = 'auto'
camera.meter_mode = 'average'
camera.awb_mode = 'auto'
camera.image_effect = 'none'
camera.color_effects = None
camera.rotation = 0
camera.hflip = False
camera.vflip = False
camera.crop = (0.0, 0.0, 1.0, 1.0)

camera.start_recording('VIDEO.h264')
sleep(5)
camera.stop_recording()
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
nouvel utilisateur
-------------------------------------------------------------------------------------
```bash
sudo adduser USER
sudo adduser USER sudo

sudo visudo
USER ALL=(ALL) NOPASSWD: ALL

sudo deluser --remove-home pi
sudo usermod -a -G video USER
sudo login USER
sudo raspi-config
#3/B1/B2
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
capture video pour 30 min (-t 0 pour infini)
-------------------------------------------------------------------------------------
```bash
raspivid -n -vf -hf -fps 25 -w 1280 -h 720 -b 3500000 -t 1800000 -o - > VIDEO.h264
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
slowmotion
-------------------------------------------------------------------------------------
```bash
#! /bin/bash
SAVEDIR=/home/pi/capture
while [ true ]; do
filename=$(date -u +"%d%m%Y_%H%M").jpg
/opt/vc/bin/raspistill -o $SAVEDIR/$filename
sleep 30m;
done;
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
surveillance motion
-------------------------------------------------------------------------------------
```bash
sudo apt-get install motion

sudo nano /etc/motion/motion.conf
daemon on
framerate 2 
width 640
heigh 480
ffmpeg_video_codec mpeg4 
webcam_localhost off 

sudo nano /etc/default/motion
start_motion_daemon=yes

sudo service motion start
###>>>http://IPRASPBERRY:8081
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
serveur streaming vlc
-------------------------------------------------------------------------------------
```bash
###>>>serveur
raspivid -t 0 -n --width 1280 --height 720 -o - | cvlc stream:///dev/stdin --sout '#standard{access=http,mux=ts,dst=:8090}' :demux=h264
###>>>client
cvlc http://IPRASPBERRY:8090
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
ip statique
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/network/interfaces
auto wlan0
iface lo inet loopback
iface eth0 inet static
address IPRASPBERRYETH0
netmask 255.255.255.0
gateway 192.168.1.1
allow-hotplug wlan0
iface wlan0 inet static
address IPRASPBERRYWLAN
netmask 255.255.255.0
gateway 192.168.1.1
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
wifi config
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
network={
ssid="BOXSSID"
psk="BOXSSIDPASSWORD"
key_mgmt=WPA-PSK
}

sudo nano/etc/network/interfaces
auto wlan0
iface wlan0 inet dhcp
   wpa-ssid BOXSSID
   wpa-psk BOXSSIDPASSWORD

###>>>recherche ip dynamique
sudo dhclient wlan0
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
hotspot
-------------------------------------------------------------------------------------
```bash
sudo apt-get install dnsmasq hostapd
sudo systemctl stop dnsmasq
sudo systemctl stop hostapd

###>>>IP static
sudo nano /etc/dhcpcd.conf
###>>>a la fin
denyinterfaces wlan0

sudo nano /etc/network/interfaces
allow-hotplug wlan0
iface wlan0 inet static  
    address 192.168.0.1
    netmask 255.255.255.0
    network 192.168.0.0

sudo service dhcpcd restart
sudo ifdown wlan0
sudo ifup wlan0

###>>>serveur dhcp
sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig  

sudo nano /etc/dnsmasq.conf
# Interface a utiliser
interface=wlan0
# Limiter le fonctionnement a linterface declaree
bind-interfaces
# Transferer les requetes DNS sur les DNS d OpenDns
server=208.67.222.222
# Ne pas forwarder les requetes locales (sans nom de domaine)
domain-needed
# Fausse resolution inverse pour les reseaux prives. Toutes les requetes DNS inverses pour des adresses IP privees (ie 192.168.x.x, etc…) qui ne sont pas trouvees dans /etc/hosts ou dans le fichier de baux DHCP se voient retournees une reponse pas de tel domaine (no such domain) au lieu detre transmises
bogus-priv
# Plage DHCP : on attribuera les adresses IP pour une duree de 12h
dhcp-range=192.168.0.2,192.168.0.20,255.255.255.0,12h

###>>>point d access
sudo nano /etc/hostapd/hostapd.conf
# Nom de linterface wifi (celle par défaut)
interface=wlan0
# Utilisation du driver nl80211, 
driver=nl80211
# Nom du réseau wifi (SSID)
ssid=RASPBERRYSSID
# Mode du wifi (bande 2.4GHz)
hw_mode=g
# Canal 6
channel=6
# Activer le 802.11n
ieee80211n=1
# Activer le WMM
wmm_enabled=1
# Activer les canaux 40MHz avec 20ns dintervalle
ht_capab=[HT40][SHORT-GI-20][DSSS_CCK-40]
# Accepter toutes les adresses MAC (pas de restriction dappareil)
macaddr_acl=0
# Utiliser lauthentification WPA
auth_algs=1
# Ne pas diffuser le SSID
ignore_broadcast_ssid=0
# Utiliser WPA2
wpa=2
# Utiliser une cle partagee
wpa_key_mgmt=WPA-PSK
# Mot de passe du réseau
wpa_passphrase=RASPBERRYSSIDPASSWORD
# Utilisation dAES plutôt que de TKIP
rsn_pairwise=CCMP

sudo /usr/sbin/hostapd /etc/hostapd/hostapd.conf

sudo nano /etc/default/hostapd
DAEMON_CONF="/etc/hostapd/hostapd.conf"

sudo service hostapd start  
sudo service dnsmasq start

###>>>partage de connexion internet
sudo apt-get install hostapd bridge-utils
sudo systemctl stop hostapd

sudo nano /etc/dhcpcd.conf
###>>>a la fin
denyinterfaces wlan0
denyinterfaces eth0

sudo brctl addbr br0
sudo brctl addif br0 eth0 wlan0

sudo nano /etc/network/interfaces
allow-hotplug wlan0
iface wlan0 inet manual
auto br0
iface br0 inet dhcp
bridge_ports eth0 wlan0

sudo nano /etc/hostapd/hostapd.conf
###>>>comme pour point d access modifier :
bridge=br0
#driver=nl80211
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
VNC
-------------------------------------------------------------------------------------
```bash
sudo apt-get install tightvncserver
vncserver :1

###>>>installer VNC viewer sur la machine hote et entrer IPRASPBERRYETH0:1
cd /etc/init.d

sudo nano vncboot
USER=pi
HOME=/home/pi

export USER HOME

case "$1" in
 start)
  echo "Starting VNC Server"
  #Insert your favoured settings for a VNC session
  su - $USER -c "/usr/bin/vncserver :1 -geometry 1280x800 -depth 16 -pixelformat rgb565"
  ;;

 stop)
  echo "Stopping VNC Server"
  /usr/bin/vncserver -kill :1
  ;;

 *)
  echo "Usage: /etc/init.d/vncboot {start|stop}"
  exit 1
  ;;
esac

exit 0

sudo chmod +x /etc/init.d/vncboot
sudo update-rc.d vncboot defaults
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
RPI monitor
-------------------------------------------------------------------------------------
```bash
sudo wget http://goo.gl/vewCLL -O /etc/apt/sources.list.d/rpimonitor.list
sudo apt-key adv --recv-keys --keyserver keyserver.ubuntu.com 2C0D3C0F
sudo apt-get update
sudo apt-get install rpimonitor
sudo /etc/init.d/rpimonitor update
/etc/init.d/rpimonitor restart

###>>>http://IPRASPBERRYPI:8888/
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
pihole bloqueur pub
-------------------------------------------------------------------------------------
```bash
curl -sSL https://install.pi-hole.net | bash
###>>>ports a ouvrir : 53 80 4711
###>>>http://IPRASPBERRY/admin/
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
mode kiosque
-------------------------------------------------------------------------------------
```bash
sudo apt install x11-xserver-utils
sudo raspi-config
mode desktop autologin

sudo nano /etc/xdg/lxsession/LXDE-pi/autostart
#@lxpanel --profile LXDE
#@pcmanfm --desktop --profile LXDE
#@xscreensaver -no-splash
@xset s off
@xset -dpms
@/usr/bin/chromium-browser --noerrdialogs --kiosk --incognito "https://URL"
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
rasp dashboard
-------------------------------------------------------------------------------------
```bash
#!/bin/bash

# Install dependencies
sudo apt-get update
sudo apt-get install -y i3 dmenu xorg xinit chromium

# Autologin
sudo systemctl enable autologin@.service

# i3 configuration
mkdir -p ~/.i3 && cat > ~/.i3/config << EOF
    exec --no-startup-id xset -dpms
    exec --no-startup-id xset s off
    exec --no-startup-id xdotool mousemove 9999 9999
    exec --no-startup-id chromium https://framasoft.fr
    exec --no-startup-id sleep 5 && xdotool key F11
EOF
echo 'exec i3' >> ~/.xinitrc
echo '[[ -z $DISPLAY && $XDG_VTNR -eq 1 ]] && exec startx' > ~/.bash_profile

exit 0
```

**[`^        back to top        ^`](#)**
_____________________________________________________________________________________
compilation opencv (3h sur raspberry 3)
-------------------------------------------------------------------------------------
```bash
sudo apt-get install build-essential cmake pkg-config
sudo apt-get install libjpeg-dev libtiff5-dev libjasper-dev libpng12-dev
sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev
sudo apt-get install libxvidcore-dev libx264-dev
sudo apt-get install libgtk2.0-dev
sudo apt-get install libatlas-base-dev gfortran
sudo apt-get install python2.7-dev python3-dev
wget -O opencv.zip https://github.com/Itseez/opencv/archive/3.3.0.zip
unzip opencv.zip
wget -O opencv_contrib.zip https://github.com/Itseez/opencv_contrib/archive/3.3.0.zip
unzip opencv_contrib.zip
wget https://bootstrap.pypa.io/get-pip.py
sudo python get-pip.py
sudo pip install virtualenv virtualenvwrapper
sudo rm -rf ~/.cache/pip
echo -e "\n# virtualenv and virtualenvwrapper" >> ~/.profile
echo "export WORKON_HOME=$HOME/.virtualenvs" >> ~/.profile
echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.profile
reboot
source ~/.profile
mkvirtualenv cv -p python3

###>>>(source ~/.profile
###>>>workon cv)
###>>>(cv)
pip install numpy
cd opencv-3.3.0/
mkdir build
cd build
cmake -D CMAKE_BUILD_TYPE=RELEASE \
#    -D CMAKE_INSTALL_PREFIX=/usr/local \
#    -D INSTALL_PYTHON_EXAMPLES=ON \
#    -D OPENCV_EXTRA_MODULES_PATH=~/opencv_contrib-3.3.0/modules \
#    -D BUILD_EXAMPLES=ON ..
#--   Python 3:
#--     Interpreter:                 /home/pi/.virtualenvs/cv/bin/python3 (ver 3.5.3)
#--     Libraries:                   /usr/lib/arm-linux-gnueabihf/libpython3.5m.so (ver 3.5.3)
#--     numpy:                       /home/pi/.virtualenvs/cv/lib/python3.5/site-packages/numpy/core/include (ver 1.13.1)
#--     packages path:               lib/python3.5/site-packages

make -j4|| make clean && make
sudo make install
sudo ldconfig
ls -l /usr/local/lib/python3.4/site-packages/
###>>>cv2.cpython-35m-arm-linux-gnuabihf.so
cd /usr/local/lib/python3.4/site-packages/
sudo mv cv2.cpython-35m-arm-linux-gnuabihf.so cv2.so
cd ~/.virtualenvs/cv/lib/python3.4/site-packages/
ln -s /usr/local/lib/python3.4/site-packages/cv2.so cv2.so

source ~/.profile 
workon cv
python
#>>> import cv2
#>>> cv2.__version__
#'3.3.0'
```

**[`^        back to top        ^`](#)**
