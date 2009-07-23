The settings.yml Configuration File
===================================

Most aspects of symfony can be configured either via a configuration file
written in YAML, or with plain PHP. In this section, the main configuration
file for an application, `settings.yml`, will be described.

The main `settings.yml` configuration file for an application can be found in
the `apps/APP_NAME/config/` directory.

As discussed in the introduction, the `settings.yml` file is
[**environment-aware**](03-Configuration-Files-Principles#chapter_03_environment_awareness), and benefits from
the [**configuration cascade mechanism**](03-Configuration-Files-Principles#chapter_03_configuration_cascade).

Each environment section has two sub-sections: `.actions` and `.settings`. All
configuration directives go under the `.settings` sub-section, except for the
default actions to be rendered for some common pages.

>**NOTE**
>The `settings.yml` configuration file is cached as a PHP file; the process is
>automatically managed by the ~`sfDefineEnvironmentConfigHandler`~
>[class](14-Other-Configuration-Files#chapter_14_config_handlers_yml).

<div class="pagebreak"></div>

Settings
--------

  * `.actions`

    * [`error_404`](04-Settings#chapter_04_sub_error_404)
    * [`login`](04-Settings#chapter_04_sub_error_404)
    * [`secure`](04-Settings#chapter_04_sub_secure)
    * [`module_disabled`](04-Settings#chapter_04_sub_module_disabled)

  * `.settings`

    * [`cache`](04-Settings#chapter_04_sub_cache)
    * [`charset`](04-Settings#chapter_04_sub_charset)
    * [`check_lock`](04-Settings#chapter_04_sub_check_lock)
    * [`check_symfony_version`](04-Settings#chapter_04_sub_check_symfony_version)
    * [`compressed`](04-Settings#chapter_04_sub_compressed)
    * [`csrf_secret`](04-Settings#chapter_04_sub_csrf_secret)
    * [`default_culture`](04-Settings#chapter_04_sub_default_culture)
    * [`default_timezone`](04-Settings#chapter_04_sub_default_timezone)
    * [`enabled_modules`](04-Settings#chapter_04_sub_enabled_modules)
    * [`error_reporting`](04-Settings#chapter_04_sub_error_reporting)
    * [`escaping_strategy`](04-Settings#chapter_04_sub_escaping_strategy)
    * [`escaping_method`](04-Settings#chapter_04_sub_escaping_method)
    * [`etag`](04-Settings#chapter_04_sub_etag)
    * [`i18n`](04-Settings#chapter_04_sub_i18n)
    * [`lazy_cache_key`](04-Settings#chapter_04_sub_lazy_cache_key)
    * [`logging_enabled`](04-Settings#chapter_04_sub_logging_enabled)
    * [`no_script_name`](04-Settings#chapter_04_sub_no_script_name)
    * [`max_forwards`](04-Settings#chapter_04_sub_max_forwards)
    * [`standard_helpers`](04-Settings#chapter_04_sub_standard_helpers)
    * [`strip_comments`](04-Settings#chapter_04_sub_strip_comments)
    * [`use_database`](04-Settings#chapter_04_sub_use_database)
    * [`web_debug`](04-Settings#chapter_04_sub_web_debug)
    * [`web_debug_web_dir`](04-Settings#chapter_04_sub_web_debug_web_dir)

<div class="pagebreak"></div>

The `.actions` Sub-Section
--------------------------

*Default configuration*:

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

The `.actions` sub-section defines the action to execute when common pages
must be rendered. Each definition has two components: one for the module
(suffixed by `_module`), and one for the action (suffixed by `_action`).

### ~`error_404`~

The `error_404` action is executed when a 404 page must be rendered.

### ~`login`~

The `login` action is executed when a non-authenticated user tries to access a
secure page.

### ~`secure`~

The `secure` action is executed when a user doesn't have the required
credentials.

### ~`module_disabled`~

The `module_disabled` action is executed when a user requests a disabled
module.

The `.settings` Sub-Section
---------------------------

The `.settings` sub-section is where the framework configuration occurs. The
paragraphs below describe all possible settings and are roughly ordered by
importance.

All settings defined in the `.settings` section are available anywhere in the
code by using the `sfConfig` object and prefixing the setting with `sf_`. For
instance, to get the value of the `charset` setting, use:

    [php]
    sfConfig::get('sf_charset');

### ~`escaping_strategy`~

*Default*: `off`

The `escaping_strategy` setting is a Boolean setting that determines if the
output escaper sub-framework is enabled. When enabled, all variables made
available in the templates are automatically escaped by calling the helper
function defined by the `escaping_method` setting (see below).

Be careful that the `escaping_method` is the default helper used by symfony,
but this can be overridden on a case by case basis, when outputting a variable
in a JavaScript script tag for example.

The output escaper sub-framework uses the `charset` setting for the escaping.

It is highly recommended to change the default value to `on`.

>**TIP**
>This settings can be set when you create an application with the
>`generate:app` task by using the `--escaping-strategy` option.

### ~`escaping_method`~

*Default*: `ESC_SPECIALCHARS`

The `escaping_method` defines the default function to use for escaping
variables in templates (see the `escaping_strategy` setting above).

You can choose one of the built-in values: ~`ESC_SPECIALCHARS`~, ~`ESC_RAW`~,
~`ESC_ENTITIES`~, ~`ESC_JS`~, ~`ESC_JS_NO_ENTITIES`~, and
~`ESC_SPECIALCHARS`~, or create your own function.

Most of the time, the default value is fine. The `ESC_ENTITIES` helper can
also be used, especially if you are only working with English or European
languages.

### ~`csrf_secret`~

*Default*: `false`

The `csrf_secret` is a unique secret for your application. If not set to
`false`, it enables CSRF protection for all forms defined with the form
framework. This settings is also used by the `link_to()` helper when it needs
to convert a link to a form (to simulate a `DELETE` HTTP method for example).

It is highly recommended to change the default value to a unique secret.

>**TIP**
>This settings can be set when you create an application with the
>`generate:app` task by using the `--csrf-secret` option.

### ~`charset`~

*Default*: `utf-8`

The `charset` setting is the charset that will be used everywhere in the
framework: from the response `Content-Type` header, to the output escaping
feature.

Most of the time, the default is fine.

### ~`enabled_modules`~

*Default*: `[default]`

The `enabled_modules` is an array of module names to enable for this
application. Modules defined in plugins or in the symfony core are not enabled
by default, and must be listed in this setting to be accessible.

Adding a module is as simple as appending it to the list (the order of the
modules do not matter):

    [yml]
    enabled_modules: [default, sfGuardAuth]

The `default` module defined in the framework contains all the default actions
set in the `.actions` sub-section of `settings.yml`. It is recommended that
you customize all of them, and then remove the `default` module from this
setting.

### ~`default_timezone`~

*Default*: none

The `default_timezone` setting defines the default timezone used by PHP. It
can be any [timezone](http://www.php.net/manual/en/class.datetimezone.php)
recognized by PHP.

>**NOTE**
>If you don't define a timezone, you are advised to define one in the
>`php.ini` file. If not, symfony will try to guess the best timezone by
>calling the
>[`date_default_timezone_get()`](http://www.php.net/date_default_timezone_get)
>PHP function.

### ~`cache`~

*Default*: `off`

The `cache` setting enables or disables template caching.

>**TIP**
>The general configuration of the cache system is done in
>the [`view_cache_manager`](05-Factories#chapter_05_view_cache_manager) and
>[`view_cache`](05-Factories#chapter_05_view_cache) sections of the `factories.yml`
>configuration file. The fined-grained configuration is done in
>the [`cache.yml`](09-Cache#chapter_09) configuration file.

### ~`etag`~

*Default*: `on` by default except for the `dev` and `test` environments

The `etag` setting enables or disables the automatic generation of `ETag` HTTP
headers. The ETag generated by symfony is a simple md5 of the response
content.

### ~`i18n`~

*Default*: `off`

The `i18n` setting is a Boolean that enables or disables the i18n
sub-framework. If your application is internationalized, set it to `on`.

>**TIP**
>The general configuration of the i18n system is to be done in the
>[`i18n`](05-Factories#chapter_05_i18n) section of the `factories.yml` configuration
>file.

### ~`default_culture`~

*Default*: `en`

The `default_culture` setting defines the default culture used by the i18n
sub-framework. It can be any valid culture.

### ~`standard_helpers`~

*Default*: `[Partial, Cache, Form]`

The `standard_helpers` setting is an array of helper groups to load for all
templates (name of the group helper without the `Helper` suffix).

### ~`no_script_name`~

*Default*: `on` for the `prod` environment of the first application created,
`off` for all others

The `no_script_name` setting determines whether the front controller script
name is prepended to generated URLs or not. By default, it is set to `on` by
the `generate:app` task for the `prod` environment of the first application
created.

Obviously, only one application and environment can have this setting set to
`on` if all front controllers are in the same directory (`web/`). If you want
more than one application with `no_script_name` set to `on`, move the
corresponding front controller(s) under a sub-directory of the web root
directory.

### ~`lazy_cache_key`~

*Default*: `on` for new projects, `off` for upgraded projects

When enabled, the `lazy_cache_key` setting delays the creation of a cache key
until after checking whether an action or partial is cacheable. This can
result in a big performance improvement, depending on your usage of template
partials.

This setting was introduced in symfony 1.2.7 to improve performance without
breaking backward compatibility with previous 1.2 releases. It will be removed
in symfony 1.3 as the optimization will always be enabled.

>**CAUTION**
>This setting is only available for symfony 1.2.7 and up.

### ~`logging_enabled`~

*Default*: `on` for all environments except `prod`

The `logging_enabled` setting enables the logging sub-framework. Setting it to
`false` bypasses the logging mechanism completely and provides a small
performance gain.

>**TIP**
>The fined-grained configuration of the logging is to be done in the
>`factories.yml` configuration file.

### ~`web_debug`~

*Default*: `off` for all environments except `dev`

The `web_debug` setting enables the web debug toolbar. The web debug toolbar
is injected into a page when the response content type is HTML.

### ~`error_reporting`~

*Default*:

  * `prod`:  E_PARSE | E_COMPILE_ERROR | E_ERROR | E_CORE_ERROR | E_USER_ERROR
  * `dev`:   E_ALL | E_STRICT
  * `test`:  (E_ALL | E_STRICT) ^ E_NOTICE
  * default: E_PARSE | E_COMPILE_ERROR | E_ERROR | E_CORE_ERROR | E_USER_ERROR

The `error_reporting` setting controls the level of PHP error reporting (to be
displayed in the browser and written to the logs).

>**TIP**
>The PHP website has some information about how to use
>[bitwise operators](http://www.php.net/language.operators.bitwise).

The default configuration is the most sensible one, and should not be altered.

>**NOTE**
>The display of errors in the browser is automatically disabled for
>front controllers that have `debug` disabled, which is the case by default
>for the `prod` environment.

### ~`compressed`~

*Default*: `off`

The `compressed` setting enables native PHP response compression. If set to
`on`, symfony will use [`ob_gzhandler`](http://www.php.net/ob_gzhandler) as a
callback function for `ob_start()`.

It is recommended to keep it to `off`, and use the native compression
mechanism of your web server instead.

### ~`use_database`~

*Default*: `on`

The `use_database` determines if the application uses a database or not.

### ~`check_lock`~

*Default*: `off`

The `check_lock` setting enables or disables the application lock system
triggered by some tasks like `cache:clear` and `project:disable`.

If set to `on`, all requests to disabled applications are automatically
redirected to the symfony core `lib/exception/data/unavailable.php` page.

>**TIP**
>You can override the default unavailable template by adding a
>`config/unavailable.php` file to your project or application.

### ~`check_symfony_version`~

*Default*: `off`

The `check_symfony_version` enables or disables checking of the current
symfony version upon every request. If enabled, symfony will clear the cache
automatically when the framework is upgraded.

It is highly recommended to not set this to `on` as it adds a small overhead,
and because it is simple to just clear the cache when you deploy a new version
of your project. This setting is only useful if several projects share the
same symfony code, which is not recommended.

### ~`web_debug_web_dir`~

*Default*: `/sf/sf_web_debug`

The `web_debug_web_dir` sets the web path to the web debug toolbar assets
(images, stylesheets, and JavaScript files).

### ~`strip_comments`~

*Default*: `on`

The `strip_comments` determines if symfony should strip the comments when
compiling core classes. This setting is only used if the application `debug`
setting is set to `off`.

If you have blank pages in the production environment only, try to set this
setting to `off`.

### ~`max_forwards`~

*Default*: `5`

The `max_forwards` setting sets the maximum number of internal forwards
allowed before symfony throws an exception. This is to avoid infinite loops.
