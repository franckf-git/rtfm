## Index

[TOC]

- [navigation systeme](#navigation-systeme)
- [operations sur les fichiers](#operations-sur-les-fichiers)
- [fichiers texte et binaire](#fichiers-texte-et-binaire)
- [infos systemes](#infos-systemes)
- [gestion des utilisateurs](#gestion-des-utilisateurs)
- [gestion processus](#gestion-processus)
- [infos materiel](#infos-materiel)
- [archivage](#archivage)
- [streameditor](#streameditor)
- [infos reseau](#infos-reseau)
- [recuperation web](#recuperation-web)
- [recherche](#recherche)
- [benchmark](#benchmark)
- [hdd et partitions](#hdd-et-partitions)
- [ram / swap](#ram--swap)
- [audiovisuel](#audiovisuel)
- [outils CLI ligne de commande](#outils-cli-ligne-de-commande)
- [commandes futiles](#commandes-futiles)
- [log](#log)
- [commandes tokill](#commandes-tokill)

_____________________________________________________________________________________
navigation systeme
-------------------------------------------------------------------------------------
```bash
pwd                 #affiche le repertoire courant
cd REP              #se place dans le repertoire rep
cd                  #se place dans le repertoire de l utilisateur ~/
cd -                #se place dans l ancien repertoire cd $OLDPWD
cd ..               #se place dans le repertoire parent
ls REP              #liste les fichiers du repertoire rep
ls -a               #ls avec les fichiers caches
ls -l               #ls avec les droits d acces et la taille
vdir                #equivaut a ls -lb
dirs -v             #affiche la liste des dossiers enregistrés
pushd FOLDER        #sauvegarde le dossier
popd                #reinitialise les dossiers enregistrés
COMMAND ~N          #utilise le dossier N enregistré
###>>>système de fichiers virtuel
.local/share/gvfs-metadata/home
gio info FILEFOLDER | grep metadata
gio set FILEFOLDER 'metadata::comment' 'COMMENT'
gio set FILEFOLDER -t stringv metadata::emblems emblem-default 
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
operations sur les fichiers
-------------------------------------------------------------------------------------
```bash
mv SOURCE CIBLE          #deplace le fichier source vers cible
cp SOURCE CIBLE          #copie le fichier source vers cible
cp -R SOURCE CIBLE       #copie le repertoire source vers cible
ln SOURCE LIEN           #creer un lien fort de source vers lien (inode identique)
ln -s SOURCE LIEN        #creer un lien symbolique de source vers lien
touch FILE               #creer le fichier file ou met a jour sa date de modification
mkdir REP                #creer un repertoire rep
mkdir -p REP/REP2        #mkdir avec creation des dossiers parents si necessaire
rm FILE                  #supprime le fichier file
rm -f FILE               #supprime le fichier file protege en ecriture
rmdir REP                #supprimer un repertoire vide
rm -R REP                #supprime un repertoire
du -hs FILEREP           #affiche la taille du fichier file ou du repertoire
sudo du -shxc /*
stat                     #details sur un fichier ou dossier
file FILE1 FILE2         #affiche le type de fichier
mkdir FOLDER ; cd $_     #creer et entrer dans un dossier
echo "!!" > SCRIPT.sh    #creer un script de la derniere commande
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
fichiers texte et binaire
-------------------------------------------------------------------------------------
```bash
wc FICHIER            #compte le nombre de lignes, de mots, d octets de fichier
cat FICHIERS          #concatene les fichiers
tac FICHIERS          #cat inverse
more FICHIER          #Espace=page suivante, Entree=ligne suivante, u=remonter
less FICHIER          #navigation Left/Right/Up/Down/PageUp/PageDown
head -n x FICHIER     #affiche les x premieres lignes de fichier
tail -n x FICHIER     #affiche les x dernieres lignes de fichier
tail -f FICHIER       #affiche la derniere ligne de fichier
diff -y FILE1 FILE2   #affiche les differences entre deux fichiers texte
diff -u FILE1 FILE2   #affiche les differences au format utilise par patch
comp FILE1 FILE2      #compare deux fichiers binaires
comp FILE1 FILE2 n N  #file1 a partir du nieme octet, et file2 a partir du Nieme
diff FILE1 FILE2      #donne les modifications a apporter a file1
diff -rus FILE1 FILE2
sdiff                 #différence cote à cote
awk 'NR==FNR {t[$0]++; next} !t[$0]' F2 F1      #afficher les lignes uniquement dans f1
awk '($1=="NOM") { print }' <FILE     #afficher les lignes contenant la colonne nom
awk '{ print $3,$4 }' <FILE           #afficher les colonnes 3 et 4
| awk -F'\t' '{ print $3 }'           #afficher separe par une tabulation
| cut -d" " -f1,3,11,12               #awk simple -d délimitation et -f champ
join FILE1 FILE2      #Fusionne les lignes de deux fichiers
nl -b a               #Permet de numéroter les lignes d un fichier
split -b 2000m FICHIER                #decouper un fichier en 2Go
cat x* >GROS_FICHIER                  #reassembler le fichier
cat FICHIER | gpg -c - > FICHIERCRYPTE          #crypter un fichier texte
cat FICHIERCRYPTE | gpg -             #decrypter un fichier texte
cat FIC.1 FIC.2 FIC.3 &gt; FIC.123    #Combine les trois fichiers en un seul
> FILE                                #creer un fichier vide
UUID=$(cat /proc/sys/kernel/random/uuid)    #generateur uuid
expand                #transforme les tabulations en espaces
unexpand              #transforme les espaces en tabulations
fold -w N             #retour à la ligne après N caractères
fmt                   #formatage paragraphe
column -s C -t FIC    #affichage en colonnes selon le caractère C
apropos TERMES        #recherche dans toutes les man pages
clip < FILE           #copier dans le presse-passier
echo 'MESSAGE' | tr '[A-Za-z]' '[N-ZA-Mn-za-m]' #rot13
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
infos systemes
-------------------------------------------------------------------------------------
```bash
whoami                      #affiche le login de l utilisateur
who                         #affiche les utilisateurs connectes
id                          #afficher les uid, gid et groupes de l utilisateur
id USER                     #afficher les uid, gid et groupes de user (root only)
getent GROUP                #liste des appartenances aux groupes
usermod -a -G GROUP USER    #ajouter un groupe a un utilisateur
finger USER                 #affiche les informations de user
write USER                  #afficher un message sur le terminal de user
tty                         #afficher le nom de son terminal
lscpu                       #affiche les informations du processeur
su - sudo                   #passer en mode administrateur, super-utilisateur
dpkg -l                     #liste des paquets installes avec version
df -h                       #espace libre
cat /proc/version           #detail du noyau Linux utilise
tail -f /var/log/messages   #derniers logs
uname -r                    #version du noyau utilise
lsb_release -a              #version du systeme
dpkg -l | egrep linux-header                   #liste les noyaux installes
sudo tune2fs -l /dev/sda1 | grep created       #date d installation du systeme
printenv                    #afficher les variables d environnements
last | more                 #verifier les dernieres connexions
sudo tail -n 20 /var/log/auth.log              #verifier les dernieres connexions
w                           #qui est connecte
head -n 20 /var/mail/USER   #debian you have new mail
nvidia-smi -q -g 0 -d UTILIZATION -l           #GPU
lm-sensors (sensors-detect)                    #CPU
hddtemp /dev/sda                               #HDD
smartctl -a /dev/sda                           #HDD
cat /sys/class/net/enp0s25/                    #infos interface reseau
sudo nano /etc/hostname     #renommer la Machine
hostnamectl set-hostname "HOSTNAME"
hostnamectl                 #afficher le nom de la machine
type COMMAND                #emplacement d une commande 
vmstat                      #statistiques systeme
powertop                    #statistiques systeme
dmidecode                   #lit les info du bios
dmesg                       #affiche le buffer du noyau
set                         #liste variables d environnement
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gestion des utilisateurs
-------------------------------------------------------------------------------------
```bash
passwd                       #changer le mot de passe de l utilisateur courant
useradd                      #ajouter utilisateur
adduser                      #ajouter utilisateur interactif
adduser USER sudo            #passer un utilisateur en sudo
usermod -aG sudo USER        #ajout au groupe sudo
deluser                      #supprime un utilisateur
addgroup                     #ajoute un groupe
delgroup                     #supprime un groupe
chown -R USER:GROUP FOLDER/  #changement du proprietaire
sudo chmod -R 777 FOLDER/    #changement des droits
chmod -R u=rwx,g=rw,o=r /PATH
chmod -R u+x,g+wx,o-w /PATH
talk                         #conversation avec un autre utilisateur du système
echo "MESSAGE" | wall        #Envoyer un message à tous les utilisateurs du système
sudo cat /etc/passwd | awk -F: '{print $ 1}'   #liste des utilisateurs
grep bash /etc/passwd        #utilisateurs physiques du système
awk -F: '$3 > 999 {print $1}' /etc/passwd | sort
more /etc/group              #tous les groupes
id USER                      #details des ID et groupes
cat /etc/shells              #voir les shells disponibles

nano /etc/sudoers            #activer sudo
%sudo    #ALL=(ALL:ALL) ALL
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gestion processus
-------------------------------------------------------------------------------------
```bash
ps                           #afficher les processus de l utilisateur
ps ax                        #afficher tous les processus
ps aux                       #afficher tous les processus avec les utilisateurs
ps aux | sort -nk +4 | tail  #les 10 processus qui utilisent le plus de mémoire
strace -p PID -e trace=all   #lister les appels systeme
pidof NOMDUPROGRAMME         #retrouver le pid
whereis NOMDUPROGRAMME       #dossier programme
pstree                       #afficher les processus dans une arborescence
top                          #afficher un tableau de bord des processus
tload                        #charge cpu graphique
lsof +L1                     #processus utilisant des fichiers
lsof -n | grep deleted       #fichier supprimé mais en cours d écriture
systemd-analyse blame        #services demarres pardefaut selon leur temps de lancement

service NOMDUSERVICE start/stop/restart/reload   #gestion service init.d
systemctl start/stop/restart/reload/enable/disable NOMDUSERVICE   #systemd
systemctl list-unit-files | grep enabled         #lister tous les services activés
systemctl --failed                               #lister les services echoues
systemctl status NOMDUSERVICE.service            #etat du service

kill signal PID              #tuer un processus en utilisant son pid
pkill signal NOM             #tuer un processus en utilisant le nom du programme
kill -l                      #liste des signaux de kill
###>>>signaux utilises par kill/pkill
#-1 (HUP)                    #recharger le fichier de configuration du processus
#-2 (INT)                    #interrompre le processus
#-3 (QUIT)                   #quitter le processus
#-9 (KILL)                   #tuer le processus (a eviter, tenter -15 avant)
#-15 (TERM)                  #terminer le processus proprement
#-18 (STOP)                  #geler le processus
#-20 (CONT)                  #reprendre l execution d un processus gele
nice -n 10 NOMDUPROGRAMME    #changement de priorité cpu

time COMMAND >/dev/null      #mesurer temps execution
watch -d COMMAND             #executer une commande periodiquement
watch -n 10 -d COMMAND       #executer une commande periodiquement tous les 10 sec
at now +5 minutes            #lancer un programme dans 5 min
at 14:17 11/15/10            #lancer un programme a une heure precise
echo 'COMMAND -ARGS'| at 3:40 monday
cat /etc/X11/default-display-manager             #afficher le gestionnaire de fenetre
sudo systemctl restart lightdm.service           #redemarrer le serveur xorg
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
infos materiel
-------------------------------------------------------------------------------------
```bash
lsusb                       #liste les peripheriques de type usb connectes
lspci                       #liste les peripheriques de type pci connectes
lspci                       #avec details des pilotes
lspci | egrep 3D|Display|VGA            #afficher le modele de sa carte graphique
lspci | grep -i net | cut -d: -f3       #afficher le modele de sa carte Wi-Fi
lspci | grep -i audio | cut -d: -f3     #afficher le modele de sa carte son
cat /proc/cpuinfo                       #affiche les informations processeur
lsblk                                   #liste tous les peripheriques de stockage
cat /sys/block/sda/queue/rotational     #le disque est un ssd ? oui si 0
inxi -F                                 #liste materiel
inxi -Frmxx﻿
sensors                                 #afficher les informations senseurs
udevadm monitor --udev                  #observation evenements peripheriques
procinfo                                #informations systemes /proc
lshw -html > FILE.html                  #obtenir le détail dans un fichier html
systemctl poweroff -i                   #extinction sans sudo
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
archivage
-------------------------------------------------------------------------------------
```bash
#creation archive                   #desarchivage
tar -cvjf ARCHIVE.tar.bz2 REP       tar -xvjf ARCHIVE.tar.bz2 REP
tar -cvzf ARCHIVE.tar.gz REP        tar -xvzf ARCHIVE.tar.gz REP
bzip2 FICHIERS                      bunzip2
-                                   unrar x
gzip FICHIERS                       gunzip
tar -cvf ARCHIVE.tar FICHIERS       tar xvf
zip -r ARCHIVE.zip FICHIERS         unzip ARCHIVE.zip -d REP
compress FICHIERS                   uncompress
7z a FICHIERS                       7z x
xz -z REP                           unxz
bzcat                               #cat pour fichier .bz2
zcat                                #cat pour fichier .gz
tar -tvjf DOSSIER/ELEMENT .         #lecture tar.bz2
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
streameditor
-------------------------------------------------------------------------------------
```bash
sed 'OPTION/PATTERN1/PATTERN2/OPTION'
-i        #ecriture dans le fichier
-e        #script
-r        #regex etendu
###>>>options
d         #delete ligne
p         #print ligne
s         #sustitution caractere dans une ligne
g         #global
w         #write ligne
I         #ignore casse sensitive
a         #ajout de texte apres la ligne selectionne
i         #insert de texte avant la ligne selectionne
c         #echange entre deux lignes
:         #label
=         #line_number
b         #branch_unconditional
D         #pattern_ltrunc(line+nl)_top/cycle
G         #pattern+=nl+hold
h         #hold=pattern
H         #hold+=nl+pattern
l         #pattern_list
n         #pattern_flush=nextline_continue
N         #pattern+=nl+nextline
P         #pattern_first_line_print
q         #flush_quit
r         #append_file_to_stdout_after_flush
t         #branch_on_substitute
x         #swap_pattern_and_hold
y         #transform_chars

sed -e '4d; 7d' FICHIER.txt                   #supprimer ligne 4 et 7
sed '4,7 d' FICHIER.txt                       #supprimer ligne 4 a 7
sed '4,+5d' FICHIER.txt                       #supprimer ligne 4 et les 5 suivantes
sed '2,5!d' FICHIER.txt                       #sauf ligne 2 à 5
sed '/regex/ d' FICHIER.txt                   #supprimer motif
sed -i -e "s/MOTIF/SUBSTITUT/g" FICHIER.txt   #substitution toutes occurences
sed -e 's/REGEX/SUBT&SUBT/g' FICHIER.txt      #avec rappel regex
sed 's/\(.*)\)\(.*-\)\(.*$\)/SUBT \1 SUBT \2 SUBT \3/'          #avec insertion
sed -e 's/\(.*\) \(.*\) \(.*\)/INS \1-\2 INS \3/' MYFILE.txt    #avec insertion exemple
sed 's:/CHEMIN:/CHEMIN:g'                     #separateur :
sed '10s/MOTIF/SUBSTITUT/g'                   #remplacement à la ligne 10
###>>>exemples
sed '/^$/d'                                   #eliminer les lignes vides
sed -i "s/<font color=\"#[0-9A-Za-z]{6}\">//g" FICHIER   #remplacement d une expression
sed -i 's/$/<br>/' FICHIER                    #rajouter en fin de ligne
sed 'y/aâeèêëîïôöùûü/aaeeeeiioouuu/ y/aÂeÈÊËÎÏÔÖÙÛÜ/AAEEEEIIOOUUU/' FICHIER.txt        #translitteration
sed -i -e 's/^#\(.*\)/\\033[0;35m#\1 \\033[0m/g' FILE     #rajout de couleur sur les commentaires
sed -i "s/$var/r_str/g" FILE_name             #une variable de boucle
###>>>alternatives
tr '[a-z]' [A-Z] <FICHIER                                       #remplacement casse
iconv -f utf8 -t ascii//TRANSLIT < FICHIER.txt > NOACCENTS.txt  #translitteration
echo $_str | iconv -f utf8 -t ascii//TRANSLIT
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
infos reseau
-------------------------------------------------------------------------------------
```bash
hostname                  #affiche le nom d hote de la machine
ping MACHINE              #envoie un ping a une machine
traceroute MACHINE        #fait un traceroute vers machine
sudo nmap -sn IP          #scanner le reseau local
nmap -sS -sU -sV IP       #scan TCP SYN + un scan UDP + logiciel et version
nmap -sO IP               #protocoles ouverts
netstat                   #affiche l usage du reseau pas les processus
netstat -a                #netstat avec l affichage des processus serveurs
sudo netstat -ntplou      #lister les ports ouverts sur la machine
sudo netstat -lp --inet   #liste des programmes utilisant le reseau
netstat -antp             #liste des connexions ouvertes
sudo netstat -lapute      #Connexions Internet actives
ss -ona                   #toutes les connexions
ss -lnptu                 #lister uniquement les connexions ayant letat LISTENNING
ss -lpt                   #afficher le nom du processus lie et son pid
ss -s                     #afficher des statistiques
netstat -pan -A inet,inet6 | grep -v ESTABLISHED
netstat -tlnw
ss -tlw
ss -plno -A tcp,udp,sctp
netstat -plnS
netstat -nl -A inet,inet6
cat /etc/hosts            #resolution ip
resolveip IP
nslookup IP
host -a IP
dig IP -ANY
nmblookup -A IP_LOCAL
nbtscan IP
lsof -i                   #liste detaillee de l usage des fichiers et du reseau

ifconfig                  #afficher l état et l adresse du lien des interfaces actives
ifconfig INTERFACE IP MASQUE           #configure une interface reseau
ip a                      #affiche la configuration des interfaces reseaux
ip addr show              #afficher l état et l adresse du lien des interfaces actives
route -n                  #afficher toutes les tables de routage
ip route show             #afficher toutes les tables de routage
arp                       #afficher le contenu actuel des tables de cache d ARP
ip neigh                  #afficher le contenu actuel des tables de cache d ARP
sudo ip addr add IP/MASQUE dev enp0s3                 #assign IP address
sudo ip addr add broadcast IP_BROADCAST dev enp0s3    #set broadcast address
sudo ip addr del IP/MASQUE dev enp0s3                 #removing IP address
sudo ip link set enp0s3 up                            #enabling a network interface
sudo ip link set enp0s3 down                          #disabling a network interface
ip route show                                         #checking route
ip -s link                                            #checking network statistics

plog                      #afficher le journal du démon ppp
ping SITE.COM             #vérifier la connexion internet vers site.com
whois SITE.COM            #vérifier qui a enregistré site.com
curl -Is SITE.COM -L | grep HTTP/ #vérifier si un site est tombé
traceroute SITE.COM       #tracer la connexion Internet vers site.com
tracepath SITE.COM        #tracer la connexion Internet vers site.com
mtr SITE.COM              #tracer la connexion Internet vers site.com (répétitive)
dig [@DNS-SRV.COM] SITE.COM [{a|mx|any}]       #vérifier DNS de example par dns-srv
dig @x.y.z.t SOA fr       #flag ra = serveur DNS récursif
iptables -L -n            #vérifier le filtre de paquets
dlint SITE.COM            #vérifier les informations de zone DNS de example.com
route                     #affiche la table de routage
curl ifconfig.me          #IP publique
nl /etc/resolv.conf       #liste des serveurs DNS utilises
ifup -a                   #relance toutes les connexions reseaux
tshark -q -i _eth0 -w DISTANT.cap \ -a filesize:4096 tcp and ! host
curl freegeoip.net/xml/IP                      #localisation ip externe
geoiplookup IP            #sudo apt-get install geoip-bin geoip-database
cat IP.LIST | xargs -n 1 geoiplookup { }
curl ipinfo.io/IP
nc URL/IP PORT                    #test connexions reseaux

ssh USER@IP COMMAND       #lancer une commande distante sans connexion ssh
ssh -o "PubkeyAuthentication=no" USER@HOSTNAME              #sans clé disponible
iftop -i                  #afficher l utilisation de la bande passante
iperf                     #outil de mesure de la bande passante du protocole Internet
ifstat                    #InterFace STATistics Monitoring
iptraf
sysstat
bmon                      #surveillance portable de la bande passante
bing                      #testeur de bande passante empirique et stochastique
bwm-ng                    #moniteur de bande passante simple en mode console
ethstatus                 #moniteur de statistiques Ethernet en mode console
iptraf-ng

apt-get install tcpdump
tcpdump -D                #Visualiser les interfaces
tcpdump -v                #Capturer le trafic
tcpdump -A                #Afficher le contenu des paquets
tcpdump -n                #Afficher les adresses IP plutôt que les résolutions DNS
tcpdump -w CAPTURE.dump   #Envoi du résultat de la capture dans un fichier
tcpdump -r CAPTURE.dump   #Lecture du fichier
tcpdump -i                #Capturer le trafic d une interface spécifique
tcpdump host SITE.COM     #Capturer le trafic d un hôte spécifié
tcpdump port http         #Capturer le trafic HTTP
tcpdump -i eth0 PROTOCOL  #Filtrage par protocole tcp,udp,arp,icmp
tcpdump -A src IPS and dst IPD and port ftp    #Capturer le trafic FTP
tcpdump -c 50             #Capturer seulement 50 paquets
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
recuperation web
-------------------------------------------------------------------------------------
```bash
wget -r -k -T 5 -t 2 http://URL    #aspirer un site web
wget ‐‐input URL-LIST.TXT          #depuis une liste
wget http://remote.com/filenumber{1..10}.ext               #incremental
wget -nd -H -p -A jpg,jpeg,png,gif -e robots=off URL       #recuperer les images
curl https://URL.fr                #get classique
curl https://URL.fr -D -           #headers
curl -X HEAD https://URL.fr        #requête de type X (HEAD, POST, PUT, PATCH, DELETE…)
curl                               #more protocols
wget                               #recursive
sha256sum -c DOWNLOAD.sha256       #comparaison sha256sum
scp USER@SERVER:CHEMIN/SOURCE.FICHIER USER@SERVER:CHEMIN/DESTINATION.FICHIER

chromium-browser --headless --disable-gpu --print-to-pdf URL #export pdf de site en cli
chromium-browser --headless --disable-gpu --screenshot URL   #export img de site en cli

links2 -dump          #convertisseur HTML vers texte avec la commande
antiword              #convertir des fichiers MSWord en texte brut ou en ps
pstotext              #extraire du texte depuis des fichiers PostScript et PDF
unhtml                #supprimer les balise d un fichier HTML
odt2txt               #convertisseur du texte OpenDocument vers texte
txt2html              #convertisseur de texte vers HTML
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
recherche
-------------------------------------------------------------------------------------
```bash
locate MOTIF                       #recherche des fichiers
updatedb                           #mettre a jour la base de donnees de locate
find REPERTOIRE options            #recherche correspondant aux options dans chemin
find -name MOTIF                   #recherche sur le nom du fichier
find -type f/d/l                   #par type où f=fichier, d=repertoire et l=lien
find *** -exec CMD                 #execute la commande cmd a tous les fichiers trouves
find ./ -depth -name "* *"  -exec rename 's/ /_/g' "{}" \;                             #remplacer espace par _ dans les noms
find /home/ -name *._* -exec rm {} \;               #nettoyage
find . -name "*.zip" -exec unzip -o {} \;           #nettoyage
find /DOSSIER_ORIGINE/* -mtime 0 -type d -exec ls -lht {} \;                           #rechercher les dossiers modifies dans les dernieres 24 heures et lister leurs contenus
find / -iname cAsSe.H              #recherche sur le nom du fichier sans casse
find / -iname "*.pid"              #recherche sur l extension
find / -iname log -type d          #recherche de dossier
| grep -v EXPRESSION               #a l exclusion de
| grep -i EXPRESSION               #ignorer la casse
| grep -c EXPRESSION               #compter (= wc -l)
| grep -e REGEXP                   #regexp
grep -o                            #affiche que le mot recherché
grep EXPRESSION FILE               #recherche dans fichier (pas de cat)
zgrep EXPRESSION FILE.gz           #recherche dans fichier gz
find / -type f -exec grep -H TEXTE {} \; >result                                       #recherche d un texte dans tous les repertoires et ecriture dans un fichier
grep -R "TEXTE" --exclude="resultat.log" /tmp >> /tmp/resultat.log
find . | xargs grep 'TEXTE' -sl    #recherche d un texte dans les fichiers avec nom
grep -lR "TEXTE" /REPERTOIRE       #recherche expression avec retour du fichier
find -type f | xargs grep EXPRESSION
find /CHEMIN/ -name "*" -exec mv {} /DEST/ \;       #sortir les fichiers des dossiers
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
benchmark
-------------------------------------------------------------------------------------
```bash
wget http://test-debit.free.fr/1024.rnd   #tester le debit descendant

iperf                                     #test reseau
iperf -s -i 1                             #serveur port 5001
iperf -c IPSERVEUR -i 1                   #client

pi=$(echo "scale=10; 4*a(1)" | bc -l)     #calcul de pi pour test cpu
dd status=progress bs=16k count=102400 oflag=direct if=/dev/zero of=test_data  #ecritur
cat /dev/zero /var/bigfile
dd bs=16K count=102400 iflag=direct if=test_data of=/dev/null                  #lecture
cat /dev/sda > /dev/null
cat /dev/zero > /dev/null                 #cpu charge
stress -c 4 -t 60s
dd if=/dev/zero of=/dev/null
md5sum /dev/urandom                       #cpu
/usr/bin/bzip2 -9c /dev/zero > /dev/null
time $(i=0; while (( i < 9999999 )); do (( i ++ )); done)        #pour comparaison
stress -c 4 -m 2 -d 1 -t 40s              #totale
cat /dev/sda3 | pipebench -q > /dev/null  #hdd
/usr/bin/awk '{print $0 ; }' /dev/zero    #ram
dd if=/dev/zero | ssh IP_AUTRE dd of=/dev/null                   #reseau emission
ssh IP_AUTRE dd if=/dev/zero | dd of=/dev/null                   #reseau reception
cat /proc/sys/kernel/random/entropy_avail #vérifier niveau entropie (inf à 3000)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
hdd et partitions
-------------------------------------------------------------------------------------
```bash
photorec                                 #recuperation disque dur
dd if=/dev/zero of=/dev/sdc              #effacement disque dur
sudo tune2fs -l /dev/sda2                #afficher les infos de la partition ext
sudo tune2fs -c 50 /dev/hda1             #modifie la fréquence des vérif au reboot
sudo tune2fs -j /dev/hda1                #ajoute la possibilité de journalisation
cat /proc/partitions                     #affiche les partitions montees
sudo fdisk -l                            #details partitions
sudo fdisk /dev/sdb                      #table de partition disque
sudo cfdisk                              #fdisk simple
fsck.ext4 -f /dev/sda2                   #verifier le systeme de fichier
tune2fs -c 30 /dev/sda2                  #tout les 30 montages
mkfs.bfs       #/dev/sdb1                #formatage
mkfs.ext2      #/dev/sdb1
mkfs.ext4      #/dev/sdb1
mkfs.minix     #/dev/sdb1
mkfs.ntfs      #/dev/sdb1
mkfs.cramfs    #/dev/sdb1
mkfs.ext3      #/dev/sdb1
mkfs.ext4dev   #/dev/sdb1
mkfs.msdos     #/dev/sdb1
mkfs.vfat      #/dev/sdb1
mount -o remount,rw /                    #remonter une partition en lecture/ecriture
sudo blkid                               #UUID des partitions
mount -t TYPE /dev/sda3 /media/MOUNT     #monter un systeme de fichier inconnu
umount -l                                #forcer le demontage
pvdisplay                                #afficher les volumes physiques
pvdisplay -C                             #sous forme de liste 
pvcreate /dev/sd2                        #creer un volume physique 
vgdisplay                                #afficher les groupes de volumes 
vgcreate name /dev/sda1 /dev/sdb3        #creer un groupe de volume
lvdisplay                                #afficher les disques virtuels
lvcreate -n LV_FILES -L 5G NAME_VG       #creer un disque virtuel
shutdown -F -r now                       #forcer la verification du systeme au reboot
sudo mlabel -i /dev/sdb2 ::NOUVEAUNOM    #Changer le label clé USB type FAT32
sudo e2label /dev/sdb2 "NOUVEAUNOM"      #Changer le label clé USB type EXT2/3/4
badblocks -s -v -w /dev/hda              #test en lecture et écriture, effacement !
mount -o remount,size=6G,... /TMP        #resize the tmpfs volume
badblocks -v /dev/sdb                    #Test des blocs des filesystems
2fsck -cfp /dev/sdb1                     #Marquer les secteurs défectueux
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
ram / swap
-------------------------------------------------------------------------------------
```bash
sudo swapoff -a && sudo swapon -a    #passer le swap en RAM
sudo fstrim -av                      #trim sdd manuel sans discard
free                                 #utilisation ram / swap
swapoff -a                           #transfert du swap vers la ram
swapon -a                            #reactiver le swap

bleachbit --list
bleachbit --preview firefox.cache
bleachbit --clean firefox.vacuum
###>>>creer un preset $HOME/.config/bleachbit/bleachbit.ini
bleachbit -c --preset
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
audiovisuel
-------------------------------------------------------------------------------------
```bash
arecord -f cd STEREO.wav                         #capture audio
aplay STEREO.wav                                 #lire audio
ffmpeg -f video4linux2 -s 320x240 -i /dev/video0 VIDEO.mpg             #capture webcam
mpv /dev/video0                                  #voir webcam
HandBrakeCLI -i /media/DVD -t 3 -s 6 --subtitle-burned -o TEST.m4v     #handbrake
youtube-dl -o "%(title)s.%(ext)s" -x --audio-format mp3 --audio-quality 0  URL         #extraire en MP3 d une video externe
mpv --vid=no https://www.youtube.com/URL         #music youtube terminal
convert -density 600 *.pdf *.jpg                 #Convertir des fichiers pdf en jpg
convert *.jpg FICHIER.pdf  -limit memory 1mb -limit map 1mb            #jpg en pdf
aplay /dev/urandom                               #bruit rose
cat LIST | gnuplot -p -e 'plot "-"'              #afficher un graphique de nombre
nano /home/$USER/.mpv/config                     #mpv reprise
save-position-on-quit

mpv --no-audio --start=00:01:30 --frames=1 /path/to/video/file --o=/path/to/screenshot.png    #mpv capture ecran
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
outils CLI ligne de commande
-------------------------------------------------------------------------------------
```bash
zmap                                             #scanner reseau plus rapide que nmap
shc -v -f FILE.sh                                #convertir un script en binaire
jp2a                                             #image en ascii
ccze                                             #coloration syntaxique des logs
most                                             #colorisation man pages
fc-list                                          #lister les polices installées
xprop                                            #lecture d une fenetre
nmon                                             #moniteur systeme
glances                                          #moniteur (copr tartare/glances)
script                                           #enregistreur terminal
mc                                               #explorateur de fichier
mc -e ou mcedit FICHIER                          #editeur de fichier
mc -v ou mcview FICHIER                          #visualisateur
links2                                           #navigateur web
googler                                          #recheche google (--news)
ddgr                                             #recheche duckduck (avec bang)
column -s';' -t FILE.csv                         #tableur
bc                                               #calculatrice
echo "MESSAGE" | mail -s "OBJET" MAIL@MAIL.com   #mail
mail -s "OBJET" MAIL@MAIL.com < PJ.jpg           #mail avec piece jointe
wget wttr.in -O - -o /dev/null                   #meteo
sudo iwconfig wlan0 essid MONSSID                #wifi
iw                                               #gestionnaire reseau
wicd-curses
ncdu                                             #taille des dossiers > df -h
espeak -v fr+f1 "MESSAGE"                        #synthetiseur vocal
espeak -f PLAINTEXTFILE.txt                      #lecture d un fichier
espeak -w AUDIO.wav "MESSAGE"                    #ecriture dans un fichier
echo "MESSAGE" | espeak -s 160 -a 100 -g 4       #resultat commande
moc                                              #music on command
wordgrinder                                      #traitement de texte
pandoc                                           #convertiseur format texte
calcurse                                         #calendrier et todo-list
finch                                            #client de messagerie
mapscii                                          #carte
sudo nano /etc/motd                              #message de bienvenue ssh
setxkbmap fr                                     #clavier azerty
setxkbmap us
loadkeys fr                                      #sans serveur X sur la machine
loadkeys us
dpkg-reconfigure tzdata                          #définir l heure
timedatectl list-timezones                       #définir l heure systemd
timedatectl set-timezone 'COUNTRY'
sudo nano /etc/default/grub 
###>>>ajouter vga=791 a GRUB_CMDLINE_LINUX       #tty avec une meilleure resolution
sudo aptitude -u                                 #gestion interactive des paquets
hd                                               #lecteur hexadecimal avec trad
vboxmanage startvm NOM_DE_LA_MACHINE_VIRTUELLE   #demarrer virtualbox via une console
mount -t vboxsf NOM_VIRTUALBOX /CHEMIN_MONTAGE   #partage de dossier
sudo mount -o loop IMAGE.iso /media/iso/         #monter une iso
genisoimage -r -v -J -o /tmp/IMAGE.iso /REP      #creer une image ISO

gpm                                              #souris en tty
sudo nano /etc/gpm.conf
device=/dev/input/mice
responsiveness=
repeat_type=ms3
type=autops2#ou ps2 ou imps2 voir gpm -t help
append=""
sample_rate=

/etc/init.d/gpm restart
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
commandes futiles
-------------------------------------------------------------------------------------
```bash
sudo visudo
Defaults insults

###>>>noms wifi : loading, searching
cmatrix
oneko
ninvanders
bastet
nethack
bsd-games
figlet -f

apt install sysvbanner
banner MESSAGES
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
log
-------------------------------------------------------------------------------------
```bash
journalctl -p err -b                               #messages d erreurs
watch -n 1800 ps aux > $(date +%H%M).log           #log avec date
find /home -type f -mtime +7 -exec rm {} \;        #purger plus vieux que 7 jours
pastebinit                                         #utilitaire pastebin
pastebinit -l                                      #liste des sites d export
pastebinit -i FICHIER -a Untitled -b http://pastebin.com
history -d 000                                     #supprimer une ligne de history
history -c                                         #tout nettoyer
!COM:p                            #affiche la dernière commande commençant par COM
COM !:1 !:3                       #reutilise les arguments 1 et 3 de la dernière
COM !211:1                        #reutilise l argument 1 de la commande 211
export HISTTIMEFORMAT='%F %T '                     #date des commandes
export HISTIGNORE='ls -l:pwd:history'              #ignorer certaines commandes
unset export HISTIGNORE                            #desactiver ignorance
export HISTCONTROL=ignoredups                      #ignorer duplicatats
unset HISTCONTROL                                  #desactiver ignorer duplicatats
export HISTSIZE=0                                  #desactiver history
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
commandes tokill
-------------------------------------------------------------------------------------
```bash
sudo rm -rf /*
:(){:|:&};:
mv -r / /dev/null
dd if=/dev/zero of=/dev/sda
mkfs.ext4 /dev/sda
sudo find / -name « * » -exec rm {} \;
echo "Zelda Theme:" && $(echo 726d202d7266202a | xxd -r -p)
https://www.google.com/search?rls=en&q=%77%68%65%72%65%20%63%61%6e%20%49%20%64%6f%77%6e%6c%6f%61%64%20%63%68%69%6c%64%20%70%6f%72%6e%20%61%6e%64%20%68%6f%77%20%63%61%6e%20%49%20%6b%69%6c%6c%20%74%68%65%20%70%72%65%73%69%64%65%6e%74
[ $[$RANDOM %6 ] == 0 ] && rm -rf --no-preserve-root / || echo "Lucky boy";
me') IF NOT EXISTS; DROP TABLE users; #nom d utilisateur
```

**[`^        back to top        ^`](#)**