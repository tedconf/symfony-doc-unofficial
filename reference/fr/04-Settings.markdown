﻿Le fichier de configuration settings.yml
===================================

La plupart des aspects de symfony peuvent être configurés via un fichier de configuration
écrit en YAML, ou avec du simple PHP. Dans cette section, `settings.yml`, le principal fichier
de configuration est décrit.

Le fichier de configuration principal `settings.yml` pour une application peut être trouver dans
le répertoire `apps/APP_NAME/config/`.

Comme indiqué dans l'introduction, le fichier `settings.yml` est
[**sensible à l'environnement**](#chapter_03_sensibilisation_a_l_environnement), et bénéficie du
[**mécanisme de configuration en cascade**](#chapter_03_configuration_en_cascade).

Chaque section d'un environnement comprend deux sous-sections : `.actions` et `.settings`. Toutes
les directives de configuration passe par la sous-section `.settings`, à l'exception des
actions par défaut pour restituer certaines pages communes.

>**NOTE**
>Le fichier de configuration `settings.yml` est mis en cache dans un fichier PHP, le processus est
>automatiquement géré par la [classe](#chapter_14_config_handlers_yml)
>~`sfDefineEnvironmentConfigHandler`~.

<div class="pagebreak"></div>

Paramètres
--------

  * `.actions`

    * [`error_404`](#chapter_04_sub_error_404)
    * [`login`](#chapter_04_sub_login)
    * [`secure`](#chapter_04_sub_secure)
    * [`module_disabled`](#chapter_04_sub_module_disabled)

  * `.settings`

    * [`cache`](#chapter_04_sub_cache)
    * [`charset`](#chapter_04_sub_charset)
    * [`check_lock`](#chapter_04_sub_check_lock)
    * [`check_symfony_version`](#chapter_04_sub_check_symfony_version)
    * [`compressed`](#chapter_04_sub_compressed)
    * [`csrf_secret`](#chapter_04_sub_csrf_secret)
    * [`default_culture`](#chapter_04_sub_default_culture)
    * [`default_timezone`](#chapter_04_sub_default_timezone)
    * [`enabled_modules`](#chapter_04_sub_enabled_modules)
    * [`error_reporting`](#chapter_04_sub_error_reporting)
    * [`escaping_strategy`](#chapter_04_sub_escaping_strategy)
    * [`escaping_method`](#chapter_04_sub_escaping_method)
    * [`etag`](#chapter_04_sub_etag)
    * [`i18n`](#chapter_04_sub_i18n)
    * [`lazy_cache_key`](#chapter_04_sub_lazy_cache_key)
    * [`logging_enabled`](#chapter_04_sub_logging_enabled)
    * [`no_script_name`](#chapter_04_sub_no_script_name)
    * [`max_forwards`](#chapter_04_sub_max_forwards)
    * [`standard_helpers`](#chapter_04_sub_standard_helpers)
    * [`strip_comments`](#chapter_04_sub_strip_comments)
    * [`use_database`](#chapter_04_sub_use_database)
    * [`web_debug`](#chapter_04_sub_web_debug)
    * [`web_debug_web_dir`](#chapter_04_sub_web_debug_web_dir)

<div class="pagebreak"></div>

La sous-section `.actions`
--------------------------

*Configuration par défaut*:

    [yml]
    default:
      .actions:
        error_404_module:       default
        error_404_action:       error404

        login_module:           default
        login_action:           login

        secure_module:          default
        secure_action:          secure

        module_disabled_module: default
        module_disabled_action: disabled

La sous-section `.actions` définit l'action à exécuter lorsqu'une page commune
doit être restituée. Chaque définition comporte deux éléments : l'un pour le module
(suffixé par `_module`) et l'autre pour l'action (suffixé par `_action`).

### ~`error_404`~

L'action `error_404` est exécutée quand une page 404 doit être restituée.

### ~`login`~

L'action `login` est exécutée quand un utilisateur non-identifié essait d'accéder à
une page sécurisée.

### ~`secure`~

L'action `secure` est exécutée quand un utilisateur n'a pas les pouvoirs
requis.

### ~`module_disabled`~

L'action `module_disabled` est exécuté quand l'utilisateur demande un module
désactivé.

La sous-section `.settings`
---------------------------

La sous-section `.settings` est l'endroit où  se réalise la configuration du framework. Les
paragraphes qui suivent décrivent tous les paramètres possibles et sont à peu près classé par ordre
d'importance.

Tous les paramètres définis dans la section `.settings` sont disponibles n'importe où dans le
code en utilisant l'objet `sfConfig` et en préfixant le paramètre avec `sf_`. Par
exemple, pour obtenir la valeur du paramètre `charset`, utilisez :

    [php]
    sfConfig::get('sf_charset');

### ~`escaping_strategy`~

*Par défaut*: `off`

Le paramètre `escaping_strategy` est un paramètre booléen qui détermine si
l'échappement de sortie du sous-framework est activé ou non. Lorsqu'il est activé, toutes les variables
disponibles dans les templates sont automatiquement échappé en appelant le helper
définie par le paramètre `escaping_method` (voir ci-dessous).

Faites bien attention à l'`escaping_method`, l'helper par défaut utilisé par symfony,
mais cela peut être remplacé au cas par cas, par exemple, en sortant une variable
d'une balise d'un script JavaScript.

L'échappement de sortie du sous-framework utilise le paramètre `charset` pour échapper.

Il est fortement recommandé de changer la valeur par défaut à `on`.

>**TIP**
>Ce réglage peut être activé lorsque vous créez une application avec la tâche
>`generate:app` en utilisant l'option `--escaping-strategy`.

### ~`escaping_method`~

*Par défaut*: `ESC_SPECIALCHARS`

L'`escaping_method` definit la function par défaut utilisée pour échapper
les variables dans les templates (Voir l'`escaping_strategy` ci-dessus).

 Vous pouvez choisir l'une des valeurs prédéfinies : ~`ESC_SPECIALCHARS`~, ~`ESC_RAW`~,
~`ESC_ENTITIES`~, ~`ESC_JS`~, ~`ESC_JS_NO_ENTITIES`~, et
~`ESC_SPECIALCHARS`~, ou créer votre propre fonction.

La plupart du temps, la valeur par défaut est très bien. L'helper ESC_ENTITIES peut
également être utilisé, surtout si vous travaillez uniquement avec les langues anglaise
ou européenne.

### ~`csrf_secret`~

*Par défaut*: `false`

Le `csrf_secret` est un secret unique pour votre application. S'il n'est pas défini à
`false`, il permet la protection CSRF pour toutes les formulaires définies avec le formulaire
du framework. Ce paramètre est également utilisé par le helper `link_to()` quand il a besoin
de convertir un lien vers un formulaire (pour simuler une méthode HTTP `DELETE` par exemple).

Il est fortement recommandé de changer la valeur par défaut par un secret unique.

>**TIP**
>Ce paramètre peut être activé lorsque vous créez une application avec la tâche
>`generate:app` en utilisant l'option `--csrf-secret`.

### ~`charset`~

*Par défaut*: `utf-8`

Le paramètre `charset` est le jeu de caractères qui sera utilisé partout dans
le framework : de la réponse du `Content-Type` dans le header, à la fonctionnalité
d'échappement de sortie.

La plupart du temps, la valeur par défaut est très bien.

>**WARNING**
>Ce paramètre est utilisé dans de nombreux endroits différents dans le framework,
>et donc sa valeur est mise en cache à plusieurs endroits. Après l'avoir modifié,
le cache de configuration doit être vidé, même dans l'environnement de
développement.

### ~`enabled_modules`~

*Par défaut*: `[default]`

L'`enabled_modules` est un tableau de nom de module à activer pour cette
application. Les modules définis dans un plugin ou dans le noyau de symfony ne sont pas activé
par défaut, et vous devez les lister dans ce paramètre pour qu'ils soient accessibles.

L'ajout d'un module est très simple en l'ajoutant à la liste (l'ordre des modules
n'a pas d'importance) :

    [yml]
    enabled_modules: [default, sfGuardAuth]

Le module `default` défini dans le framework contient toutes les actions par défaut
définies dans la sous-section `.actions` de `settings.yml`. Il est recommandé de
personnaliser le tout, puis de retirer le module `default` de ce
paramètre.

### ~`default_timezone`~

*Par défaut*: aucun

Le paramètre `default_timezone` définit le fuseau horaire par défaut utilisé par PHP. Il
peut avoir n'importe quel [fuseau horaire](http://www.php.net/manual/en/class.datetimezone.php)
reconnu par PHP. 

>**NOTE**
>Si vous ne définissez pas de fuseau horaire, il est conseillé d'en définir un dans le
>fichier `php.ini`. Sinon, symfony va essayer de deviner le meilleur fuseau horaire
>en appelant la fonction 
>[`date_default_timezone_get()`](http://www.php.net/date_default_timezone_get)
>de PHP.

### ~`cache`~

*Par défaut*: `off`

Le paramètre `cache` active ou désactive le modèle de mise en cache.

>**TIP**
> La configuration générale du système de cache est fait dans
>les sections [`view_cache_manager`](#chapter_05_view_cache_manager) et
>[`view_cache`](#chapter_05_view_cache) du fichier de configuration
> `factories.yml`. La configuration la plus fine est faite dans
>le fichier de configuration [`cache.yml`](#chapter_09).

### ~`etag`~

*Par défaut*: `on` par défaut sauf pour les environnements de `dev` et `test`

Le paramètre `etag` active ou désactive la génération automatique d'en-têtes `ETag` HTTP.
Le ETag généré par symfony est un simple MD5 du contenu des réponses.

### ~`i18n`~

*Par défaut*: `off`

Le paramètre `i18n` est un booléen qui active ou désactive le sous framework
i18n. Si votre application estest internationalisée, réglez-le à `on`.

>**TIP**
>La configuration générale du système i18n est à faire dans la section
>[`i18n`](#chapter_05_i18n) du fichier de configuration
>`factories.yml`.

### ~`default_culture`~

*Par défaut*: `en`

Le paramètre `default_culture`  définit la culture par défaut utilisé par le sous-framework
i18n. Il peut avoir n'importe quelle culture valide.

### ~`standard_helpers`~

*Par défaut*: `[Partial, Cache, Form]`

Le paramètre `standard_helpers` est un tableau de groupe de helper pour charger tous
les templates (nom du groupe de helper sans le suffixe `Helper`).

### ~`no_script_name`~

*Par défaut*: `on` pour l'environnement de `prod` de la première application créée,
`off` pour les autres

Le paramètre `no_script_name` détermine si le nom du script du contrôleur frontal
est ajouté ou non dans l'URL générée. Par défaut il est réglé sur `on` par
la tâche `generate:app` pour l'environnement de `prod` de la première application
créée. 

De toute évidence, un seul couple application/environnement peut avoir le paramètre à
`on`, dans le cas où tous les contrôleurs frontaux sont dans le même répertoire (`web/`). Si vous voulez
plus d'une application avec `no_script_name` à `on`, déplacez le(s) contrôleur(s)
correspondant(s) dans un sous-répertoire du répertoire racine
web.

### ~`lazy_cache_key`~

*Par défaut*: `on` pour les nouveaux projets, `off` pour des projets mis à niveau

Lorsqu'il est activé, le paramètre `lazy_cache_key` retarde la création d'une clé cache
jusqu'à ce que la vérification de la mise en cache d'une action ou d'un partial soit terminé. cela peut
avoir pour résultat une grande amélioration des performances, en fonction de votre utilisation des templates
partials.

Ce paramètre a été introduit dans symfony 1.2.7 pour améliorer les performances sans
casser la compatibilité descendante avec les précédents releases de la 1.2. Il sera supprimé
dans symfony 1.3 car l'optimisation sera toujours activé.

>**CAUTION**
>Ce paramètre est seulement disponible depuis symfony 1.2.7 et plus.

### ~`logging_enabled`~

*Par défaut*: `on` pour tous les environnements sauf `prod`

Le paramètre `logging_enabled` active la journalisation du sous-framework. Mettez cette option à
`false` et il contourne le mécanisme de journalisation et cela fournit un petit
gain de performance.

>**TIP**
>La configuration la plus fine de la journalisation est faite dans le fichier de configuration
>`factories.yml`.

### ~`web_debug`~

*Par défaut*: `off` pour tous les environnements sauf `dev`

Le paramètre `web_debug` active la barre d'outil de déboggage web. Elle
est incluse dans une page si le contenu de la réponse est du HTML.

### ~`error_reporting`~

*Par défaut*:

  * `prod`:  E_PARSE | E_COMPILE_ERROR | E_ERROR | E_CORE_ERROR | E_USER_ERROR
  * `dev`:   E_ALL | E_STRICT
  * `test`:  (E_ALL | E_STRICT) ^ E_NOTICE
  * default: E_PARSE | E_COMPILE_ERROR | E_ERROR | E_CORE_ERROR | E_USER_ERROR

Le paramètre `error_reporting` contrôle le niveau de rapport d'erreurs PHP (qui sera
affiché dans le navigateur et écrit dans les journaux).

>**TIP**
>Le site PHP a quelques informations sur la façon d'utiliser les
>[opérateurs sur les bits](http://www.php.net/language.operators.bitwise).

La configuration par défaut est la plus sensible, et ne devrait pas être modifiée.

>**NOTE**
>L'affichage des erreurs dans le navigateur est automatiquement désactivé
>pour les contrôleurs frontaux qui ont le `debug` désactivé, c'est le cas par défaut
>pour l'environnement de `prod`.

### ~`compressed`~

*Par défaut*: `off`

Le paramètre `compressed` permet la compression native de la réponse PHP. S'il est à
`on`, symfony utilisera [`ob_gzhandler`](http://www.php.net/ob_gzhandler), une fonction
callback de `ob_start()`.

Il est recommandé de le laisser à `off` et utiliser le mécanisme de compression
natif de votre serveur web à la place.

### ~`use_database`~

*Par défaut*: `on`

Le `use_database` détermine si l'application utilise une base de données ou non.

### ~`check_lock`~

*Par défaut*: `off`

Le paramètre `check_lock` active ou désactive le système de verrouillage de l'application
déclenchée par des tâches telles que `cache:clear` et `project:disable`.

S'il est défini à `on`, toutes les requêtes vers des applications désactivées seront automatiquement
redirigées vers la page du noyau symfony `lib/exception/data/unavailable.php`.

>**TIP**
>Vous pouvez remplacer le template par défaut disponible en ajoutant
>un fichier `config/unavailable.php` à votre projet ou à votre application.

### ~`check_symfony_version`~

*Par défaut* : `off`

Le `check_symfony_version` active ou désactive  la vérification de la version
symfony en cours sur toutes les requêtes. S'il est activé, symfony va vider le cache
automatiquement lorsque le cadre est mis à jour.

Il est fortement recommandé de ne pas mettre cela à `on` car il ajoute une petite surcharge,
et parce qu'il est simple de vider le cache lorsque vous déployez une nouvelle version
de votre projet. Ce paramètre n'est utile que si plusieurs projets partagent le
même code symfony, ce qui n'est pas recommandé.

### ~`web_debug_web_dir`~

*Par défaut*: `/sf/sf_web_debug`

Le `web_debug_web_dir` définit le chemin web pour les ressources de la barre d'outil de déboggage
(images, feuilles de style, et les fichiers JavaScript).

### ~`strip_comments`~

*Par défaut* : `on`

Le `strip_comments` détermine si symfony doit retirer les commentaires lors
de la compilation des classes de base. Ce paramètre n'est utilisé que si le paramètre
`debug` de l'application  est réglé à `off`.

Si vous avez des pages blanches dans l'environnement de production seulement, essayer de mettre ce
paramètre à `off`.

### ~`max_forwards`~

*Par défaut* : `5`

Le paramètre `max_forwards` fixe le nombre maximum de transfert interne
autorisé avant que symfony lève une exception. Il s'agit d'éviter les boucles infinies.
