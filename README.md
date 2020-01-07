# Dojo-Git


Git Voyager
===========

Ce repository n'est pas une nouvelle mission pour aller sur la lune (quoique), mais comme son nom ne l'indique pas assez, c'est un terrain de jeu afin de mettre en pratique différentes notions de git.

Pour chaque exercice présenté ici, il vous faudra :
 * switcher sur la branche correspondante
 * prendre connaissance des détails de l'exercice
 * Tenter une première résolution sans s'inspirer de la réponse possible
 * valider le résultat via la ligne de commande, ou un contrôle visuel

Pour visualiser les commits git au cours de cet atelier, il peut être utile d'utiliser la commande suivante pour la création d'un alias :
    git config --local alias.lol "log --graph --oneline --all"

Une fois cette commande exécutée, il sera possible de visualiser le graph des commits en lançant la commande :
    git lol

En cas de blocage, il ne faut pas hésiter à solliciter votre voisin ou le mentor Git de service.


4 nuances de rebase interactif(-like)
=====================================

Voyons maintenant comment il est possible de réorganiser des commits en voyant 4 méthodes différentes. 


Rebase-like en mode "copier/coller"
-----------------------------------

C'est peut être la méthode la plus rassurante au début grâce à la réutilisation d'outils tels que la création de branche et le cherry-pick.

**branche** : rebase-like-1

**objcetif**

Nous avons sur cette branche, 5 commits organisés ainsi : 
* Initial - D - E - B - C - A

Le but à atteindre est d'obtenir la chaine de commit suivante :
* Initial - A - B - C - D - E

La méthode appliquée ici peut permettre, par exemple, de recréer une branche à partir d'une branche release alors qu'elle a été faite à partir de develop à l'origine.
Tout dépendra du commit à partir duquel commencera le "copier/coller" aka cherry-pick.

**Méthodes**

Nous allons utiliser 
checkout pour repositionner notre branche sur le commit initial, 
créer une nouvelle branche, 
réaliser des cherry-pick pour reprendre les commits dans l'ordre. 
valider le résultat

git checkout release
git checkout -b rebase-like-1-copy
git lol
  
git cherry-pick rebase-like-1 rebase-like-1~2
git lol
git cherry-pick rebase-like-1~1 rebase-like-1~4 rebase-like-1~3
git lol




Le fameux rebase interactif
---------------------------

**branche** : rebase-like-2

**objectif**

Nous avons sur cette branche, 5 commits organisés ainsi : 
* Initial - D - E - B - C - A

Le but à atteindre est d'obtenir la chaine de commit suivante :
* Initial - A - B - C - D - E

Si le but est simplement de réorganiser l'ordre des commits plus rapidement que la méthode précédente.

**Méthodes**

    git rebase -i HEAD~5

Presque le même résultat, mais avec un `rebase --onto` en plus, on obtient le même résultat.


La fusion de commit avec rebase interactif
------------------------------------------

**branche** : rebase-like-3

**objectif**

Nous avons sur cette branche, 5 commits organisés ainsi : 
* Initial - D - E - B - C - A

Le but à atteindre est d'obtenir la chaine de commit suivante :
* Initial - A - BD - E

On peut également jouer à fusionner/supprimer des commits tout en faisant des réorganisations entre eux. On regroupe ainsi des commits effectués sur un même fichier par exemple.

**Méthodes**


    git rebase -i HEAD~5

Utilisation du mot clé fixup ou squash à l'endroit adéquat (indice, ça remplace un pick)


La méthode du reset
-------------------

**branche** : rebase-like-4

**objectif**

Nous avons sur cette branche, 5 commits organisés ainsi : 
* Initial - D - E - B - C - A

Le but à atteindre est d'obtenir la chaine de commit suivante :
* Initial - ABCDE

S'il ne s'agit que de créer un seul commit à partir de tous les derniers présents sur la branche,on peut aussi utiliser le reset

**Méthodes**




Lost In Git
===========

Git conserve une trace des commits tant qu'il existe une référence vers celui-ci. Il ne faut pas avoir peur de faire des erreurs, car elles sont quasiment toujours récupérables. La commande `git reflog`, par exemple, c'est un peu un GPS qui rappelle par où (quel commit) on est passé.
    git reflog

Pour finir de se convaincre que rien n'est perdu, voici une petite commande dont le résultat peut rappeler des souvenirs :
    git log --graph --oneline --all --reflog

Si l'alias a été ajouté, la commande `git lol --reflog` marche aussi.

Kill the file
=============

Dans cette partie, nous allons voir 2 méthodes pour supprimer un fichier d'un commit.

Le dernier commit
-----------------

**branche** : kill-file-1

**objectif**

Dans le dernier commit, le fichier *kill-file-1* s'est invité. La mission est ici de le supprimer.

**Méthodes**

  git reset --soft HEAD~
  
  git status
  
  git reset HEAD kill-file-1
  
  git status
  
  git commit -m "kill-file-1 not inside"
  


Pas le dernier commit
---------------------

**branche** : kill-file-2

**objectif**

Dans l'avant-dernier commit, le fichier *kill-file-1* s'est invité. La mission est ici de le supprimer.

**Méthodes**

git rebase -i HEAD~2

git status

git lol

git reset HEAD^

git status

git add content

git commit -m "kill-file-2 not inside"

git status

git rebase --continue

git status

