Installation de symfony
====================

### Répertoire du projet

Avant d'installer symfony, vous devez d'abord créer un répertoire qui sera l'hôte
de tous les fichiers liés à votre projet :

    $ mkdir -p /home/sfproject
    $ cd /home/sfproject

Ou sur Windows:

    c:\> mkdir c:\dev\sfproject
    c:\> cd c:\dev\sfproject

>**NOTE**
>Les utilisateurs de Windows sont invités pour exécuter symfony, à mettre en place leur nouveau
>projet dans un chemin qui ne contient pas d'espaces.
>Evitez d'utiliser le répertoire `Documents and Settings`, y compris n'importe où
>sous `Mes Documents`.

-

>**TIP**
>Si vous créez le répertoire du projet symfony sous le répertoire racine
>web, vous n'aurez pas besoin de configurer votre serveur web. Bien sûr, pour
>les environnements de production, nous vous conseillons fortement de configurer votre serveur web,
>comme expliqué dans la section configuration du serveur web.

### Installation de symfony

Créer un répertoire pour accueillir les fichiers de la bibliothèque du framework symfony :

    $ mkdir -p lib/vendor

Maintenant, vous devez installer symfony. Comme le framework symfony dispose de plusieurs versions
stables, vous devez choisir celle que vous souhaitez installer en lisant la
[page d'installation](http://www.symfony-project.org/installation) sur le
site de symfony.

Aller à la page d'installation de la version que vous venez de choisir,
[symfony 1.3](http://www.symfony-project.org/installation/1_3) par exemple.

Sous la section "**Source Download**", vous trouverez l'archive sous le format `.tgz`
ou `.zip`. Téléchargez l'archive, puis mettez-la sous le nouveau répertoire créé
`lib/vendor/` et décompressez la :

    $ cd lib/vendor
    $ tar zxpf symfony-1.3.0.tgz
    $ mv symfony-1.3.0 symfony
    $ rm symfony-1.3.0.tgz

Sous Windows, décompressez le fichier zip en utilisant l'Explorateur Windows.
Après avoir renommé le répertoire en symfony, vous devriez y avoir une structure
de répertoire similaire à `c:\dev\sfproject\lib\vendor\symfony`.

>**TIP**
>Si vous utilisez Subversion, il est même préférable d'utiliser la propriété `svn:externals`
>pour bien intégrer symfony dans votre projet dans le répertoire `lib/vendor/`,
>ceci bénéficie de la correction automatique de bogues fixés dans la version
>stable :
>
>     http://svn.symfony-project.com/branches/1.3/

Vérifiez que symfony est correctement installé en utilisant la ligne de commande de symfony
pour afficher la version de symfony (note le `V` est en majuscule) :

    $ cd ../..
    $ php lib/vendor/symfony/data/bin/symfony -V

Sur Windows:

    c:\> cd ..\..
    c:\> php lib\vendor\symfony\data\bin\symfony -V

>**TIP**
>Si vous êtes curieux de savoir ce que cet outil en ligne de commande peut faire pour vous, tapez
>`symfony` pour lister les options et les tâches disponibles :
>
>     $ php lib/vendor/symfony/data/bin/symfony
>
>Sur Windows:
>
>     c:\> php lib\vendor\symfony\data\bin\symfony
>
>La ligne de commande symfony est le meilleur ami du développeur. Il fournit de nombreux
>utilitaires qui permettent d'améliorer votre productivité sur les activités quotidiennes comme
>le vidage du cache, la génération de code, et bien plus encore.