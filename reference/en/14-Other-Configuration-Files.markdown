Other Configuration Files
=========================

This chapter describes other symfony configuration files, that rarely need to
be changed.

~`autoload.yml`~
----------------

The `autoload.yml` configuration determines which directories need to be
autoloaded by symfony. Each directory is scanned for PHP classes and
interfaces.

As discussed in the introduction, the `autoload.yml` file benefits from the
[**configuration cascade mechanism**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade), and
can include [**constants**](#chapter_03-Configuration-Files-Principles_sub_constants).

>**NOTE**
>The `autoload.yml` configuration file is cached as a PHP file; the
>process is automatically managed by the ~`sfAutoloadConfigHandler`~
>[class](#chapter_14-Other-Configuration-Files_sub_config_handlers_yml).

The default configuration is fine for most projects:

    [yml]
    autoload:
      # project
      project:
        name:           project
        path:           %SF_LIB_DIR%
        recursive:      on
        exclude:        [model, symfony]

      project_model:
        name:           project model
        path:           %SF_LIB_DIR%/model
        recursive:      on

      # application
      application:
        name:           application
        path:           %SF_APP_LIB_DIR%
        recursive:      on

      modules:
        name:           module
        path:           %SF_APP_DIR%/modules/*/lib
        prefix:         1
        recursive:      on

Each configuration has a name and must be set under a key with that name. It
allows for the default configuration to be overridden.

>**TIP**
>As you can see, the `lib/vendor/symfony/` directory is excluded by default,
>as symfony uses a different autoloading mechanism for core classes.

Several keys can be used to customize the autoloading behavior:

 * `name`: A description
 * `path`: The path to autoload
 * `recursive`: Whether to look for PHP classes in sub-directories
 * `exclude`: An array of directory names to exclude from the search
 * `prefix`: Set to `true` if the classes found in the path should only be autoloaded for a given module (`false` by default)
 * `files`: An array of files to explicitly parse for PHP classes
 * `ext`: The extension of PHP classes (`.php` by default)

For instance, if you embed a large library within your project under the
`lib/` directory, and if it already supports autoloading, you can exclude it
from the symfony default autoloading system to benefit from a performance
boost by modifying the `project` autoload configuration:

    [yml]
    autoload:
      project:
        name:           project
        path:           %SF_LIB_DIR%
        recursive:      on
        exclude:        [model, symfony, vendor/large_lib]

~`config_handlers.yml`~
-----------------------

The `config_handlers.yml` configuration file describes the configuration
handler classes used to parse and interpret all other YAML configuration
files. Here is the default configuration used to load the `settings.yml`
configuration file:

    [yml]
    config/settings.yml:
      class:    sfDefineEnvironmentConfigHandler
      param:
        prefix: sf_

Each configuration file is defined by a class (`class` entry) and can be
further customized by defining some parameters under the `param` section.

The default `config_handlers.yml` file defines the parser classes as follows:

 | Configuration File | Config Handler Class               |
 | ------------------ | ---------------------------------- |
 | `autoload.yml`     | `sfAutoloadConfigHandler`          |
 | `databases.yml`    | `sfDatabaseConfigHandler`          |
 | `settings.yml`     | `sfDefineEnvironmentConfigHandler` |
 | `app.yml`          | `sfDefineEnvironmentConfigHandler` |
 | `factories.yml`    | `sfFactoryConfigHandler`           |
 | `core_compile.yml` | `sfCompileConfigHandler`           |
 | `filters.yml`      | `sfFilterConfigHandler`            |
 | `routing.yml`      | `sfRoutingConfigHandler`           |
 | `generator.yml`    | `sfGeneratorConfigHandler`         |
 | `view.yml`         | `sfViewConfigHandler`              |
 | `security.yml`     | `sfSecurityConfigHandler`          |
 | `cache.yml`        | `sfCacheConfigHandler`             |
 | `module.yml`       | `sfDefineEnvironmentConfigHandler` |

~`core_compile.yml`~
--------------------

The `core_compile.yml` configuration file describes the PHP files that are
merged into one big file in the `prod` environment, to speed up the time it
takes for symfony to load. By default, the main symfony core classes are
defined in this configuration file. If your application relies on some classes
that need to be loaded for each request, you can create a `core_compile.yml`
configuration file in your project or application and add them to it. Here is
an extract of the default configuration:

    [yml]
    - %SF_SYMFONY_LIB_DIR%/autoload/sfAutoload.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfComponent.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfAction.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfActions.class.php

As discussed in the introduction, the `core_compile.yml` file benefits from the
[**configuration cascade mechanism**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade), and
can include [**constants**](#chapter_03-Configuration-Files-Principles_sub_constants).

>**NOTE**
>The `core_compile.yml` configuration file is cached as a PHP file; the
>process is automatically managed by the ~`sfCompileConfigHandler`~
>[class](#chapter_14-Other-Configuration-Files_sub_config_handlers_yml).
