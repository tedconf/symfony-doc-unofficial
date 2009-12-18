Installation du ﻿Projet
=============

Dans symfony, les **applications** partageant le même modèle de données sont regroupés dans des
**projets**. Pour la plupart des projets, vous avez deux applications différentes : un
frontend et un backend.

### Création du projet

Depuis le répertoire `sfproject/`, exécuter la tâche symfony `generate:project` pour
créer le projet symfony:

    $ php lib/vendor/symfony/data/bin/symfony generate:project PROJECT_NAME

Sur Windows:

    c:\> php lib\vendor\symfony\data\bin\symfony generate:project PROJECT_NAME

La tâche `generate:project` génére la structure par défaut des répertoires et
les fichiers nécessaires pour un projet symfony :

 | Répertoire  | Description
 | ----------- | ----------------------------------
 | `apps/`     | Accueille toutes les applications du projet
 | `cache/`    | Les fichiers mis en cache par le framework
 | `config/`   | Les fichiers de configuration du projet
 | `lib/`      | Les bibliothèque et les classes du projet
 | `log/`      | Les fichiers log du framework
 | `plugins/`  | Les plugins installés
 | `test/`     | Les fichiers de test unitaire et fonctionnel
 | `web/`      | Le répertoire racine Web (voir ci-dessous)

>**NOTE**
>Pourquoi symfony génère beaucoup de dossiers ? Un des principaux avantages d'un
>framework full-stack est de normaliser vos développements. Grâce à
>la structure par défaut des fichiers et des répertoires de symfony, tout développeur
>ayant une certaine connaissance de symfony peut prendre en charge la maintenance d'un projet symfony.
>En quelques minutes, il sera capable de plonger dans le code, de corriger des bugs,
>et d'ajouter de nouvelles fonctionnalités.

La tâche `generate:project` a également créé un raccourci `symfony` dans le
répertoire racine du projet pour diminuer le nombre de caractères que vous allez écrire
lors de l'exécution d'une tâche.

Ainsi, à partir de maintenant, au lieu d'utiliser le chemin complet pour le programme
symfony, vous pouvez utiliser le raccourci `symfony`.

### Création de l'application

Maintenant, créez l'application frontend en exécutant la tâche `generate:app` :

    $ php symfony generate:app --escaping-strategy=on
      ➥ --csrf-secret=UniqueSecret frontend

>**TIP**
>Parce que le raccourci symfony est exécutable, les utilisateurs Unix peuvent remplacer toutes
>les occurrences de '`php symfony`' par '`./symfony`' à partir de maintenant.
>
>Sur Windows vous pouvez copier le fichier '`symfony.bat`' vers votre projet et utilisez
>'`symfony`' à la place de '`php symfony`' :
>
>     c:\> copy lib\vendor\symfony\data\bin\symfony.bat .

Basé sur le nom de l'application donné en *argument*, la tâche `generate:app`
crée par défaut la structure du répertoire nécessaire à l'application sous le
répertoire `apps/frontend/` :

 | Répertoire   | Description
 | ------------ | -------------------------------------
 | `config/`    | Les fichiers de configuration de l'application
 | `lib/`       | Les bibliothèques et les classes de l'application
 | `modules/`   | Le code de l'application (MVC)
 | `templates/` | Les fichiers template globaux

>**TIP**
>Lorsque vous appelez la tâche generate:app vous avez aussi passé deux *options* liées à la
>sécurité :
>
>  * `--escaping-strategy`: Active l'output escaping pour  empêcher les attaques XSS 
>  * `--csrf-secret`: Active les jetons de session dans les formulaires pour prévenir des attaques CSRF
>
>En passant ces deux options facultatives à la tâche, vous avez sécurisé votre
>futur développement des deux vulnérabilités les plus répandues trouvées sur le
>web. C'est vrai, symfony prend automatiquement des mesures de sécurité à
>notre place.
>
>Si vous ne savez rien sur
>[XSS](http://en.wikipedia.org/wiki/Cross-site_scripting) ou
>[CSRF](http://en.wikipedia.org/wiki/CSRF), prenez le temps d'en apprendre d'avantage à propos
>de ces failles de sécurité.

### Droits sur les répertoires structurés

Avant d'essayer d'accéder à votre projet nouvellement créé, vous devez configurer l'écriture
sur les répertoires `cache/` et `log/` à des niveaux appropriés,
afin que votre serveur web puisse écrire dedans :

    $ chmod 777 cache/ log/

>**SIDEBAR**
>Conseils pour les personnes utilisant un outil de SCM
>
>symfony écrit seulement dans deux répertoires du projet symfony :
>`cache/` et `log/`. Le contenu de ces répertoires peut-être ignoré
>par votre SCM (En utilisant la propriété `svn:ignore`, si vous utilisez Subversion
>par exemple).

### Le chemin de symfony

Vous pouvez obtenir la version symfony utilisée par votre projet en tapant :

    $ php symfony -V

L'option `-V` affiche également le chemin vers le répertoire d'installation de symfony,
qui est stocké dans `config/ProjectConfiguration.class.php` :

    [php]
    // config/ProjectConfiguration.class.php
    require_once '/Users/fabien/symfony-1.2/lib/autoload/sfCoreAutoload.class.php';

Pour une meilleure portabilité, changez le chemin absolu de l'installation de symfony
par un relatif :

    [php]
    // config/ProjectConfiguration.class.php
    require_once dirname(__FILE__).'/../lib/vendor/symfony/lib/autoload/sfCoreAutoload.class.php';

De cette façon, vous pouvez déplacer le répertoire du projet n'importe où, sur votre machine ou une
autre, et il marchera bien.

### Configuration de la base de données

Une des premières choses que vous pouvez faire est de configurer la connexion à la base de
donnée pour votre projet. Le framework Symfony supporte toutes les
[PDO](http://www.php.net/PDO)-soutenus par des bases de données (MySQL, PostgreSQL, SQLite,
Oracle, MSSQL, ...). Au sommet de PDO, symfony est livré avec deux outils ORM:
Propel et Doctrine. Propel est celui par défaut, mais le passage à Doctrine est
assez facile (voir la section suivante pour plus d'informations).

La configuration de la base de données est trés simple en utilisant la tâche `configure:database` :

    $ php symfony configure:database "mysql:host=localhost;dbname=dbname" root mYsEcret

La tâche `configure:database` comporte 3 arguments: le
[~PDO DSN~](http://www.php.net/manual/en/pdo.drivers.php), le nom de l'utilisateur, et
le mot de passe pour accéder à la base de données. Si vous n'avez pas besoin d'un mot de passe pour
accéder à votre base de donnée sur le serveur de développement, omettez simplement le troisième argument.

### Changer pour Doctrine

Si vous souhaitez utiliser Doctrine au lieu de Propel, vous devez d'abord activer
`sfDoctrinePlugin` et désactiver `sfPropelPlugin`. Cela peut être fait en changeant
le code suivant dans votre `config/ProjectConfiguration.class.php`:

    [php]
    public function setup()
    {
      $this->enableAllPluginsExcept(array('sfPropelPlugin', 'sfCompat10Plugin'));
    }

Après avoir effectué ces modifications, lancez ces commandes :

    $ php symfony plugin:publish-assets
    $ php symfony cc
    $ rm web/sfPropelPlugin
    $ rm config/propel.ini
    $ rm config/schema.yml
    $ rm config/databases.yml

Ensuite, exécutez la commande suivante pour configurer votre base de données pour Doctrine :

    $ php symfony configure:database --name=doctrine --class=sfDoctrineDatabase "mysql:host=localhost;dbname=jobeet" root mYsEcret
