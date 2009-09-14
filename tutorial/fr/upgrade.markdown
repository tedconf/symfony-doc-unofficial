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

  * Vider le cache de Symfony:

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

 * It is always better to be explicit, rather than implicit (no magic and no
   WTF effect; see the user mailing-list for a lot of complaints on this
   issue)

 * It provides a small speed improvement

How to upgrade?

  * The `common` filter need to be removed from all `filters.yml`
    configuration files (this is automatically done by the
    `project:upgrade1.3` task).

  * You need to add `include_stylesheets()` and `include_javascripts()` calls
    in your layout(s) to have the same behavior as before (this is
    automatically done by the `project:upgrade1.3` task for HTML layouts
    contained in the `templates/` directories of your applications - they must
    have a `<head>` tag though; and you need to manually upgrade any other
    layout, or any page that does not have a layout but still relies on
    JavaScripts files and/or stylesheets).

Tasks
-----

The following task classes have been renamed:

  symfony 1.2               | symfony 1.3
  ------------------------- | --------------
  `sfConfigureDatabaseTask` | `sfDoctrineConfigureDatabaseTask` or `sfPropelConfigureDatabaseTask`
  `sfDoctrineLoadDataTask`  | `sfDoctrineDataLoadTask`
  `sfDoctrineDumpDataTask`  | `sfDoctrineDataDumpTask`
  `sfPropelLoadDataTask`    | `sfPropelDataLoadTask`
  `sfPropelDumpDataTask`    | `sfPropelDataDumpTask`

### Formatters

The `sfFormatter::format()` third argument has been removed.

Escaping
--------

The `esc_js_no_entities()`, refered to by `ESC_JS_NO_ENTITIES` was updated to
correctly handle non-ANSI characters. Before this change only characters with
ANSI value `37` to `177` were not escaped. Now it will only escape backslashes
`\`, quotes `'` & `"` and linebreaks `\n` & `\r`.

Doctrine Integration
--------------------

### Required Doctrine Version

The externals to Doctrine have been updated to use the latest and greatest 
Doctrine 1.2 version. You can read about what is new in Doctrine 1.2 
[here](http://www.doctrine-project.org/upgrade/1_2).

### Admin Generator Delete

The admin generator batch delete was changed to fetch the records and issue the
`delete()` method to each one individually instead of issuing a single DQL query
to delete them all. The reason is so that events for deleting each individual 
record are invoked.

### Override Doctrine Plugin Schema

You can override the model included in a plugins YAML schema simply by defining 
that same model in your local schema.

See: http://trac.symfony-project.org/ticket/6656

With this change you could now create `config/doctrine/sfGuardUser.schema.yml` 
with the following inside.

    sfGuardUser:
      package: sfDoctrineGuardPlugin.lib.model.doctrine
      # ...

You can customize the schema and your local version will be used instead of the
one included in the plugin.

>**NOTE**
>The package option is a feature of Doctrine and is used for the schemas of
>symfony plugins. This does not mean the package feature can be used independently
>to package your models. It must be used directly and only with symfony plugins.

### Query logging

The Doctrine integration logs queries run using `sfEventDispatcher` rather
than by accessing the logger object directly. Additionally, the subject of
these events is either the connection or statement that is running the query.
Logging is done by the new `sfDoctrineConnectionProfiler` class, which can be
accessed via a `sfDoctrineDatabase` object.

Plugins
-------

If you use the `enableAllPluginsExcept()` method to manage enabled plugins in
your `ProjectConfiguration` class, be warned that we now sort the plugins by
name to ensure consistency across different platforms.

Widgets
-------

The `sfWidgetFormInput` class is now abstract. Text input fields are now
created with the `sfWidgetFormInputText` class. This change was made to ease
introspection of form classes.

Mailer
------

Symfony 1.3 has a new mailer factory. When creating a new application, the
`factories.yml` has sensible defaults for the `test` and `dev` environments.
But if you upgrade an existing project, you might want to update your
`factories.yml` with the following configuration for these environments:

    [yml]
    mailer:
      param:
        delivery_strategy: none

With the previous configuration, emails won't be sent. Of course, they will
still be logged, and the `mailer` tester will still work in your functional
tests.

If you'd rather want to receive all emails to a single address, you can use
the `single_address` delivery strategy (in the `dev` environment for
instance):

    [yml]
    dev:
      mailer:
        param:
          delivery_strategy: single_address
          delivery_address:  foo@example.com

YAML
----

sfYAML is now more compatible with the 1.2 spec. Here are the changes you
might need to do in your configuration files:

 * Booleans can now only be represented with the `true` or `false` strings. If
   you used the alternative strings in the following list, you must replace
   them with either `true` or `false`:

    * `on`, `y`, `yes`, `+`
    * `off`, `n`, `no`, `-`

The `project:upgrade` task tells you where you use old syntax but does not fix
them (to avoid loosing comments for instance). You must fix them by hand.

If you don't want to check all your YAML files, you can force the YAML parser
to use the 1.1 YAML specification by using the `sfYaml::setSpecVersion()`
method:

    [php]
    sfYaml::setSpecVersion('1.1');
