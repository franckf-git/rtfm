## Index

[TOC]

- [changer serveur DNS en CLI](#changer-serveur-dns-en-cli)
- [DNScrypt](#dnscrypt)
- [transformer une machine en routeur](#transformer-une-machine-en-routeur)
- [monter un partage samba au demarrage](#monter-un-partage-samba-au-demarrage)
- [changement adresse mac](#changement-adresse-mac)
- [desactiver ipv6](#desactiver-ipv6)
- [tor en cli](#tor-en-cli)
- [protonvpn](#protonvpn)
- [montage nfs](#montage-nfs)
- [gestion ssmtp mailutils](#gestion-ssmtp-mailutils)

_____________________________________________________________________________________
changer serveur DNS en CLI
-------------------------------------------------------------------------------------
```bash
cat /etc/resolv.conf 
sudo nano /etc/dhcph.conf
static domain_name_servers=80.67.169.12

sudo nano /etc/dhcp/dhclient.conf
supersede domain-name-servers 80.67.169.12;

sudo nano /etc/network/interfaces
dns-nameservers 80.67.169.12
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
DNScrypt
-------------------------------------------------------------------------------------
```bash
install dnscrypt-proxy
dig SITE.TEST | grep server
#mettre 127.0.0.1 en dns dans network connections
/usr/share/dnscrypt-proxy/dnscrypt-resolvers.csv
#choix selon pays, DNSsec et log
dnscrypt-proxy -R CHOIXDNS
dig SITE.TEST | grep server
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
transformer une machine en routeur
-------------------------------------------------------------------------------------
```bash
###>>>creer interface reseau virtuel
ifconfig eth0:0 192.168.11.254 netmask 255.255.255.0
###>>>activer le routage
echo 1 > /proc/sys/net/ipv4/ip_forward
###>>>mettre une route dans sa table de routage
route add default gw 192.168.10.254
###>>>route retour
route add default gw 192.168.11.254

###>>>sniffer paquets
tcpdump -i eth0 icmp

route -n
traceroute
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
monter un partage samba au demarrage
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/fstab
//IP_SERVEUR/PARTAGE    /MEDIA/PARTAGE cifs credentials=/root/.smbcredentials,iocharset=utf8,gid=1000,uid=1000,_netdev 0 0

nano /root/.smbcredentials
username=IDENTIFIANT
password=MOT_DE_PASSE
domain=NOM_DE_DOMAINE(facultatif)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
changement adresse mac
-------------------------------------------------------------------------------------
```bash
/etc/init.d/networking stop
ifconfig eth0 hw ether 02:01:02:03:04:08
/etc/init.d/networking start
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
desactiver ipv6
-------------------------------------------------------------------------------------
```bash
###>>>redhat
sysctl -w net.ipv6.conf.all.disable_ipv6=1
sysctl -w net.ipv6.conf.default.disable_ipv6=1
###>>>debian
sudo nano /etc/sysctl.conf
#ajouter à la fin
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
tor en cli
-------------------------------------------------------------------------------------
```bash
sudo apt-get install tor

sudo nano /etc/tor/torrc 
SocksPort 9050
ControlPort 9051
CookieAuthentication 0

sudo systemctl restart tor.service
tor --verify-config
torify wget http://checkip.dyndns.org -O - -o /dev/null | cut -d : -f 2 | cut -d \< -f 1

sudo apt-get install privoxy

sudo nano /etc/privoxy/config
listen-address   localhost:8118
forward-socks5   /   localhost:9050
forward-socks4a   /   localhost:9050

sudo systemctl restart privoxy.service
/usr/bin/chromium --proxy-server="socks5://127.0.0.1:8118"
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
protonvpn
-------------------------------------------------------------------------------------
```bash
###>>>inscription sur ProtonVPN.com
###>>>Account > OpenVPN Login > OpenVPN Login | OpenVPN Password
sudo apt-get install openvpn
wget https://protonvpn.com/support/wp-content/uploads/2017/06/ProtonVPN_config_linux.zip
###>>>ou Download > Download All configurations
mkdir .protonvpn
unzip ProtonVPN_config_linux.zip -d .protonvpn
cd .protonvpn
sudo openvpn PAYS-00.protonvpn.com.udp1194.ovpn
###>>>insérez identifiants de connexion : OpenVPN Login | OpenVPN Password
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
montage nfs
-------------------------------------------------------------------------------------
```bash
###>>>serveur ports 2049 (TCP et UDP)
sudo apt install nfs-kernel
sudo mkdir /media/USBHDD1
sudo mkdir /media/USBHDD2
sudo fdisk -l
sudo mount -t auto /dev/sda1 /media/USBHDD1
sudo mount -t auto /dev/sdb1 /media/USBHDD2
nano /etc/exports
/CHEMIN/PARTAGE/ IP_CLIENT(rw,sync,no_subtree_check) #pas d espace entre ip et ()

exportfs -a
/etc/init.d/nfs-kernel-server reload
showmount -e 
nfstat

###>>>client
sudo apt install nfs-common
sudo showmount -e IP_SERVEUR
sudo mkdir /media/MONTAGE
sudo nano /etc/fstab
IP_SERVEUR:/CHEMIN/PARTAGE/ /media/MONTAGE nfs4 defaults,user,auto,noatime,intr 0 0
/dev/sda1 /media/USBHDD1 auto noatime 0 0
/dev/sdb1 /media/USBHDD2 auto noatime 0 0

sudo mount -a
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
gestion ssmtp mailutils
-------------------------------------------------------------------------------------
```bash
sudo apt install ssmtp mailutils

sudo nano /etc/ssmtp/ssmtp.conf
root=USER@MAIL.com
UseSTARTTLS=YES
mailhub=smtp-mail.MAIL.com:587
AuthUser=USER@MAIL.com
AuthPass=PASSWORD
rewriteDomain=
FromLineOverride=YES
hostname=USER@MAIL.com

sudo nano /etc/ssmtp/revaliases
root:USER@MAIL.com:smtp-mail.MAIL.com:587
user:USER@MAIL.com:smtp-mail.MAIL.com:587

echo "CONTENU DU MESSAGE" | mail -v -s "SUJET DU COURRIEL" DESTINATAIRE@MAIL.com

###>>>securisation car le mot de passe du mail est en clair
sudo chown root:mail /usr/sbin/ssmtp /etc/ssmtp{,/{ssmtp.conf,revaliases}}
sudo chmod 2711 /usr/sbin/ssmtp
sudo chmod o-rwx /etc/ssmtp /etc/ssmtp/ssmtp.conf; # 0750, 0640
sudo dpkg-statoverride --add root mail 2711 /usr/sbin/ssmtp
sudo dpkg-statoverride --add root mail 0750 /etc/ssmtp
sudo dpkg-statoverride --add root mail 0640 /etc/ssmtp/ssmtp.conf
sudo dpkg-statoverride --add root mail 0644 /etc/ssmtp/revaliases
```

**[`^        back to top        ^`](#)**