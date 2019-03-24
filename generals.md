## Index

[TOC]

- [Raccourcis terminal](#raccourcis-terminal)
- [tabbed](#tabbed)
- [surf](#surf)
- [Clavier AZERTY](#clavier-azerty)
- [Clavier QWERTY](#clavier-qwerty)
- [Liste des masques de sous-reseaux](#liste-des-masques-de-sous-reseaux)
- [OSI 3](#osi-3)
- [plage d adresses methode du nombre magique](#plage-d-adresses-methode-du-nombre-magique)
- [ports tcp/udp](#ports-tcpudp)
- [Consoles Chromium](#consoles-chromium)
- [duck duck go](#duck-duck-go)
- [serveur ntp france](#serveur-ntp-france)
- [Google et openDNS](#google-et-opendns)
- [serveur dns libre](#serveur-dns-libre)
- [DNSsec](#dnssec)
- [Public DNS Servers](#public-dns-servers)
- [date](#date)
- [regex](#regex)
- [megaoctet 1000-1024](#megaoctet-1000-1024)
- [dossiers linux](#dossiers-linux)
- [creer man page](#creer-man-page)
- [utilisateurs linux](#utilisateurs-linux)
- [chmod](#chmod)
- [python](#python)
- [testeur](#testeur)
- [pygame event.key](#pygame-eventkey)
- [table ASCII ( 0 - 127 )](#table-ascii--0---127-)
- [lorem ipsum](#lorem-ipsum)
- [Creative Commons by-nc-sa](#creative-commons-by-nc-sa)
- [256 couleurs](#256-couleurs)

_____________________________________________________________________________________
Raccourcis terminal
-------------------------------------------------------------------------------------
```
# Consoles
Ctrl+Alt+Del      Reboots Computer (/usr/lib/systemd/system/ctrl-alt-del.target)
Alt+F1, F2, F3,   Switch to n-th virtual console
Alt+ ←            Switch to previous virtual console
Alt+ →            Switch to next virtual console
Scroll Lock       When Scroll Lock is activated, input/output is locked
Shift+PgUp/PgDown Scrolls console buffer up/down
Ctrl+c            Kills current task
Ctrl+d            Inserts an EOF
Ctrl+z            Pauses current Task
# Cursor
Ctrl+l            Clear the screen
Ctrl+b            Move cursor one character to the left
Ctrl+f            Move cursor one character to the right
Alt+b             Move cursor one word to the left
Alt+f             Move cursor one word to the right
Ctrl+a            Move cursor to start of the line
Ctrl+e            Move cursor to end of the line
# Copy and Paste
Ctrl+u            Cut everything from line start to cursor
Ctrl+k            Cut everything from the cursor to end of the line
Alt+d             Cut the current word after the cursor
Ctrl+w            Cut the current word before the cursor
Ctrl+y            Paste the previous cut text
Alt+y             Paste the second latest cut text
Alt+Ctrl+y        Paste the first argument of the previous command
Alt+./_           Paste the last argument of the previous command
# History
Ctrl+p            Move to the previous line
Ctrl+n            Move to the next line
Ctrl+s            Search
Ctrl+r            Reverse search
Ctrl+j            End search
Ctrl+g            Abort search
Alt+r             Restores all changes made to line
# Completion
Tab               Auto-complete a name
Alt+?             List all possible completions
Alt+*             Insert all possible completions
# Others
Esc+t             Switch arguments (service vs systmctl)
Ctrl+x+e          Continue editing your current shell line in a text editor (uses $EDITOR)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
tabbed
-------------------------------------------------------------------------------------
```
tabbed -c surf -e
tabbed urxvt -embed

Shift+Ctrl+Enter     New tab
Shift+Ctrl+l         Next higher number tab
Shift+Ctrl+h         Next lower number tab
Ctrl+Tab             Toggle between last 2 tabs
Ctrl+1               Switch to tab 1
Ctrl+2               Switch to tab 2
Ctrl+3               Switch to tab 3 etc...
Ctrl+q               Close this tab
Ctrl+t               Tab picklist
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
surf
-------------------------------------------------------------------------------------
```
Ctrl+g              Go to URL
Ctrl+p              Go to URL in cut and paste clipboard
Ctrl+y              Put current website URL into cut and paste clipboard
Ctrl+h              Previous page in history like back button on other browsers
Ctrl+l              Undo the last Ctrl+h
Ctrl+r              Refresh/reload from cache
Ctrl+Shift+r        Refresh/reload without cache
Ctrl+o              Show the sourcecode of the current page
Ctrl+Shift+o        Open the Web Inspector
Ctrl+Shift+j        Zoom out
Ctrl+Shift+k        Zoom in
Ctrl+Shift+q        Normal zoom
Shift+Ctrl+b        Toggle scrollbars
Shift+Ctrl+g        Toggle geolocation
Ctrl+Shift+c        Toggle caret browsing
Ctrl+Shift+i        Toggle auto+loading of images
Ctrl+Shift+m        Toggle if the stylefile file should be loaded
Ctrl+Shift+s        Toggle script execution
F11                 Toggle fullscreen
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Clavier AZERTY
-------------------------------------------------------------------------------------
```
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┲━━━━━━━━━━━┓
│   │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ 7 │ 8 │ 9 │ 0 │ ° │ + ┃           ┃
│ ² │ & │ é │ " │ ' │ ( │ - │ è │ _ │ ç │ à │ ) │ = ┃ Backspace ┃
┢━━━┷━┱─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┺━┳━━━━━━━━━┫
┃     ┃ A │ Z │ E │ R │ T │ Y │ U │ I │ O │ P │ ö │ £ │         ┃
┃Tab  ┃ a │ z │ e │ r │ t │ y │ u │ i │ o │ p │ ^ │ $ │  Entrée ┃
┣━━━━━┻┱──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬        ┃
┃Caps  ┃ Q │ S │ D │ F │ G │ H │ J │ K │ L │ M │ % │ µ │        ┃
┃Lock  ┃ q │ s │ d │ f │ g │ h │ j │ k │ l │ m │ ù │ * │        ┃
┣━━━━━┳┹──┬┴──┬┴───┴──┬┴──┬┴──┬┴──┬┴──┬┴──┬┴──┳┴━━┲┺━━━┺━━━━━━━━┫
┃     │ > │ W │ X │ C │ V │ B │ N │ ? │ . │ / │ § ┃             ┃
┃Shift┃ < │ w │ x │ c │ v │ b │ n │ , │ ; │ : │ ! ┃  Shift      ┃
┣━━━━━╋━━━┷━┳━┷━┱─┴───┴───┴───┴───┴───┲━━━━━━━╋━━━┺━━━┳━━━━━━━━━┫
┃     ┃     ┃   ┃                     ┃       ┃       ┃         ┃
┃Ctrl ┃Meta ┃Alt┃      Espace         ┃Alt    ┃ Menu  ┃   Ctrl  ┃
┗━━━━━┻━━━━━┻━━━┹─────────────────────┺━━━━━━━┻━━━━━━━┻━━━━━━━━━┛
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Clavier QWERTY
-------------------------------------------------------------------------------------
```
┌───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┬───┲━━━━━━━━━━━┓
│ ~ │ ! │ @ │ # │ $ │ % │ ^ │ & │ * │ ( │ ) │ _ │ + ┃           ┃
│ ` │ 1 │ 2 │ 3 │ 4 │ 5 │ 6 │ 7 │ 8 │ 9 │ 0 │ - │ = ┃ Backspace ┃
┢━━━┷━┱─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┴─┬─┺━┳━━━━━━━━━┫
┃     ┃ Q │ W │ E │ R │ T │ Y │ U │ I │ O │ P │ { │ } │ |       ┃
┃Tab  ┃   │   │   │   │   │   │   │   │   │   │ [ │ ] │ \       ┃
┣━━━━━┻┱──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬──┴┬━━━━━━━━━━━━┫
┃Caps  ┃ A │ S │ D │ F │ G │ H │ J │ K │ L │ : │ " │            ┃
┃Lock  ┃   │   │   │   │   │   │   │   │   │ ; │ ' │ Entrée     ┃
┣━━━━━┳┹──┬┴──┬┴───┴──┬┴──┬┴──┬┴──┬┴──┬┴──┬┴──┳┴━━━┺━━━━━━━━━━━━┫
┃     │ Z │ X │ C │ V │ B │ N │ M │ < │ > │ ? ┃                 ┃
┃Shift┃   │   │   │   │   │   │   │ , │ . │ / ┃    Shift        ┃
┣━━━━━╋━━━┷━┳━┷━┱─┴───┴───┴───┴───┴───┲━━━━━━━╋━━━━━━━┳━━━━━━━━━┫
┃     ┃     ┃   ┃                     ┃       ┃       ┃         ┃
┃Ctrl ┃Meta ┃Alt┃      Espace         ┃Alt gr ┃ Menu  ┃   Ctrl  ┃
┗━━━━━┻━━━━━┻━━━┹─────────────────────┺━━━━━━━┻━━━━━━━┻━━━━━━━━━┛
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Liste des masques de sous-reseaux
-------------------------------------------------------------------------------------
```
Classe   adresses de réseau            masque de réseau    masque de réseau /bits  nombre de sous-réseaux
A        10.x.x.x                      255.0.0.0           /8                      1
B        172.16.x.x — 172.31.x.x       255.255.0.0         /16                     16
C        192.168.0.x — 192.168.255.x   255.255.255.0       /24                     256

CIDR    bits disponibles    Masque de sous-reseau    Nombre d hotes par sous-reseau
/1            31            128.0.0.0                   2^31-2 = 2147483646
/2            30            192.0.0.0                   2^30-2 = 1073741822
/3            29            224.0.0.0                   2^29-2 = 536870910
/4            28            240.0.0.0                   2^28-2 = 268435454
/5            27            248.0.0.0                   2^27-2 = 134217726
/6            26            252.0.0.0                   2^26-2 = 67108862
/7            25            254.0.0.0                   2^25-2 = 33554430
/8            24            255.0.0.0                   2^24-2 = 16777214
/9            23            255.128.0.0                 2^23-2 = 8388606
/10           22            255.192.0.0                 2^22-2 = 4194302
/11           21            255.224.0.0                 2^21-2 = 2097150
/12           20            255.240.0.0                 2^20-2 = 1048574
/13           19            255.248.0.0                 2^19-2 = 524286
/14           18            255.252.0.0                 2^18-2 = 262142
/15           17            255.254.0.0                 2^17-2 = 131070
/16           16            255.255.0.0                 2^16-2 = 65534
/17           15            255.255.128.0               2^15-2 = 32766
/18           14            255.255.192.0               2^14-2 = 16382
/19           13            255.255.224.0               2^13-2 = 8190
/20           12            255.255.240.0               2^12-2 = 4094
/21           11            255.255.248.0               2^11-2 = 2046
/22           10            255.255.252.0               2^10-2 = 1022
/23           9             255.255.254.0               2^9-2 = 510
/24           8             255.255.255.0               2^8-2 = 254
/25           7             255.255.255.128             2^7-2 = 126
/26           6             255.255.255.192             2^6-2 = 62
/27           5             255.255.255.224             2^5-2 = 30
/28           4             255.255.255.240             2^4-2 = 14
/29           3             255.255.255.248             2^3-2 = 6
/30           2             255.255.255.252             2^2-2 = 2
/31           1             255.255.255.254             2^1=2
/32           0             255.255.255.255             2^0=1
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
OSI 3
-------------------------------------------------------------------------------------
```
IPv4
192.168.0.67
Bloc                   Usage    
0.0.0.0/8              Ce réseau
10.0.0.0/8             Adresses privées 15
100.64.0.0/10          Espace partagé pour Carrier Grade NAT
127.0.0.0/8            adresse de bouclage (localhost)
169.254.0.0/16         adresses locales autoconfigurées (APIPA)
172.16.0.0/12          Adresses privées
192.0.0.0/24           Réservé par l IETF
192.0.2.0/24           Réseau de test TEST-NET-1
192.88.99.0/24         6to4 anycast    RFC 306824
192.168.0.0/16         Adresses privées
198.18.0.0/15          Tests de performance
198.51.100.0/24        Réseau de test TEST-NET-2
203.0.113.0/24         Réseau de test TEST-NET-3
224.0.0.0/4            Multicast Multidiffusion
240.0.0.0/4            Réservé à un usage ultérieur non précisé
255.255.255.255/32     broadcast limité

IPv6
2001:0db8:0000:85a3:0000:0000:ac1f:8001
Bloc            Usage
::/128          Adresse non spécifiée
::1/128         Adresse de bouclage
::ffff:0:0/96   Adresse IPv6 mappant IPv4
0100::/64       sollicitation de trou noir
2000::/3        Adresses unicast routables sur Internet
2001::/32       Teredo
2001:2::/48     Tests de performance
2001:10::/28    Orchid
2001:db8::/32   documentation
2002::/16       6to4
fc00::/7        Adresses locales uniques
fe80::/10       Adresses locales lien
ff00::/8        Adresses multicast
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
plage d adresses methode du nombre magique
-------------------------------------------------------------------------------------
```
194.34.56.0/29
29=11111111.11111111.11111111.11111000 (sur 32bits)
29-24=5 >                     11111000
11111000=248
194.34.56.0/255.255.255.248
256-248=8
8 adresses disponibles de 194.34.56.0 a 194.34.56.7
194.34.56.0 adresse de reseau
194.34.56.7 adresse de broadcast
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
ports tcp/udp
-------------------------------------------------------------------------------------
```
more /etc/services
20           tcp     ftp-data - File Transfer Protocol
21           tcp     ftp - File Transfer Protocol
22           tcp     SSH - Secure Shell
23           tcp     Telnet
25           tcp     SMTP - Simple Mail Transfer Protocol (sans authentification)
53           udp     DNS - Domain Name Service
67           udp     DHCP (Client) - Dynamic Host Configuration Protocol
68           udp     DHCP (Serveur) - Dynamic Host Configuration Protocol
69           udp     tftp - Trivial File Transfer
80           tcp     HTTP - Hypertext Transfer Protocol
110          tcp     pop3 - Post Office Protocol v3
123          udp     ntp - Network Time Protocol
137          tcp     netbios-ns - NETBIOS Name Service
143          tcp     imap4 - Internet Message Access Protocol v4
161          udp     SNMP - Simple Network Management Protocol
162          udp     SNMPtrap - Simple Network Management Protocol Trap
389          tcp     LDAP - Lightweight Directory Access Protocol
443          tcp     https - HyperText Transfer Protocol Secure
445          tcp     CIFS - Common Internet File System (SMB - Samba)
546          udp     DHCPv6 - Dynamic Host Configuration Protocol v6
993          tcp     IMAPS
995          tcp     POP3S
1433         tcp     Microsoft SQL Server
1521         tcp     Oracle SQL
3306         tcp     MySQL
5432         tcp     PostgreSQL
5900         tcp     VNC Server
6667         tcp     IRC
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Consoles Chromium
-------------------------------------------------------------------------------------

##### cacher la barre de defilement
```
#overlay-scrollbars
Enabled
```

##### informations

```
chrome://version/
chrome://system/
chrome://conflicts/
chrome://flash/
chrome://components/
chrome://sandbox/
```

##### sites visités

```
chrome://site-engagement/
chrome://media-engagement/
chrome://view-http-cache/
chrome://quota-internals/
```

##### stratégies

```
chrome://policy/
chrome://flags/
```

##### extensions

```
chrome://extensions
chrome://inspect/#extensions
chrome://net-internals/#modules
```

##### confidentialité et données personnelles

```
chrome://settings/passwords
chrome://settings/siteData
chrome://settings/clearBrowserData
```

##### activité

```
chrome://net-export/
chrome://net-internals/#import
```

##### activité réseau

```
chrome://net-internals/#dns
chrome://net-internals/#sockets
chrome://net-internals/#quic
chrome://net-internals/#http2
chrome://net-internals/#timeline
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
duck duck go
-------------------------------------------------------------------------------------
```
password 25 strong   #generateur mot de passe
sha265 CHAINE        #hash sha256
md5 CHAINE           #hash md5
is SITE up           #disponibilité site
ip address           #addresse publique
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
serveur ntp france
-------------------------------------------------------------------------------------
```bash
sudo nano /etc/ntp.conf
server 0.fr.pool.ntp.org
server 1.fr.pool.ntp.org
server 2.fr.pool.ntp.org
server 3.fr.pool.ntp.org

ntpq -p
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Google et openDNS
-------------------------------------------------------------------------------------
```bash
cp /etc/resolv.conf /etc/resolv.conf.save
sudo nano /etc/resolv.conf
nameserver 2001:4860:4860::8888                  ; google-public-dns-a.google.com
nameserver 2001:4860:4860::8844                  ; google-public-dns-b.google.com
nameserver 8.8.8.8                               ; google-public-dns-a.google.com
nameserver 8.8.4.4                               ; google-public-dns-b.google.com

nameserver 2620:0:ccc::2
nameserver 2620:0:ccd::2
nameserver 208.67.220.220
nameserver 208.67.222.222

dig a www.google.fr | grep time
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
serveur dns libre
-------------------------------------------------------------------------------------
```
nameserver 80.67.188.188                         ; ns0.ldn-fai.net
nameserver 2001:913::8                           ; ns0.ldn-fai.net

nameserver 80.67.169.12                          ; ns0.fdn.fr
nameserver 80.67.169.40                          ; ns1.fdn.fr
nameserver 2001:910:800::12                      ; ns0.fdn.fr
nameserver 2001:910:800::40                      ; ns1.fdn.fr

nameserver 4.2.2.6                               ; vnsc-lc-dsl.genuity.net
nameserver 4.2.2.5                               ; vnsc-bak-dsl.genuity.net
nameserver 4.2.2.4                               ; vnsc-pri-dsl.genuity.net
nameserver 4.2.2.3                               ; vnsc-lc.sys.gtei.net
nameserver 4.2.2.2                               ; vnsc-bak.sys.gtei.net
nameserver 4.2.2.1                               ; vnsc-pri.sys.gtei.net

nameserver 163.172.189.181                       ; dns.vtbox.net
nameserver 217.182.205.213                       ; sdns.vtbox.net
nameserver 2001:bc8:4400:2100::25:5              ; dns.vtbox.net
nameserver 2001:41d0:302:2100::7a77              ; sdns.vtbox.net

nameserver 89.234.141.66                         ; recursif.arn-fai.net
nameserver 2a00:5881:8100:1000::3                ; recursif.arn-fai.net
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
DNSsec
-------------------------------------------------------------------------------------
```
nameserver 2001:4f8:3:2bc:1::64:20               ; bind.odvr.dns-oarc.net
nameserver 149.20.64.20                          ; bind.odvr.dns-oarc.net
nameserver 2001:4f8:3:2bc:1::64:21               ; unbound.odvr.dns-oarc.net
nameserver 149.20.64.21                          ; unbound.odvr.dns-oarc.net
nameserver 2001:4f8:3:2bc:1::64:22               ; iana-testbed.odvr.dns-oarc.net
nameserver 149.20.64.22                          ; iana-testbed.odvr.dns-oarc.net
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Public DNS Servers
-------------------------------------------------------------------------------------
```
#Alternate DNS
198.101.242.72
23.253.163.53

#BlockAid Public DNS
205.204.88.60
178.21.23.150

#Censurfridns
91.239.100.100
89.233.43.71
2001:67c:28a4::
2002:d596:2a92:1:71:53::

#Chaos Computer Club Berlin
213.73.91.35

#Christoph Hochstätter
209.59.210.167
85.214.117.11

#ClaraNet
212.82.225.7
212.82.226.212

#Comodo Secure DNS
8.26.56.26
8.20.247.20

#DNS.Watch
84.200.69.80
84.200.70.40
2001:1608:10:25::1c04:b12f
2001:1608:10:25::9249:d69b

#DNSReactor
104.236.210.29
45.55.155.25

#Dyn
216.146.35.35
216.146.36.36

#FDN
80.67.169.12
2001:910:800::12

#FoeBud
85.214.73.63

#FoolDNS
87.118.111.215
213.187.11.62

#FreeDNS
37.235.1.174
37.235.1.177

#Freenom World
80.80.80.80
80.80.81.81

#German Privacy Foundation e.V.
87.118.100.175
94.75.228.29
85.25.251.254
62.141.58.13

#Google Public DNS
8.8.8.8
8.8.4.4
2001:4860:4860::8888
2001:4860:4860::8844

#GreenTeamDNS
81.218.119.11
209.88.198.133

#Hurricane Electric
74.82.42.42
2001:470:20::2

#Level3
209.244.0.3
209.244.0.4

#Neustar DNS Advantage
156.154.70.1
156.154.71.1

#New Nations
5.45.96.220
185.82.22.133

#Norton DNS
198.153.192.1
198.153.194.1

#OpenDNS
208.67.222.222
208.67.220.220
2620:0:ccc::2
2620:0:ccd::2

#OpenNIC
58.6.115.42
58.6.115.43
119.31.230.42
200.252.98.162
217.79.186.148
81.89.98.6
78.159.101.37
203.167.220.153
82.229.244.191
216.87.84.211
66.244.95.20
207.192.69.155
72.14.189.120

#PowerNS
194.145.226.26
77.220.232.44

#Quad9
9.9.9.9
2620:fe::fe

#SafeDNS
195.46.39.39
195.46.39.40

#SkyDNS
193.58.251.251

#SmartViper Public DNS
208.76.50.50
208.76.51.51

#ValiDOM
78.46.89.147
88.198.75.145

#Verisign
64.6.64.6
64.6.65.6
2620:74:1b::1:1
2620:74:1c::2:2

#Xiala.net
77.109.148.136
77.109.148.137
2001:1620:2078:136::
2001:1620:2078:137::

#Yandex.DNS
77.88.8.88
77.88.8.2
2a02:6b8::feed:bad
2a02:6b8:0:1::feed:bad

#puntCAT
109.69.8.51
2a00:1508:0:4::9

#cloudflare
nameserver 1.0.0.1
nameserver 1.1.1.1
nameserver 2606:4700:4700::1111
nameserver 2606:4700:4700::1001
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
date
-------------------------------------------------------------------------------------
```bash
%H     heure (00..23)
%I     heure (01..12)
%k     heure ( 0..23)
%l     heure ( 1..12)
%M     minute (00..59)
%p     notation locale pour AM ou PM
%r     heure actuelle (sur 12 heures) (hh:mm:ss [AP]M)
%s     secondes ecoulees depuis le 01-01-1970  a  00:00:00
%S     secondes (00..61)
%T     heure actuelle (sur 24 heures) (hh:mm:ss)
%X     representation locale de l heure (%H:%M:%S)
%Z     fuseau  horaire
%a     abreviation locale du jour de la semaine (Sun..Sat)
%A     nom local du jour de la semaine (Sunday..Saturday)
%b     abreviation locale du nom du mois (Jan..Dec)
%B     nom local du mois (January..December)
%c     date  et  heure  locales EST
%d     jour du mois (01..31)
%D     date (mm/jj/aa)
%j     jour de l annee (001..366)
%m     mois (01..12)
%w     Jour  de  la  semaine  (0..6). 0=Dimanche
%W     numero de semaine dans l annee (00..53)
%x     representation locale de la date (mm/jj/aa)
%y     deux derniers chiffres de l annee (00..99)
%Y     annee (1970...)

date -d last-saturday #retrouver le samedi précédent
date -d "AAAA-MM-JJ -$(date -d AAAA-MM-JJ +%w) days + $(date -d 'saturday' +%u) day - 1 week" #retrouver le samedi précédent à partir d une date
if [ $(date +%w) = 6 ]; then #si on est samedi
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
regex
-------------------------------------------------------------------------------------
```
Vim       ASCII                    Description
          [\x00-\x7F]              Caracteres ASCII
          A-Za-z0-9                Caracteres alphanumeriques
\w        A-Za-z0-9_               Caracteres alphanumeriques et  _ 
\W        ^A-Za-z0-9_              Caracteres ne composant pas les mots
\a        A-Za-z                   Caracteres alphabetiques
\s  \t                             Espace et tabulation
\< \>     (?<=\W)(?=\w)|(?<=\w)(?=\W)        Positions de debut et fin de mots
          (?<=\W)(?=\W)|(?<=\w)(?=\w)        NOT a un debut ou une fin de mot
          \x00-\x1F\x7F            Caracteres de contrôle
\d        0-9                      Chiffres decimaux
\D        ^0-9                     Autre chose qu un chiffre decimal
          \x21-\x7E                Caracteres visibles
\l        a-z                      Lettres en minuscule
\p        \x20-\x7E                Caracteres imprimables
        ][!""#$%&''()*+./:;<=>?@\^_``{|}~-    Caracteres de ponctuation
\_s       \t\r\n\v\f               Caracteres d espacement
\S        ^ \t\r\n\v\f             Autre chose qu un caractere d espacement
\u        A-Z                      Lettres capitales
\x        A-Fa-f0-9                Chiffres hexadecimaux

/.../i            ignore les différences de casse.
/.../x            ignore les espaces et les sauts de ligne (selon les choix).
/.../s            traite l expression régulière comme une simple ligne

^            Début de ligne
$            Fin de ligne
.            N importe quel caractère excepté celui de début de ligne
[ ... ]      N importe lequel des caractères entre crochets ou une plage de caractères
[^ ... ]     Aucun des caractères listés entre crochets
\#           Correspond au caractère (#) suivant le backslash

\a           Caracteres alphabetiques A-Za-z
\b           Début de mot
\B           Fin de mot
\xNN         Un caractère avec un code ASCII hexadécimal NN
\n           0x10 (lf)
\r           0x13 (cr)  (retour de chariot)
\t           0x09 (tabulation)
\s           Un espace blanc (tab/space /cr/lf)
\S           Pas un espace blanc
\w           Un caractère alphanumérique A-Za-z0-9_
\W           Un caractère non alphanumérique
\d           Un chiffre 0-9
\D           Pas un chiffre
\u           Un caractère en majuscule
\l           Un caractère en minuscule

( ... )      Groupe des caractères dans une seule expression
|            Expression précédente ou suivante (OU logique)
*            Correspond zéro ou plusieurs fois qui précède.
+            Correspond une ou plusieurs fois qui précède
?            Correspond zéro ou une fois qui précède
{n}          A répéter n fois
{n,}         A répéter n a l infini
{n,m}        A répéter n à m fois

[a-z]                      lowercase letter
[A-Z]                      uppercase letter
[a-zA-Z]                   single letter
[0-9]                      single number
[a-zA-Z0-9]                single letter or number
[[:alnum:]]                Alphanumeric [a-z A-Z 0-9]
[[:alpha:]]                Alphabetic [a-z A-Z]
[[:blank:]]                Blank characters (spaces or tabs)
[[:cntrl:]]                Control characters
[[:digit:]]                Numbers [0-9]
[[:graph:]]                Any visible characters (excludes whitespace)
[[:lower:]]                Lowercase letters [a-z]
[[:print:]]                Printable characters (non-control characters)
[[:punct:]]                Punctuation characters
[[:space:]]                Whitespace
[[:upper:]]                Uppercase letters [A-Z]
[[:xdigit:]]               Hex digits [0-9 a-f A-F]

/./          toute ligne qui contient au moins un caractère
/../         toute ligne qui contient au moins deux caractères
/^#/         toute ligne qui commence par dièse
/^$/         toute ligne vide
/}$/         toute ligne qui termine par }
/} *$/       toute ligne qui termine par } et un caractère
/[abc]/      toute ligne qui contient a b ou c
/^[abc]/     toute ligne qui commence par a b ou c
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
megaoctet 1000-1024
-------------------------------------------------------------------------------------
```
Un kilooctet (ko ou kB) = 1000 octets
Un Mégaoctet (Mo ou MB) = 1000 ko = 1 000 000 octets
Un Gigaoctet (Go ou GB) = 1000 Mo = 1 000 000 000 octets
Un Téraoctet (To) = 1000 Go = 1 000 000 000 000 octets 

Un kibioctet (kio ou kiB) vaut 2^10 octets = 1024 octets
Un Mébioctet (Mio ou MiB) vaut 2^20 octets = 1 048 576 octets
Un Gibioctet (Gio ou GiB) vaut 2^30 octets = 1 073 741 824 octets
Un Tébioctet (Tio ou TiB) vaut 2^40 octets = 1 099 511 627 776 octets 

###>>>abus de language
Un kilooctet (ko) = 2^10 octets = 1024 octets
Un Mégaoctet (Mo) = 2^20 octets = 1024 ko = 1 048 576 octets
Un Gigaoctet (Go) = 2^30 octets = 1024 Mo = 1 073 741 824 octets
Un Téraoctet (To) = 2^40 octets = 1024 Go = 1 099 511 627 776 octets
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
dossiers linux
-------------------------------------------------------------------------------------
```bash
man hier
```
```
/              Racine elle contient les repertoires principaux
/bin           Executables utilisables par tous les utilisateurs (ls pwd cp)
/boot          Fichiers permettant a Linux de demarrer
/dev           Point d entree de tous les peripheriques
/dev/urandom   Générateur de nombres aléatoires
/dev/null      Toute information envoyée vers ce périphérique est détruite
/dev/zero      Renvoie uniquement le caractère null
/etc           Contient les commandes et fichiers necessaires a l administrateur
/home          Repertoire personnel des utilisateurs
/lib           Contient les bibliotheques partagees du systeme lors du demarrage
/lib64         Idem /lib mais pour les 64bits
/mnt /media    Contient les point de montage des partitions temporaires (USB)
/opt           Pour l installation de programmes compiles (non dans les depots)
/proc          Creee par le noyau dans la memoire, informations sur le systeme
/root          Repertoire personnel de l administrateur
/sbin          Contient les programmes utilisables par l admin uniquement
/srv           Repertoire de donnees pour divers services
/tmp           Repertoire fichier temporaires
/usr/bin       Contient des programmes installes
/usr/src       Fichiers de code source
/usr/lib       Librairies des programmes installes
/usr/sbin      Programmes reserves a l admin systeme
/usr/local/bin Stocker les exécutables non-gérés par la distribution
/var           Contient les donnees variables (log bdd site web)
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
creer man page
-------------------------------------------------------------------------------------
```
.\" Manpage for {{ COMMAND }}.
.\" Contact me to correct errors or typos.
.TH man 8 "{{ DATE }}" "{{ VERSION }}" "{{ COMMAND }} man page"
.SH NAME
{{ COMMAND }} \- {{ DESCRIPTION }}
.SH SYNOPSIS
{{ COMMAND }} {{ ARGUMENTS }}
.SH DESCRIPTION
{{ COMMAND }} {{ DETAILS }}
.SH OPTIONS
The {{ COMMAND }} take this {{ ARGUMENTS }}
.SH SEE ALSO
COMMANDS, {{ COMMAND }}
.SH BUGS
No known bugs.
.SH AUTHOR
{{ AUTHOR }}
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
utilisateurs linux
-------------------------------------------------------------------------------------
```
user:x:1000:1000:Nom User:/home/user:/bin/bash
identifiant de connexion:mot de passe crypté dans (/etc/shadow):UID droits d accès de vos fichiers:GID groupe primaire:nom complet et infos personnelles:répertoire de connexion:shell de connexion

administrateur root UID = 0
utilisateurs système UID compris entre 1 et 999, gérant les droits d accès de services sur la machine
utilisateurs physique UID sera supérieur ou égal à 1000
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
chmod
-------------------------------------------------------------------------------------
```
Directory/User/Group/Others    Logique      Decimal
aucun droit                    (---)          0
execution seulement            (--x)          1
ecriture seulement             (-w-)          2
ecriture et execution          (-wx)          3
lecture seulement              (r--)          4
lecture et execution           (r-x)          5
lecture et ecriture            (rw-)          6
tous les droits                (rwx)          7

r = 4
w = 2
x = 1

r w x = 7
4+2+1 = 7

r w - r - - - - - = 640
4+2+0|4+0+0|0+0+0 = 640


u user
g group
o other
a all
+ ajout
- retire
= affecte
ex : chmod g+w, o+r-w FILE

```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
python
-------------------------------------------------------------------------------------
```
[ ]    specification de classe de caracteres ([a-z] intervalle de a a z)
.      la classe de caractere predefinie des caracteres graphiques visibles ou blancs
*      quantificateur pour zero une ou plusieurs occurrences equivaut a {0}
?      quantificateur pour au plus une occurrence de ce qui precede equivaut a {01}
+      quantificateur pour une ou plus occurrences de ce qui precede equivaut a {1}
|      alternative: soit ce qui precede soit ce qui suit
( )    delimiteurs de groupe (avec capture)
\autre un des caracteres speciaux definis ci-dessus mais interpretes litteralement
\t     tabulation horizontale
\n     saut de ligne
\v     tabulation verticale
\f     saut de page
\r     retour chariot
\ooo   caractere litteral dont le code octal (entre 0 et 377) est ooo
\xNN   caractere litteral dont le code hexadecimal est NN (sur 2 chiffres)
\B     condition vraie sauf a la limite d un mot l oppose de \b
\w     un caractere lettre ou chiffre equivaut a [0-9a-zA-Z_]
\W     un caractere ni lettre ni chiffre le complement de \w
\s     un caractere espace equivaut a[ \t\n\r\f]
\S     un caractere non espace le complement de \s
\d     un chiffre equivaut a [0-9] (equivaut a la classe [[:digit:]] de POSIX)
\D     un caractere non-chiffre le complement de \d
{mn}   quantificateur borne pour au moins m et au plus n occurrences de ce qui precede
help() modules
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
testeur
-------------------------------------------------------------------------------------
```
ooo-----------------------titre
|-----description
|nomdufichierreference.ext
|
|------xxx-test1-description
|code
|yyy-resultat
|logs
|
|------xxx-test2-description
|code
|yyy-resultat
|logs
|
|------zzz-solution-description
|code
|!!!-resultat
|logs
|______ooo
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
pygame event.key
-------------------------------------------------------------------------------------
```
      KeyASCII      ASCII   Nom commun
      K_BACKSPACE   \b      retour arrière
      K_TAB         \t      tabulation
      K_CLEAR               effacer
      K_RETURN      \r      entrée
      K_PAUSE               pause
      K_ESCAPE      ^[      échap
      K_SPACE               espace
      K_EXCLAIM     !       point d exclamation
      K_QUOTEDBL    ""       guillemet double
      K_HASH        #       dièse
      K_DOLLAR      $       dollar
      K_AMPERSAND   &       esperluette
      K_QUOTE       ''      guillemet simple
      K_LEFTPAREN   (       parenthèse gauche
      K_RIGHTPAREN  )       parenthèse droite
      K_ASTERISK    *       astérisque
      K_PLUS        +       plus
      K_COMMA       ,       apostrophe
      K_MINUS       -       moins
      K_PERIOD      .       point
      K_SLASH       /       "slash"
      K_0           0       0
      K_1           1       1
      K_2           2       2
      K_3           3       3
      K_4           4       4
      K_5           5       5
      K_6           6       6
      K_7           7       7
      K_8           8       8
      K_9           9       9
      K_COLON       :       deux-points
      K_SEMICOLON   ;       point-virgule
      K_LESS        <       inférieur
      K_EQUALS      =       égal
      K_GREATER     >       supérieur
      K_QUESTION    ?       point d interrogation
      K_AT          @       arobase
      K_LEFTBRACKET [       crochet gauche
      K_BACKSLASH   \       "backslash"
      K_RIGHTBRACKET ]      crochet droite
      K_CARET       ^       accent circonflexe
      K_UNDERSCORE  _       tiret bas
      K_BACKQUOTE   ``      accent grave
      K_a           a       a
      K_b           b       b
      K_c           c       c
      K_d           d       d
      K_e           e       e
      K_f           f       f
      K_g           g       g
      K_h           h       h
      K_i           i       i
      K_j           j       j
      K_k           k       k
      K_l           l       l
      K_m           m       m
      K_n           n       n
      K_o           o       o
      K_p           p       p
      K_q           q       q
      K_r           r       r
      K_s           s       s
      K_t           t       t
      K_u           u       u
      K_v           v       v
      K_w           w       w
      K_x           x       x
      K_y           y       y
      K_z           z       z
      K_DELETE              supprimer
      K_KP0                 pavé numérique 0
      K_KP1                 pavé numérique 1
      K_KP2                 pavé numérique 2
      K_KP3                 pavé numérique 3
      K_KP4                 pavé numérique 4
      K_KP5                 pavé numérique 5
      K_KP6                 pavé numérique 6
      K_KP7                 pavé numérique 7
      K_KP8                 pavé numérique 8
      K_KP9                 pavé numérique 9
      K_KP_PERIOD   .       pavé numérique point
      K_KP_DIVIDE   /       pavé numérique diviser
      K_KP_MULTIPLY *       pavé numérique multiplier
      K_KP_MINUS    -       pavé numérique moins
      K_KP_PLUS     +       pavé numérique plus
      K_KP_ENTER    \r      pavé numérique entrée
      K_KP_EQUALS   =       pavé numérique égal
      K_UP                  flèche haut
      K_DOWN                flèche bas
      K_RIGHT               flèche droite
      K_LEFT                flèche gauche
      K_INSERT              inser.
      K_HOME                début
      K_END                 fin
      K_PAGEUP              page précédente
      K_PAGEDOWN            page suivante
      K_F1                  F1
      K_F2                  F2
      K_F3                  F3
      K_F4                  F4
      K_F5                  F5
      K_F6                  F6
      K_F7                  F7
      K_F8                  F8
      K_F9                  F9
      K_F10                 F10
      K_F11                 F11
      K_F12                 F12
      K_F13                 F13
      K_F14                 F14
      K_F15                 F15
      K_NUMLOCK             verrouillage numérique
      K_CAPSLOCK            verrouillage majuscule
      K_SCROLLOCK           arrêt défilement
      K_RSHIFT              majuscule droite
      K_LSHIFT              majuscule gauche
      K_RCTRL               contrôle droite
      K_LCTRL               contrôle gauche
      K_RALT                Alt. droite
      K_LALT                Alt. gauche
      K_RMETA               Meta droite
      K_LMETA               Meta gauche
      K_LSUPER              Touche Windows droite
      K_RSUPER              Touche Windows gauche
      K_MODE                mode shift
      K_HELP                aide
      K_PRINT               imprim. écran
      K_SYSREQ              sysrq
      K_BREAK               pause
      K_MENU                menu
      K_POWER               power
      K_EURO                euro
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
table ASCII ( 0 - 127 )
-------------------------------------------------------------------------------------
```
man ascii #table de caractère

       Decimal   Octal   Hex  Binaire   Caractere
       -------   -----   ---  --------    ------
         000      000    00   00000000      NUL    (Null char.)
         001      001    01   00000001      SOH    (Start of Header)
         002      002    02   00000010      STX    (Start of Text)
         003      003    03   00000011      ETX    (End of Text)
         004      004    04   00000100      EOT    (End of Transmission)
         005      005    05   00000101      ENQ    (Enquiry)
         006      006    06   00000110      ACK    (Acknowledgment)
         007      007    07   00000111      BEL    (Bell)
         008      010    08   00001000       BS    (Backspace)
         009      011    09   00001001       HT    (Horizontal Tab)
         010      012    0A   00001010       LF    (Line Feed)
         011      013    0B   00001011       VT    (Vertical Tab)
         012      014    0C   00001100       FF    (Form Feed)
         013      015    0D   00001101       CR    (Carriage Return)
         014      016    0E   00001110       SO    (Shift Out)
         015      017    0F   00001111       SI    (Shift In)
         016      020    10   00010000      DLE    (Data Link Escape)
         017      021    11   00010001      DC1    (XON)(Device Control 1)
         018      022    12   00010010      DC2    (Device Control 2)
         019      023    13   00010011      DC3    (XOFF)(Device Control 3)
         020      024    14   00010100      DC4    (Device Control 4)
         021      025    15   00010101      NAK    (Negative Acknowledgement)
         022      026    16   00010110      SYN    (Synchronous Idle)
         023      027    17   00010111      ETB    (End of Trans. Block)
         024      030    18   00011000      CAN    (Cancel)
         025      031    19   00011001       EM    (End of Medium)
         026      032    1A   00011010      SUB    (Substitute)
         027      033    1B   00011011      ESC    (Escape)
         028      034    1C   00011100       FS    (File Separator)
         029      035    1D   00011101       GS    (Group Separator)
         030      036    1E   00011110       RS    (Request to Send)(Record Separator)
         031      037    1F   00011111       US    (Unit Separator)
         032      040    20   00100000       SP    (Space)
         033      041    21   00100001        !    (exclamation mark)
         034      042    22   00100010        ""   (double quote)
         035      043    23   00100011        #    (number sign)
         036      044    24   00100100        $    (dollar sign)
         037      045    25   00100101        %    (percent)
         038      046    26   00100110        &    (ampersand)
         039      047    27   00100111        ''   (single quote)
         040      050    28   00101000        (    (left opening parenthesis)
         041      051    29   00101001        )    (right closing parenthesis)
         042      052    2A   00101010        *    (asterisk)
         043      053    2B   00101011        +    (plus)
         044      054    2C   00101100        ,    (comma)
         045      055    2D   00101101        -    (minus or dash)
         046      056    2E   00101110        .    (dot)
         047      057    2F   00101111        /    (forward slash)
         048      060    30   00110000        0
         049      061    31   00110001        1
         050      062    32   00110010        2
         051      063    33   00110011        3
         052      064    34   00110100        4
         053      065    35   00110101        5
         054      066    36   00110110        6
         055      067    37   00110111        7
         056      070    38   00111000        8
         057      071    39   00111001        9
         058      072    3A   00111010        :    (colon)
         059      073    3B   00111011        ;    (semi-colon)
         060      074    3C   00111100        <    (less than sign)
         061      075    3D   00111101        =    (equal sign)
         062      076    3E   00111110        >    (greater than sign)
         063      077    3F   00111111        ?    (question mark)
         064      100    40   01000000        @    (AT symbol)
         065      101    41   01000001        A
         066      102    42   01000010        B
         067      103    43   01000011        C
         068      104    44   01000100        D
         069      105    45   01000101        E
         070      106    46   01000110        F
         071      107    47   01000111        G
         072      110    48   01001000        H
         073      111    49   01001001        I
         074      112    4A   01001010        J
         075      113    4B   01001011        K
         076      114    4C   01001100        L
         077      115    4D   01001101        M
         078      116    4E   01001110        N
         079      117    4F   01001111        O
         080      120    50   01010000        P
         081      121    51   01010001        Q
         082      122    52   01010010        R
         083      123    53   01010011        S
         084      124    54   01010100        T
         085      125    55   01010101        U
         086      126    56   01010110        V
         087      127    57   01010111        W
         088      130    58   01011000        X
         089      131    59   01011001        Y
         090      132    5A   01011010        Z
         091      133    5B   01011011        [    (left opening bracket)
         092      134    5C   01011100        \    (back slash)
         093      135    5D   01011101        ]    (right closing bracket)
         094      136    5E   01011110        ^    (caret cirumflex)
         095      137    5F   01011111        _    (underscore)
         096      140    60   01100000        ``
         097      141    61   01100001        a
         098      142    62   01100010        b
         099      143    63   01100011        c
         100      144    64   01100100        d
         101      145    65   01100101        e
         102      146    66   01100110        f
         103      147    67   01100111        g
         104      150    68   01101000        h
         105      151    69   01101001        i
         106      152    6A   01101010        j
         107      153    6B   01101011        k
         108      154    6C   01101100        l
         109      155    6D   01101101        m
         110      156    6E   01101110        n
         111      157    6F   01101111        o
         112      160    70   01110000        p
         113      161    71   01110001        q
         114      162    72   01110010        r
         115      163    73   01110011        s
         116      164    74   01110100        t
         117      165    75   01110101        u
         118      166    76   01110110        v
         119      167    77   01110111        w
         120      170    78   01111000        x
         121      171    79   01111001        y
         122      172    7A   01111010        z
         123      173    7B   01111011        {    (left opening brace)
         124      174    7C   01111100        |    (vertical bar)
         125      175    7D   01111101        }    (right closing brace)
         126      176    7E   01111110        ~    (tilde)
         127      177    7F   01111111      DEL    (delete)
                                              ß # ALT + 225 | ALTGR + s
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
lorem ipsum
-------------------------------------------------------------------------------------
```
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Cras viverra placerat ligula, et congue nisl hendrerit a. Donec porttitor mi eu nulla elementum, sit amet gravida nibh hendrerit. Curabitur felis tellus, elementum vel sagittis at, cursus eget quam. Nunc sit amet nibh non arcu laoreet convallis. Aliquam ornare suscipit sem, quis feugiat turpis consequat ac. Cras et cursus erat. Ut tincidunt placerat fermentum. Pellentesque quis nunc nunc. Sed congue quam ut nulla aliquet cursus. Proin in suscipit orci, et iaculis felis. Ut venenatis eros dolor, congue fermentum orci aliquet a. Maecenas lacinia semper vulputate. Integer nisi augue, porta eget erat a, ullamcorper hendrerit magna. Phasellus pulvinar nunc et libero mattis tristique. Nunc quis dignissim sem. Duis vitae mauris pulvinar, sagittis eros at, aliquam mi. Nunc porta fermentum eros tempor maximus. Morbi luctus lobortis magna eu porta. Nam iaculis est vel lobortis varius. Morbi varius ante a ligula ultricies tristique. Etiam eu mauris diam. Morbi ac dolor porttitor, semper massa a, vehicula sapien. Nulla sodales, mi sit amet placerat vehicula, ex ante ultricies urna, a efficitur erat neque id sem. Vivamus vulputate lobortis tristique. Donec ex urna, porttitor et imperdiet pulvinar, tincidunt id libero. Nunc volutpat urna vel gravida tincidunt. Fusce eget luctus enim. Etiam condimentum volutpat lorem sed tristique. Suspendisse commodo erat id molestie mattis. Aenean eu magna diam. Donec ut condimentum velit. Morbi ut eros eget velit dictum aliquam. Proin enim nunc, aliquet vitae varius finibus, sollicitudin commodo ipsum. Aenean sed justo blandit, laoreet metus non, pretium purus. Maecenas finibus cursus odio. Etiam nunc eros, dictum quis consequat ut, tempor ac felis. Aliquam commodo felis vel tortor vestibulum dignissim. Donec gravida ligula ex, et laoreet augue egestas quis. Maecenas elit ligula, faucibus eget magna finibus, accumsan dictum velit. Curabitur lectus velit, mollis ut placerat maximus, interdum at tellus. Fusce feugiat ipsum condimentum sapien bibendum convallis. Fusce convallis urna non odio tincidunt, sit amet volutpat lectus pellentesque. Sed vel dapibus leo. Fusce at sem vel ex sollicitudin blandit. Sed tristique risus in lacus vulputate, nec eleifend nibh dapibus. Pellentesque pulvinar lorem nec magna fringilla, non aliquet enim elementum. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Donec sit amet mollis lectus, at bibendum nulla. Nulla facilisi. Ut ut fermentum massa. Integer varius sem nec quam eleifend malesuada. Nunc interdum erat ut augue vehicula iaculis. Phasellus eget ex ipsum. Praesent ipsum velit, maximus ac hendrerit vitae, finibus blandit erat. Pellentesque urna ligula, dictum at aliquam id, rhoncus quis arcu. Quisque pellentesque quam felis, nec venenatis metus mollis vitae. Donec interdum nisi ligula, sed ornare dolor malesuada mollis. Morbi nec eros eget metus tempus sollicitudin. Duis pulvinar nisi sit amet nisl ultrices volutpat non ac ipsum. Donec finibus lorem at hendrerit tempor. Quisque mollis libero eget lectus egestas iaculis. Aliquam erat volutpat. Proin commodo metus id quam condimentum sollicitudin.
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
Creative Commons by-nc-sa
-------------------------------------------------------------------------------------
```
<a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/"><img alt="Licence Creative Commons" style="border-width:0" src="https://i.creativecommons.org/l/by-nc-sa/4.0/88x31.png" /></a><br />Ce(tte) œuvre est mise à disposition selon les termes de la <a rel="license" href="http://creativecommons.org/licenses/by-nc-sa/4.0/">Licence Creative Commons Attribution - Pas d Utilisation Commerciale - Partage dans les Mêmes Conditions 4.0 International</a>.

## License

[![Creative Commons License](http://i.creativecommons.org/l/by/4.0/88x31.png)](https://creativecommons.org/licenses/by/4.0/)

This work is licensed under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by/4.0/).

## License

[![Creative Commons License](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)

This work is licensed under a [Creative Commons Attribution-ShareAlike 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
```

**[`^        back to top        ^`](#)**

_____________________________________________________________________________________
256 couleurs
-------------------------------------------------------------------------------------
```
00 #000000
01 #AA0000
02 #00AA00
03 #AA5500
04 #0000AA
05 #AA00AA
06 #00AAAA
07 #AAAAAA
08 #555555
09 #FF5555
10 #55FF55
11 #FFFF55
12 #5555FF
13 #FF55FF
14 #55FFFF
15 #FFFFFF
16 #000000
17 #00005f
18 #000087
19 #0000af
20 #0000d7
21 #0000ff
22 #005f00
23 #005f5f
24 #005f87
25 #005faf
26 #005fd7
27 #005fff
28 #008700
29 #00875f
30 #008787
31 #0087af
32 #0087d7
33 #0087ff
34 #00af00
35 #00af5f
36 #00af87
37 #00afaf
38 #00afd7
39 #00afff
40 #00d700
41 #00d75f
42 #00d787
43 #00d7af
44 #00d7d7
45 #00d7ff
46 #00ff00
47 #00ff5f
48 #00ff87
49 #00ffaf
50 #00ffd7
51 #00ffff
52 #5f0000
53 #5f005f
54 #5f0087
55 #5f00af
56 #5f00d7
57 #5f00ff
58 #5f5f00
59 #5f5f5f
60 #5f5f87
61 #5f5faf
62 #5f5fd7
63 #5f5fff
64 #5f8700
65 #5f875f
66 #5f8787
67 #5f87af
68 #5f87d7
69 #5f87ff
70 #5faf00
71 #5faf5f
72 #5faf87
73 #5fafaf
74 #5fafd7
75 #5fafff
76 #5fd700
77 #5fd75f
78 #5fd787
79 #5fd7af
80 #5fd7d7
81 #5fd7ff
82 #5fff00
83 #5fff5f
84 #5fff87
85 #5fffaf
86 #5fffd7
87 #5fffff
88 #870000
89 #87005f
90 #870087
91 #8700af
92 #8700d7
93 #8700ff
94 #875f00
95 #875f5f
96 #875f87
97 #875faf
98 #875fd7
99 #875fff
100 #878700
101 #87875f
102 #878787
103 #8787af
104 #8787d7
105 #8787ff
106 #87af00
107 #87af5f
108 #87af87
109 #87afaf
110 #87afd7
111 #87afff
112 #87d700
113 #87d75f
114 #87d787
115 #87d7af
116 #87d7d7
117 #87d7ff
118 #87ff00
119 #87ff5f
120 #87ff87
121 #87ffaf
122 #87ffd7
123 #87ffff
124 #af0000
125 #af005f
126 #af0087
127 #af00af
128 #af00d7
129 #af00ff
130 #af5f00
131 #af5f5f
132 #af5f87
133 #af5faf
134 #af5fd7
135 #af5fff
136 #af8700
137 #af875f
138 #af8787
139 #af87af
140 #af87d7
141 #af87ff
142 #afaf00
143 #afaf5f
144 #afaf87
145 #afafaf
146 #afafd7
147 #afafff
148 #afd700
149 #afd75f
150 #afd787
151 #afd7af
152 #afd7d7
153 #afd7ff
154 #afff00
155 #afff5f
156 #afff87
157 #afffaf
158 #afffd7
159 #afffff
160 #d70000
161 #d7005f
162 #d70087
163 #d700af
164 #d700d7
165 #d700ff
166 #d75f00
167 #d75f5f
168 #d75f87
169 #d75faf
170 #d75fd7
171 #d75fff
172 #d78700
173 #d7875f
174 #d78787
175 #d787af
176 #d787d7
177 #d787ff
178 #d7af00
179 #d7af5f
180 #d7af87
181 #d7afaf
182 #d7afd7
183 #d7afff
184 #d7d700
185 #d7d75f
186 #d7d787
187 #d7d7af
188 #d7d7d7
189 #d7d7ff
190 #d7ff00
191 #d7ff5f
192 #d7ff87
193 #d7ffaf
194 #d7ffd7
195 #d7ffff
196 #ff0000
197 #ff005f
198 #ff0087
199 #ff00af
200 #ff00d7
201 #ff00ff
202 #ff5f00
203 #ff5f5f
204 #ff5f87
205 #ff5faf
206 #ff5fd7
207 #ff5fff
208 #ff8700
209 #ff875f
210 #ff8787
211 #ff87af
212 #ff87d7
213 #ff87ff
214 #ffaf00
215 #ffaf5f
216 #ffaf87
217 #ffafaf
218 #ffafd7
219 #ffafff
220 #ffd700
221 #ffd75f
222 #ffd787
223 #ffd7af
224 #ffd7d7
225 #ffd7ff
226 #ffff00
227 #ffff5f
228 #ffff87
229 #ffffaf
230 #ffffd7
231 #ffffff
232 #080808
233 #121212
234 #1c1c1c
235 #262626
236 #303030
237 #3a3a3a
238 #444444
239 #4e4e4e
240 #585858
241 #626262
242 #6c6c6c
243 #767676
244 #808080
245 #8a8a8a
246 #949494
247 #9e9e9e
248 #a8a8a8
249 #b2b2b2
250 #bcbcbc
251 #c6c6c6
252 #d0d0d0
253 #dadada
254 #e4e4e4
255 #eeeeee
```

**[`^        back to top        ^`](#)**