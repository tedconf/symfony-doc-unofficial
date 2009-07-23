The databases.yml Configuration File
====================================

The ~`databases.yml`~ configuration allows for the configuration of the
database connection. It is used by both ORMs bundled with symfony: Propel and
Doctrine.

The main `databases.yml` configuration file for a project can be found in
the `config/` directory.

>**NOTE**
>Most of the time, all applications of a project share the same
>database. That's why the main database configuration file is in the
>project `config/` directory. You can of course override the default
>configuration by defining a `databases.yml` configuration file in your
>application configuration directories.

As discussed in the introduction, the `databases.yml` file is
[**environment-aware**](#chapter_03-Configuration-Files-Principles_sub_environment_awareness), benefits from
the [**configuration cascade mechanism**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade),
and can include [**constants**](#chapter_03-Configuration-Files-Principles_sub_constants).

Each connection described in `databases.yml` must include a name, a database
handler class name, and a set of parameters (`param`) used to configure the
database object:

    [yml]
    CONNECTION_NAME:
      class: CLASS_NAME
      param: { ARRAY OF PARAMETERS }

The `class` name should extend the `sfDatabase` base class.

If the database handler class cannot be autoloaded, a `file` path can be
defined and will be automatically included before the factory is created:

    [yml]
    CONNECTION_NAME:
      class: CLASS_NAME
      file:  ABSOLUTE_PATH_TO_FILE

>**NOTE**
>The `databases.yml` configuration file is cached as a PHP file; the
>process is automatically managed by the ~`sfDatabaseConfigHandler`~
>[class](#chapter_14-Other-Configuration-Files_sub_config_handlers_yml).

-

>**TIP**
>The database configuration can also be configured by using the
>`database:configure` task.  This task updates the `databases.yml`
>according to the arguments you pass to it.

Propel
------

*Default Configuration*:

    [yml]
    dev:
      propel:
        param:
          classname:  DebugPDO

    test:
      propel:
        param:
          classname:  DebugPDO

    all:
      propel:
        class:        sfPropelDatabase
        param:
          classname:  PropelPDO
          dsn:        mysql:dbname=##PROJECT_NAME##;host=localhost
          username:   root
          password:   
          encoding:   utf8
          persistent: true
          pooling:    true

The following parameters can be customized under the `param` section:

 | Key          | Description                              | Default Value |
 | ------------ | ---------------------------------------- | ------------- |
 | `classname`  | The Propel adapter class                 | `PropelPDO`   |
 | `dsn`        | The PDO DSN (required)                   | -             |
 | `username`   | The database username                    | -             |
 | `password`   | The database password                    | -             |
 | `pooling`    | Whether to enable pooling                | `true`        |
 | `encoding`   | The default charset                      | `UTF-8`       |
 | `persistent` | Whether to create persistent connections | `false`       |
 | `options`    | A set of Propel options                  | -             |

Doctrine
--------

*Default Configuration*:

    [yml]
    all:
      doctrine:
        class:        sfDoctrineDatabase
        param:
          dsn:        mysql:dbname=##PROJECT_NAME##;host=localhost
          username:   root
          password:   
          attributes:
            quote_identifier: false
            use_native_enum: false
            validate: all
            idxname_format: %s_idx
            seqname_format: %s_seq
            tblname_format: %s

The following parameters can be customized under the `param` section:

 | Key          | Description                              | Default Value |
 | ------------ | ---------------------------------------- | ------------- |
 | `dsn`        | The PDO DSN (required)                   | -             |
 | `username`   | The database username                    | -             |
 | `password`   | The database password                    | -             |
 | `encoding`   | The default charset                      | `UTF-8`       |
 | `attributes` | A set of Doctrine attributes             | -             |

The following attributes can be customized under the `attributes` section:

 | Key                 | Description                              | Default Value |
 | ------------------- | ---------------------------------------- | ------------- |
 | `quote_identifier`  | Whether to wrap identifiers with quotes  | `false`       |
 | `use_native_enum`   | Whether to use native enums              | `false`       |
 | `validate`          | Whether to enable data validation        | `true`        |
 | `idxname_format`    | Format for index names                   | `%s_idx`      |
 | `seqname_format`    | Format for sequence names                | `%s_seq`      |
 | `tblname_format`    | Format for table names                   | `%s`          |
