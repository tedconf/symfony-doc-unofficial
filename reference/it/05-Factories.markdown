Il file di configurazione The factories.yml
===========================================

I factory sono oggetti del core necessari al framework durante la vita di ogni richiesta.
Sono inizializzati nel file di configurazione `factories.yml` e sempre accessibili tramite l'oggetto
`sfContext`:

    [php]
    // restituisce il factory per l'oggetto User
    sfContext::getInstance()->getUser();

Per una applicazione il file di configurazione `factories.yml` può essere trovato nella directory 
`apps/APP_NAME/config/`.

Come abbiamo detto durante l'introduzione, il file `factories.yml` è
[**consapevole dell'ambiente**](03-Configuration-Files-Principles#chapter_03_environment_awareness), beneficia del 
[**meccanismo di configurazione a cascata**](#chapter_03_configuration_cascade),
e può includere [**costanti**](#chapter_03_constants).

Il file di configurazione `factories.yml` contiene una lista di factory dichiarate:

    [yml]
    FACTORY_1:
      # definizione della factory 1

    FACTORY_2:
      # definizione della factory 2

    # ...

I nomi delle factory supportate sono: `controller`, `logger`, `i18n`, `request`,
`response`, `routing`, `storage`, `user`, `view_cache`, and
`view_cache_manager`.

Quando `sfContext` inizializza le factory, legge dal file `factories.yml`
i nomi delle classi delle factory (`class`) ed i relativi parametri (`param`)
per configurare i corrispettivi oggetti:

    [yml]
    NOME_DEL_FACTORY:
      class: NOME_DELLA_CLASSE
      param: { ARRAY DI PARAMETRI }

La possibilità di modificare le factory significa che è possibile usare una classe
personalizzata per istanziare un oggetto del core di symfony piuttosto che quella 
predefinita. È inoltre possibile cambiare anche il comportamento di queste classi 
modificando i parametri inviati alle stesse.

Se la classe di una factory non può essere caricata automaticamente, deve essere definito 
un parametro `file` che sarà utilizzato per indicare il percorso della classe che verrà
automaticamente usato prima che la factory sia creata:

    [yml]
    FACTORY_NAME:
      class: NOME_DELLA_CLASSE
      file:  PERCORSO_ASSOLUTO_DEL_FILE

>**NOTA**
>Il file di configurazione `factories.yml` viene salvato in cache come file PHP; Il processo
>è automaticamente gestito dalla [class](#chapter_14_config_handlers_yml) 
> ~`sfFactoryConfigHandler`~.

<div class="pagebreak"></div>

Factory
---------

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

`request`
---------

*sfContext Accessor*: `$context->getRequest()`

*Configurazione standard*:

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

L'impostazione `path_info_array` definisce l'array PHP globale che sarà usato per recuperare informazioni. 
In alcune configurazioni il valore predefinito potrebbe essere cambiato da `SERVER` ad `ENV`.

### ~`path_info_key`~

L'impostazione `path_info_key` definisce la chiave sotto la quale l'informazione relativa a `PATH_INFO`
può essere trovata.

Se è usato ~IIS~ con un modulo di riscrittura delle URL come `IIFR` o `ISAPI`, è necessario impostare questo parametro
a `HTTP_X_REWRITE_URL`.

### ~`formats`~

L'impostazione `formats` definisce un array di estensioni di file ed il corrispettivo
`Content-Type`. E' usata automaticamente dal framework per gestire il `Content-Type` di una risposta, 
in base all'estensione dell'URI richiesta.

### ~`relative_url_root`~

L'impostazione `relative_url_root` definisce la parte di URL che precede il front
controller. Normalmente, è automaticamente gestita dal framework e non necessita
cambiamenti.

`response`
----------

*sfContext Accessor*: `$context->getResponse()`

*Configurazione standard*:

    [yml]
    response:
      class: sfWebResponse
      param:
        logging:           %SF_LOGGING_ENABLED%
        charset:           %SF_CHARSET%
        send_http_headers: true

*Configurazione standard per l'ambiente di `test`*:

    [yml]
    response:
      class: sfWebResponse
      param:
        send_http_headers: false

### ~`send_http_headers`~

L'impostazione `send_http_headers` specifica quando deve essere inviato un
response headers insieme ad un response content. Questa opzione è particolarmente 
comoda per fare test, in quanto gli header sono inviati tramite la funzione PHP 
`header()` che invia avvisi se si sta provando ad inviare headers dopo a qualche tipo
di output.

### ~`charset`~

L'impostazione `charset` definisce il charset da utilizzare nel response. Il valore predefinito,
preso dal parametro `charset` nel file `settings.yml`, è quello che serve la maggior parte
delle volte.

### ~`http_protocol`~

L'impostazione `http_protocol` definisce la versione del protocollo HTTP da utilizzare 
per la risposta. Come valore predefinito utilizza `$_SERVER['SERVER_PROTOCOL']` 
altrimenti usa `HTTP/1.0`.

`user`
------

*sfContext Accessor*: `$context->getUser()`

*Configurazione standard*:

    [yml]
    user:
      class: myUser
      param:
        timeout:         1800
        logging:         %SF_LOGGING_ENABLED%
        use_flash:       true
        default_culture: %SF_DEFAULT_CULTURE%

>**NOTA**
>La classe `myUser` eredita da `sfBasicSecurityUser`,
>che può essere configurata nel file di configurazione
>[`security.yml`](#chapter_08).

### ~`timeout`~

L'impostazione `timeout` definisce il timeout per l'autenticazione utente.
Non è correlata al timeout della sessione. Il valore predefinito rimuove l'autenticazione
ad un utente dopo 30 minuti di inattività.

Questa impostazione è usata solo dalle classi user che ereditano dalla classe base 
`sfBasicSecurityUser`, come nel caso della classe generata dal sistema `myUser`.

>**NOTA**
>Per evitare comportamenti inaspettati, la classe user forza automaticamente la massima durata
>per il garbage collector delle sessioni (`session.gc_maxlifetime`) in modo che 
>sia maggiore, o uguale, al timeout.

### ~`use_flash`~

L'impostazione `use_flash` abilita o disabilita il componente flash.

### ~`default_culture`~

L'impostazione `default_culture` definisce la direttiva di traduzione da usare per 
l'utente che entra nel sito per la prima volta. Se non dichiarato utilizza il valore 
`default_culture` dichiarato nel file `settings.yml`.

>**ATTENZIONE**
>Se l'impostazione ~`default_culture`~ viene cambiata in `factories.yml` o
>in `settings.yml`, è necessario eliminare i cookie del browser per vedere le modifiche.

`storage`
---------

La factory storage è usata dalla factory user per salvare i dati dell'utente tra
una richiesta HTTP e l'altra.

*sfContext Accessor*: `$context->getStorage()`

*Configurazione standard*:

    [yml]
    storage:
      class: sfSessionStorage
      param:
        session_name: symfony

*Configurazione standard per l'ambiente di `test`*:

    [yml]
    storage:
      class: sfSessionTestStorage
      param:
        session_path: %SF_TEST_CACHE_DIR%/sessions

### ~`auto_start`~

L'impostazione `auto_start` abilita o disabilita la partenza automatica della sessione di PHP 
(usando la funzione `session_start()` del linguaggio).

### ~`session_name`~

The `session_name` option defines the name of the cookie used by symfony to
store the user session. By default, the name is `symfony`, which means that
all your applications share the same cookie (and as such the corresponding
authentication and authorizations).

### `session_set_cookie_params()` parameters

The `storage` factory calls the
[`session_set_cookie_params()`](http://www.php.net/session_set_cookie_params)
function with the value of the following options:

 * ~`session_cookie_lifetime`~: Lifetime of the session cookie, defined in
                                seconds.
 * ~`session_cookie_path`~:   Path on the domain where the cookie will work.
                              Use a single slash (`/`) for all paths on the
                              domain.
 * ~`session_cookie_domain`~: Cookie domain, for example `www.php.net`. To
                              make cookies visible on all subdomains then the
                              domain must be prefixed with a dot like `.php.net`.
 * ~`session_cookie_secure`~: If `true` cookie will only be sent over secure
                              connections.
 * ~`session_cookie_httponly`~: If set to `true` then PHP will attempt to send the
                                `httponly` flag when setting the session cookie.

>**NOTA**
>The description of each option comes from the `session_set_cookie_params()`
>function description on the PHP website

### ~`session_cache_limiter`~

If the `session_cache_limiter` option is set, PHP's
[`session_cache_limiter()`](http://www.php.net/session_cache_limiter)
function is called and the option value is passed as an argument.

### Database Storage-specific Options

When using a storage that inherits from the `sfDatabaseSessionStorage` class,
several additional options are available:

 * ~`database`~:     The database name (required)
 * ~`db_table`~:     The table name (required)
 * ~`db_id_col`~:    The primary key column name (`sess_id` by default)
 * ~`db_data_col`~:  The data column name (`sess_data` by default)
 * ~`db_time_col`~:  The time column name (`sess_time` by default)

`view_cache_manager`
--------------------

*sfContext Accessor*: `$context->getViewCacheManager()`

*Configurazione standard*:

    [yml]
    view_cache_manager:
      class: sfViewCacheManager

>**ATTENZIONE**
>This factory is only created if the [`cache`](#chapter_04_sub_cache)
>setting is set to `on`.

The view cache manager configuration does not include a `param` key. This
configuration is done via the `view_cache` factory, which defines the
underlying cache object used by the view cache manager.

`view_cache`
------------

*sfContext Accessor*: none (used directly by the `view_cache_manager` factory)

*Configurazione standard*:

    [yml]
    view_cache:
      class: sfFileCache
      param:
        automatic_cleaning_factor: 0
        cache_dir:                 %SF_TEMPLATE_CACHE_DIR%
        lifetime:                  86400
        prefix:                    %SF_APP_DIR%/template

>**ATTENZIONE**
>This factory is only defined if the [`cache`](#chapter_04_sub_cache)
>setting is set to `on`.

The `view_cache` factory defines a cache class that must inherit from
`sfCache` (see the Cache section for more information).

`i18n`
------

*sfContext Accessor*: `$context->getI18N()`

*Configurazione standard*:

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

>**ATTENZIONE**
>This factory is only defined if the [`i18n`](#chapter_04_sub_i18n)
>setting is set to `on`.

### ~`source`~

The `source` option defines the container type for translations.

*Built-in containers*: `XLIFF`, `SQLite`, `MySQL`, and `gettext`.

### ~`debug`~

The `debug` option sets the debugging mode. If set to `on`, un-translated
messages are decorated with a prefix and a suffix (see below).

### ~`untranslated_prefix`~

The `untranslated_prefix` defines a prefix to used for un-translated messages.

### ~`untranslated_suffix`~

The `untranslated_suffix` defines a suffix to used for un-translated messages.

### ~`cache`~

The `cache` option defines a anonymous cache factory to be used for caching
i18n data (see the Cache section for more information).

`routing`
---------

*sfContext Accessor*: `$context->getRouting()`

*Configurazione standard*:

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
        generate_shortest_url:            true
        extra_parameters_as_query_string: true
        cache:
          class: sfFileCache
          param:
            automatic_cleaning_factor: 0
            cache_dir:                 %SF_CONFIG_CACHE_DIR%/routing
            lifetime:                  31556926
            prefix:                    %SF_APP_DIR%/routing

### ~`variable_prefixes`~

*Default*: `:`

The `variable_prefixes` option defines the list of characters that starts a
variable name in a route pattern.

### ~`segment_separators`~

*Default*: `/` and `.`

The `segment_separators` option defines the list of route segment separators.
Most of the time, you don't want to override this option for the whole
routing, but for specific routes.

### ~`generate_shortest_url`~

*Default*: `true` for new projects, `false` for upgraded projects

If set to `true`, the `generate_shortest_url` option will tell the routing
system to generate the shortest route possible. Set it to `false` if you want
your routes to be backward compatible with symfony 1.0 and 1.1.

### ~`extra_parameters_as_query_string`~

*Default*: `true` for new projects, `false` for upgraded projects

When some parameters are not used in the generation of a route, the
`extra_parameters_as_query_string` allows those extra parameters to be
converted to a query string. Set it to `false` to fallback to the behavior of
symfony 1.0 or 1.1. In those versions, the extra parameters were just ignored
by the routing system.

### ~`cache`~

The `cache` option defines an anonymous cache factory to be used for caching
routing configuration and data (see the Cache section for more information).

### ~`suffix`~

*Default*: none

The default suffix to use for all routes. This option is deprecated and is not
useful anymore.

### ~`load_configuration`~

*Default*: `true`

The `load_configuration` option defines whether the `routing.yml` files must
be automatically loaded and parsed. Set it to `false` if you want to use the
routing system of symfony outside of a symfony project.

### ~`lazy_routes_deserialize`~

*Default*: `false`

If set to `true`, the `lazy_routes_deserialize` setting enables lazy
unserialization of the routing cache. It can improve the performance of your
applications if you have a large number of routes and if most matching routes
are among the first ones. It is strongly advised to test the setting before
deploying to production, as it can harm your performance in certain
circumstances.

>**ATTENZIONE**
>This setting is only available for symfony 1.2.7 and up.

### ~`lookup_cache_dedicated_keys`~

*Default*: `false`

The `lookup_cache_dedicated_keys` setting determines how the routing cache is
constructed. When set to `false`, the cache is stored as one big value; when
set to `true`, each route has its own cache store. This setting is a
performance optimization setting.

As a rule of thumb, setting this to `false` is better when using a file-based
cache class (`sfFileCache` for instance), and setting it to `true` is better
when using a memory-based cache class (`sfAPCCache` for instance).

>**ATTENZIONE**
>Questo parametro è disponibile solo dalla versione 1.2.7 di symfony in avanti.

`logger`
--------

*sfContext Accessor*: `$context->getLogger()`

*Configurazione standard*:

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

*Configurazione standard for the `prod` environment*:

    [yml]
    logger:
      class:   sfNoLogger
      param:
        level:   err
        loggers: ~

>**ATTENZIONE**
>This factory is always defined, but the logging only occurs if the
>`logging_enabled` setting is set to `on`.

### ~`level`~

The `level` option defines the level of the logger.

*Possible values*: `EMERG`, `ALERT`, `CRIT`, `ERR`, `WARNING`, `NOTICE`,
`INFO`, or `DEBUG`.

### ~`loggers`~

The `loggers` option defines a list of loggers to use. The list is an array of
anonymous logger factories.

*Built-in logger classes*: `sfConsoleLogger`, `sfFileLogger`, `sfNoLogger`,
`sfStreamLogger`, and `sfVarLogger`.

`controller`
------------

*sfContext Accessor*: `$context->getController()`

*Configurazione standard*:

    [yml]
    controller:
      class: sfFrontWebController

Anonymous Cache Factories
-------------------------

Several factories (`view_cache`, `i18n`, and `routing`) can take advantage of
a cache object if defined in their configuration. The configuration of the
cache object is similar for all factories. The `cache` key defines an
anonymous cache factory. Like any other factory, it takes a `class` and a
`param` entries. The `param` entry can take any option available for the given
cache class.

The `prefix` option is the most important one as it allows to share or
separate a cache between different environments/applications/projects.

*Built-in cache classes*: `sfAPCCache`, `sfEAcceleratorCache`, `sfFileCache`,
`sfMemcacheCache`, `sfNoCache`, `sfSQLiteCache`, and `sfXCacheCache`.
