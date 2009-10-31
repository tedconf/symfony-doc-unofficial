﻿Le fichier de configuration factories.yml
====================================

Les factories sont des objets du noyau n�cessaires pour le framework au cours de la vie de toutes les
requ�tes. Ils sont configur�s dans le fichier de configuration `factories.yml` et
toujours accessible via l'objet `sfContext` :

    [php]
    // R�cup�re le factory de l'utilisateur
    sfContext::getInstance()->getUser();

Le fichier principal de configuration `factories.yml` pour une application se trouve dans
le r�pertoire `apps/APP_NAME/config/`.

Comme indiqu� dans l'introduction, le fichier `factories.yml` est
[**sensible � l'environnement**](#chapter_03_sensibilisation_a_l_environnement), b�n�ficie du
[**m�canisme de configuration en cascade**](#chapter_03_configuration_en_cascade)
et peut inclure [**des constantes**](#chapter_03_constantes).

Le fichier de configuration `factories.yml` contient une liste de factory nomm�s :

    [yml]
    FACTORY_1:
      # definition de factory 1

    FACTORY_2:
      # definition de factory 2

    # ...

Les noms de factory support�s sont : `controller`, `logger`, `i18n`, `request`,
`response`, `routing`, `storage`, `user`, `view_cache`, et
`view_cache_manager`.

Lorsque le `sfContext` initialise les factories, il lit le fichier `factories.yml`
pour le nom de la classe du factory (`class`) et les param�tres (`param`)
utilis�s pour configurer l'objet factory :

    [yml]
    FACTORY_NAME:
      class: CLASS_NAME
      param: { ARRAY OF PARAMETERS }

�tre en mesure de personnaliser les factories signifie que vous pouvez utiliser une classe personnalis�e
pour les objets du noyau de symfony � la place de celui par d�faut. Vous pouvez �galement modifier
le comportement par d�faut de ces classes en personnalisant les param�tres qui lui sont envoy�s.

Si la classe du factory ne peut pas �tre charg�es automatiquement, un chemin du fichier peut �tre d�fini et
sera automatiquement inclus avant la cr�ation du factory :

    [yml]
    FACTORY_NAME:
      class: CLASS_NAME
      file:  ABSOLUTE_PATH_TO_FILE

>**NOTE**
>Le fichier de configuration `factories.yml` est mis en cache dans un fichier PHP. Le
>processus est automatiquement g�r� par [la classe](#chapter_14_config_handlers_yml)
>~`sfFactoryConfigHandler`~.

<div class="pagebreak"></div>

Factories
---------

 * [`mailer`](#chapter_05_mailer)

  * [`charset`](#chapter_05_sub_charset)
  * [`delivery_address`](#chapter_05_sub_delivery_address)
  * [`delivery_strategy`](#chapter_05_sub_delivery_strategy)
  * [`spool_arguments`](#chapter_05_sub_spool_arguments)
  * [`spool_class`](#chapter_05_sub_spool_class)
  * [`transport`](#chapter_05_sub_transport)

 * [`request`](#chapter_05_request)

   * [`formats`](#chapter_05_sub_formats)
   * [`path_info_array`](#chapter_05_sub_path_info_array)
   * [`path_info_key`](#chapter_05_sub_path_info_key)
   * [`relative_url_root`](#chapter_05_sub_relative_url_root)

 * [`response`](#chapter_05_response)

   * [`charset`](#chapter_05_sub_charset)
   * [`http_protocol`](#chapter_05_sub_http_protocol)
   * [`send_http_headers`](#chapter_05_sub_send_http_headers)

 * [`user`](#chapter_05_user)

   * [`default_culture`](#chapter_05_sub_default_culture)
   * [`timeout`](#chapter_05_sub_timeout)
   * [`use_flash`](#chapter_05_sub_use_flash)

 * [`storage`](#chapter_05_storage)

   * [`auto_start`](#chapter_05_sub_auto_start)
   * [`database`](#chapter_05_sub_database_storage_specific_options)
   * [`db_table`](#chapter_05_sub_database_storage_specific_options)
   * [`db_id_col`](#chapter_05_sub_database_storage_specific_options)
   * [`db_data_col`](#chapter_05_sub_database_storage_specific_options)
   * [`db_time_col`](#chapter_05_sub_database_storage_specific_options)
   * [`session_cache_limiter`](#chapter_05_sub_session_cache_limiter)
   * [`session_cookie_domain`](#chapter_05_sub_session_set_cookie_params_parameters)
   * [`session_cookie_httponly`](#chapter_05_sub_session_set_cookie_params_parameters)
   * [`session_cookie_lifetime`](#chapter_05_sub_session_set_cookie_params_parameters)
   * [`session_cookie_path`](#chapter_05_sub_session_set_cookie_params_parameters)
   * [`session_cookie_secure`](#chapter_05_sub_session_set_cookie_params_parameters)
   * [`session_name`](#chapter_05_sub_session_name)

 * [`view_cache_manager`](#chapter_05_view_cache_manager)

   * [`cache_key_use_vary_headers`](#chapter_05_sub_cache_key_use_vary_headers)
   * [`cache_key_use_host_name`](#chapter_05_sub_cache_key_use_host_name)

 * [`view_cache`](#chapter_05_view_cache)
 * [`i18n`](#chapter_05_i18n)

   * [`cache`](#chapter_05_sub_cache)
   * [`debug`](#chapter_05_sub_debug)
   * [`source`](#chapter_05_sub_source)
   * [`untranslated_prefix`](#chapter_05_sub_untranslated_prefix)
   * [`untranslated_suffix`](#chapter_05_sub_untranslated_suffix)

 * [`routing`](#chapter_05_routing)

   * [`cache`](#chapter_05_sub_cache)
   * [`extra_parameters_as_query_string`](#chapter_05_sub_extra_parameters_as_query_string)
   * [`generate_shortest_url`](#chapter_05_sub_generate_shortest_url)
   * [`lazy_routes_deserialize`](#chapter_05_sub_lazy_routes_deserialize)
   * [`lookup_cache_dedicated_keys`](#chapter_05_sub_lookup_cache_dedicated_keys)
   * [`load_configuration`](#chapter_05_sub_load_configuration)
   * [`segment_separators`](#chapter_05_sub_segment_separators)
   * [`suffix`](#chapter_05_sub_suffix)
   * [`variable_prefixes`](#chapter_05_sub_variable_prefixes)

 * [`logger`](#chapter_05_logger)

   * [`level`](#chapter_05_sub_level)
   * [`loggers`](#chapter_05_sub_loggers)

 * [`controller`](#chapter_05_controller)

<div class="pagebreak"></div>

`mailer`
--------

*sfContext Accessor*: `$context->getMailer()`

*Configuration par d�faut* :

    [yml]
    mailer:
      class: sfMailer
      param:
        logging:           %SF_LOGGING_ENABLED%
        charset:           %SF_CHARSET%
        delivery_strategy: realtime
        transport:
          class: Swift_SmtpTransport
          param:
            host:       localhost
            port:       25
            encryption: ~
            username:   ~
            password:   ~

*Configuration par d�faut pour l'environnement `test`* :

    [yml]
    mailer:
      param:
        delivery_strategy: none

*Configuration par d�faut pour l'environnement `dev`* :

    [yml]
    mailer:
      param:
        delivery_strategy: none

### ~`charset`~

L'option `charset` d�finit le jeu de caract�res � utiliser pour les messages �lectroniques. Par
d�faut, il utilise le param�tre `charset` de `settings.yml`.

### ~`delivery_strategy`~

L'option `delivery_strategy` d�finit comment les messages e-mail sont livr�s par le
mailer. Quatre strat�gies sont disponibles par d�faut, ce qui devrait convenir � tous les
besoins communs :

 * `realtime`:       Les messages sont envoy�s en temps r�el.

 * `single_address`: Les messages sont envoy�s � une seule adresse.

 * `spool`:          Les messages sont stock�s dans une file d'attente.

 * `none`:           Les messages sont tout simplement ignor�s.

### ~`delivery_address`~

L'option `delivery_address` d�finit le b�n�ficiaire de tous les messages lorsque le
`delivery_strategy` est � `single_address`.

### ~`spool_class`~

L'option `spool_class` d�finit la classe de spool � utiliser lorsque le
`delivery_strategy` est � `spool`:

  * ~`Swift_FileSpool`~: Les messages sont stock�s sur le syst�me de fichiers.

  * ~`Swift_DoctrineSpool`~: Les messages sont stock�s dans un mod�le de Doctrine.

  * ~`Swift_PropelSpool`~: Les messages sont stock�s dans un mod�le de Propel.

>**NOTE**
>Lorsque le spool est instanci�, l'option ~`spool_arguments`~ est utilis�e comme les
>arguments du constructeur.

### ~`spool_arguments`~

L'option `spool_arguments` d�finit les arguments du constructeur du spool.
Voici les options disponibles pour les classes int�gr�es des files d'attente :

 * `Swift_FileSpool`:

    * Le chemin absolu du r�pertoire de file d'attente (les messages sont stock�s dans
      ce r�pertoire)

 * `Swift_DoctrineSpool`:

    * Le mod�le de Doctrine � utiliser pour stocker les messages (`MailMessage` par
      d�faut)

    * Le nom de la colonne � utiliser pour le stockage de messages (`message` par d�faut)

    * La m�thode � appeler pour r�cup�rer les messages � envoyer (facultatif). Il
      re�oit les options de la file d'attente comme un argument.

 * `Swift_PropelSpool`:

    * Le mod�le de Propel � utiliser pour stocker les messages (`MailMessage` par d�faut)

    * Le nom de la colonne � utiliser pour le stockage de messages (`message` par d�faut)

    * La m�thode � appeler pour r�cup�rer les messages � envoyer (facultatif). Il
      re�oit les options de la file d'attente comme un argument.

La configuration ci-dessous montre une configuration typique pour un spool de Doctrine :

    [yml]
    # configuration in factories.yml
    mailer:
      class: sfMailer
      param:
        delivery_strategy: spool
        spool_class:       Swift_DoctrineSpool
        spool_arguments:   [ MailMessage, message, getSpooledMessages ]

### ~`transport`~

L'option `transport` d�finit le transport � utiliser pour envoyer effectivement des messages
�lectroniques.

Le param�tre `class` peut �tre n'importe quelle classe qui impl�mente `Swift_Transport`,
et trois sont fournis par d�faut :

  * ~`Swift_SmtpTransport`~: Utilise un serveur SMTP pour envoyer des messages.

  * ~`Swift_SendmailTransport`~: Utilise `sendmail` pour envoyer des messages.

  * ~`Swift_MailTransport`~: Utilise la fonction native PHP `mail()` pour envoyer
    des messages.

 Vous pouvez configurer le transport en d�finissant le param�tre `param`. La
section ["Transport Types"](http://swiftmailer.org/docs/transport-types) de
la documentation officielle de Swift Mailer d�crit tout ce que vous devez savoir sur
les classes int�gr�es dans les transports et leurs diff�rents param�tres.

`request`
---------

*sfContext Accessor*: `$context->getRequest()`

*Configuration par d�faut* :

    [yml]
    request:
      class: sfWebRequest
      param:
        logging:           %SF_LOGGING_ENABLED%
        path_info_array:   SERVER
        path_info_key:     PATH_INFO
        relative_url_root: ~
        formats:
          txt:  text/plain
          js:   [application/javascript, application/x-javascript, text/javascript]
          css:  text/css
          json: [application/json, application/x-json]
          xml:  [text/xml, application/xml, application/x-xml]
          rdf:  application/rdf+xml
          atom: application/atom+xml

### ~`path_info_array`~

L'option `path_info_array` d�finit le tableau global PHP qui sera utilis�e pour
r�cup�rer des informations. Sur certaines configurations, vous voudrez changer la
valeur par d�faut `SERVER` par `ENV`.

### ~`path_info_key`~

L'option `path_info_key` d�finit la cl� sous laquelle l'information `PATH_INFO`
peut �tre trouv�e.

Si vous utilisez ~IIS~ avec un module de r��criture comme `IIFR` ou `ISAPI`, vous devez
changer cette valeur par `HTTP_X_REWRITE_URL`.

### ~`formats`~

L'option `formats` d�finit un tableau des extensions de fichiers et leur
`Content-Type` correspondant. Il est utilis� par le framework pour g�rer automatiquement
le `Content-Type` de la r�ponse, bas�e sur l'extension de l'URI de la requ�te.

### ~`relative_url_root`~

L'option `relative_url_root` d�finit la partie de l'URL avant que le contr�leur
frontal. La plupart du temps, il est automatiquement d�tect� par le framework
et n'a pas besoin d'�tre chang�e.

`response`
----------

*sfContext Accessor*: `$context->getResponse()`

*Configuration par d�faut* :

    [yml]
    response:
      class: sfWebResponse
      param:
        logging:           %SF_LOGGING_ENABLED%
        charset:           %SF_CHARSET%
        send_http_headers: true

*Configuration par d�faut pour l'environnement `test`* :

    [yml]
    response:
      class: sfWebResponse
      param:
        send_http_headers: false

### ~`send_http_headers`~

L'option `send_http_headers` sp�cifie si la r�ponse devrait envoyer des ent�tes de r�ponse HTTP
ainsi que le contenu des r�ponses. Ce r�glage est surtout
utile pour les tests, les ent�tes sont envoy�es avec la fonction PHP `header()` qui
envoie des avertissements si vous essayez d'envoyer des ent�tes, apr�s des sorties.

### ~`charset`~

L'option `charset` d�finit le jeu de caract�res � utiliser pour la r�ponse. Par d�faut,
il utilise le param�tre `charset` de `settings.yml`, qui est ce que vous voulez la plupart
du temps.

### ~`http_protocol`~

L'option `http_protocol` d�finit la version du protocole HTTP � utiliser pour
la r�ponse. Par d�faut, il v�rifie la valeur de `$_SERVER['SERVER_PROTOCOL']` si elle est
disponible ou par d�faut � `HTTP/1.0`.

`user`
------

*sfContext Accessor*: `$context->getUser()`

*Configuration par d�faut* :

    [yml]
    user:
      class: myUser
      param:
        timeout:         1800
        logging:         %SF_LOGGING_ENABLED%
        use_flash:       true
        default_culture: %SF_DEFAULT_CULTURE%

>**NOTE**
>Par d�faut, la classe `myUser` h�rite de `sfBasicSecurityUser`,
>qui peut �tre configur�e dans le fichier de configuration
>[`security.yml`](#chapter_08).

### ~`timeout`~

L'option `timeout` d�finit le timeout pour l'authentification des utilisateurs. Il n'est pas
li� au timeout de la session. Le r�glage par d�faut d�s-authentifie
automatiquement un utilisateur au bout de 30 minutes d'inactivit�.

Ce param�tre n'est utilis� que par les classes d'utilisateurs qui h�ritent de la
classe de base `sfBasicSecurityUser`, ce qui est le cas de la classe g�n�r�e
`myUser`.

>**NOTE**
>Pour �viter un comportement inattendu, la classe utilisateur force automatiquement la
>dur�e de vie maximale pour le ramasse-miettes de session (`session.gc_maxlifetime`)
>� une valeur plus grande que le timeout.

### ~`use_flash`~

L'option `use_flash` active ou d�sactive le composant flash.

### ~`default_culture`~

L'option `default_culture` d�finit la culture � utiliser par d�faut pour un utilisateur qui
arrive sur le site pour la premi�re fois. Par d�faut, il utilise le
param�tre `default_culture` de `settings.yml`, qui est votre choix la plupart
du temps.

>**CAUTION**
>Si vous changer le param�tre ~`default_culture`~ dans `factories.yml` ou dans
>`settings.yml`, vous devez effacer les cookies dans votre navigateur pour v�rifier
>le r�sultat.

`storage`
---------

Le factory storage est utilis� par le factory user pour maintenir les donn�es utilisateur entre
les requ�tes HTTP.

*sfContext Accessor*: `$context->getStorage()`

*Configuration par d�faut* :

    [yml]
    storage:
      class: sfSessionStorage
      param:
        session_name: symfony

*Configuration par d�faut pour l'environnement `test`* :

    [yml]
    storage:
      class: sfSessionTestStorage
      param:
        session_path: %SF_TEST_CACHE_DIR%/sessions

### ~`auto_start`~

L'option `auto_start` active ou d�sactive la fonctionnalit� de PHP
d'auto-d�marrage de session (via la fonction `session_start()`).

### ~`session_name`~

L'option `session_name` d�finit le nom du cookie utilis� par symfony pour
stocker la session utilisateur. Par d�faut, le nom est `symfony`, ce qui signifie que
toutes vos applications stockeront dans le m�me cookie (ainsi que l'authentification
et les autorisations correspondantes).

### Les param�tres de `session_set_cookie_params()`

Le factory `storage` appelle la fonction
[`session_set_cookie_params()`](http://www.php.net/session_set_cookie_params)
avec la valeur des options suivantes :

 * ~`session_cookie_lifetime`~: dur�e de vie du cookie de session, d�fini en
                                secondes.
 * ~`session_cookie_path`~:   Chemin sur le domaine o� le cookie va fonctionner.
                              Utilisez un simple slash (`/`)  pour tous les chemins sur le
                              domaine.
 * ~`session_cookie_domain`~: domaine de cookie, par exemple `www.php.net`. Pour
                              faire des cookies visible sur tous les sous-domaines alors
                              le domaine doit �tre pr�fix� avec un point comme `.php.net`.
 * ~`session_cookie_secure`~: Si c'est � `true` alors le cookie ne sera envoy�e que sur des connexions
                              s�curis�es.
 * ~`session_cookie_httponly`~: Si c'est � `true` alors PHP tentera d'envoyer le flag
                                `httponly` lors du param�trage du cookie de session.

>**NOTE**
>La description de chaque option provient de la description de la fonction
>`session_set_cookie_params()` sur le site de PHP

### ~`session_cache_limiter`~

Si l'option `session_cache_limiter` est mise, la fonction PHP
[`session_cache_limiter()`](http://www.php.net/session_cache_limiter)
est appel�e et la valeur de l'option est pass�e en argument.

### Options de la base de donn�es sp�cifique de stockage

Lorsque vous utilisez un stockage qui h�rite de la classe `sfDatabaseSessionStorage`,
plusieurs options suppl�mentaires sont disponibles:

 * ~`database`~:     Le nom de la base de donn�es (obligatoire)
 * ~`db_table`~:     Le nom de la table (obligatoire)
 * ~`db_id_col`~:    Le nom de la colonne de la cl� primaire (`sess_id` par d�faut)
 * ~`db_data_col`~:  Le nom de la colonne donn�e (`sess_data` par d�faut)
 * ~`db_time_col`~:  Le nom de la colonne temps (`sess_time` par d�faut)

`view_cache_manager`
--------------------

*sfContext Accessor*: `$context->getViewCacheManager()`

*Configuration par d�faut* :

    [yml]
    view_cache_manager:
      class: sfViewCacheManager
      param:
        cache_key_use_vary_headers: true
        cache_key_use_host_name:    true

>**CAUTION**
>Ce factory est cr�� si le param�tre [`cache`](#chapter_04_sub_cache)
>est � `on`.

La plupart de la configuration de ce factory se fait via le factory `view_cache`, qui
d�finit l'objet du cache sous-jacent utilis� par le gestionnaire de cache la vue.

### ~`cache_key_use_vary_headers`~

L'option `cache_key_use_vary_headers` pr�cise si les cl�s du cache doivent �tre
inclus dans la partie des ent�tes qui varient. En pratique, cela veut dire que le cache de la page doit
�tre d�pendant de l'ent�te HTTP, comme sp�cifi� dans le param�tre du cache `vary` (valeur
par d�faut : `true`).

### ~`cache_key_use_host_name`~

L'option `cache_key_use_host_name` pr�cise si les cl�s du cache doivent �tre
inclus dans la partie du nom de l'h�te. En pratique, cela veut dire que le cache doit �tre
d�pendant du nom de l'h�te (valeur par d�faut : `true`).

`view_cache`
------------

*sfContext Accessor*: none (utilis� directement par le factory `view_cache_manager`)

*Configuration par d�faut* :

    [yml]
    view_cache:
      class: sfFileCache
      param:
        automatic_cleaning_factor: 0
        cache_dir:                 %SF_TEMPLATE_CACHE_DIR%
        lifetime:                  86400
        prefix:                    %SF_APP_DIR%/template

>**CAUTION**
>Ce factory est uniquement d�fini si le param�tre [`cache`](#chapter_04_sub_cache)
>est � `on`.

Le factory `view_cache` d�finit une classe de cache qui doit h�riter de
`sfCache` (Voir la section Cache pour plus d'information).

`i18n`
------

*sfContext Accessor*: `$context->getI18N()`

*Configuration par d�faut* :

    [yml]
    i18n:
      class: sfI18N
      param:
        source:               XLIFF
        debug:                off
        untranslated_prefix:  "[T]"
        untranslated_suffix:  "[/T]"
        cache:
          class: sfFileCache
          param:
            automatic_cleaning_factor: 0
            cache_dir:                 %SF_I18N_CACHE_DIR%
            lifetime:                  31556926
            prefix:                    %SF_APP_DIR%/i18n

>**CAUTION**
>Ce factory est uniquement d�fini si le param�tre [`i18n`](#chapter_04_sub_i18n)
>est � `on`.

### ~`source`~

L'option `source` d�finit le type de conteneur pour les traductions.

*Conteneur int�gr�s* : `XLIFF`, `SQLite`, `MySQL`, et `gettext`.

### ~`debug`~

L'option `debug` d�finit le mode de d�bogage. S'il est d�fini � `on`, les messages
non-traduits sont d�cor�es avec un pr�fixe et un suffixe (voir ci-dessous).

### ~`untranslated_prefix`~

Le `untranslated_prefix` d�finit un pr�fixe � utiliser pour les messages non-traduits.

### ~`untranslated_suffix`~

Le `untranslated_suffix` d�finit un suffixe � utiliser pour les messages non-traduits.

### ~`cache`~

L'option `cache` d�finit un factory de cache anonyme pour �tre utilis� pour la mise
en cache des donn�es i18n (voir la section cache pour plus d'informations).

`routing`
---------

*sfContext Accessor*: `$context->getRouting()`

*Configuration par d�faut* :

    [yml]
    routing:
      class: sfPatternRouting
      param:
        load_configuration:               true
        suffix:                           ''
        default_module:                   default
        default_action:                   index
        debug:                            %SF_DEBUG%
        logging:                          %SF_LOGGING_ENABLED%
        generate_shortest_url:            false
        extra_parameters_as_query_string: false
        cache:                            ~

### ~`variable_prefixes`~

*Par d�faut* : `:`

L'option `variable_prefixes` d�finit la liste des caract�res qui d�butent
le nom d'une variable dans un mod�le de parcours.

### ~`segment_separators`~

*Par d�faut* : `/` et `.`

L'option `segment_separators` d�finit la liste des s�parateurs de segment du parcours.
La plupart du temps, vous ne voulez pas r��crire cette option pour l'ensemble
du routage, mais pour des parcours sp�cifiques.

### ~`generate_shortest_url`~

*Par d�faut* : `true` pour les nouveaux projets, `false` pour les projets mis � niveau

Si elle est � `true`, l'option `generate_shortest_url` dira au syst�me de
routage de g�n�rer le parcours le plus court possible. R�glez-le � `false` si vous voulez
que vos parcours soient compatible avec symfony 1.0 et 1.1.

### ~`extra_parameters_as_query_string`~

*Par d�faut* : `true` pour les nouveaux projets, `false` pour les projets mis � niveau

Lorsque certains param�tres ne sont pas utilis�s dans la g�n�ration d'un parcours,
l'`extra_parameters_as_query_string` permet � ces param�tres suppl�mentaires d'�tre
convertie en une cha�ne de caract�re d'une query. R�glez-le � `false` pour revenir sur le comportement
de symfony 1.0 ou 1.1. Dans ces versions, les param�tres suppl�mentaires �taient tout simplement ignor�s
par le syst�me de routage.

### ~`cache`~

*Par d�faut* : none

L'option `cache`  L'option cache d�finit un factory de cache anonyme qui est utilis� pour la mise en cache
de la configuration du routage et des donn�es (voir la section cache pour plus d'informations).

### ~`suffix`~

*Par d�faut* : none

La valeur par d�faut du suffixe � utiliser pour tous les parcours. Cette option est obsol�te et n'est
plus utile.

### ~`load_configuration`~

*Par d�faut* : `true`

L'option `load_configuration` d�finit si les fichiers `routing.yml` doivent
�tre automatiquement charg�s et analys�s. R�glez le � `false` si vous souhaitez utiliser le
syst�me de routage de symfony � l'ext�rieur d'un projet symfony.

### ~`lazy_routes_deserialize`~

*Par d�faut* : `false`

Si le param�tre `lazy_routes_deserialize` est � `true`, il permet une relecture
paresseuse du cache de routage. Il peut am�liorer les performances de vos
applications si vous avez un grand nombre de parcours et si la plupart des parcours
correspondent aux premiers. Il est fortement conseill� de tester le param�tre avant
de le d�ployer en production, car il peut nuire sur les performances dans certaines
circonstances.

### ~`lookup_cache_dedicated_keys`~

*Par d�faut* : `false`

Le param�tre `lookup_cache_dedicated_keys` d�termine comment le cache de routage est
construit. Lorsqu'il est positionn� � `false`, le cache est stock� comme une grande valeur. Lorsqu'il
est positionn� � `true` chaque parcours a son propre stockage de cache. Ce param�tre est un
param�tre d'optimisation de performance.

En r�gle g�n�rale, la valeur `false` est mieux lorsque vous utilisez une classe de cache
bas�e sur un fichier (`sfFileCache` par exemple), et la valeur `true` est mieux
lorsque vous utilisez une classe de cache bas�e sur la m�moire (`sfAPCCache` par exemple). 

`logger`
--------

*sfContext Accessor*: `$context->getLogger()`

*Configuration par d�faut* :

    [yml]
    logger:
      class: sfAggregateLogger
      param:
        level: debug
        loggers:
          sf_web_debug:
            class: sfWebDebugLogger
            param:
              level: debug
              condition:       %SF_WEB_DEBUG%
              xdebug_logging:  true
              web_debug_class: sfWebDebug
          sf_file_debug:
            class: sfFileLogger
            param:
              level: debug
              file: %SF_LOG_DIR%/%SF_APP%_%SF_ENVIRONMENT%.log

*Configuration par d�faut pour l'environnement de `prod`* :

    [yml]
    logger:
      class:   sfNoLogger
      param:
        level:   err
        loggers: ~

>**CAUTION**
>Ce factory est toujours d�fini, mais la journalisation se produit que si
>le param�tre `logging_enabled` est � `on`.

### ~`level`~

L'option `level` d�finit le niveau du journal.

*Valeurs possibles*: `EMERG`, `ALERT`, `CRIT`, `ERR`, `WARNING`, `NOTICE`,
`INFO`, or `DEBUG`.

### ~`loggers`~

L'option `loggers` d�finit la liste des journaux � utiliser. La liste est un tableau de
factory de journaux anonymes.

*Les classes de journaux int�gr�es*: `sfConsoleLogger`, `sfFileLogger`, `sfNoLogger`,
`sfStreamLogger`, et `sfVarLogger`.

`controller`
------------

*sfContext Accessor*: `$context->getController()`

*Configuration par d�faut* :

    [yml]
    controller:
      class: sfFrontWebController

Les factories de cache anonyme
-------------------------

Plusieurs factories (`view_cache`, `i18n`, et `routing`) peuvent profiter
d'un objet du cache si il est d�fini dans leur configuration. La configuration
de l'objet du cache est similaire pour toutes les factories. La cl� de `cache` d�finit
un factory de cache anonyme. Comme tout factory, il prend une `class` et une
entr�e `param`. L'entr�e `param` peut prendre n'importe quelle option disponible pour la
classe du cache.

L'option `prefix` est la plus importante car elle permet de partager ou de
s�parer un cache entre diff�rents environnements/applications/projets.

*Les classes de cache int�gr�es*: `sfAPCCache`, `sfEAcceleratorCache`, `sfFileCache`,
`sfMemcacheCache`, `sfNoCache`, `sfSQLiteCache`, et `sfXCacheCache`.
