## Index

- [openscad syntax](#openscad-syntax)
- [irc - discord](#irc---discord)
- [lxc](#lxc)
- [raid logiciel mdadm](#raid-logiciel-mdadm)
- [imagemagick traitement image en cli](#imagemagick-traitement-image-en-cli)
- [install virtualbox](#install-virtualbox)
- [installation des addons invites virtualbox](#installation-des-addons-invites-virtualbox)
- [monter un disque virtualbox](#monter-un-disque-virtualbox)
- [QEMU](#qemu)
- [gestionnaire de machines virtuelles](#gestionnaire-de-machines-virtuelles)
- [disques smart](#disques-smart)
- [correction de commande en python](#correction-de-commande-en-python)
- [outil de benchmarking simple](#outil-de-benchmarking-simple)
- [multi-fenetre terminal](#multi-fenetre-terminal)
- [QR code](#qr-code)
- [i3-wm](#i3-wm)

---

## openscad syntax

```
var = value;
module name(…) { … }
name();
function name(…) = …
name();
include <….scad>
use <….scad>

###>>>2D
circle(radius | d=diameter)
square(size,center)
square([width,height],center)
polygon([points])
polygon([points],[paths])
text(text, size, font,
     halign, valign, spacing,
     direction, language, script)

###>>>3D
sphere(radius | d=diameter)
cube(size, center)
cube([width,depth,height], center)
cylinder(h,r|d,center)
cylinder(h,r1|d1,r2|d2,center)
polyhedron(points, triangles, convexity)

###>>>Transformations
translate([x,y,z])
rotate([x,y,z])
scale([x,y,z])
resize([x,y,z],auto)
mirror([x,y,z])
multmatrix(m)
color("colorname",alpha)
color([r,g,b,a])
offset(r|delta,chamfer)
hull()
minkowski()

###>>>Boolean operations
union()
difference()
intersection()

###>>>Modify characters
* disable
! show only
# highlight / debug
% transparent / background
```

**[`^ back to top ^`](#)**

---

## irc - discord

````
/join NOMSALON                    #Rejoindre le salon NomSalon
/list                             #Affiche une liste des salons
/nick NOUVEAUPSEUDO               #Changer son nom pour nouveaupseudo
/cycle                            #Quitter et rejoindre un salon quasi-instantannément
/whois PSEUDO                     #Obtenir des informations sur pseudo
/ignore PSEUDO                    #Masquer toute intervention de pseudo
/close                            #Fermer la fenêtre active
/part                             #Quitter le salon
/quit                             #Quitter le client IRC
/msg PSEUDO [MESSAGE]             #Envoie un message privé à un utilisateur
/query PSEUDO                     #Discuter en privé avec pseudo
/notify PSEUDO                    #Permet de savoir quand un utilisateur se connecte
/notice PSEUDO [MESSAGE]          #Envoie une notice à pseudo qui sera lu de lui seul
/me VOTRE_ACTION                  #Faire une action
/dcc                              #Envoyer un contenu binaire (images, sons…)
/kick PSEUDO LE_MOTIF_DU_KICK     #Pour expulser (kicker)
/ban MASQUE_DE_BAN                #Pour bannir (ban)
/unban MASQUE_DE_BAN              #Pour dé-bannir (unban)
/topic #CHANNEL MESSAGE           #Change le topic (sujet) du channel
/server SERVEUR[:PORT]            #Permet d accéder au serveur spécifié

###>>>discord
*italique*
**gras**
***italique gras***
~~barré~~
__souligné__
__*italique souligné*__
__**gras souligné**__
__***italique gras souligné***__
`code`
```language
bloc de code

````

**[`^ back to top ^`](#)**

---

## lxc

```bash
sudo apt install lxc lxc-templates
###>>>Paquets optionnels
sudo apt bridge-utils libvirt-bin debootstrap
lxc-checkconfig                                    #vérification de la configuration

lxc-create -n NOMDUCONTENEUR -t TYPEDUCONTENEUR    #creer un conteneur
###>>>ex sudo lxc-create -n ma_machine -t download -- -d debian -r jessie -a amd64
lxc-ls --active                                    #liste des connecteurs actifs
lxc-ls --fancy
lxc-start -n NOMDUCONTENEUR                        #Démarrer un conteneur
lxc-console -n NOMDUCONTENEUR                      #Se connecter à un conteneur démarré
lxc-info -n NOMDUCONTENEUR                         #Avoir des infos sur le conteneur
lxc-stop -n NOMDUCONTENEUR                         #Arrêter un conteneur
lxc-halt -n NOMDUCONTENEUR                         #Arrêter un conteneur proprement
lxc-clone NOM NOUVEAUNOM                           #Cloner un conteneur
lxc-snapshot -n NOM                                #Instantanés de conteneurs
lxc-snapshot -n NOM -L                             #Pour voir les snapshots réalisés
lxc-snapshot -n NOM -r NOMDUSNAPSHOT               #Pour restaurer un snapshot
lxc-snapshot -n NOM -d NOMDUSNAPSHOT               #Pour supprimer un snapshot
lxc-destroy -n NOM                                 #Supprimer un conteneur

nano /home/lxc/NOMDUCONTENEUR/config
lxc.network.type=veth               #utilisation d un bridge
lxc.network.name = veth0            #nom de l interface réseau dans le conteneur LXC
lxc.network.veth.pair = veth-test   #nom de l interface sur l hôte
lxc.network.flags = up              #active l interface
lxc.network.link = br0              #fait le lien sur le bridge br0 de l hôte
lxc.network.ipv4 = IPCONTENEUR/24   #adresse IPv4 du conteneur
lxc.network.ipv4.gateway = IPCONTENEUR.254                  #passerelle conteneur
lxc.mount.entry=/MONTAGE/ CHEMIN/CONTENEUR none rw,bind 0 0 #Points de montage
```

**[`^ back to top ^`](#)**

---

## raid logiciel mdadm

```bash
su
apt-get install mdadm
systemctl enable mdmonitor
systemctl start mdmonitor

###>>>raid1 aka mirroring
mdadm --create /dev/md0 --level=1 --raid-devices=2 /dev/sdb /dev/sdc
###>>>raid5
mdadm --create /dev/md0 --level=5 --raid-devices=4 /dev/sdb /dev/sdc /dev/sdb /dev/sde
###>>>processus en cours
cat /proc/mdstat
###>>>infos raid
mdadm --detail /dev/md0

mkfs.ext4 /dev/md0
mount /dev/md0 on /mnt type ext4 (rwrelatimedata=ordered)
###>>>[attention au restart]

###>>>ajouter un disque
mdadm --manage /dev/md0 --add /dev/sdd
###>>>supprimer un disque
mdadm --manage /dev/md0 --fail /dev/sdc --remove /dev/sdc
###>>>stopper un raid
mdadm --stop /dev/md0

#Create a RAID array
mdadm --create /dev/md/test --homehost=any --metadata=1.0 --level=1 --raid-devices=2 /dev/sda1 /dev/sdb1
#Assemble (and start) a RAID array
mdadm --assemble /dev/md/test /dev/sda1 /dev/sdb1
#Stop a RAID array
mdadm --stop /dev/md/test
#Delete a RAID array
mdadm --zero-superblock /dev/sda1 /dev/sdb1
#Check the status of all assembled RAID arrays
cat /proc/mdstat
```

**[`^ back to top ^`](#)**

---

## imagemagick traitement image en cli

```bash
display IMAGE.png
identify IMAGE.jpg
animate IMAGES.gif
animate -delay 100 *.png

convert -quality 20 IMAGE.png IMAGE.jpg
convert IMAGES_*.png ANIME.gif
convert IMAGES.jpg -resize 50% -rotate 90 IMAGE.jpg
convert *.BMP -set filename:f '%t.png' +adjoin '%[filename:f]'
convert -background lightblue -fill blue -font FONT -pointsize 72 label:MESSAGE IMAGE.gif
convert IMAGE.gif label:'MESSAGE' -gravity Center -append IMAGE_FINAL.jpg

mogrify -resize 800x600 *.jpg
mogrify -fill white -pointsize 16 -annotate +10+26 'test' IMAGE.jpg
mogrify -font FONT -gravity Center -annotate 0 "%f" \

import IMAGE.png
import -window root IMAGE.png
import -pause 10 IMAGE.png

compare IMAGEA.png IMAGEB.png DIFFERENCE.png
composite -compose Over -gravity NorthEast IMAGE1.png IMAGE2.jpeg IMAGE_FINAL.jpeg
composite -watermark valeur_luminosite Premiere Deuxieme IMAGE_resultante
```

**[`^ back to top ^`](#)**

---

## install virtualbox

```bash
sudo nano /etc/apt/sources.list
deb http://download.virtualbox.org/virtualbox/debian stretch contrib

wget https://www.virtualbox.org/download/oracle_vbox_2016.asc
sudo apt-key add oracle_vbox_2016.asc
sudo apt-get update
sudo apt-get install virtualbox-5.1
```

**[`^ back to top ^`](#)**

---

## installation des addons invites virtualbox

```bash
###>>>en utilisant l iso et le .run ou
sudo apt-get install virtualbox-guest-x11
###>>>Login as root
apt-get update
apt-get upgrade
apt-get install build-essential module-assistant
###>>>Configure your system for building kernel modules by running m-a prepare
###>>>Click on Install Guest Additions from the Devices menu, then run
mount /media/cdrom
sh /media/cdrom/VBoxLinuxAdditions.run
reboot
```

**[`^ back to top ^`](#)**

---

## monter un disque virtualbox

```bash
###>>>VDI
sudo apt-get install qemu-kvm
sudo modprobe nbd
sudo qemu-nbd -c /dev/nbd0 IMAGE.vdi
sudo mount  -o noatime,noexec /dev/nbd0p1 /tmp/vdi/

sudo umount /tmp/vdi/
sudo qemu-nbd -d /dev/nbd0

###>>>VDH
sudo apt-get install libvhdi-utils
vhdiinfo MONDISQUE.vhd
mkdir -p /media/MONDISQUE/
sudo vhdimount MONDISQUE.vhd /media/MONDISQUE/

fdisk -lu /media/MONDISQUE/vhdi1
###>>>Sector size (logical/physical): 512 bytes / 512 bytes
vhdi1p1      *           2048  1026047  1024000  500M  7 HPFS/NTFS/exFAT
vhdi1p2               1026048 67106815 66080768 31,5G  7 HPFS/NTFS/exFAT
###>>>vhdi1p2 démarre au secteur 1026048 avec des secteurs de 512 octets

sudo losetup -o $((1026048*512)) /dev/loop0 vhdi1
mkdir -p /media/BACKUP
mount -o ro,noload /dev/loop0 /media/BACKUP

umount /media/BACKUP
losetup -d /dev/loop0
umount /media/MONDISQUE/
```

**[`^ back to top ^`](#)**

---

## QEMU

```bash
sudo apt install qemu qemu-kvm qemu-launcher
sudo ln -s /usr/bin/qemu-system-x86_64 /usr/bin/qemu

###>>>live-cd
qemu -cdrom IMAGE.iso -m 1024

###>>>installation
qemu-img create -f qcow2 HDD.qcow2 5G
qemu -hda HDD.qcow2 -cdrom IMAGE.iso -boot d -m 256
###>>>qcow pour disque dynamique
###>>>ou
qemu-img create HDD.img 2G
qemu -k fr -cdrom IMAGE.iso -hda HDD.img -user-net -boot a

###>>>demarrer le windows dualboot
qemu -boot c -hda /dev/hda

###>>>demarrage
qemu -hda HDD.img -m 256
#options : -enable-audio -soundhw all -full-screen -no-acpi
-net nic -net user #reseau
--enable-kvm       #kvm rapidité
qemu-system-x86_64 -hda HDD.qcow2 -m 1024

###>>>compression
tar c --sparse -f BACKUP.tar HDD.img

###>>>monter image qemu
modprobe nbd max_part=8
kvm-nbd --connect=/dev/nbd0 HDD.qcow
fdisk -l /dev/nbd0
mount /dev/nbd0p1 /mnt
###>>>ou
mount -o loop,offset=32256 /PATH/TO/HDD.img /mnt/MOUNTPOINT
```

**[`^ back to top ^`](#)**

---

## gestionnaire de machines virtuelles

```bash
sudo apt install gnome-boxes virt-manager qemu-kvm qemu gir1.2-spice-client-gtk-3.0 ovmf
sudo systemctl start libvirtd
```

**[`^ back to top ^`](#)**

---

## disques smart

```bash
sudo apt-get install smartmontools smart-notifier

###>>>pour activer SMART sur un disque
sudo smartctl --smart=on --offlineauto=on --saveauto=on /dev/sdX

###>>>infos sur le disque
sudo smartctl -H -i /dev/sdX
sudo smartctl -a /dev/sdX
sudo smartctl -l selftest /dev/sdX     #accéder aux résultats
sudo smartctl --scan | grep -i usb

###>>>tests
sudo smartctl -t short /dev/sdX
sudo smartctl -t long /dev/sdX
sudo smartctl -l selftest /dev/sdX
sudo smartctl -a /dev/sdX
```

**[`^ back to top ^`](#)**

---

## correction de commande en python

```bash
sudo apt-get update
sudo apt-get install python3-dev python3-pip
sudo pip3 install thefuck
echo `eval $(thefuck –alias)` >> ~/.bashrc
source ~/.bashrc

apt installz apache2
fuck
###>>>sudo apt install apache2
```

**[`^ back to top ^`](#)**

---

## outil de benchmarking simple

```bash
sudo apt-get install stress
stress -c                     ###>>>nb de coeurs
stress -m                     ###>>>nb de process de 256mo de ram
stress -m 4 --vm-bytes 300M   ###>>>4 process de 300mo
stress -d                     ###>>>nb de process de 1G disque
stress -d 2 --hdd-bytes 512M  ###>>>2 process de 512mo
stress -c 4 -m 2 -d 1 -t 40s
```

**[`^ back to top ^`](#)**

---

## multi-fenetre terminal

### tmux

```bash
[CTRL+b] ,                #Renommer le terminal courant
[CTRL+b] "                #Split vertical du terminal
[CTRL+b] %                #Split horizontal du terminal
[CTRL+b] [SPACE]          #Passer à l'autre split
[CTRL+b] [ALT+flèches]    #Redimmensionner le panel
[CTRL+b] o                #Aller au panel suivant
[CTRL+b] q                #Afficher les numéros
[CTRL+b] [flèches]        #Aller au prochain panel
[CTRL+b] ;                #Basculer dans le dernier panel actif
[CTRL+b] !                #Créer un nouveau terminal
[CTRL+b] x                #Fermer le panel courant
[CTRL+b] c                #Créer un nouveau terminal
[CTRL+b] &                #Terminer le terminal actuel
[CTRL+b] n                #Aller au terminal suivant
[CTRL+b] p                #Aller au terminal précédent
[CTRL+b] w                #Afficher la liste des terminaux
[CTRL+b] 0-9              #Choisir terminal
[CTRL+b] d                #Détacher tmux
[CTRL+b] :                #Ouvrir l'invite de commande
[CTRL+b] (                #Aller à la session précédente
[CTRL+b] )                #Aller à la session suivante
[CTRL+b] s                #Lister les sessions disponibles
[CTRL+b] t                #Horloge
[CTRL+b] ?                #Liste de tous les raccourcis
[CTRL+b] {                #Déplacer le panel vers précédent
[CTRL+b] }                #Déplacer le panel vers suivant

# changer la clé d'ouverture en CTRL + A
nano ~/.tmux.conf
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix
set -g mouse on
set -g status-position bottom
set -g status-bg colour237
set -g status-fg colour137
set -g status-right-length 100
set -g status-left-length 70

# tmux au demarrage
# bashrc
[ -z "$TMUX"  ] && { tmux attach || exec tmux new-session && exit;}
# ou
(exec tmux attach || exec tmux new-session)
# ou bash_profile
if [ -z "$TMUX" ]; then
    tmux attach -t default || tmux new -s default
fi
```

### screen

```bash
screen -S NOM                #creer un terminal NOM [[CRTL+a d]] pour quitter
screen -dmS "NOM" COMMAND    #screen detache avec command
screen -r NOM                #revenir au terminal NOM ou numero
screen -x NOM                #syncro du terminal NOM
screen -ls                   #liste des terminaux
screen -X -S NOM kill        #kill NOM

[CTRL+a] |                #Split vertical du terminal
[CTRL+a] [SHIFT+s]        #Split horizontal du terminal
[CTRL+a] [TAB]            #Aller au panel suivant
[CTRL+a] c                #Créer un nouveau terminal
[CTRL+a] [SPACE]          #Aller au terminal suivant
[CTRL+a] [BACKSPACE]      #Aller au terminal précédent
[CTRL+a] "                #Afficher et sélectionner les terminaux
[CTRL+a] 0-9              #Choisir terminal
[CTRL+a] [SHIFT+x]        #Fermer le panel courant
[CTRL+a] \                #Tout fermer
[CTRL+a] p                #Aller au panel précedent
[CTRL+a] n                #Aller au panel suivant
```

**[`^ back to top ^`](#)**

---

## QR code

```bash
###>>>generation
sudo apt-get install qrencode
qrencode -t EPS -o IMAGE.png 'MESSAGE'
qrencode -t ASCII -o QR.txt 'MESSAGE'
qrencode -o IMAGE.png URL
qrencode -o IMAGE.png 'BEGIN:VCARD VERSION:4.0 FN: N:NOM;PRENOM;;; ADR:;;ADRESS. NBR;VILLE;;POSTAL; END:VCARD'

###>>>lecture
sudo apt-get install libdecodeqr-examples
libdecodeqr-simpletest IMAGE.png
```

**[`^ back to top ^`](#)**

---

## i3-wm

### Controlling i3

```
$mod+Shift+r       #Restart i3
$mod+Shift+e       #Exit i3
$mod+Shift+c       #Reload config fil
```

### Manage Windows

```
$mod+Shift+q       #Kill current window
$mod+Shift+NUM     #Move current window to workspace number
$mod+Shift+f       #Set window to floating mode
$mod+j             #Focus on window to the left
$mod+k             #Focus on window above
$mod+l             #Focus on window below
$mod+;             #Focus on window to the right
$mod+Shift+DIRKEY  #Move window in direction keys
$mod+Shift+j       #Move window left
$mod+Shift+k       #Move window up
$mod+Shift+l       #Move window down
$mod+Shift+;       #Move window right
```

### Workspaces

```
$mod+NUM           #Switch to workspace
$mod+Shift+NUM     #Move current window to workspace number
```

### Containers

```
$mod+e             #Default container
$mod+h             #Horizontal split container
$mod+v             #Vertical split container
$mod+w             #Tabbed container
$mod+f             #Toggle fullscreen mode
$mod+s             #Toggle stacking mode
$mod+Shift+SPACE   #Toggle floating mode
$mod+r             #Resize mode (<Esc> or <Enter> to leave)
```

### Applications

```
$mod+Enter         #Open new terminal window
$mod+d             #Open dmenu
```

**[`^ back to top ^`](#)**
