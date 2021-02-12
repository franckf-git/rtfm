## Index

[TOC]

- [crontab -e](#crontab--e)
- [cron quotidienne](#cron-quotidienne)
- [commandes periodiques en dessous de une minute](#commandes-periodiques-en-dessous-de-une-minute)
- [Ansible](#ansible)
- [SQL](#sql)
- [Cassandra](#cassandra)
- [syslog-ng](#syslog-ng)
- [dokuwiki](#dokuwiki)
- [markdown](#markdown)
- [git](#git)
- [git commit messages](#git-commit-messages)
- [export web page  CLI](#export-web-page-cli)

---

## crontab -e

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
*/6 : toutes les 6 unites de temps

0 9 * * * /home/user/TEST.sh

Mot cle        Equivalent
@yearly        0 0 1 1 *
@daily         0 0 * * *
@hourly        0 * * * *
@reboot        Executer au demarrage

exemples :
30 */2 * * *               -> 30 minutes past the hour every 2 hours
15,45 23 * * *             -> 11:15PM and 11:45PM every day
0 1 ? * SUN                -> 1AM every Sunday
0 1 * * SUN                -> 1AM every Sunday (same as above)
0 0 1 jan/2 * 2011-2013    -> midnight on January 1, 2011 and the first of every odd month until the end of 2013
24 7 L * *                 -> 7:24 AM on the last day of every month
24 7 * * L5                -> 7:24 AM on the last friday of every month
24 7 * * Lwed-fri          -> 7:24 AM on the last wednesday, thursday, and friday of every month
```

**[`^ back to top ^`](#)**

---

## cron quotidienne

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

**[`^ back to top ^`](#)**

---

## commandes periodiques en dessous de une minute

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

**[`^ back to top ^`](#)**

---

## Ansible

- [Ansible Documentation](https://docs.ansible.com/ansible/latest/index.html)
- [All modules](https://docs.ansible.com/ansible/latest/modules/list_of_all_modules.html)
- [Best Practices Essentials](https://www.ansible.com/blog/ansible-best-practices-essentials)
- [Best Practices](https://docs.ansible.com/ansible/latest/user_guide/playbooks_best_practices.html#best-practices)

```bash
# Installation et configuration
yum install epel-release
yum install ansible
yum install nano nmap
nmap -sP 192.168.1.0/24
nano /etc/hosts
192.168.1.5 HOSTNAMEDISTANT

ssh-keygen
cat /etc/hosts
ssh-copy-id MACHINE4
ssh MACHINE4
ssh-copy-id MACHINE5
ssh-copy-id MACHINE6
nano /etc/ansible/hosts
[GROUP]
192.168.1.5 ansible_port=2222 ansible_user=root ansible_ssh_private_key_file=PRIVATEKEY
#ou
HOSTNAMEDISTANT

ansible -m ping all

# Instructions en CLI
ansible -m yum -a 'name=* state=latest' ALLMIMES
ansible -m yum -a 'name=epel-release state=latest' ALLMIMES
ansible -m yum -a 'name=htop state=latest' ALLMIMES
ansible -m yum -a 'name=nano state=latest' ALLMIMES
ansible -m shell -a 'uname -a' MACHINE5

# Les playbooks
nano upgrade.yml
---
- hosts: ALLMIMES
  remote_user: root

  tasks:
    - name: update
      apt: update_cache=yes
    - name: upgrade
      apt: upgrade=dist
    - name: install nano
      apt: name=nano state=present
    - name: install common packages for all servers
      apt:
        update_cache=yes
        state=latest
        name={{item}}
      with_items:
      - curl
      - htop
      - ncdu
      - pwgen
      - strace
      - sudo
      - tar
      - unzip
      - vim
      - wget
      - whois
      - screen

ansible-playbook upgrade.yml --verbose
```

**[`^ back to top ^`](#)**

---

## SQL

```bash
sudo apt-get install mysql-server mysql-client
#or
dnf install mariadb mariadb-server

systemctl start mariadb
systemctl enable mariadb
tail /var/log/mariadb/mariadb.log

mysql_install_db #script to initialize the database
mysql_secure_installation #secure your MySQL installation
#Set root password? [Y/n] y
#Remove anonymous users? [Y/n] y
#Disallow root login remotely? [Y/n] y
#Remove test database and access to it? [Y/n] y
#Reload privilege tables now? [Y/n] y
mysql -h localhost -u root -pMOTDEPASS
cat ~/.mysql_history #history des requetes sql
```

```sql
-- creation base de donnees et affectation des droits
SET NAMES 'utf8';
GRANT ALL PRIVILEGES ON basededonnee * TO 'USER'@'localhost' IDENTIFIED BY 'MOTDEPASS';
```

```bash
ls /var/lib/mysql/DATABASE #dossier de la base
mysql -h localhost -u USER -pMOTDEPASS --default-character-set=utf8
mysql -A -hIP -uUSER -p BASE -e "SHOW TABLES LIKE 'TABLE%';"
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

-- creation table avec clé étrangere
CREATE TABLE table1 (id1, colonne1, PRIMARY KEY (id1))ENGINE=INNODB;
CREATE TABLE table2 (id2, colonne2, id1 PRIMARY KEY (id1), INDEX (id1), FOREIGN KEY (id1) REFERENCES table1 (id1) ON UPDATE CASCADE ON DELETE CASCADE)ENGINE=INNODB;

-- afficher table
SHOW TABLES;           -- liste les tables de la base de donnees
DESCRIBE table;  -     -- liste les colonnes et leurs caracteristiques
DROP TABLE table;
SELECT * FROM table;

SHOW GLOBAL VARIABLES LIKE 'innodb_fil%';
SHOW INDEX FROM mytable;
SHOW TABLE STATUS;

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
SELECT * FROM table WHERE id BETWEEN 1 and 10;
SELECT * FROM table ORDER BY id LIMIT 6 OFFSET 0;
SELECT * FROM table WHERE colonne1 LIKE '%val%';           -- insensible à la casse
SELECT * FROM table WHERE colonne1 LIKE BINARY '%val%';    -- sensible à la casse
SELECT * FROM table WHERE colonne1 IN ('valeur', 'valeur', 'valeur', 'valeur', 'valeur', 'valeur', 'valeur');    --IN equivaut a une suite de OR
-- comparaison de tables
SELECT * FROM TABLEA WHERE CHAMPA NOT IN( SELECT CHAMPB FROM TABLEB);
-- jointure
SELECT t1.col1 , t2.col2 FROM table1 AS t1 JOIN table2 AS t2 on t1.COLID = t2.COLID;
select t1.col1, t1.col2, t3.col4 from table1 as t1 join table2 as t2 on t1.col1 = t2.col1 join table3 as t3 on t2.col2 = t3.col1;
```

```bash
mysqldump -u USER -p --opt NOM_DE_LA_BASE > SAUVEGARDE.sql
mysqldump -hIP -uUSER -p BASE TABLE1 T2 T3 > DUMP.sql
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

reparation de bases SQL

```sql
ANALYZE TABLE myTable;
CHECK TABLE myTable;
SHOW PROCESSLIST;
KILL <id>;
REPAIR TABLE myTable;
OPTIMIZE TABLE myTable:

SHOW CREATE TABLE myTable;
ALTER TABLE myTable ENGINE = InnoDB;
```

```bash
mysqlcheck --repair --databases db_name ...
mysqlcheck --repair --all-databases
/usr/bin/mariadbcheck  -h localhost -u root -prdserdse --repair --all-databases

mysqldump --all-databases > dump.sql
mysql < dump.sql
```

**[`^ back to top ^`](#)**

---

## Cassandra

```bash
wget http://www.apache.org/dyn/closer.lua/cassandra/3.0.15/apache-cassandra-3.0.15-bin.tar.gz
tar -xvzf apache-cassandra-3.0.15-bin.tar.gz apache-cassandra-3.0.15

./apache-cassandra-3.11.1/bin/cassandra
./apache-cassandra-3.11.1/bin/cqlsh localhost -u cassandra -p cassandra
```

```bash
ps auwx | grep cassandra
sudo kill pid
sudo rm -rf /var/lib/cassandra/data/data/system/*
```

- Install Cassandra on each node.
- Choose a name for the cluster.
- Get the IP address of each node.
- Determine which nodes will be seed nodes.
- Determine the snitch and replication strategy.

> cassandra.yaml :
>
> - cluster_name : nom du cluster
> - listen_address : adresse IP ou nom de machine du noeud
> - seed_provider : adresse IP pour la découverte du réseau
> - endpoint_snitch : SimpleSnitch est utilisé pour la configuration en datacenter unique
> - authenticator authorizer et role_manager : gestion des autorisations d’accès à Cassandra
> - commitlog_directory : placer dans une autre partition (ou disque physique !) que les fichiers de bases

```sql
-- creation base de donnees
CREATE KEYSPACE basededonnee WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
USE basededonnee :
DROP DATABASE IF EXISTS basededonnee;
```

[Création des tables](https://docs.datastax.com/en/cql/3.1/cql/cql_reference/create_table_r.html)

- p : clé de partition (primaire)
- c : clé (colonne) de clustering
- s : index secondaire

```sql
CREATE TABLE one_p_two_c ( one varchar, two int, three int, primary KEY ((one), two)) WITH CLUSTERING ORDER BY (two ASC);
CREATE TABLE one_p_two_p ( one varchar, two int, three int, primary KEY ((one, two))) ;
CREATE TABLE one_p_two_c_thr_c ( one varchar, two int, three int, primary KEY ((one), two, three)) WITH CLUSTERING ORDER BY (two ASC);
CREATE TABLE one_p_two_p_thr_c ( one varchar, two int, three int, primary KEY ((one, two), three)) WITH CLUSTERING ORDER BY (three ASC);
INSERT INTO table (one , two , three ) VALUES ( 'text', int, int);
INSERT INTO table (one , two , three ) VALUES ( 'text', int, int);

-- afficher table
SELECT * FROM table;
SELECT * FROM one_p_two_c_thr_c   WHERE one IN ( 'text','text') AND two IN (int,int) AND three = int ;
SELECT * FROM one_p_two_c  WHERE one IN ( 'text','text') AND two IN (int,int);

-- index secondaire
CREATE TABLE one_p_two_s ( one varchar, two int, three int, primary KEY (one)) ;
CREATE INDEX ON one_p_two_s (two) ;
```

[CQL commands](https://docs.datastax.com/en/cql/3.3/cql/cql_reference/cqlCommandsTOC.html)

```sql
ALTER KEYSPACE             -- Changes keyspace replication and enable/disable commit log.
ALTER MATERIALIZED VIEW    -- Changes the table properties of a materialized view.
ALTER ROLE                 -- Changes password, and set superuser or login options.
ALTER TABLE                -- Modifies the columns and properties of a table.
ALTER TYPE                 -- Modify a user-defined type.
ALTER USER                 -- Alter existing user options.
BATCH                      -- Applies multiple data modification with atomicity.
CONSISTENCY [level]        -- Determines nodes must respond (QUORUM, ALL, ONE, ANY).
CREATE AGGREGATE           -- Create user-defined aggregate.
CREATE INDEX               -- Define a new index on a single column of a table.
CREATE FUNCTION            -- Creates custom function that execute user provided code.
CREATE KEYSPACE            -- Define a new keyspace.
CREATE MATERIALIZED VIEW   -- Create a materialized view.
CREATE TABLE               -- Define a new table.
CREATE TRIGGER             -- Registers a trigger on a table.
CREATE TYPE                -- Create a customized collection, user-defined type (UDT).
CREATE ROLE                -- Create roles for access control to database objects.
DELETE                     -- Removes data from columns or removes the entire row.
DROP AGGREGATE             -- Drop a user-defined aggregate.
DROP FUNCTION              -- Drop user-defined function (UDF).
DROP INDEX                 -- Drop the named index.
DROP KEYSPACE              -- Remove the keyspace.
DROP MATERIALIZED VIEW     -- Remove the named materialized view.
DROP ROLE                  -- Removes a role.
DROP TABLE                 -- Remove the named table.
DROP TRIGGER               -- Removes registration of a trigger.
DROP TYPE                  -- Drop a user-defined type.
GRANT                      -- Defines resource authorization.
INSERT                     -- Inserts an entire row or upserts data into existing rows.
LIST PERMISSIONS           -- Lists role permissions on resources.
LIST ROLES                 -- Lists roles and shows superuser and login status.
REVOKE                     -- Remove privileges on database objects from roles.
SELECT                     -- Retrieve data from a Cassandra table.
TRUNCATE                   -- Remove all data from a table.
UPDATE                     -- Update columns in a row.
USE                        -- Connects the client session to a keyspace.
```

```bash
nodetool repair             # Repairs one or more tables.
```

```
COPY nomtable (colonne, colonne, ...) TO 'dump.csv';    # sauvegarde
nodetool snapshot                                       # sauvegarde
COPY nomtable (colonne, colonne, ...) FROM 'dump.csv';  # restoration
```

**[`^ back to top ^`](#)**

---

## syslog-ng

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

**[`^ back to top ^`](#)**

---

## dokuwiki

`URL.com/PAGE/&do=export_xhtml` cleaner view (for print)

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

**[`^ back to top ^`](#)**

---

## markdown

```
*italic text*
_italic text_
**my bold text**
__my bold text__
***my bold and italic text***
___my bold and italic text___

jump line, two spaces at the end

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
3) Ordered List
4) Ordered List
* Unordered List
* Unordered List
- Unordered List
- Unordered List

    code
sentence `code` follow

~~~bash
code
~~~

```

code

```

(LINE)
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

![progress](http://progressed.io/bar/28 "progress")
![progress](http://progressed.io/bar/59?title=completed "progress")
```

**[`^ back to top ^`](#)**

---

## git

```bash
git init NOM-DU-PROJET                      #Crée un dépôt local
git init --bare                             # creer un depot sur le serveur
git remote -v                               # liste des depots distants
git clone URL                               #Télécharge un projet et son historique
git status                                  #Liste les fichiers modifiés à commiter
git diff                                    #Montre les modifications
git add FICHIER                             #Ajoute un instantané du fichier
git diff --staged                           #Montre les différences de fichier
git reset FICHIER                           #Enleve le fichier de l index
git commit -m "MESSAGE"                     #Enregistre des instantanés de fichiers
git commit -a                               #déposer des modifications sur le dépôt
git branch                                  #Liste toutes les branches locales
git branch -r                               # liste des branches distantes
git branch NOM-DE-BRANCHE                   #Crée une nouvelle branche
git checkout NOM-DE-BRANCHE                 #Bascule sur la branche et met à jour
git checkout -b BRANCH                      #creer la branch et passer dessus
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
git stash apply                             # applique les modifications et revient sur le stash
git stash save MESSAGE                      # remise avec un message
git stash show stash@{0} -p                 # voir les modifications
git stash branch BRANCH                     # creer une branche a partir des modifs
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
git remote set-url origin git@GITSERVER:USER/REPO.git #remplacer le depot
git tag                                     #Listing the available tags
git tag -a v1.4 -m "my version 1.4"         #Annotated Tags
git tag v1.4-lw                             #Lightweight Tags
git tag -a v1.2 9fceb02                     #tag after commits
git tag -d v1.4-lw                          #Deleting Tags
git blame FILE                              #Check modif by users
git switch -                                #Switch between the last 2 branch
git restore myFile.txt                      #Discarding your local changes in a certain file
git restore .                               #Undoing all of the local changes in the working copy (be careful)

git grep 'import' $(git rev-list --all)     #full search in history

git instaweb                                # create web server to display repo

###>>>installation
sudo apt-get install git
git config --global user.name "Your Name"
git config --global user.email youremail@mailsite.com
git config --global color.ui true
git config --global format.pretty oneline

###>>>utilisation
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

###>>>config file with git
git init --bare $HOME/.dotfiles # bare repo
alias dotfiles='/usr/bin/git --git-dir=$HOME/.dotfiles/ --work-tree=$HOME' # alias
mydotfiles config --local status.showUntrackedFiles no # show only what is add
```

**[`^ back to top ^`](#)**

---

## git commit messages

commitlint
```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

type:
feat - a new feature
fix - a bug fix
docs - changes in documentation
style - everything related to styling
refactor - code changes that neither fixes a bug or adds a feature
test - everything related to testing
chore - updating build tasks, package manager configs, etc
build -
ci -
perf -
revert -

add "!" if BREAKING CHANGE

body: details, fix #34

footer: "BREAKING CHANGE:"

branchlint
```
<type>/<name>/<issue_ID>
```

type :
feature
bugfix
hotfix
chore
experiment

name :
explicit - ex: add-mail-parser OR change-bdd

issue_ID :
optional
id of gitlab/gihub/youtrack issue (close automatictly when merge)
if the branch is for only one issue 

> can be use for automatic changelogs

**[`^ back to top ^`](#)**

---

## git flow

```bash
git flow init                       # pour initialiser Git et GitFlow dans un projet.
git flow feature start <nom>        # pour démarrer le développement d'une nouvelle fonctionnalité.
git flow feature finish <nom>       # pour terminer le développement d'une nouvelle fonctionnalité.
git flow release start <version>    # pour démarrer le développement d'une nouvelle release.
git flow release finish <nom>       # pour terminer le développement d'une nouvelle release.
git flow hotfix start <version>     # pour démarrer le développement d'un nouveau hotfix.
git flow hotfix finish <nom>        # pour terminer le développement d'un nouveau hotfix.
```

**[`^ back to top ^`](#)**

---

## export web page  CLI

```bash
# https://peter.sh/experiments/chromium-command-line-switches/
chromium --headless --disable-gpu --print-to-pdf https://site.com
chromium --headless --disable-gpu --dump-dom https://site.com
chromium --headless --disable-gpu --screenshot https://site.com
chromium --headless --hide-scrollbars --run-all-compositor-stages-before-draw --screenshot=single-page-app.png --fullPage --window-size=1920,8640 https://site.com--virtual-time-budget=10000
```

**[`^ back to top ^`](#)**

