_____________________________________________________________________________________
interaction entre les commandes
-------------------------------------------------------------------------------------
```bash
command &             #commande en arriere plan
nohup commande        #commande en arriere plan et hors console
bg commande           #commande en arriere plan
fg commande           #Reprend la main
command && command    #execute si la premiere est un success
command ; command     #execute toujours la seconde commande
command || command    #execute si la premiere est un echec
command !(*.file)     #execute la commande sauf pour les file
command | command     #la seconde commande execute le resultat de la premiere
(Command_x1 && Command_x2) || (Command_x3 && Command_x4)   #separe les commandes
test\(1\).txt         #pour les caracteres speciaux
command >> file.log   #rediriger les resultats a la fin d un fichier
command 2>> file.log  #rediriger les erreurs a la fin d un fichier
command >> file.log 2>&1   #rediriger les resultats et les erreurs
commande > file.log 2>&1   #rediriger à la fois la sortie standard et l erreur standard
commande < file.log        #rediriger l entrée standard de la commande vers le fichier
commande << délimiteur                                                                #rediriger l entrée standard de la commande vers les lignes suivantes jusqu’à ce que le délimiteur soit rencontré
command && echo "tout va bien" || echo "ca rate"       #exemples
command | command -   #- recupere la sortie de commande
variable=$(command)   #commande dans une variable
(regex)(regex)        #peut etre appeler avec $1,$2...
sudo !!               #exectue la derniere commande en root
xterm -geometry 170x60 -sb -rightbar -hold -e /home/scripts.sh                        #script auto dans fenetre pour logs
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
```

_____________________________________________________________________________________
shell
-------------------------------------------------------------------------------------
```bash
#! /bin/bash          #en debut de script
0                     #TRUE
1                     #FALSE
exit $?               #en fin de script
/usr/local/bin        #emplacement des scripts personnels
caller 0              #débogage si dans une fonction
echo -e '\E[37;44m'"\033[1mMESSAGE\033[0m"             #colorisation du code

set -x                #Activation du débogage
set +x                #Désactivation du débogage
test option fichier   #verification
   -s                 #fichier non vide
   -f                 #fichier ordinaire
   -d                 #fichier répertoire
   -e                 #fichier existe

test chaîne1-nb1 option chaîne2-nb2   #comparaison
   =                  #chaîne1 identique à chaîne2
   !=                 #chaîne1 différente de chaîne2
   -eq                #nb1 égal à nb2 (equal)
   -ne                #nb1 différent de nb2 (non equal)
   -lt                #nb1 inférieur à nb2 (less than)
   -le                #nb1 inférieur ou égal à nb2 (less or equal)
   -gt                #nb1 supérieur à nb2 (greater than)
   -ge                #nb1 supérieur ou égal à nb2 (greater or equal)

TMOUT=SEC             #en début de script arret dans SEC secondes
read variable         #entree de données
let "variable++"      #opérations arithmétiques
$(commande)           #variable venant d une commande
$variable             #reformatage la variable
"$variable"           #conserve les espaces et retour a la ligne
'$variable'           #interprétation littérale pas de variable
variable1=`<fichier1`
###>>>plus rapide que
variable2=`cat fichier2`

sh -n nomscript       #vérifie les erreurs de syntaxe sans exécuter
sh -v nomscript       #affiche chaque commande avant de l exécuter
sh -nv nomscript      #permet une vérification verbeuse de la syntaxe
sh -x nomscript       #affiche le résultat abrégée de chaque commande

./script.sh argument1 argument2       #ajouter des aguments
$0                    #appeler le nom du script
$1                    #appeler l agument1
$2                    #appeler l agument2
MINPARAMS=            #declarer un nbr min
${10}                 #appeler l agument sup à 9
$*                    #tous les paramètres
$#                    #nbr de paramètres
${!#}                 #tout dernier paramètres
echo $?               #verification code erreur

liste="un deux trois"                   #creation d une liste
tab=( un deux "trois" )                 #creation d un tableau
tab=([0]=un [1]=deux [2]="trois")
tab[0]=element1
tab[1]=element2
tab[2]=element3
tab[${#tab[*]}]="nouvel élément"        #ajouter un élément
len=${#tab[*]}                          #compter les elements du tableau
echo ${tab[2]}                          #afficher un élément
echo ${tab[@]}                          #afficher tous les éléments
for i in ${!tab[@]}; do commande done   #utiliser le tableau

for ((i=1;i<=n;i++))                    #boucle entre 1 et n
for i in {1..5} ; do echo $i ; done     #boucle entre 1 et 5
for i in *                              #application a tous
for i in `command`                      #au resultat de la commande
for fichier in $repertoire/*            #pour les fichiers d un repertoire
done | sort >> "$FICHIER_DE_SORTIE"     #log de boucle

if [ condition1 ]                       #conditionnel
then
  commande1
  commande2
  commande3
elif [ condition2 ]
then
  commande4
  commande5
else
  commande_par_defaut
fi

if [ -x "$nom_fichier" ]; then          #conditionnel sur une ligne

for i in '                              #boucle liste
...
...'
do
   command $i
done

while fonction                          #boucle si vrai
   do
       commande1 $condition
done

while true                              #boucle infinie
   do
       commande1
done

while  [ condition != etat ]            #boucle si vrai(espaces !)
   do
       commande1
done

until  [ condition != etat ]            #boucle si faux(espaces !)
   do
       commande1
done

nom_de_fonction()                       #creation de fonction
   {
       commande1
   }

if [ -n "$1" ]                          #condition dossier
then
  repertoire=$1     # Si nom du répertoire donné en argument au script...
else
  repertoire=$PWD   # Sinon, utilise le répertoire courant.
fi

for i in "var1 1bis" "var2 2bis" ...    #boucle avec 2 variables
do
   set -- $i
....command $1 ; command $2
done

fonction ()                             #appel de fonction
{
  if [ $condition ]
  then
   return 0 # true
  else
   return 1 # false
  fi
}

COMMANDE <<EOF                          #document intégré macro
...
EOF

(                                       #sous-shell dédié
VARIABLES
COMMANDE
...
exit
)

nom_fonction ()                         #fonction
{
commande $1
...
}

cat $nomfichier |                       #lire et agir sur les lignes d un fichier
while read ligne
do
  ...
done

boucle                                  #gestion de boucles
do
   if condition
   then
       break                            #termine la boucle
###>>>ou
       continue    #provoque un saut jusqu à la prochaine itération de la boucle
   fi
done

case "$variable" in                     #bloc conditionnel (if/then/else)
"$condition1" ) commande... ;;
"$condition2" ) commande... ;;
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

###>>>PLUS HAUTE PRÉCÉDENCE
var++ var--                   #post-incrément, post-décrément                         #Opérateurs style C
++var --var                   #pre-incrément, pre-décrément
! ~                           #négation                                               #inverse le sens de l opérateur qui suit
**                            #exposant                                               #opération arithmétique
* / %                         #multiplication, division, modulo                       #opération arithmétique
+ -                           #addition, soustraction                                 #opération arithmétique
<< >>                         #décalage à gauche et à droite                          #bit
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
###>>>PLUS BASSE PRÉCÉDENCE
```

_____________________________________________________________________________________
résumé shell
-------------------------------------------------------------------------------------
###Variables spéciales du shell
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

###Opérateurs de test : comparaison binaire
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

###Opérateurs de test : fichiers
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

###Substitution et expansion de paramètres
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

###Opérations sur les chaînes
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

###Syntaxes diverses
```
Expression                        Interprétation
Crochets
if [ CONDITION ]                  Construction de tests
if [[ CONDITION ]]                Construction de tests étendue
Tableau[1]=element1               Initialisation d un tableau
[a-z]                             Suite de caractères au sein d une Regex
Accolades
${variable}                       Substitution de paramètres
${!variable}                      Référence de variable indirecte
{ commande1; ... commandeN; }     Bloc de code
{chaine1,string2,string3,...}     Expansion
{a..z}                            Expansion d accolades
{}                                Remplacement de texte, après find et xargs
Parenthèses
( commande1; commande2 )          Groupe de commandes exécutées dans un sous-shell
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
"$variable"                       Guillemets "faibles"
'chaine'                          Guillements 'forts'
Guillemets inverses
result=`COMMANDE`                 Substitution de commande, manière habituelle
```