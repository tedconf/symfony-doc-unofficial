Mise à Jour de Projets 1.2 vers 1.3
==================================

Ce document décrit les changements réalisés dans Symfony 1.3 et ce 
qu'il faut accomplir pour mettre à jour vos projets Symfony 1.2.

Si vous souhaitez davantage d'informations concernant les ajouts et 
changements de Symfony 1.3, vous pouvez consulter le tutoriel 
[What's new?](http://www.symfony-project.org/tutorial/1_3/whats-new).

>**CAUTION**
>Symfony 1.3 est compatible avec PHP 5.2.4 ou ultérieurs.
>Il devrait aussi fonctionner pour des versions comprises entre PHP 5.2.0 et 5.2.3 
>mais ce n'est pas garanti.

Comment mettre à jour ?
-----------------------

Pour mettre à jour un projet:

  * Vérifiez que tous les plugins utilisés par votre projet sont compatible
    avec Symfony 1.3.

  * Si vous n'utilisez pas d'outil de SCM, veillez à faire une sauvegarde 
    complète de votre projet.

  * Démarrez le processus de mise à jour vers Symfony 1.3

  * Actualisez vos plugins vers leur version 1.3

  * Lancez la tâche `project:upgrade1.3` depuis le répertoire de votre projet 
    pour réaliser une mise à jour automatique:

        $ php symfony project:upgrade1.3

    Cette tâche peut être exécutée plusieurs fois sans risque d'effets de bord. 
    Elle doit être exécutée chaque fois qu'une mise à jour vers une nouvelle 
    version de Symfony 1.3 beta / RC ou bien finale est réalisé.

  * Cette mise à jour implique de reconstruire vos classes de modèles et de formulaires 
    suite à quelques changements décrits plus bas:

        $ php symfony doctrine:build-model
        $ php symfony doctrine:build-forms
        $ php symfony doctrine:build-filters

  * Videz le cache de Symfony:

        $ php symfony cc

Les prochaines sections expliquent les principaux changements réalisés dans Symfony 1.3 
qui nécessitent une mise à jour automatique ou manuelle.

Composants obsolètes
--------------------

Au cours du développement de Symfony 1.3, nous avons rendu obsolètes et supprimé 
quelques paramètres de configurations, classes, méthodes, fonctions et tâches. Nous 
vous invitons à vous référer au document DEPRECATED_IN_1_3 pour plus 
d'informations.

Autochargement des classes
--------------------------

Depuis Symfony 1.3, les fichiers se trouvant sous le répertoire `lib/vendor/` 
ne sont plus chargés automatiquement par défaut. Si vous souhaitez charger 
automatiquement certains sous-répertoires de `lib/vendor/`, alors ajoutez 
une nouvelle entrée dans le fichier de configuration `autoload.yml` de 
l'application.

    [yml]
    autoload:
      vendor_some_lib:
        name:      vendor_some_lib
        path:      %SF_LIB_DIR%/vendor/some_lib_dir
        recursive: on

Le chargement automatique du répertoire `lib/vendor` était problématique pour 
plusieurs raisons:

  * Si vous déposez une nouvelle librairie PHP qui dispose déjà de son propre 
    système d'autochargement des classes dans le répertoire `lib/vendor`, alors 
    Symfony réanalysera tous les fichiers et ajoutera un surplus d'informations 
    inutiles dans le cache (see #5893 - http://trac.symfony-project.org/ticket/5893).

  * Si le framework Symfony ne se trouve pas exactement dans le répertoire 
    `lib/vendor/symfony/`, le mécanisme d'autochargement des classes du projet 
    réanalysera le répertoire entier de Symfony faisant ainsi survenir de nouveaux 
    problèmes (see #6064 - http://trac.symfony-project.org/ticket/6064).

L'autochargement des classes de Symfony 1.3 est désormais insensible à la casse.

Le Routing
----------

Les méthodes `sfPatternRouting::setRoutes()`, `sfPatternRouting::prependRoutes()`,
`sfPatternRouting::insertRouteBefore()`, et `sfPatternRouting::connect()` ne 
retournent désormais plus les routes sous forme de tableaux comme c'était le 
cas précédemment.

L'option `lazy_routes_deserialize` a été supprimée puisqu'elle n'est plus 
nécessaire à présent.

Depuis Symfony 1.3, le cache pour le routing est désactivé dans la mesure où il 
s'agit de la meilleure otion pour la plupart des projets en ce qui concerne les 
performances. Ainsi, si vous n'avez pas personnalisé le cache du routing, il sera 
automatiquement désactivé pour toutes vos applications. Si, après une mise à jour vers 
Symfony 1.3, votre projet semble plus lent, vous pourrez alors envisager d'ajouter 
du cache au routing afin de vérifier si cela améliore les performances. Le code 
suivant correspond à la configuration par défaut de Symfony 1.2 que vous pouvez 
récupérer et restituer dans le fichier `factories.yml` de votre projet:

    [yml]
    routing:
      cache:
        class: sfFileCache
        param:
          automatic_cleaning_factor: 0
          cache_dir:                 %SF_CONFIG_CACHE_DIR%/routing
          lifetime:                  31556926
          prefix:                    %SF_APP_DIR%/routing

JavaScripts et Feuilles de Styles
---------------------------------

### Suppression des filtres communs

Le filtre `sfCommonFilter` a été supprimé. Il était utilisé pour injecter des 
balises de code JavaScripts et des feuilles de styles automatiquement dans le 
contenu de la réponse. Vous devez désormais inclure manuellement ces ressources 
en appelant explicitement les helpers `include_stylesheets()` et `include_javascripts()` 
dans le layout de votre application:

    [php]
    <?php include_javascripts() ?>
    <?php include_stylesheets() ?>

Ce filtre a été supprimé pour plusieurs raisons:

 * Nous avons maintenant une meilleure solution, plus simple et plus flexible 
   grâce aux helpers `include_stylesheets()` et `include_javascripts()`.

 * Bien que le filtre puisse être facilement désactivé, il n'en démeure pas moins 
   qu'il s'agit d'une tâche plus complexe dans la mesure où vous devez connaître 
   son existence et surtout comment il fonctionne "magicallement" en coulisses.
 
 * Using the helpers provides more fined-grained control over when and where
   the assets are included in the layout (the stylesheets in the `head` tag,
   and the JavaScripts just before the end of the `body` tag for instance)

 * Il est toujours préférable d'être explicite plutôt qu'implicite. Prenez le 
   temps de lire la mailing-list des utilisateurs afin de découvrir de nombreuses 
   plaintes au sujet de la magie et des comportements extravagants.

 * Cela apporte une légère amélioration des performances.

Comment mettre à jour ?

  * Le filtre `common` a besoin d'être supprimé de tous les fichiers 
    de configuration `filters.yml`. Ceci est automatiquement réalisé
    par la tâche `project:upgrade1.3`.

  * Vous avez besoin d'ajouter les appels aux helpers `include_stylesheets()` et
    `include_javascripts()` dans vos layouts afin d'obtenir le même comportement 
    qu'avant. Ceci est automatiquement pris en charge par la tâche `project:upgrade1.3` 
    pour tous les layouts HTML situés dans le répertoire `templates/` de chaque 
    application, à condition que ces derniers disposent d'un tag <head>. Tous les autres 
	  layouts ou pages qui nécessitent des fichiers JavaScripts ou des feuilles de styles 
	  doivent être mis à jour manuellement.

Tâches automatiques
-------------------

Les classes de tâches automatiques suivantes ont été renommées:

  symfony 1.2               | symfony 1.3
  ------------------------- | --------------
  `sfConfigureDatabaseTask` | `sfDoctrineConfigureDatabaseTask` or `sfPropelConfigureDatabaseTask`
  `sfDoctrineLoadDataTask`  | `sfDoctrineDataLoadTask`
  `sfDoctrineDumpDataTask`  | `sfDoctrineDataDumpTask`
  `sfPropelLoadDataTask`    | `sfPropelDataLoadTask`
  `sfPropelDumpDataTask`    | `sfPropelDataDumpTask`

### Les Formatters

Le troisième argument de la méthode `sfFormatter::format()` a été supprimé.

Echappement des données
-----------------------

Le helper `esc_js_no_entities()` relatif à la constante `ESC_JS_NO_ENTITIES` 
a été mis à jour afin de prendre en charge les caractères non ANSI. Avant ce 
changement, tous les caractères dont la valeur ANSI est comprise entre `37` et 
`117` n'étaient pas échappés. Désormais, le caractère `\`, les apostrophes et 
guillemets (`'` & `"`) ainsi que les retours à la ligne (`\n` & `\r`) seront 
échappés.

Intégration de l'ORM Doctrine
-----------------------------

### Version minimale de Doctrine requise

Le lien vers le dépôt externe de Doctrine a été mis à jour afin d'utiliser 
la toute dernière et incroyable version 1.2 de Doctrine. Vous pouvez aussi 
consulter les dernières nouveautés de Doctrine 1.2 
[here](http://www.doctrine-project.org/upgrade/1_2).

### Suppression en Masse dans le Génération d'Administration

La suppression en masse du générateur d'administration a été modifié afin de 
récupérer les enregistrements, puis d'appliquer la méthode `delete()` sur 
chaque objet au lieu de tous les supprimer avec une seule requête DQL. Ainsi, 
les évènements rattachés aux objets seront invoqués au moment de leur 
suppression.

### Rédéfinition des Schémas de Données des Plugins

Vous pouvez désormais surcharger le model inclus dans le schéma de données YAML 
d'un plugin en définissant simplement ce même modèle dans votre schéma local.

Voir: http://trac.symfony-project.org/ticket/6656

Grâce à ce changement, vous pouvez maintenant créer le schéma suivant dans le 
fichier `config/doctrine/sfGuardUser.schema.yml`.

    sfGuardUser:
      package: sfDoctrineGuardPlugin.lib.model.doctrine
      # ...

Vous pouvez personnaliser le schéma à votre guise car ce sera désormais la version 
locale qui sera utilisée à la génération du modèle au lieu de celle livrée avec le 
plugin.

>**NOTE**
>L'option `package` est une nouveauté de Doctrine et utilisée pour les schémas des 
>plugins Symfony. Cela ne veut pas dire que l'option `package` peut être utilisée 
>indépendamment pour paqueter vos modèles. Elle doit être utilisée directement et 
>uniquement avec les plugins Symfony.

### Enregistrement des Requêtes SQL

Doctrine enregistre les requêtes SQL exécutées à l'aide du dispatcheur 
d'évènement `sfEventDispatcher` au lieu d'accéder directement à l'objet 
logger correspondant. De plus, le sujet de ces évènements est soit la 
connexion ou bien l'objet (statement) qui exécute la requête. L'enregistrement 
est délégué à la nouvelle classe `sfDoctrineConnectionProfiler`, qui peut 
être atteinte depuis un objet `sfDoctrineDatabase`.

Les Plugins
-----------

Si avez recours à la méthode `enableAllPluginsExcept()` de votre classe 
`ProjectConfiguration`pour gérer les plugins activés, alors gardez à l'esprit 
que tous les plugins sont désormais triés par nom afin d'assurer une cohérence
à travers les différentes plate-formes.

Les Widgets
-----------

La classe `sfWidgetFormInput` est maintenant déclarée abstraite. Les champs de 
type texte sont désormais créés à partir de la classe `sfWidgetFormInputText`. 
Ce changement a été introduit afin de faciliter l'introspection des classes de 
formulaire.

Le Gestionnaire d'Envoi d'E-Mail
--------------------------------

Symfony 1.3 dispose à présent d'un tout nouveau mécanisme d'envoi d'emails. 
Lorsqu'une nouvelle application est créée, le fichier `factories.yml` se dote 
de nouveaux paramètres par défaut pour les environnements `dev` et `test`. Mais 
si vous mettez à jour un projet existant, vous voudrez peut-être mettre à jour 
votre fichier `factories.yml` avec la configuration suivante pour ces environnements:

    [yml]
    mailer:
      param:
        delivery_strategy: none

Avec la configuration précédente, les emails ne seront pas envoyés. Bien entendu, 
ils resteront enregistrés, et le testeur `mailer` continuera de fonctionner dans 
vos tests fonctionnels.

Si vous souhaitez plutôt recevoir tous vos les emails à une même adresse, vous 
pouvez spécifier la valeur `single_address` en guise de stratégie d'envoi des 
emails (pour l`environnements `dev` par exemple):

    [yml]
    dev:
      mailer:
        param:
          delivery_strategy: single_address
          delivery_address:  foo@example.com

YAML
----

sfYAML est désormais davantage compatible avec les spécifications YAML 1.2. 
La partie suivante liste les changements que vous devrez opérer dans vos 
fichiers de configuration:

 * Les valeurs Booléennes peuvent maintenant être représentées uniquement 
   à partir des chaînes de caractères `true` ou `false`. Si vous utilisiez 
   une ou plusieurs des chaînes alternatives suivantes, vous devrez les 
   remplacer par les valeurs `true` ou `false` correspondantes:

    * `on`, `y`, `yes`, `+`
    * `off`, `n`, `no`, `-`

La tâche automatique `project:upgrade` vous informe des endroits où vous 
utilisez l'ancienne syntaxe mais ne les corrige pas à votre place afin 
d'éviter de perdre des commentaires par exemple. Vous devez les corriger 
vous même manuellement.

Si vous souhaitez vérifier tous vos fichiers YAML, vous pouvez forcer 
l'analyseur syntaxique YAML à utiliser les spécifications YAML 1.1 en 
utilisant la méthode `sfYaml::setSpecVersion()`:

    [php]
    sfYaml::setSpecVersion('1.1');
