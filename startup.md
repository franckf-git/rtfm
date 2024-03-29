## Index

- [stopper les services listen inutiles](#stopper-les-services-listen-inutiles)
- [mes dossiers](#mes-dossiers)
- [disque ssd](#disque-ssd)
- [baisser le niveau de mise en swap pour economiser le hdd](#baisser-le-niveau-de-mise-en-swap-pour-economiser-le-hdd)
- [mode laptop gestion plus econome du disque dur](#mode-laptop-gestion-plus-econome-du-disque-dur)
- [securisation de /dev/shm/](#securisation-de-devshm)
- [connexion sans login](#connexion-sans-login)
- [fenetre de connexion lightdm login deroulant](#fenetre-de-connexion-lightdm-login-deroulant)
- [lightdm personnalisation](#lightdm-personnalisation)
- [grub](#grub)
- [changer l ordre de démarrage du grub](#changer-l-ordre-de-démarrage-du-grub)
- [ecran de chargement fedora](#ecran-de-chargement-fedora)
- [reconfiguration du clavier](#reconfiguration-du-clavier)
- [lancer un script au branchement d une cle USB](#lancer-un-script-au-branchement-d-une-cle-usb)
- [LVM](#lvm)
- [script au demarrage](#script-au-demarrage)
- [gestion batterie](#gestion-batterie)
- [gestion webcam](#gestion-webcam)
- [mode graphique systemd](#mode-graphique-systemd)

_____________________________________________________________________________________
stopper les services listen inutiles
-------------------------------------------------------------------------------------
```bash
sudo systemctl disable exim4.service
sudo systemctl disable rpcbind.service
sudo systemctl disable rpcbind.socket
netstat -lt
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
mes dossiers
-------------------------------------------------------------------------------------
```bash
mkdir ~/Files ~/Downloads ~/Transferts ~/Medias ~/Unclear
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
disque ssd
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/fstab
###>>>ajouter discard ext4 ou discard,ssd btrfs
UUID=12345678-1234-1234-1234-123456789012 /      ext4    defaults,discard,noatime,nosuid,noexec,nodev        1 1
/dev/mapper/vg_localhost-lv_root            /       btrfs    defaults,discard,noatime,ssd     1 1

sudo nano /etc/lvm/lvm.conf
issue_discards = 1

echo noop | sudo tee /sys/block/sda/queue/scheduler
cat /sys/block/sda/queue/scheduler
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
baisser le niveau de mise en swap pour economiser le hdd
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/sysctl.conf
###>>>ajouter
vm.swappiness = 10
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
mode laptop gestion plus econome du disque dur
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/sysctl.conf
vm.laptop_mode = 5
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
securisation de /dev/shm/
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/fstab
###>>>rajout
tmpfs /dev/shm tmpfs  defaults,noatime,nosuid,noexec,nodev   0 0

sudo mount /dev/shm
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
connexion sans login
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/lightdm/lightdm.conf
[SeatDefaults]
pam-service=lightdm
pam-autologin-service=lightdm-autologin
autologin-user=USER
autologin-user-timeout=0

groupadd -r autologin
gpasswd -a USER autologin
###>>>mettre fond d ecran dans /usr/share/images/desktop-base/
update-alternatives --config FICHIER.jpg

###>>>ou
# Install the xorg-xinit package

echo startx >> ~/.bashrc

~/.xinitrc
#!/bin/bash
feh --bg-scale ~/.bg.png
exec /usr/local/bin/dwm
exec openbox-session
exec chromium
# etc
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
fenetre de connexion lightdm login deroulant
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/lightdm/lightdm.conf
[SeatDefaults]
greeter-hide-users=false
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
lightdm personnalisation
-------------------------------------------------------------------------------------
```bash
sudo nano  /etc/lightdm/lightdm-gtk-greeter.conf
background=/usr/share/images/desktop-base/wallpaper.png
#user-background=
#theme-name=
#icon-theme-name=
font-name=monoscape

sudo apt-get install numlockx
sudo nano /etc/lightdm/lightdm.conf
greeter-setup-script=/usr/bin/numlockx on
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
grub
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/default/grub
GRUB_DEFAULT=0
GRUB_HIDDEN_TIMEOUT=0
GRUB_HIDDEN_TIMEOUT_QUIET=true
GRUB_TIMEOUT=10
GRUB_DISTRIBUTOR=`lsb_release -i -s 2> /dev/null || echo Debian`
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
GRUB_CMDLINE_LINUX=""
GRUB_TERMINAL=console

sudo rm /boot/grub/.background_cache.png
###>>>BIOSS
sudo update-grub
###>>>ou
grub2-mkconfig -o /boot/grub2/grub.cfg
###>>>UEFI
grub2-mkconfig -o /boot/efi/EFI/fedora/grub.cfg
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
changer l ordre de démarrage du grub
-------------------------------------------------------------------------------------
```bash
sudo mv /etc/grub.d/30_os-prober_proxy /etc/grub.d/09_os-prober_proxy
sudo update-grub
#ou
sudo nano /etc/default/grub
GRUB_DEFAULT=0-TO-5

sudo update-grub

#ou
sudo nano /etc/default/grub
GRUB_DEFAULT=saved

sudo update-grub
sudo grub-set-default 0-TO-5
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
ecran de chargement fedora
-------------------------------------------------------------------------------------
```bash
plymouth-set-default-theme --list
sudo plymouth-set-default-theme tribar -R
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
reconfiguration du clavier
-------------------------------------------------------------------------------------
```bash
# on genere le fichier de map
xmodmap -pke > ~/.Xmodmap
# on trouve la clef a remaper
xev | awk -F'[ )]+' '/^KeyPress/ { a[NR+2] } NR in a { printf "%-3s %s\n", $5, $8 }'
# on modifie le fichier Xmodmap et on teste avec
xmodmap ~/.Xmodmap
# exemple : keycode 49 = Super_L
# touche ; shift + touche ; mode_switch + touche ; mode_switch + shift + touche ; AltGr + touche ; AltGr + shift + touche ; AltGr + mode_switch + touche ; AltGr + mode_switch + shift + touche ;
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
history avec date et exclure en commençant par espace
-------------------------------------------------------------------------------------
```bash
echo '
export HISTTIMEFORMAT=\"%m/%d/%y %T \"
export HISTCONTROL=ignorespace:ignoredups

 if [ -f ~/.bashrc ]; then
    source ~/.bashrc
 fi
'>> ~/.bash_profile
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
lancer un script au branchement d une cle USB
-------------------------------------------------------------------------------------
```bash
udevadm monitor --udev
lsusb
udevadm info -a --name=/dev/sdX --attribute-walk

sudo nano /etc/udev/rules.d/91-CLE_USB.rules
SUBSYSTEM=="block", ACTION=="add", ATTRS{serial}=="........", SYMLINK="reparator", RUN="/usr/local/bin/CLE_USB.sh add"
SUBSYSTEM=="block", ACTION=="remove", ATTRS{serial}=="........", RUN="/usr/local/bin/CLE_USB.sh remove"
ACTION=="add", SUBSYSTEM=="block", KERNEL=="sd[a-z]1", ATTRS{vendor}=="CONSTRUCTEUR", RUN+="/usr/local/bin/BACKUP.sh"

udevadm control --reload-rules
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
LVM
-------------------------------------------------------------------------------------
## Decrease LVM
```bash
###>>>Shrinking a root volume
vgchange -a y

###>>>Shrinking a non-root volume
umount /MONTAGE/
e2fsck -fy /MONTAGE/
resize2fs /MONTAGE/ 5G
###>>>ou
lvreduce -L 5G /dev/vg/DISK_NAME
resize2fs /MONTAGE/

mount /MONTAGE/ /mnt
```

## Increase LVM
```bash
vgdisplay | grep free
lvextend -L+5G /MONTAGE/
resize2fs /MONTAGE/
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
script au demarrage
-------------------------------------------------------------------------------------
```bash
###>>>1
crontab -e 
@reboot /usr/local/bin/SCRIPT.sh

###>>>2
sudo nano /etc/rc.local
###>>>ou
sudo nano /etc/rc.d/rc.local
#!/bin/sh -e
/usr/local/bin/SCRIPT.sh || exit 1
exit 0

sudo chmod a+x /etc/rc.d/rc.local

###>>>3
sudo nano /etc/init.d/SCRIPT
#! /bin/sh
### BEGIN INIT INFO
# Provides: myprogram
# Should-Start: console-screen dbus network-manager
# Required-Start: $remote_fs $network $local_fs
# Required-Stop: $remote_fs 
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: start myprogram at boot time
### END INIT INFO
# /etc/init.d/script

# The following part always gets executed
echo "This part always gets executed"

# The following part carries out specific functions depending on arguments
case "$1" in
  start)
    echo "Starting script"
    echo "script is alive"
    ;;
  stop)
    echo "Stopping script"
    echo "script is dead"
    ;;
  *)
    echo "Usage: /etc/init.d/SCRIPT {start|stop}"
    exit 1
    ;;
esac

exit 0

sudo chmod 755 /etc/init.d/SCRIPT
sudo update-rc.d SCRIPT defaults
###>>>pour le desactiver
sudo update-rc.d -f SCRIPT remove

###>>>4
sudo nano /etc/init/SCRIPT.conf
description     "SCRIPT"
start on startup
task
exec /PATH/TO/MY/SCRIPT.sh

###>>>5
sudo nano /etc/systemd/system/SCRIPT.service
[Unit]
Description=Service SCRIPT

[Service]
Type=simple
ExecStart=/usr/local/bin/SCRIPT.sh start ; AUTRE_COMMAND
#ExecStart=/bin/sh -c COMMANDE
ExecStop=/usr/local/bin/SCRIPT.sh stop
RemainAfterExit=yes

[Install]
WantedBy=default.target
Alias=SCRIPT.service

chmod a+x /etc/systemd/system/SCRIPT.service
systemctl --system daemon-reload
systemctl start SCRIPT.service
systemctl enable SCRIPT.service
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gestion batterie
-------------------------------------------------------------------------------------
```bash
sudo powertop
sudo powertop --calibrate
sudo powertop --auto-tune          #appliquer
sudo powertop --calibrate          #permanent

sudo nano /etc/systemd/system/powertop.service
[Unit]
Description=Powertop tunings

[Service]
Type=oneshot
ExecStart=/usr/bin/powertop --auto-tune

[Install]
WantedBy=multi-user.target

sudo systemctl enable powertop

###>>>ou
dnf install tlp tlp-rdw
sudo tlp stat
sudo tlp start
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gestion webcam
-------------------------------------------------------------------------------------
```bash
###>>>désactivation
sudo nano /etc/modprobe.d/blacklist.conf
blacklist uvcvideo

###>>>désactivation et activation
sudo rmmod -f uvcvideo
sudo modprobe uvcvideo

###>>>controle
sudo apt-get install v4l-utils
v4l2-ctl --info
v4l2-ctl --list-ctrls
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
mode graphique systemd
-------------------------------------------------------------------------------------
```bash
systemctl set-default graphical.target
```

**[`^        back to top        ^`](#)**
