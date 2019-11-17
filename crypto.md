## Index

[TOC]

- [gpg](#gpg)
- [GPG/PGP](#gpgpgp)
- [creer une archive chiffree](#creer-une-archive-chiffree)
- [chiffrement disque dur externe](#chiffrement-disque-dur-externe)
- [chiffrement de dossier par ecryptfs](#chiffrement-de-dossier-par-ecryptfs)
- [chiffrement de dossier par encfs](#chiffrement-de-dossier-par-encfs)
- [creer une archive chiffrée](#creer-une-archive-chiffrée)
- [Chiffrement de fichier](#chiffrement-de-fichier)

---

## gpg

```bash
gpg --gen-key                            #générer une nouvelle clé
gpg --gen-revoke ID_USER                 #générer une clé de révocation pour ID_USER
gpg --edit-key ID_USER                   #éditer la clé de manière interactive
gpg -o FICHIER --export                  #exporter toutes les clés vers fichier
gpg --import FICHIER                     #importer toutes les clés depuis fichier
gpg --send-keys ID_USER                  #envoyer la clé de ID_USER vers le serveur
gpg --recv-keys ID_USER                  #recevoir la clé de ID_USER du serveur de clés
gpg --list-keys ID_USER                  #afficher la liste des clés de ID_USER
gpg --list-sigs ID_USER                  #afficher la liste des signatures de ID_USER
gpg --check-sigs ID_USER                 #vérifier la signature de ID_USER
gpg --fingerprint ID_USER                #vérifier l empreinte de ID_USER
gpg --refresh-keys                       #mettre à jour le porte-clé local

gpg -a -s FICHIER.asc                            #signer fichier dans un fichier
gpg --armor --sign FICHIER                       # , ,
gpg --clearsign FICHIER                          #signer un fichier en clair
gpg --clearsign FICHIER | mail mail@mail.com     #envoyer un message signé en clair
gpg --clearsign --not-dash-escaped FICHIER       #signer en clair un fichier
gpg --verify FICHIER                             #vérifier fichier signé en texte clair
gpg -o FICHIER.sig -b FICHIER                    #créer une signature détachée
gpg -o FICHIER.sig --detach-sig FICHIER          # , ,
gpg --verify FICHIER.sig FICHIER                 #vérifier un fichier avec fichier.sig
gpg -o FICHIER_CHIFFRE.gpg -r NOM -e FICHIER     #chiffrement par clé publique vers binaire
gpg -o FICHIER_CHIFFRE.gpg --recipient NOM --encrypt FICHIER    # , ,
gpg -o FICHIER_CHIFFRE.asc -a -r NOM -e FICHIER  #chiffrement par clé publique vers ASCII
gpg -o FICHIER_CHIFFRE.gpg -c FICHIER            #chiffrement symétrique vers .gpg
gpg -o FICHIER_CHIFFRE.gpg --symmetric FICHIER   # , ,
gpg -o FICHIER_CHIFFRE.asc -a -c FICHIER         #chiffrement symétrique vers ASCII
gpg -o FICHIER -d FICHIER_CHIFFRE.gpg -r NOM     #déchiffrement
gpg -o FICHIER --decrypt FICHIER_CHIFFRE.gpg     # , ,
```

**[`^ back to top ^`](#)**

---

## GPG/PGP

```bash
###>>>formater une cle avec luks
###>>>nom banal et mdp fort

nano /home/USER/.gnupg/gpg.conf
fixed-list-mode
keyid-format 0xlong
with-fingerprint
personal-cipher-preferences AES256
personal-digest-preferences SHA512
default-preference-list SHA512 SHA384 SHA256 RIPEMD160 AES256 TWOFISH BLOWFISH ZLIB BZIP2 ZIP Uncompressed
cipher-algo AES256
digest-algo SHA512
cert-digest-algo SHA512
compress-algo ZLIB
disable-cipher-algo 3DES
weak-digest SHA1
verify-options show-uid-validity
list-options show-uid-validity
no-emit-version
no-comments
export-options export-minimal
s2k-cipher-algo AES256
s2k-digest-algo SHA512
s2k-mode 3
s2k-count 65011712

###>>>liste des cles deja presentes
gpg -K                 #privee
gpg -k                 #public
###>>>sec  'SECret key'
###>>>ssb  'Secret SuBkey'
###>>>pub  'PUBlic key'
###>>>sub  'public SUBkey'

###>>>generation de cle
gpg --gen-key
###>>>1
###>>>4096
###>>>0
###>>>o

###>>>creation de sous-cle de signature
gpg --edit-key IDENTIFIANT
###>>>S = Pour signer
###>>>C = Pour certifier une autre signature
###>>>E = Pour chiffrer
###>>>A = Pour authentifier
###>>>addkey
###>>>4
###>>>4096
###>>>0
###>>>o
###>>>save

###>>>certicat de revocation
gpg --output /CLEUSB/revocation.asc --gen-revoke IDENTIFIANT
###>>>o
###>>>1
###>>>o

###>>>export cle maitre sur cle usb armor et binaire
gpg -a --export-secret-key IDENTIFIANT > /CLEUSB/secret_key.asc
gpg --export-secret-key IDENTIFIANT > /CLEUSB/secret_key.gpg
gpg -a --export IDENTIFIANT > /CLEUSB/public_key.asc
gpg --export IDENTIFIANT > /CLEUSB/public_key.gpg

###>>>export sous-cles sur autre cleusb
gpg -a --export-subkeys IDENTIFIANT > /CLEUSB2/secret_subkeys.asc
gpg --export-subkeys IDENTIFIANT > /CLEUSB2/secret_subkeys.gpg

###>>>nettoyage machine
gpg --delete-secret-keys IDENTIFIANT
###>>>o
###>>>o
gpg -K
gpg --delete-keys IDENTIFIANT
###>>>o
gpg -k

###>>>import des sous-cles sur une nouvelle machine
gpg --import /CLEUSB2/secret_subkeys.gpg

###>>>serveurs de cle
gpg --keyserver pgp.mit.edu --send-key IDENTIFIANT

###>>>echange de cle : faire une empreinte pour verification
###>>>conf with-fingerprint
gpg -k

###>>>steganographie
sudo apt install steghide
steghide embed -cf IMAGE.jpg -ef CLE.txt
steghide extract -sf IMAGE.jpg
```

**[`^ back to top ^`](#)**

---

## creer une archive chiffree

```bash
tar -cvzf - DOSSIER/ | openssl enc -e -aes-256-ecb -in - -out DOSSIER  -pass pass:MOTDEPASS
sha256sum DOSSIER
openssl enc -d -aes-256-ecb -in FICHIER-CHIFFRE -out FICHIER-DECHIFFRE
```

**[`^ back to top ^`](#)**

---

## chiffrement disque dur externe

```bash
sudo apt-get install cryptsetup
sudo cryptsetup luksFormat -h sha256 /dev/XXX
sudo cryptsetup luksOpen /dev/XXX NOMDISQUE
sudo mkfs.ext4 /dev/mapper/NOMDISQUE
```

**[`^ back to top ^`](#)**

---

## chiffrement de dossier par ecryptfs

```bash
mkdir .DOSSIER DOSSIER .ecryptfs
touch .ecryptfs/DOSSIER.conf .ecryptfs/DOSSIER.sig
echo "/CHEMINCOMPLET/.DOSSIER /CHEMINCOMPLET/DOSSIER ecryptfs" > .ecryptfs/DOSSIER.conf
ecryptfs-add-passphrase
echo [sig] > .ecryptfs/DOSSIER.sig

mount.ecryptfs_private DOSSIER
###>>>alimenter le dossier
umount.ecryptfs_private DOSSIER

ecryptfs-add-passphrase
mount.ecryptfs_private DOSSIER
```

**[`^ back to top ^`](#)**

---

## chiffrement de dossier par encfs

```bash
apt install encfs
#attention vunérable
encfs /home/.DOSSIER_CRYPTE /home/MONTAGE_CLAIR
###>>>configuration
###>>>alimenter le dossier MONTAGE_CLAIR
fusermount -u MONTAGE_CLAIR
```

**[`^ back to top ^`](#)**

---

## creer une archive chiffrée

```bash
apt-get install p7zip-full
7z a /CHEMIN/ARCHIVE_SECURE.7z /CHEMIN/FICHIER -p
###>>>attention avec mot de passe en clair dans le terminal
7z a /CHEMIN/ARCHIVE_SECURE.7z /CHEMIN/FICHIER -pPASSWORD
###>>>extraction
7z x /CHEMIN/ARCHIVE_SECURE.7z
```

**[`^ back to top ^`](#)**

---

## Chiffrement de fichier

```bash
apt install gnupg2
gpg2 --full-gen-key
gpg2 --list-keys
gpg2 --encrypt FICHIER
gpg2 --output FICHIER --decrypt FICHIER.gpg
```

**[`^ back to top ^`](#)**
