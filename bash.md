## Index

[TOC]

- [interaction entre les commandes](#interaction-entre-les-commandes)
- [shell](#shell)
- [résumé shell](#résumé-shell)
  - [Variables spéciales du shell](#variables-spéciales-du-shell)
  - [Opérateurs de test : comparaison binaire](#opérateurs-de-test--comparaison-binaire)
  - [Opérateurs de test : fichiers](#opérateurs-de-test--fichiers)
  - [Substitution et expansion de paramètres](#substitution-et-expansion-de-paramètres)
  - [Opérations sur les chaînes](#opérations-sur-les-chaînes)
  - [Syntaxes diverses](#syntaxes-diverses)
- [PS1](#ps1)

---

## interaction entre les commandes

```bash
COMMAND &             #commande en arriere plan
nohup COMMANDE        #commande en arriere plan et hors console
bg COMMANDE           #commande en arriere plan
fg COMMANDE           #Reprend la main
'COMMAND'             #sans interpretation bash echo > $exemple
"COMMAND"             #interprete les cmd echo > EXEMPLE
COMMAND && COMMAND    #execute si la premiere est un success
COMMAND ; COMMAND     #execute toujours la seconde commande
COMMAND || COMMAND    #execute si la premiere est un echec
COMMAND !(*.FILE)     #execute la commande sauf pour les file
COMMAND | COMMAND     #la seconde commande execute le resultat de la premiere
(COMMAND1 && COMMAND2) || (COMMAND3 && COMMAND4)   #separe les commandes
TEST\(1\).txt         #pour les caracteres speciaux
COMMAND >> FILE.log   #rediriger les resultats a la fin d un fichier
COMMAND 2>> FILE.log  #rediriger les erreurs a la fin d un fichier
COMMAND >> FILE.log 2>&1   #rediriger les resultats et les erreurs
COMMANDE > FILE.log 2>&1   #rediriger à la fois la sortie standard et l erreur standard
COMMANDE < FILE.log        #rediriger l entrée standard de la commande vers le fichier
COMMANDE << DELIMITEUR     #rediriger l entrée standard de la commande vers les lignes
DELIMITEUR                 #+ suivantes jusqu à ce que le délimiteur soit rencontré
COMMAND && echo "OK" || echo "FAIL"       #exemples
COMMAND | COMMAND -   # - recupere la sortie de commande
VARIABLE=$(COMMAND)   #commande dans une variable
(REGEX)(REGEX)        #peut etre appeler avec $1,$2...
sudo !!               #exectue la derniere commande en root
xterm -geometry 170x60 -sb -rightbar -hold -e /home/SCRIPTS.sh                        #script auto dans fenetre pour logs
$_                    #le dernier argument final de la dernière commande
$?                    #code de sortie d une commande
diff <(ls -l) <(ls -al)

grep            #filtrer des donnees
sort            #trier les lignes
wc              #compter le nombre de lignes
uniq            #supprimer les doublons
cut             #couper une partie du fichier
sed             #remplacement de caracteres
du -ah          #taille de dossier
sleep           #pause dans l execution
usleep          #sleep en micro-secondes
ls ti*          #tito titi
ls ti?i         #timi titi
ls t[ia]*       #titi tata titi
ls t[a-m]*      #tata tete tme

###>>> check and linting for shell scripts
sudo dnf install ShellCheck
```

**[`^ back to top ^`](#)**

---

## completions personnelles

```bash
#lister les completions dans une fonctions
nano .bashrc
function _LISTCOMPLETION {
   COMPREPLY=( "ELEMENT1" "ELEMENT2" )
   return 0
}
#affecter la completion
complete -F _LISTCOMPLETION COMMAND
#en faisant COMMAND<SPACE><TAB> la completion apparait

#selectionner certains extensions pour un commande
complete -f -X '!*.@(EXT1|EXT2)' COMMAND

###>>> ou plus simple
FONCTION { }
complete -W "ARG1 ARG2 ARG3" FONCTION
```

**[`^ back to top ^`](#)**

---

## shell

```bash
#! /bin/bash          #en debut de script
#! /bin/bash -x       # affichera chaque avant execution
0                     #TRUE
1                     #FALSE
exit $?               #en fin de script
/usr/local/bin        #emplacement des scripts personnels
caller 0              #débogage si dans une fonction
echo -e '\E[37;44m'"\033[1mMESSAGE\033[0m"             #colorisation du code

set -x                #Activation du débogage
set +x                #Désactivation du débogage
test OPTION FICHIER   #verification
   -s                 #fichier non vide
   -f                 #fichier ordinaire
   -d                 #fichier répertoire
   -e                 #fichier existe

test CHAINE1-NB1 OPTION CHAINE2-NB2   #comparaison
   =                  #chaîne1 identique à chaîne2
   !=                 #chaîne1 différente de chaîne2
   -eq                #nb1 égal à nb2 (equal)
   -ne                #nb1 différent de nb2 (non equal)
   -lt                #nb1 inférieur à nb2 (less than)
   -le                #nb1 inférieur ou égal à nb2 (less or equal)
   -gt                #nb1 supérieur à nb2 (greater than)
   -ge                #nb1 supérieur ou égal à nb2 (greater or equal)

[ -a FICHIER ]                 existe et est un fichier
[ -e FICHIER ]
[ -f FICHIER ]                 existe et est un fichier normal
[ -b BLOCKSPECIALFILE ]        existe et est de type bloc spécial
[ -c CHARACTERSPECIALFILE ]    existe et est de type caractère spécial
[ -d DOSSIER ]                 existe et est un dossier
[ -h LIEN_SYMBOLIQUE ]         existe et est un lien symbolique
[ -l LIEN_SYMBOLIQUE ]
[ -r FICHIER_LISIBLE ]         existe et est lisible par le script
[ -s FICHIER_NON_VIDE ]        existe et possède une taille non nulle
[ -w FICHIER_WRITABLE ]        existe et le script peut y écrire
[ -x EXECUTABLE ]              existe et est exécutable depuis le script
[ -n STRING_NON_VIDE ]
[ -z STRING_VIDE ]

TMOUT=SEC             #en début de script arret dans SEC secondes
read VARIABLE         #entree de données
read -p "Press enter to continue"               #pause
read -n 1 -s -r -p "Press any key to continue"
let "VARIABLE++"      #opérations arithmétiques
$(COMMANDE)           #variable venant d une commande
$VARIABLE             #reformatage la variable
"$VARIABLE"           #conserve les espaces et retour a la ligne
'$VARIABLE'           #interprétation littérale pas de variable
VARIABLE${1:-VALEUR}  #prend valeur par default si arg non defini
VARIABLE${1:MESSAGE DERREUR}   #affiche un message d erreur si arg non defini
VARIABLE1=`<FICHIER1`
###>>>plus rapide que
VARIABLE2=`cat FICHIER2`

sh -n SCRIPT          #vérifie les erreurs de syntaxe sans exécuter
sh -v SCRIPT          #affiche chaque commande avant de l exécuter
sh -nv SCRIPT         #permet une vérification verbeuse de la syntaxe
sh -x SCRIPT          #affiche le résultat abrégée de chaque commande

./SCRIPT.sh ARGUMENT1 ARGUMENT2       #ajouter des aguments
$0                    #appeler le nom du script
$1                    #appeler l agument1
$2                    #appeler l agument2
MINPARAMS=            #declarer un nbr min
${10}                 #appeler l agument sup à 9
$*                    #tous les paramètres
$#                    #nbr de paramètres
${!#}                 #tout dernier paramètres
echo $?               #verification code erreur

LISTE="UN DEUX TROIS"                   #creation d une liste
TAB=( UN DEUX "TROIS" )                 #creation d un tableau
TAB=([0]=UN [1]=DEUX [2]="TROIS")
TAB[0]=ELEMENT1
TAB[1]=ELEMENT2
TAB[2]=ELEMENT3
TAB[${#TAB[*]}]="NOUVEL ELEMENT"        #ajouter un élément
LEN=${#TAB[*]}                          #compter les elements du tableau
echo ${TAB[2]}                          #afficher un élément
echo ${TAB[@]}                          #afficher tous les éléments
for i in ${!TAB[@]}; do COMMANDE done   #utiliser le tableau

for ((i=1;i<=n;i++))                    #boucle entre 1 et n
for i in {1..5} ; do echo $i ; done     #boucle entre 1 et 5
for i in *                              #application a tous
for i in `COMMAND`                      #au resultat de la commande
for FICHIER in $REPERTOIRE/*            #pour les fichiers d un repertoire
done | sort >> "$FICHIER_DE_SORTIE"     #log de boucle

if [ CONDITION1 ]                       #conditionnel
then
  COMMANDE1
  COMMANDE2
  COMMANDE3
elif [ CONDITION2 ]
then
  COMMANDE4
  COMMANDE5
else
  COMMANDE_PAR_DEFAUT
fi

if [ -x "$NOM_FICHIER" ]; then          #conditionnel sur une ligne

for i in '                              #boucle liste
...
...'
do
   COMMAND $i
done

for i in /PATH
do
echo $i
done

for i in "$@"
do
echo $i
done

while FONCTION                          #boucle si vrai
   do
       COMMANDE1 $CONDITION
done

while true                              #boucle infinie
   do
       COMMANDE1
done

while  [ CONDITION != ETAT ]            #boucle si vrai(espaces !)
   do
       COMMANDE1
done

until  [ CONDITION != ETAT ]            #boucle si faux(espaces !)
   do
       COMMANDE1
done

FONCTION()                              #creation de fonction
   {
       COMMANDE1
   }

if [ -n "$1" ]                          #condition dossier
then
  REPERTOIRE=$1     # Si nom du répertoire donné en argument au script...
else
  REPERTOIRE=$PWD   # Sinon, utilise le répertoire courant.
fi

for i in "VAR1 1BIS" "VAR2 2BIS" ...    #boucle avec 2 variables
do
   set -- $i
....COMMAND $1 ; COMMAND $2
done

FONCTION ()                             #appel de fonction
{
  if [ $CONDITION ]
  then
   return 0 # true
  else
   return 1 # false
  fi
}

echo "Voulez faire cela (Y/N)?"         #choix et interaction
read ENTREE
if [$ENTREE = "N"]
  then
    COMMAND
fi

COMMANDE <<EOF                          #document intégré macro
...
EOF

(                                       #sous-shell dédié
VARIABLES
COMMANDE
...
exit
)

FONCTION ()                             #fonction
{
COMMANDE $1
...
}

cat $NOMFICHIER |                       #lire et agir sur les lignes d un fichier
while read LIGNE
do
  ...
done

BOUCLE                                  #gestion de boucles
do
   if CONDITION
   then
       break                            #termine la boucle
###>>>ou
       continue    #provoque un saut jusqu à la prochaine itération de la boucle
   fi
done

case "$VARIABLE" in                     #bloc conditionnel (if/then/else)
"$CONDITION1" ) COMMANDE... ;;
"$CONDITION2" ) COMMANDE... ;;
esac

###>>>si un argument est manquant
E_SANSARGS=65
if [ -z "$1" ]
then
  echo "Usage: `basename $0` [ARGUMENT]"
  exit $E_SANSARGS
fi

(( a = NBR ))                #Initialise une valeur
(( a++ ))                    #Post-incrémente a
(( a-- ))                    #Post-décrémente a
(( ++a ))                    #Pre-incrémente a
(( --a ))                    #Pre-décrémente a

let "TOTAL = A *-+%/ B" ; echo $TOTAL     #entier
let "m += 15"                             #equiv. "m = m + 15"
echo "32.0 + 1.4" | bc                    #flottant
```

**[`^ back to top ^`](#)**

---

## résumé shell

### Variables spéciales du shell

```
Variable   Signification
$0         Nom du script
$1         Paramètre de position #1
$2 - $9    Paramètres de position #2 - #9
${10}      Paramètre de position #10
$#         Nombre de paramètres de position
"$*"       Tous les paramètres de position (en un seul mot)
"$@"       Tous les paramètres de position (en des chaînes séparées)
${#*}      Nombre de paramètres de position
${#@}      Nombre de paramètres de position
$?         Code de retour
$$         Numéro d'identifiant du processus (PID) généré par le script
$-         Options passées au script (utilisant set)
$_         Dernier argument de la commande précédente
$!         Identifiant du processus (PID) du dernier job exécuté en tâche de fond
```

### Opérateurs de test : comparaison binaire

```
Opérateur     Signification     -----     Opérateur    Signification
Comparaison arithmétique                  Comparaison de chaînes
-eq           Égal à                      =            Égal à
                                          ==           Égal à
-ne           Différent de                !=           Différent de
-lt           Plus petit que              \<           Plus petit que (ASCII)
-le           Plus petit que ou égal à
-gt           Plus grand que              \>           Plus grand que (ASCII)
-ge           Plus grand que ou égal à
                                          -z           Chaîne vide
                                          -n           Chaîne non vide
Comparaison arithmétique (( ... ))
>             Plus grand que
>=            Plus grand que ou égal à
<             Plus petit que
<=            Plus petit que ou égal à
```

### Opérateurs de test : fichiers

```
Opérateur     Tests si                            -----    Opérateur     Tests si
-e   Le fichier existe                                         -s     Le fichier est vide
-f   Le fichier est un fichier standard
-d   Le fichier est un répertoire                              -r     Le fichier a un droit de lecture
-h   Le fichier est un lien symbolique                         -w     Le fichier a un droit en écriture
-L   Le fichier est un lien symbolique                         -x     Le fichier a le droit d exécution
-b   Le fichier est un périphérique bloc
-c   Le fichier est un périphérique caractère                  -g     L option sgid est positionnée
-p   Le fichier est un tube                                    -u     L option suid est positionnée
-S   Le fichier est un socket                                  -k     L option sticky bit est positionnée
-t   Le fichier est associé à un terminal
-N   Le fichier a été modifié depuis sa dernière lecture       F1 -nt F2     Le fichier F1 est plus récent que F2
-O   Vous êtes le propriétaire du fichier                      F1 -ot F2     Le fichier F1 est plus ancien que F2
-G   L identifiant du groupe du fichier est le même que vous   F1 -ef F2     Les fichiers F1 et F2 sont des liens vers le même fichier
!    NOT (inverse le résultat des tests ci-dessus)
```

### Substitution et expansion de paramètres

```
Expression        Signification
${var}            Valeur de var (identique à $var)
${var-DEFAUT}     Si var n est pas initialisé, évalue l expression DEFAUT
${var:-DEFAUT}    Si var n est pas initialisé ou est vide, évalue l expression DEFAUT
${var=DEFAUT}     Si var n est pas initialisé, évalue l expression DEFAUT
${var:=DEFAUT}    Si var n est pas initialisé, évalue l expression DEFAUT
${var+AUTRE}      Si var est initialisé, évalue l expression AUTRE, sinon est une chaîne null
${var:+AUTRE}     Si var est initialisé, évalue l expression AUTRE, sinon est une chaîne null
${var?ERR_MSG}    Si var n est pas initialisé, affiche ERR_MSG et interrompt l exécution avec un code de sortie de 1
${var:?ERR_MSG}   Si var n est pas initialisé, affiche ERR_MSG et interrompt l exécution avec un code de sortie de 1
${!varprefix*}    Correspond à toutes les variables déclarées précédemment et commençant par varprefix
${!varprefix@}    Correspond à toutes les variables déclarées précédemment et commençant par varprefix
```

### Opérations sur les chaînes

```
Expression                                   Signification
${#chaine}                                   Longueur de $chaine
${chaine:position}                           Extrait la sous-chaîne à partir de $chaine jusqu à $position
${chaine:position:longueur}                  Extrait $longueur caractères dans la sous-chaîne à partir de $chaine jusqu à $position
${chaine#sous-chaine}                        Supprime la plus petite correspondance de $sous-chaine à partir du début de $chaine
${chaine##sous-chaine}                       Supprime la plus grande correspondance de $sous-chaine à partir du début de $chaine
${chaine%sous-chaine}                        Supprime la plus courte correspondance de $sous-chaine à partir de la fin de $chaine
${chaine%%sous-chaine}                       Supprime la plus longue correspondance de $sous-chaine à partir de la fin de $chaine
${chaine/sous-chaine/remplacement}           Remplace la première correspondance de $sous-chaine avec $remplacement
${chaine//sous-chaine/remplacement}          Remplace toutes les correspondances de $sous-chaine avec $remplacement
${chaine/#sous-chaine/remplacement}          Si $sous-chaine correspond au début de $chaine, substitue $remplacement par $sous-chaine
${chaine/%sous-chaine/remplacement}          Si $sous-chaine correspond à la fin de $chaine, substitue $remplacement par $sous-chaine
expr match "$chaine" '$sous-chaine'          Longueur de $sous-chaine* correspondant au début de $chaine
expr "$chaine" : '$sous-chaine'              Longueur de $sous-chaine* correspondant au début de $chaine
expr index "$chaine" $sous-chaine            Position numérique dans $chaine du premier caractère correspondant dans $sous-chaine
expr substr $chaine $position $longueur      Extrait $longueur caractères à partir de $chaine commençant à $position
expr match "$chaine" '\($sous-chaine\)'      Extrait $sous-chaine*, en cherchant à partir du début de $chaine
expr "$chaine" : '\($sous-chaine\)'          Extrait $sous-chaine* en cherchant à partir du début de $chaine
expr match "$chaine" '.*\($sous-chaine\)'    Extrait $sous-chaine* en cherchant à partir de la fin de $chaine
expr "$chaine" : '.*\($sous-chaine\)'        Extrait $sous-chaine* en cherchant à partir de la fin de $chaine
```

### Syntaxes diverses

```
Expression                        Interprétation
Crochets
if [ CONDITION ]                  Construction de tests
if [[ CONDITION ]]                Construction de tests étendue
Tableau[1]=element1               Initialisation d un tableau
[a-z]                             Suite de caractères au sein d une Regex
Accolades
${VARIABLE}                       Substitution de paramètres
${!VARIABLE}                      Référence de variable indirecte
{ COMMANDE1; ... COMMANDEN; }     Bloc de code
{chaine1,string2,string3,...}     Expansion
{a..z}                            Expansion d accolades
{}                                Remplacement de texte, après find et xargs
Parenthèses
( COMMANDE1; COMMANDE2 )          Groupe de commandes exécutées dans un sous-shell
Tableau=(element1 element2)       Initialisation d un tableau
result=$(COMMANDE)                Substitution de commande, nouvelle manière
>(COMMANDE)                       Substitution de processus
<(COMMANDE)                       Substitution de processus
Double parenthèses
(( var = 78 ))                    Arithmétique sur des entiers
var=$(( 20 + 5 ))                 Arithmétique sur des entiers, avec affectation de variables
(( var++ ))                       Incrément de variables style C
(( var-- ))                       Incrément de variables style C
(( var0 = var1<98?9:21 ))         Opération à trois arguments
Guillemets
"$VARIABLE"                       Guillemets "faibles"
'chaine'                          Guillements 'forts'
Guillemets inverses
result=`COMMANDE`                 Substitution de commande, manière habituelle
```

```
###>>>PLUS HAUTE PRECEDENCE
var++ var--                   #post-incrément, post-décrément                         #Opérateurs style C
++var --var                   #pre-incrément, pre-décrément
! ~                           #négation                                               #inverse le sens de l opérateur qui suit
**                            #exposant                                               #opération arithmétique
* / %                         #multiplication, division, modulo                       #opération arithmétique
+ -                           #addition, soustraction                                 #opération arithmétique
<<i i>>                       #décalage à gauche et à droite                          #bit
-z -n                         #comparaison unitaire                                   #chaîne est/n est pas null
-e -f -t -x, etc.             #comparaison unitaire                                   #fichier test
< -lt > -gt <= -le >= -ge     #comparaison composée                                   #string and integer
-nt -ot -ef                   #comparaison composée                                   #fichier test
== -eq != -ne                 #égalité / différence                                   #opérateurs de test, chaîne et entier
&                             #AND                                                    #bit
^                             #XOR                                                    #OU exclusif, bit
|                             #OU                                                     #bit
&& -a                         #ET                                                     #logique, comparaison composée
|| -o                         #OR                                                     #logique, comparaison composée
?:                            #opérateur ternaire                                     #style C
=                             #affectation   affectation                              #égal + multiplication, égal + division, égal + modulo, etc.
,                             #virgule                                                #lie un ensemble d opérations
###>>>PLUS BASSE PRECEDENCE
```

**[`^ back to top ^`](#)**

---

## PS1

```
export PS1="\\[\e[1;31m\]\$\[\e[1;31m\]>\[\e[m\]\A\[\e[34m\][\[\e[m\]\u\[\e[34m\]@\[\e[m\]\h\[\e[34m\]]\[\e[m\]\[\e[1;31m\]-\[\e[m\]\[\e[34m\][\[\e[m\]\W\[\e[34m\]]\[\e[m\]\[\e[1;31m\]:\[\e[m\] "

\u : nom de l utilisateur
\h : nom de la machine
\W : nom du dossier courant
\$ : affiche $ pour un utilisateur et # pour root
\w : chemin complet du répertoire de travail
\d : date format texte ("sam. janv. 31")
\A : heure format 24h sans secondes
\t : heure format 24h avec les secondes
\T : heure format 12h avec les secondes
\@ : heure format 12H sans secondes
\D{%d-%m-%Y %H:%M:%S%z} : Date et heure
\j : nombre de tache en cours dans le terminal
\# : le numéro de la commande
\v : version de bash
\n : nouvelle ligne

# Formatting
Bold='\e[1m'
Dim='\e[2m'
Underlined='\e[4m'
Blink='\e[5m'
Reverse='\e[7m'
Hidden='\e[8m'
Reset='\e[0m'
Combined='\e[1;4;34m'

# Foreground text
Default='\e[39m'
Black='\e[30m'
Red='\e[31m'
Green='\e[32m'
Yellow='\e[33m'
Blue='\e[34m'
Magenta='\e[35m'
Cyan='\e[36m'
Light_gray='\e[37m'
Dark_gray='\e[90m'
Light_red='\e[91m'
Light_green='\e[92m'
Light_yellow='\e[93m'
Light_blue='\e[94m'
Light_magenta='\e[95m'
Light_cyan='\e[96m'
White='\e[97m'

# Background
B_Default='\e[49m'
B_Black='\e[40m'
B_Red='\e[41m'
B_Green='\e[42m'
B_Yellow='\e[43m'
B_Blue='\e[44m'
B_Magenta='\e[45m'
B_Cyan='\e[46m'
B_Light_gray='\e[47m'
B_Dark_gray='\e[100m'
B_Light_red='\e[101m'
B_Light_green='\e[102m'
B_Light_yellow='\e[103m'
B_Light_blue='\e[104m'
B_Light_magenta='\e[105m'
B_Light_cyan='\e[106m'
B_White='\e[107m'

# Reset
Color_Off='\033[0m'       # Text Reset

# Regular Colors
Black='\033[0;30m'        # Black
Red='\033[0;31m'          # Red
Green='\033[0;32m'        # Green
Yellow='\033[0;33m'       # Yellow
Blue='\033[0;34m'         # Blue
Purple='\033[0;35m'       # Purple
Cyan='\033[0;36m'         # Cyan
White='\033[0;37m'        # White

# Bold
BBlack='\033[1;30m'       # Black
BRed='\033[1;31m'         # Red
BGreen='\033[1;32m'       # Green
BYellow='\033[1;33m'      # Yellow
BBlue='\033[1;34m'        # Blue
BPurple='\033[1;35m'      # Purple
BCyan='\033[1;36m'        # Cyan
BWhite='\033[1;37m'       # White

# Underline
UBlack='\033[4;30m'       # Black
URed='\033[4;31m'         # Red
UGreen='\033[4;32m'       # Green
UYellow='\033[4;33m'      # Yellow
UBlue='\033[4;34m'        # Blue
UPurple='\033[4;35m'      # Purple
UCyan='\033[4;36m'        # Cyan
UWhite='\033[4;37m'       # White

# Background
On_Black='\033[40m'       # Black
On_Red='\033[41m'         # Red
On_Green='\033[42m'       # Green
On_Yellow='\033[43m'      # Yellow
On_Blue='\033[44m'        # Blue
On_Purple='\033[45m'      # Purple
On_Cyan='\033[46m'        # Cyan
On_White='\033[47m'       # White

# High Intensity
IBlack='\033[0;90m'       # Black
IRed='\033[0;91m'         # Red
IGreen='\033[0;92m'       # Green
IYellow='\033[0;93m'      # Yellow
IBlue='\033[0;94m'        # Blue
IPurple='\033[0;95m'      # Purple
ICyan='\033[0;96m'        # Cyan
IWhite='\033[0;97m'       # White

# Bold High Intensity
BIBlack='\033[1;90m'      # Black
BIRed='\033[1;91m'        # Red
BIGreen='\033[1;92m'      # Green
BIYellow='\033[1;93m'     # Yellow
BIBlue='\033[1;94m'       # Blue
BIPurple='\033[1;95m'     # Purple
BICyan='\033[1;96m'       # Cyan
BIWhite='\033[1;97m'      # White

# High Intensity backgrounds
On_IBlack='\033[0;100m'   # Black
On_IRed='\033[0;101m'     # Red
On_IGreen='\033[0;102m'   # Green
On_IYellow='\033[0;103m'  # Yellow
On_IBlue='\033[0;104m'    # Blue
On_IPurple='\033[0;105m'  # Purple
On_ICyan='\033[0;106m'    # Cyan
On_IWhite='\033[0;107m'   # White
```

**[`^ back to top ^`](#)**

---

## export LESS_TERMCAP_XX

```
termcap terminfo
ks      smkx      make the keypad send commands
ke      rmkx      make the keypad send digits
vb      flash     emit visual bell
mb      blink     start blink
md      bold      start bold
me      sgr0      turn off bold, blink and underline
so      smso      start standout (reverse video)
se      rmso      stop standout
us      smul      start underline
ue      rmul      stop underline
```

**[`^ back to top ^`](#)**
