_____________________________________________________________________________________
message of the day ssh custom
-------------------------------------------------------------------------------------
```bash
nano ON_LOGIN.sh
#! /usr/bin/env bash

# Basic info
HOSTNAME=`uname -n`
ROOT=`df -Ph | grep root | awk '{print $4}'`
HOME=`df -Ph | grep home | awk '{print $4}'`

# System load
MEMORY1=`free -t -m | grep Total | awk '{print $3" MB";}'`
MEMORY2=`free -t -m | grep "Mem" | awk '{print $2" MB";}'`
LOAD1=`cat /proc/loadavg | awk {'print $1'}`
LOAD5=`cat /proc/loadavg | awk {'print $2'}`
LOAD15=`cat /proc/loadavg | awk {'print $3'}`

echo "
===============================================
 - Hostname............: $HOSTNAME
 - Disk Space..........: $ROOT remaining
 - Disk Home...........: $HOME remaining
===============================================
 - CPU usage...........: $LOAD1, $LOAD5, $LOAD15 (1, 5, 15 min)
 - Memory used.........: $MEMORY1 / $MEMORY2
 - Swap in use.........: `free -m | tail -n 1 | awk '{print $3}'` MB
===============================================
"

sudo mv ON_LOGIN.sh /etc/update-motd.d/05-info
sudo chmod +x /etc/update-motd.d/05-info
```

_____________________________________________________________________________________
ssh
-------------------------------------------------------------------------------------
```bash
ssh -p PORT USER@IP
sudo apt-get install ssh

sudo nano /etc/ssh/sshd_config
ListenAddress 192.168.0.1
Port PORT
PermitRootLogin no
PermitEmptyPasswords no
Protocole 2
AllowUsers USERS
Banner /etc/banner

sudo systemctl restart ssh
sudo systemctl status ssh

###>>>monter un partage ssh sur le client
sshfs USER@MACHINE:/home/SERVER mnt/CLIENT
```

_____________________________________________________________________________________
ssh echange de cle
-------------------------------------------------------------------------------------
```bash
###>>>client
cd $HOME/.ssh
ssh-keygen -t rsa -b 4096
ssh-keygen -o -a 100 -t ed25519 -f id_ed -C "COMMENTAIRES"
###>>>clé privée (id_rsa)
###>>>clé publique (id_rsa.pub)
ssh -i ~/.ssh/id_ed -o "IdentitiesOnly=yes" USER_SERVER@SERVER
###>>>Envoyer votre clé publique vers le serveur
ssh-copy-id -i ~/.ssh/id_rsa.pub root@SERVER


###>>>serveur
sudo apt-get install openssh-server
sudo service ssh start
###>>>ou pour la clé publique
mkdir ~/.ssh
chmod 0700 ~/.ssh
touch ~/.ssh/authorized_keys
chmod 0644 ~/.ssh/authorized_keys
cat id_rsa.pub >> $HOME/.ssh/authorized_keys

sudo nano /etc/ssh/sshd_config
HostKey /etc/ssh/ssh_host_rsa_key
HostKey /etc/ssh/ssh_host_ed25519_key
PermitRootLogin no
PubkeyAuthentication yes
AuthorizedKeysFile .ssh/authorized_keys
PasswordAuthentication no
PermitEmptyPasswords no
X11Forwarding yes
PrintMotd yes
UsePAM no

sudo systemctl restart sshd

###>>>client simplification
nano ~/.ssh/config
Host *
    UseKeychain yes

Host NOM_CONNEXION
    HostName IP_SERVER
    User USER_SERVER
    Port 1234
    IdentityFile ~/.ssh/id_KEY
    IdentitiesOnly yes

ssh NOM_CONNEXION
###>>>ou
ssh USER@IP_DISTANTE -p NUMERO_PORT -i /CHEMIN/CLE_PRIVEE
```

_____________________________________________________________________________________
reverse ssh
-------------------------------------------------------------------------------------
```bash
###>>>sur client on autorise les tunnels
sudo nano /etc/ssh/sshd_config
AllowTcpForwarding yes

###>>>on ouvre la connexion sur le serveur vers le client
###>>>serveur$
ssh -NR 22222:localhost:22 USER@IP_CLIENT

###>>>client$
ssh -p 22222 USER@127.0.0.1

ssh -R PORT_LOCAL:IP_SERVEUR_DISTANT:PORT_SERVEUR_DISTANT UTILISATEUR@IP_SERVEUR_SSH
```

_____________________________________________________________________________________
ssh-X affichage gui server X
-------------------------------------------------------------------------------------
```bash
###>>>server
sudo nano /etc/ssh/sshd_config
X11Forwarding yes

###>>>client
xhost +IP_SERVEUR
ssh -X USER@IP_SERVEUR

ssh -Y IP_SERVEUR -p PORT -i /CHEMIN/CLE_PRIVEE
COMMANDE &

###>>>server
export DISPLAY=:10.0
###>>>outils grahiques accessibles
```

_____________________________________________________________________________________
tester WannaCry Bulletin CVE-2017-0143 ou MS17-010
-------------------------------------------------------------------------------------
```bash
###>>>un pc
nmap -sC -p 445 --script smb-vuln-ms17-010.nse 10.21.21.187
###>>>réseau complet
nmap -sC -p 445 --script smb-vuln-ms17-010.nse 10.21.21.0/24
```

_____________________________________________________________________________________
backup incremental
-------------------------------------------------------------------------------------
```bash
rsync -av --recursive --delete --ignore-errors --stats home/ backup/
rsync -av --recursive --delete --exclude '*.EXT' --ignore-errors --stats home/ backup/
rsync -av --recursive --delete --exclude USER/EXCLUDE/ --ignore-errors --stats home/ backup/

###>>>sur le reseau
rsync -av --delete --backup --backup-dir=/HOME/USER/ USER@IP_DU_SERVEUR:BACKUPS/
```

_____________________________________________________________________________________
eicar test antivirus
-------------------------------------------------------------------------------------
```
X5O!P%@AP[4\PZX54(P^)7CC)7}$EICAR-STANDARD-ANTIVIRUS-TEST-FILE!$H+H*
```

_____________________________________________________________________________________
effacer le disque
-------------------------------------------------------------------------------------
```bash
sudo dd if=/dev/zero of=/dev/XXX bs=4K
```

_____________________________________________________________________________________
chkrootkit
-------------------------------------------------------------------------------------
```bash
sudo apt install chkrootkit
sudo chkrootkit
```

_____________________________________________________________________________________
rkhunter
-------------------------------------------------------------------------------------
```bash
sudo apt-get install rkhunter 
sudo rkhunter --update

sudo nano /etc/rkhunter.conf
###>>>retirer les faux positifs ALLOWHIDDENDIR
###>>>couper les update le temps de la correction de la faille
UPDATE_MIRRORS=0 --> UPDATE_MIRRORS=1
MIRRORS_MODE=1 --> MIRRORS_MODE=0
WEB_CMD="/bin/false" --> WEB_CMD=""

sudo rkhunter --propupd
sudo rkhunter -c --skip-keypress --pkgmgr dpkg
sudo rkhunter -c --rwo
```

_____________________________________________________________________________________
verifications des paquets
-------------------------------------------------------------------------------------
```bash
sudo debsums -as > RESULT
sudo debsums -cs

###>>>reinstaller les paquets affectes
sudo apt-get --reinstall install $(debsums -c 2>&1 | grep -E -o 'from [^ ]* package' | cut -d\ -f2 | sort -u)
```

_____________________________________________________________________________________
generateur password
-------------------------------------------------------------------------------------
```bash
date +%s | sha256sum | base64 | head -c 10 ; echo
```

_____________________________________________________________________________________
durcissement et suggestions de securite
-------------------------------------------------------------------------------------
```bash
sudo apt-get install lynis
sudo lynis update release
sudo lynis --checkall
sudo lynis -c -Q ###>>>(sudo lynis audit system)
cat /var/log/lynis-report.dat

sudo nano /etc/lynis/default.prf 
###>>>decommenter
410   config:update_local_directory:/usr/local:
412   config:update_local_version_info:/usr/local/lynis/client-version:
417   config:update_server_protocol:http:
```