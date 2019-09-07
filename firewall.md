## Index

[TOC]

- [iptables](#iptables)
- [firewalld](#firewalld)
- [fedora remplacer firewalld par ufw](#fedora-remplacer-firewalld-par-ufw)
- [ufw](#ufw)
- [selinux](#selinux)
- [apparmor](#apparmor)
- [isolation](#isolation)

_____________________________________________________________________________________
iptables
-------------------------------------------------------------------------------------
```bash
dnf install iptables-services
#gui firestarter
systemctl enable iptables.service
systemctl enable ip6tables.service
systemctl start iptables.service
systemctl start ip6tables.service

sudo iptables -F
sudo iptables -X
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT DROP
sudo iptables -A OUTPUT -p TCP --dport http -j ACCEPT         # Port 80   (Http)
sudo iptables -A OUTPUT -p TCP --dport https -j ACCEPT        # Port 443  (Https)
sudo iptables -A OUTPUT -p TCP --dport dns -j ACCEPT          # Port 53  (dns)
sudo iptables -A FORWARD -j LOG_DROP
sudo iptables -A INPUT -j LOG_DROP
sudo iptables -A OUTPUT -j LOG_DROP

iptables -nvL --line-numbers # suppresion iptable
iptables -D INPUT {{num}}

###>>>Pings
sudo iptables -A INPUT -p icmp -j DROP
iptables -A OUTPUT -p icmp -m conntrack --ctstate NEW,ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT
iptables -A INPUT -p icmp --icmp-type time-exceeded -j ACCEPT
iptables -A INPUT -p icmp --icmp-type destination-unreachable \
  -j ACCEPT
###>>>accepter le reseau local
iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT
iptables -A OUTPUT -d 192.168.1.0/24 -j ACCEPT
iptables -A FORWARD -s 192.168.1.0/24 -j ACCEPT
###>>>autoriser les paquets entrants sur la boucle locale
iptables -A INPUT -i lo -j ACCEPT

iptables -vL
sudo iptables-save
systemctl list-unit-files | grep iptables
systemctl start iptables
systemctl enable iptables
service iptables save

iptables -S
###>>>Persisting Rules through Reboots (Debian/Ubuntu)
sudo apt-get install -y iptables-persistent
sudo service iptables-persistent start
sudo iptables-save > /etc/iptables/rules.v4
sudo service iptables-persistent restart

###>>>Persisting Rules through Reboots (CentOS/RedHat)
sudo chkconfig iptables on
sudo service iptables save
sudo service iptables start
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
firewalld
-------------------------------------------------------------------------------------
```bash
systemctl list-unit-files | grep firewall
systemctl enable firewalld
systemctl start firewalld
systemctl status firewalld
firewall-cmd --state

###>>>les zones disponibles
firewall-cmd --get-zones
firewall-cmd --list-all-zones
###>>>la zone active en cours
firewall-cmd --get-default-zone
firewall-cmd --get-active-zones
firewall-cmd --list-all
firewall-cmd --list-interfaces
###>>>passer sur une nouvelle zone
firewall-cmd --set-default-zone=drop
firewall-cmd --zone=ZONE --list-all
firewall-cmd --reload
firewall-cmd --complete-reload
###>>>les services disponibles
firewall-cmd --get-services
firewall-cmd --permanent --get-services
firewall-cmd --info-service=SERVICE
###>>>modifications permanentes
firewall-offline-cmd
###>>>editer un service
cp /usr/lib/firewalld/services/service.xml /etc/firewalld/services/service.xml

###>>>mode panique
firewall-cmd --panic-on
firewall-cmd --panic-off
firewall-cmd --query-panic

nano /etc/firewalld/services/service.xml
firewall-cmd --reload

firewall-cmd --list-ports                            #list of allowed ports
sudo firewall-cmd --add-port=port-number/port-type   #add a port for incoming traffic
sudo firewall-cmd --runtime-to-permanent             #new settings persistent

###>>>ajouter un service à la zone active
firewall-cmd --add-service=rtmp
###>>>bloquer les ping
firewall-cmd --zone=public --permanent --add-icmp-block=destination-unreachable
firewall-cmd --reload

###>>>bloquer tous les ports sortants sauf le web
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 2 -j DROP
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 80 -j ACCEPT
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 443 -j ACCEPT
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p tcp -m tcp --dport 53 -j ACCEPT
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 1 -p udp --dport 53 -j ACCEPT
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
fedora remplacer firewalld par ufw
-------------------------------------------------------------------------------------
```bash
systemctl list-unit-files | grep firewall
systemctl disable firewalld
systemctl list-unit-files | grep ufw
systemctl enable ufw
dnf remove firewalld
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
ufw
-------------------------------------------------------------------------------------
```bash
sudo ufw enable
sudo ufw default deny incoming
sudo ufw default deny outgoing
###>>>http
sudo ufw allow out 80/tcp
###>>>https
sudo ufw allow out 443/tcp
###>>>dns
sudo ufw allow out 53/udp
###>>>samba
sudo ufw allow out proto tcp to any port 139445
sudo ufw allow out proto udp to any port 137138
###>>>steam
sudo ufw allow out proto tcp to any port 27000:27030
sudo ufw allow out proto udp to any port 27000:27030
###>>>port ssh 12122
sudo ufw allow out 12122
###>>>port ntp
sudo ufw allow out 123
###>>>Autoriser le port du whois
sudo ufw allow out 43/tcp
sudo ufw reload
sudo ufw status verbose

sudo nano /etc/default/ufw
IP6=no

###>>>authoriser toutes les connexions entrantes du port 12122 sur le reseau local
sudo ufw allow from 192.168.1.0/24 proto tcp to any port 12122 

###>>>seulement a partir d une machine
sudo ufw allow from IP proto tcp to any port 12122

###>>>autoriser les ping / ok icmp codes output
sudo nano /etc/ufw/before.rules
-A ufw-before-output -p icmp --icmp-type destination-unreachable -j ACCEPT
-A ufw-before-output -p icmp --icmp-type source-quench -j ACCEPT
-A ufw-before-output -p icmp --icmp-type time-exceeded -j ACCEPT
-A ufw-before-output -p icmp --icmp-type parameter-problem -j ACCEPT
-A ufw-before-output -p icmp --icmp-type echo-request -j ACCEPT

###>>>refuser ping ext
sudo nano /etc/ufw/before.rules
# -A ufw-before-input -p icmp --icmp-type echo-request -j ACCEPT
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
selinux
-------------------------------------------------------------------------------------
```bash
sudo apt install selinux-basics selinux-policy-default auditd
sudo apt install setools setools-gui selinux-policy-devel setools-devel
selinux-activate  #to configure GRUB
#Reboot it will take a while to label the filesystems
check-selinux-installation
touch /.autorelabel && systemctl reboot #réétiquetage du système de fichiers

###>>>Enforcing
sudo sed -i 's/SELINUX=.*/SELINUX=enforcing/' /etc/selinux/config
###>>>Permissive
sudo sed -i 's/SELINUX=.*/SELINUX=permissive/' /etc/selinux/config
sudo systemctl restart selinux-basics.service

sediff                       #SELinux policy difference tool
sedta                        #domain transition analysis for SELinux policies
seinfo                       #SELinux policy information tool
seinfoflow                   #information flow analysis for SELinux policies
sesearch                     #SELinux policy query tool
sestatus                     #afficher le status actuel de SELinux
getenforce                   #obtenir le mode SELinux courant de votre machine Enforcing par defaut
sesearch                     #effectuer une recherche dans la politique actuellement configurée pour votre machine
getsebool                    #obtenir des informations sur les booléens
matchpathcon                 #afficher le contexte que devrait posséder un fichier
apol                         #gui
setenforce                   #modifier le mode SELinux de votre machine (0,1) jusqu au prochain démarrage
setsebool                    #modifier la valeur d un booléen
restorecon                   #restaurer un contexte en fonction de la politique courante
restorecon -R -v /FOLDER     #reétiquettage du dossier
chcon                        #modifier le contexte SELinux d un fichier
semanage                     #gérer les politiques SELinux
semanage port -l | grep ssh  #changer le port ssh
semanage port -a -t ssh_port_t -p tcp 2222

ls -Z                        #contexte de sécurité fichiers
ps axZ                       #contexte de sécurité processus
###>>>utilisateur:rôle:type:niveau        contexte SELinux

###>>>logs
sudo apt install setroubleshoot-server
sealert -a /var/log/audit/audit.log | less
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
apparmor
-------------------------------------------------------------------------------------
```bash
apt install apparmor apparmor-profiles apparmor-utils apparmor-profiles-extra
perl -pi -e 's,GRUB_CMDLINE_LINUX="(.*)"$,GRUB_CMDLINE_LINUX="$1 apparmor=1 security=apparmor",' /etc/default/grub
update-grub
reboot

aa-status
cat /etc/apparmor.d/PROFILES

###>>>mode stric, relaché, désactivé ou loggué
aa-enforce /CHEMIN/VERS/EXECUTABLE_OU_CONF
aa-complain /CHEMIN/VERS/EXECUTABLE_OU_CONF
aa-disable /CHEMIN/VERS/EXECUTABLE_OU_CONF
aa-audit /CHEMIN/VERS/EXECUTABLE_OU_CONF

###>>>créer un nouveau profil depuis zéro
aa-genprof /CHEMIN/VERS/EXECUTABLE_OU_CONF

###>>>programmes qui ont au moins une connexion réseau ouverte
aa-unconfined --paranoid
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
isolation
-------------------------------------------------------------------------------------
```bash
firetools                            #gui
firemon                              #debug
firejail --net=none PROG             #sans réseau
firejail --seccomp --private PROG    #mode privé
firecfg                              #auto /usr/local/bin changer en /usr/bin/firejail
ln -s /usr/bin/firejail /usr/local/bin/PROG   #manuel
firejail --list                      #applications utilisant firejail
firejail --tree                      #verifications
```

**[`^        back to top        ^`](#)**