## Index

[TOC]

- [Serveur Web Apache](#serveur-web-apache)
- [simple serveur web](#simple-serveur-web)
- [redirection html](#redirection-html)
- [serveur Web](#serveur-web)
- [bloquer l indexation web](#bloquer-l-indexation-web)
- [html minimal](#html-minimal)
- [phpmyadmin minimal](#phpmyadmin-minimal)
- [nginx simple authentification](#nginx-simple-authentification)
- [serveur dhcp](#serveur-dhcp)
- [fail2ban](#fail2ban)
- [denyhosts (alternative fail2ban)](#denyhosts-alternative-fail2ban)
- [ftp](#ftp)
- [serveur dns](#serveur-dns)

---

## Serveur Web Apache

```bash
sudo aptitude install apache2
sudo chown -R www-data:pi /var/www/html/
sudo chmod -R 770 /var/www/html/
ab #outil utile pour une vérification rapide des performances. Pour des tests de charge plus complexes, essayez siege
apachectl graceful #demarrage sécurisé des services hhtpd
```

**[`^ back to top ^`](#)**

---

## simple serveur web

```bash
python -m SimpleHTTPServer 7777
python3 -m http.server 8080
php -S localhost:8000
```

**[`^ back to top ^`](#)**

---

## redirection html

```html
<!DOCTYPE html> <meta http-equiv="refresh" content="0;URL=NOUVELLE_URL" />
```

**[`^ back to top ^`](#)**

---

## serveur Web

```bash
###>>>debian depots
###>>>upg
###>>>ssh echange de cle
###>>>firewall ufw

sudo apt-get install -y haveged curl git unzip zip dirmngr nginx mariadb php-common php-pear php-zip php7.0-cli php7.0-common php7.0-curl php7.0-dev php7.0-fpm php7.0-gd php7.0-imap php7.0-intl php7.0-json php7.0-mbstring php7.0-mysql php7.0-opcache php7.0-pspell php7.0-readline
mysql_secure_installation
git clone https://github.com/letsencrypt/letsencrypt
cd letsencrypt
./letsencrypt-auto --help

yum install httpd php php-mysql mariadb-server mariadb sqlite php-dom php-mbstring php-gd php-pdo php-process wget bzip2 mod_ssl openssl
apt-get install apache2 php5 php5-common php5-gd php5-mysql php5-curl mariadb-server php5-sqlite sqlite wget bzip2 openssl
```

**[`^ back to top ^`](#)**

---

## bloquer l indexation web

```bash
<?php header("Location: https://URLREDIRECT"); exit; ?> #redirection php

nano /var/local/www/robots.txt
User-agent: *
Disallow: /
```

**[`^ back to top ^`](#)**

---

## html minimal

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <link rel="stylesheet" href="style.css" />
    <title>Titre de la page</title>
  </head>

  <body>
    <script>
      alert("runs");
    </script>
    <!-- Ceci est un commentaire -->
    <video width="400" height="222" controls="controls">
      <source src="1917.webm" type="video/webm" />
      Ici alternative a la vidéo
    </video>
    <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"
      ><img
        alt="Licence Creative Commons"
        style="border-width:0"
        src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png"/></a
    ><br />Ce(tte) œuvre est mise à disposition selon les termes de la
    <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"
      >Licence Creative Commons Attribution - 4.0 International</a
    >.
  </body>
</html>
```

**[`^ back to top ^`](#)**

---

## phpmyadmin minimal

```bash
mkdir -p /var/www/adminer/ && touch /var/www/adminer/index.php && wget -O - https://github.com/vrana/adminer/releases/download/v4.6.3/adminer-4.6.3-en.php > /var/www/adminer/index.php
```

**[`^ back to top ^`](#)**

---

## nginx simple authentification

```bash
sudo yum -y install epel-release
sudo yum -y install nginx httpd-tools
sudo htpasswd -c /etc/nginx/htpasswd.users USERNAME
sudo nano /etc/nginx/nginx.conf
server {
#ajouter
    auth_basic "Restricted Access";
    auth_basic_user_file /etc/nginx/htpasswd.users;
}
```

**[`^ back to top ^`](#)**

---

## serveur dhcp

```bash
sudo apt-get install isc-dhcp-server

sudo nano /etc/default/isc-dhcp-server
INTERFACES="eth0"

sudo nano /etc/dhcp/dhcpd.conf
# Le nom de votre serveur dhcp
server-name "DHCP.MONRESEAU.FR";
# Le domaine attribue a vos clients
option domain-name "MONRESEAU.FR";
# Les serveurs DNS attribues a vos clients
option domain-name-servers 192.168.0.1, 192.168.0.2, 8.8.8.8;
# La duree des baux en secondes (soit ici 2 heures)
default-lease-time 7200;
max-lease-time 7200;
# Le masque de sous-reseau
option subnet-mask 255.255.255.0;
# L adresse de broadcast, qui est optionnelle
option broadcast-address 192.168.0.255;
# Le routeur par defaut
option routers 192.168.0.254;

# Le reseau utilisé et les particularites associees
subnet 192.168.0.0 netmask 255.255.255.0 {
# Ici nous ne voulons attribuer dynamiquement que les adresses de 10 a 100
        range 192.168.0.10 192.168.0.100;
        host HOSTNAME {
                hardware ethernet MAC:AD:RE:SS:XX:XX;
                fixed-address 192.168.0.10;
        }
}

###>>>test
netstat -anup | grep dhcp
###>>>udp      0      0 0.0.0.0:67              0.0.0.0:*

###>>>forcer une demande DHCP
sudo dhclient eth0
```

**[`^ back to top ^`](#)**

---

## fail2ban

```bash
sudo apt-get install fail2ban
sudo fail2ban-client -x start

sudo nano /etc/fail2ban/jail.conf
ignoreip = 127.0.0.1 192.168.1.20 192.168.1.21 # liste des adresses ip à ignorer
findtime = 3600
bantime = 86400 # temps de banissement en secondes
maxretry = 3 # nombre de tentative échouées
#pour rajouter un service par exemple pour webmin
[webmin-auth]
enabled  = true
port     = https
filter   = webmin-auth
logpath  = /var/log/auth.log
maxretry = 3

[ssh]
enabled = true
port    = ssh,sftp
filter  = sshd
logpath  = /var/log/auth.log
maxretry = 6

[apache-w00tw00t]
enabled = true
filter = apache-w00tw00t
action = iptables[name=Apache-w00tw00t,port=80,protocol=tcp]
logpath = /var/log/apache2/*access.log
maxretry = 1

sudo nano /etc/fail2ban/filter.d/apache-w00tw00t.conf
[Definition]
failregex = ^<HOST> -.*"GET \/w00tw00t\.at\.ISC\.SANS\.DFind\:\).*".*
ignoreregex =

sudo fail2ban-client reload
sudo fail2ban-client status
```

**[`^ back to top ^`](#)**

---

## denyhosts (alternative fail2ban)

```bash
sudo apt install denyhosts

sudo nano /etc/denyhosts.conf
# Fichier de logs à analyser
SECURE_LOG = /var/log/auth.log
# Fichier des hôtes refusés (blacklist)
HOSTS_DENY = /etc/hosts.deny
# Répertoire de travail de DenyHosts
WORK_DIR = /var/lib/denyhosts
# Assurer le verrouillage du fichier PID de DenyHosts, pour qu il y ait une seule instance en
# fonctionnement
LOCK_FILE = /run/denyhosts.pid
# Fichier de log du démon DenyHosts (vide pour désactiver la journalisation)
DAEMON_LOG = /var/log/denyhosts
# Délai avant la purge Exemple : purge tous les 15 jours
PURGE_DENY = 15d
# Nombre de fois qu un hôte peut être purgé du fichier hosts.deny. Une fois la limite dépassée
# l hôte ne sera plus purgé du fichier. La valeur 0 indique illimité
PURGE_THRESHOLD = 5
# Temps entre chaque exécution de la tâche de purge des anciennes entrées dans le fichier
# hosts.deny
DAEMON_PURGE = 1h
# Indiquer à quel(s) service(s) on bloque l accès. Par exemple, bloquer l accès uniquement au SSH
# (sshd) ou à tout les services du serveur (ALL). Pour seulement enregistrer les hôtes, laisser vide
BLOCK_SERVICE = ALL
# Indiquer le nombre d échecs accepté avec un nom d utilisateur invalide. Une fois la limite
# dépassée, on bloque l hôte
DENY_THRESHOLD_INVALID = 5
# Indiquer le nombre d échecs accepté avec un nom d utilisateur valide, sauf pour l utilisateur
# root. Une fois la limite dépassée, on bloque l hôte
DENY_THRESHOLD_VALID = 10
# Indiquer le nombre d échecs accepté avec l utilisateur root. Une fois la limite dépassée, on
# bloque l hôte
DENY_THRESHOLD_ROOT = 1
# Indiquer le nombre d échec accepté avec les identifiants d utilisateurs indiqué dans le fichier
# restricted-usernames qui doit se situer dans le répertoire de travail du paquet.
DENY_THRESHOLD_RESTRICTED = 1
# Indiquer le nom de l hôte dans le fichier des hôtes refusés, lorsque c est possible de le
# récupérer. HOSTNAME_LOOKUP=YES|NO
HOSTNAME_LOOKUP=YES

sudo nano /etc/hosts.allow
ALL: 192.168.1.
sshd: IP
ALL: 192.168.12.
ALL: .MONDOMAINE.COM

sudo nano /etc/hosts.deny
#blacklist

service denyhosts restart
```

**[`^ back to top ^`](#)**

---

## ftp

```bash
sudo apt-get install vsftpd
sudo nano /etc/vsftpd.conf
Anonymous_enabled=NO
Local_enable = YES
local_unmask=022
Write_enabled=YES
Ascii_upload_enabled=YES
Ascii_download_enabled=YES
```

**[`^ back to top ^`](#)**

---

## serveur dns

```bash
###>>>ip statique et ntp valide
###>>>ouvrir le port 53 en TCP et en UDP

sudo apt-get install bind9 bind9-doc bind9utils dnsutils libirs141 net-tools rblcheck resolvconf

sudo nano /etc/bind/named.conf
include "/etc/bind/named.conf.options";
include "/etc/bind/named.conf.local";
###>>> on choisit
include "/etc/bind/named.conf.default-zones";
###>>> ou (modifier les fichiers ci-dessous en fonction)
include "/etc/bind/named.conf.internal-zones";
include "/etc/bind/named.conf.external-zones";

options {
        directory "/var/cache/bind";

        // Port d échange entre les serveurs DNS
        query-source address * port *;

        // Transmettre les requêtes à 192.168.1.1 si ce serveur ne sait pas résoudre ces adresses
        // On pourrait aussi bien renseigner les serveurs DNS du FAI plutôt que de renseigner
        // l adresse IP du routeur (xxxbox)
        forward only;
        forwarders { 192.168.1.1; };

        auth-nxdomain no;    # conform to RFC1035

        // Ecouter sur les interfaces locales uniquement (IPV4)
        listen-on-v6 { none; };
        listen-on { 127.0.0.1; 192.168.0.1; };

        // Ne pas transférer les informations de zones aux DNS secondaires
        allow-transfer { internals; };

        // Accepter les requêtes pour le réseau interne uniquement
        allow-query { internals; };

        // Autoriser les requêtes récursives pour les hôtes locaux
        allow-recursion { internals; };

        // Ne pas rendre publique la version de BIND
        version none;
        dnssec-validation auto;
};

sudo nano /etc/bind/named.conf.default-zones
zone "localhost" {
        type master;
        file "/etc/bind/db.local";
};
zone "127.in-addr.arpa" {
        type master;
        file "/etc/bind/db.127";
};
zone "0.in-addr.arpa" {
        type master;
        file "/etc/bind/db.0";
};
zone "255.in-addr.arpa" {
        type master;
        file "/etc/bind/db.255";
};

sudo nano /etc/bind/named.conf.internal-zones
view "internal" {
        match-clients {
                localhost;
                10.0.0.0/24;
        };
        # set zone for internal
        zone "srv.world" {
                type master;
                file "/etc/bind/srv.world.lan";
                allow-update { none; };
        };
        # set zone for internal *note
        zone "0.0.10.in-addr.arpa" {
                type master;
                file "/etc/bind/0.0.10.db";
                allow-update { none; };
        };
        include "/etc/bind/named.conf.default-zones";
};

sudo nano /etc/bind/named.conf.external-zones
view "external" {
        match-clients { any; };
        # allow any query
        allow-query { any; };
        # prohibit recursions
        recursion no;
        # set zone for external
        zone "srv.world" {
                type master;
                file "/etc/bind/srv.world.wan";
                allow-update { none; };
        };
        # set zone for external *note
        zone "80.0.16.172.in-addr.arpa" {
                type master;
                file "/etc/bind/80.0.16.172.db";
                allow-update { none; };
        };
};

sudo nano /etc/bind/named.conf.local
// Gérer les fichiers de logs
include "/etc/bind/named.conf.log";

// Gestion du domaine example.com
// ------------------------------
//  - Le serveur est défini comme maître sur ce domaine
//  - Il n y a aucun forwarder pour ce domaine car nous avons la main mise dessus
//    Pour tous les autres domaines, nous utiliserons le forwarder mentionné dans *named.conf.options*
//  - Les entrees sur le domaine peuvent être ajoutées dynamiquement avec le clef ns-example-com_rndc-key
zone "example.com" {
        type master;
        file "/var/cache/bind/db.example.com";
        forwarders {};
        allow-update { key ns-example-com_rndc-key; };
};
zone "0.168.192.in-addr.arpa" {
        type master;
        file "/var/cache/bind/db.example.com.inv";
        forwarders {};
        allow-update { key ns-example-com_rndc-key; };
};

// Consider adding the 1918 zones here, if they are not used in your
// organization
include "/etc/bind/zones.rfc1918";

sudo nano /etc/bind/named.conf.log
logging {
        channel update_debug {
                file "/var/log/update_debug.log" versions 3 size 100k;
                severity debug;
                print-severity  yes;
                print-time      yes;
        };
        channel security_info {
                file "/var/log/security_info.log" versions 1 size 100k;
                severity info;
                print-severity  yes;
                print-time      yes;
        };
        channel bind_log {
                file "/var/log/bind.log" versions 3 size 1m;
                severity info;
                print-category  yes;
                print-severity  yes;
                print-time      yes;
        };

        category default { bind_log; };
        category lame-servers { null; };
        category update { update_debug; };
        category update-security { update_debug; };
        category security { security_info; };
};

sudo systemctl stop bind9.service

sudo nano /etc/systemd/system/bind9.service
[Unit]
Description=BIND Domain Name Server
Documentation=man:named(8)
After=network.target

[Service]
ExecStart=/usr/sbin/named -f -u bind -t /var/bind9/chroot
ExecReload=/usr/sbin/rndc reload
ExecStop=/usr/sbin/rndc stop

[Install]
WantedBy=multi-user.target

sudo sysctl -w net.netfilter.nf_conntrack_udp_timeout=10
sudo systemctl reenable bind9
sudo mkdir -p /var/bind9/chroot/{etc,dev,var/cache/bind,var/run/named}
sudo mknod /var/bind9/chroot/dev/null c 1 3
sudo mknod /var/bind9/chroot/dev/random c 1 8
sudo chmod 660 /var/bind9/chroot/dev/{null,random}
sudo mv /etc/bind /var/bind9/chroot/etc
sudo ln -s /var/bind9/chroot/etc/bind /etc/bind
sudo cp /etc/localtime /var/bind9/chroot/etc/
sudo chown bind:bind /var/bind9/chroot/etc/bind/rndc.key
sudo chmod 775 /var/bind9/chroot/var/{cache/bind,run/named}
sudo chgrp bind /var/bind9/chroot/var/{cache/bind,run/named}

sudo nano /etc/init.d/bind9
PIDFILE=/var/bind9/chroot/var/run/named/named.pid

sudo echo "\$AddUnixListenSocket /var/bind9/chroot/dev/log" > /etc/rsyslog.d/bind-chroot.conf
sudo systemctl restart bind9.service
sudo systemctl restart rsyslog.service

###>>>tests
###>>>permet de vérifier la syntaxe des fichers de configuration de Bind9
named-checkconf -z
named-checkzone example.com /var/cache/bind/db.example.com
dig URL @localhost
###>>>Query time: ???
dig URL @localhost
###>>>Query time: plus rapide
host URL
host IP
time (for i in `seq 1 1000`;   do dig google.com @8.8.8.8 >   /dev/null 2>>/dev/null; done)
time (for i in `seq 1 1000`;   do dig google.com @127.0.0.1 >   /dev/null 2>>/dev/null; done)
```

**[`^ back to top ^`](#)**
