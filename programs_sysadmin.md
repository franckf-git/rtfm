## Index

[TOC]

- [crontab -e](#crontab--e)
- [cron quotidienne](#cron-quotidienne)
- [commandes periodiques en dessous de une minute](#commandes-periodiques-en-dessous-de-une-minute)
- [SQL](#sql)
- [syslog-ng](#syslog-ng)
- [dokuwiki](#dokuwiki)
- [markdown](#markdown)
- [git](#git)

_____________________________________________________________________________________
crontab -e
-------------------------------------------------------------------------------------
```
 .---------------- minute (0 - 59)
 |  .------------- hour (0 - 23)
 |  |  .---------- day of month (1 - 31)
 |  |  |  .------- month (1 - 12) OR jan feb mar apr ...
 |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun mon tue wed thu fri sat
 |  |  |  |  |
 *  *  *  *  *  [USER] COMMAND > LOG
1-5 : les unites de temps de 1 a 5
1,5 : les unites de temps de 1 et 5
*/6 : toutes les 6 unites de temps (toutes les 6 heures par exemple)

0 9 * * * /home/user/TEST.sh

Mot cle        Equivalent
@yearly        0 0 1 1 *
@daily         0 0 * * *
@hourly        0 * * * *
@reboot        Executer au demarrage
```

_____________________________________________________________________________________
cron quotidienne
-------------------------------------------------------------------------------------
```bash
###>>>vérifie si les paquets sont toujours marqués comme bogués
cat /etc/cron.daily/apt-listbugs
#!/bin/sh -e
...
prefclean()
{
    file="/etc/apt/preferences.d/apt-listbugs"
    backup="/var/backups/apt-listbugs.preferences"

    test -x /usr/lib/ruby/vendor_ruby/aptlistbugs/aptcleanup || return 0
    test -x /usr/bin/apt-listbugs || return 0
    test -f "$file" || return 0

    tmp=$(mktemp --tmpdir apt-listbugs_tmp_preferences.XXXXXX)

    /usr/lib/ruby/vendor_ruby/aptlistbugs/aptcleanup > "$tmp" || return 0
    if ! diff -B "$tmp" "$file" > /dev/null
    then
        if test -f "$backup"
        then
            savelog -q "$backup"
        fi
        cp -aH "$file" "$backup"
        cp "$tmp" "$file"
    fi
    /bin/rm -f "$tmp"
}

prefclean
```

_____________________________________________________________________________________
commandes periodiques en dessous de une minute
-------------------------------------------------------------------------------------
```bash
while (sleep 5 && COMMAND) &
do
wait $!
done

while true
do
bash COMMAND
sleep 1
done

* * * * * sleep 1 ; COMMAND
* * * * * sleep 2 ; COMMAND
...

* * * * * for i in {1..60}; do COMMAND ; sleep 1; done

* * * * * while true; do COMMAND & sleep 1; done
```

_____________________________________________________________________________________
SQL
-------------------------------------------------------------------------------------
```bash
sudo apt-get install mysql-server mysql-client
mysql -h localhost -u root -pMOTDEPASS
```
```sql
-- creation base de donnees et affectation des droits
SET NAMES 'utf8';
GRANT ALL PRIVILEGES ON basededonnee * TO 'USER'@'localhost' IDENTIFIED BY 'MOTDEPASS';
```
```bash
mysql -h localhost -u USER -pMOTDEPASS --default-character-set=utf8
```
```sql
SELECT 'value';

-- creation base de donnees
CREATE DATABASE basededonnee CHARACTER SET 'utf8';
USE basededonnee
DROP DATABASE IF EXISTS basededonnee;

-- creation table
CREATE TABLE table (
    id SMALLINT UNSIGNED NOT NULL AUTO_INCREMENT,
    colonne1 VARCHAR(40) NOT NULL,
    colonne2 CHAR(1),
    colonne3 DATETIME NOT NULL,
    colonne4 VARCHAR(30),
    colonne5 TEXT,
    PRIMARY KEY (id)
)
ENGINE=INNODB;

-- afficher table
SHOW TABLES;           -- liste les tables de la base de donnees
DESCRIBE table;  -     -- liste les colonnes et leurs caracteristiques
DROP TABLE table;
SELECT * FROM table;

-- modification table
ALTER TABLE nom_table ADD ...              -- permet d ajouter
ALTER TABLE nom_table DROP ...             -- permet de retirer
ALTER TABLE nom_table CHANGE ...
ALTER TABLE nom_table MODIFY ...           -- permettent de modifier une colonne
ALTER TABLE nom_table ADD COLUMN date_insertion DATE NOT NULL;
ALTER TABLE nom_table CHANGE ancien_nom nouveau_nom description_colonne;
ALTER TABLE nom_table CHANGE prenom nom VARCHAR(30) NOT NULL;                        -- Changement du type + changement du nom
ALTER TABLE nom_table CHANGE colonne colonne BIGINT NOT NULL;                        -- Changement du type sans renommer
ALTER TABLE nom_table MODIFY colonne BIGINT NOT NULL AUTO_INCREMENT;                 -- Ajout de l auto-incrementation
ALTER TABLE nom_table MODIFY nom VARCHAR(30) NOT NULL DEFAULT 'commentaires';        -- Changement de la description

-- insertion nouvelle ligne
INSERT INTO table VALUES (1, 'valeur2', 'valeur3', 'date', 'valeur5', 'valeur6');
INSERT INTO table VALUES (NULL, 'valeur2', 'valeur3', 'date', 'valeur5', NULL);
INSERT INTO table (id, colonne2, colonnedate, colonne4);
VALUES ('valeur1', 'valeur2', 'date', 'valeur4'),
       ('valeur1', NULL, '2010-08-23 05:18:00', NULL);

SOURCE \chemin\fichier.sql;
    INSERT INTO table (id, colonne2, colonnedate, colonne4)
VALUES ('valeur1', 'valeur2', 'date', 'valeur4'),
       ('valeur1', NULL, '2010-08-23 05:18:00', NULL)
    ...;

-- affichage valeurs
SELECT * from table WHERE colonne1='valeur1';
SELECT * FROM table WHERE colonnedate < 'date';
SELECT * FROM table WHERE colonne1='valeur1' AND colonne2='valeur2';
SELECT * FROM table WHERE colonne3 = NULL;
SELECT * FROM table WHERE colonne3='valeur3' AND colonne4 IS NOT NULL ORDER BY colonne4 DESC;
SELECT * FROM table ORDER BY id LIMIT 6 OFFSET 0;
SELECT * FROM table WHERE colonne1 LIKE '%val%';           -- insensible à la casse
SELECT * FROM table WHERE colonne1 LIKE BINARY '%val%';    -- sensible à la casse
SELECT * FROM table WHERE colonne1 IN ('valeur', 'valeur', 'valeur', 'valeur', 'valeur', 'valeur', 'valeur');    --IN equivaut a une suite de OR
```

```bash
mysqldump -u USER -p --opt NOM_DE_LA_BASE > SAUVEGARDE.sql
mysql NOM_BASE < CHEMIN/FICHIER_DE_SAUVEGARDE.sql
```
```sql
--ou
USE nom_base;
SOURCE fichier_de_sauvegarde.sql;

UPDATE table SET colonne1='valeur1', colonne2='valeur2' WHERE id=21;  --irreversible
DELETE FROM table WHERE colonne2 = 'valeur2';                         --supp ligne
UPDATE table SET colonne8='commentaires';
```

_____________________________________________________________________________________
syslog-ng
-------------------------------------------------------------------------------------
```bash
sudo apt install syslog-ng

sudo nano /etc/syslog-ng/syslog-ng.conf
@version: 3.5
@include "scl.conf"
@include "`scl-root`/system/tty10.conf"

options {
    chain_hostnames(off);
    flush_lines(0);
    use_dns(no);
    use_fqdn(no);
    owner("user");
    group("adm");
    perm(0640);
    stats_freq(0);
    keep_hostname(yes);
    long_hostnames(on);
    create_dirs(yes);
};

source s_src {
       system();
       internal();
};
#source s_other { file("/var/log/program/program.log"); };

filter f_pam { not message("pam_unix"); };

destination d_perso { file("/var/log/perso.log"); };

log {
  source(s_src);
  filter(f_pam);
  destination (d_perso);
};

@include "/etc/syslog-ng/conf.d/*.conf"

sudo systemctl restart syslog-ng.service
tail /var/log/perso.log
```

_____________________________________________________________________________________
dokuwiki
-------------------------------------------------------------------------------------
```
**gras**
//italique//
__souligné__
''à chasse fixe''
<sub>indices</sub>
<sup>exposants</sup>
<del>supprimé</del>

[[nom de page|texte de lien]]
[[voici:des:categories]]
[[syntax#internes|cette section]]
[[\\serveur\partage|ceci]]
[[categories:nom de page|texte de lien]]\\

((Ceci est une note de pied de page))
==== Titre niveau 3 ====
=== Titre niveau 4 ===
== Titre niveau 5 ==
---- ligne

FIXME
DELETEME
:?:
:!:

{{wiki:dokuwiki-128.png}}                #taille originale
{{wiki:dokuwiki-128.png?50}}             #largeur donnée
{{wiki:dokuwiki-128.png?200x50}}         #hauteur et largeur données
{{http://url/images/php.gif?200x50}}     #Image externe redimensionnée
{{ wiki:a_droite-128.png}}
{{wiki:a_gauche-128.png }}
{{ wiki:centre-128.png }}
{{ wiki:dokuwiki-128.png |This is the caption}}
{{wiki:dokuwiki-128.png?nolink}}         #Pas de lien
{{wiki:dokuwiki-128.png?direct}}         #Lien direct sur le media
{{wiki:dokuwiki-128.png?linkonly}}       #Uniquement le lien direct
[[http://url|{{wiki:dokuwiki-128.png}}]] #Lien vers une page
{{film.mp4|une vidéo sympa}}

  * Ceci est une liste
  * Le second élément
    * Vous pouvez avoir plusieurs niveaux
  * Un autre élément
  - La même liste mais ordonnée
  - Un autre élément
    - Utilisez simplement l indentation pour des niveaux plus profonds
  - Et voilà

> citation
>> reponse

^ Titre 1         ^ Titre 2           ^ Titre 3                            ^
| Ligne 1 Col 1   | Ligne 1 Col 2     | Ligne 1 Col 3                      |
| Ligne 2 Col 1   | colonnes fusionnées (notez la double barre verticale) ||
| Ligne 3 Col 1   | Ligne 2 Col 2     | Ligne 2 Col 3                      |

|              ^ Titre 1                  ^ Titre 2          ^
^ Titre 3      | Ligne 1 Col 2            | Ligne 1 Col 3    |
^ Titre 4      | pas de fusion cette fois |                  |
^ Titre 5      | Ligne 2 Col 2            | Ligne 2 Col 3    |

^ Titre 1       ^ Titre 2                                  ^ Titre 3       ^
| Ligne 1 Col 1 |  Cette cellule s étend\\ verticalement   | Ligne 1 Col 3 |
| Ligne 2 Col 1 | :::                                      | Ligne 2 Col 3 |
| Ligne 3 Col 1 | :::                                      | Ligne 3 Col 3 |

^          Tableau avec alignement         ^^^
|        droite|    centré    |gauche        |
|gauche        |        droite|    centré    |
| xxxxxxxxxxxx | xxxxxxxxxxxx | xxxxxxxxxxxx |

<nowiki>
afficher du texte exactement comme il est saisi
</nowiki>
<code nom_du_code fichier.code>
Ceci est du code colorisé et disponible au téléchargement
</code>
<file>
citation d un fichier 
</file>
```

_____________________________________________________________________________________
markdown
-------------------------------------------------------------------------------------
```
*italic text*
_italic text_
**my bold text**
__my bold text__
***my bold and italic text***
___my bold and italic text___

# Heading level 1
## Heading level 2
### Heading level 3
#### Heading level 4
##### Heading level 5
###### Heading level 6

Heading level 1
===============

Heading level 2
---------------

View the page by [clicking here](URL)
<url>
![message](screenshots/image.png)

> Blockquote
>
>> subBlockquote
1. Ordered List
2. Ordered List
* Unordered List
* Unordered List
- Unordered List
- Unordered List

    code
sentence `code` follow

(lines)
***
---
_________________

[^footnote]
[^footnote]: Here is the *text* of the **footnote**.

<kbd>Encart</kbd>

|                  | Colonne  | Colonne  |
 ----------------- | -------- | --------
| Ligne            | `Ligne`  | Ligne    |
| Ligne            | Ligne    | Ligne    |
| Ligne            | Ligne    | Ligne    |

Colonne  | Colonne
-------- | ---
Ligne    | Ligne
Ligne    | Ligne
Ligne    | Ligne

| Colonne  | Colonne | Colonne   |
| :------- | ----:   | :---:     |
| Ligne    | Ligne   |  Ligne    |
| Ligne    | Ligne   |  Ligne    |
| Ligne    | Ligne   |  Ligne    |

Term 1
Term 2
:   Definition A
:   Definition B

- [x] This is a complete item
- [ ] This is an incomplete item
```

_____________________________________________________________________________________
git
-------------------------------------------------------------------------------------
```bash
git init NOM-DU-PROJET                      #Crée un dépôt local
git clone URL                               #Télécharge un projet et son historique
git status                                  #Liste les fichiers modifiés à commiter
git diff                                    #Montre les modifications
git add FICHIER                             #Ajoute un instantané du fichier
git diff --staged                           #Montre les différences de fichier
git reset FICHIER                           #Enleve le fichier de l index
git commit -m "MESSAGE"                     #Enregistre des instantanés de fichiers
git commit -a                               #déposer des modifications sur le dépôt
git branch                                  #Liste toutes les branches locales
git branch NOM-DE-BRANCHE                   #Crée une nouvelle branche
git checkout NOM-DE-BRANCHE                 #Bascule sur la branche et met à jour
git merge NOM-DE-BRANCHE                    #Combine la branche courante et spécifiée
git branch -d NOM-DE-BRANCHE                #Supprime la branche spécifiée
git rm FICHIER                              #Supprime le fichier du répertoire
git rm --cached FICHIER                     #Supprime le fichier du système de suivi
git mv FICHIER NOUVEAU-NOM                  #Renomme le fichier
git ls-files --others --ignored --exclude-standard   #Tous les fichiers exclus du suivi
.gitignore                                  #Exclure du suivi (mais contenu sur tous les repos)
.git/info/exclude                           #Exclure du projet (reste en local)
cat ~/.gitconfig                            #Configuration globale
git config --global alias.lp "log --pretty" #Création d alias
git stash                                   #Remiser son travail
git stash pop                               #Applique une remise et la supprime
git stash list                              #Liste toutes les remises
git stash drop                              #Supprime la remise la plus récente
git log                                     #Montre l historique des versions
git log --follow FICHIER                    #Les actions de renommage, pour le fichier
git log -p FICHIER                          #voir le détail des modifications
git diff 1ere-BRANCHE...2eme-BRANCHE        #Montre les différences de contenu
git show COMMIT                             #Montre les modifications de métadonnées
git reset COMMIT                            #Annule tous les commits après COMMIT
git reset --hard COMMIT                     #Supprime tout l historique après COMMIT
git fetch NOM-DE-DEPOT                      #Récupère tout l historique du dépôt nommé
git merge NOM-DE-DEPOT/BRANCHE              #Fusionne la branche du dépôt
git bisect                                  #mode debug pour traquer un mauvais commit
git push ALIAS BRANCHE                      #Envoie tous les commits vers GitHub
git pull                                    #Récupère tout l historique du dépôt
git repack -a -d; git prune                 #Réempaqueter le dépôt local en un seul

###>>>installation
sudo apt-get install git
git config --global user.name "Your Name"
git config --global user.email youremail@mailsite.com
git config --global color.ui true
git config --global format.pretty oneline

###>>>utlisation
cd /home/user/GITREPOSITORY
git init
git add README.md
git add .
git commit -m "first commit"
git commit -a -m "MAJ fichier"
git log

###>>>GitHub > Create new > New repository
git remote add origin https://github.com/USER/REPO.git
git push -u origin master
git remote -v
https://github.com/USER/REPO
wget https://raw.githubusercontent.com/USER/REPO/master/FILE.md

###>>>Connecting to GitHub with SSH
###>>>a faire avant de créer le repot
ls -al ~/.ssh
ssh-keygen -t rsa -b 4096 -C "youremail@mailsite.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
clip < ~/.ssh/id_rsa.pub
###>>>GitHub > Settings > SSH and GPG keys > Add SSH key (paste)
ssh -T git@github.com
git clone git@github.com:USER/REPO.git
git remote set-url origin git@github.com:USER/REPO.git

###>>>GitHub Pages > create a new repository named USERNAME.github.io
git clone https://github.com/USERNAME/USERNAME.github.io
cd USERNAME.github.io
echo "Hello World" > index.html
git add --all
git commit -m "Initial commit"
git push -u origin master
###>>>https://USERNAME.github.io
```