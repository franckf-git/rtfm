_____________________________________________________________________________________
openscad syntax
-------------------------------------------------------------------------------------
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

_____________________________________________________________________________________
irc - discord
-------------------------------------------------------------------------------------
```
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
```
```

_____________________________________________________________________________________
lxc
-------------------------------------------------------------------------------------
```bash
lxc-create -n NOMDUCONTENEUR -t TYPEDUCONTENEUR    #creer un conteneur
###>>>ex sudo lxc-create -n ma_machine -t download -- -d debian -r jessie -a amd64
lxc-ls --active                                    #liste des connecteurs actifs
lxc-start -n NOMDUCONTENEUR                        #Démarrer un conteneur
lxc-console -n NOMDUCONTENEUR                      #Se connecter à un conteneur démarré
lxc-info -n NOMDUCONTENEUR                         #Avoir des infos sur le conteneur
lxc-stop -n NOMDUCONTENEUR                         #Arrêter un conteneur
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

_____________________________________________________________________________________
raid logiciel mdadm
-------------------------------------------------------------------------------------
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
```

_____________________________________________________________________________________
imagemagick traitement image en cli
-------------------------------------------------------------------------------------
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

_____________________________________________________________________________________
install virtualbox
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/apt/sources.list
deb http://download.virtualbox.org/virtualbox/debian stretch contrib

wget https://www.virtualbox.org/download/oracle_vbox_2016.asc
sudo apt-key add oracle_vbox_2016.asc
sudo apt-get update
sudo apt-get install virtualbox-5.1
```

_____________________________________________________________________________________
installation des addons invites virtualbox
-------------------------------------------------------------------------------------
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

_____________________________________________________________________________________
monter un disque virtualbox
-------------------------------------------------------------------------------------
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

_____________________________________________________________________________________
QEMU
-------------------------------------------------------------------------------------
```bash
sudo apt install qemu qemu-kvm qemu-launcher
sudo ln -s /usr/bin/qemu-system-x86_64 /usr/bin/qemu

###>>>live-cd
qemu -cdrom IMAGE.iso -m 1024

###>>>installation
qemu-img create -f qcow2 HDD.qcow2 5G
qemu -hda HDD.qcow2 -cdrom IMAGE.iso -boot d -m 256
qcow ###>>>pour disque dynamique
###>>>ou
qemu-img create HDD.img 2G
qemu -k fr -cdrom IMAGE.iso -hda HDD.img -user-net -boot a

###>>>demarrer le windows dualboot
qemu -boot c -hda /dev/hda

###>>>demarrage
qemu -hda HDD.img -m 256
#option : -enable-audio -soundhw all -full-screen -no-acpi --enable-kvm
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

_____________________________________________________________________________________
disques smart
-------------------------------------------------------------------------------------
```bash
sudo apt-get install smartmontools smart-notifier

###>>>pour activer SMART sur un disque
sudo smartctl --smart=on --offlineauto=on --saveauto=on /dev/sdX

###>>>infos sur le disque
sudo smartctl -H -i /dev/sdX
sudo smartctl -a /dev/sdX

###>>>tests
sudo smartctl -t short /dev/sdX
sudo smartctl -t long /dev/sdX
sudo smartctl -l selftest /dev/sdX
sudo smartctl -a /dev/sdX
```

_____________________________________________________________________________________
correction de commande en python
-------------------------------------------------------------------------------------
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

_____________________________________________________________________________________
outil de benchmarking simple
-------------------------------------------------------------------------------------
```bash
sudo apt-get install stress
stress -c                     ###>>>nb de coeurs
stress -m                     ###>>>nb de process de 256mo de ram
stress -m 4 --vm-bytes 300M   ###>>>4 process de 300mo
stress -d                     ###>>>nb de process de 1G disque
stress -d 2 --hdd-bytes 512M  ###>>>2 process de 512mo
stress -c 4 -m 2 -d 1 -t 40s
```

_____________________________________________________________________________________
multi-fenetre terminal
-------------------------------------------------------------------------------------
```bash
screen -S NOM                #creer un terminal NOM [[CRTL+a d]] pour quitter
screen -dmS "NOM" COMMAND    #screen detache avec command
screen -r NOM                #revenir au terminal NOM ou numero
screen -x NOM                #syncro du terminal NOM
screen -ls                   #liste des terminaux
screen -X -S NOM kill        #kill NOM
```

_____________________________________________________________________________________
QR code
-------------------------------------------------------------------------------------
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