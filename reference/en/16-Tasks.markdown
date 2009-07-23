Tasks
=====

The symfony framework comes bundled with a command line interface tool.
Built-in tasks allow the developer to perform a lot of fastidious and
recurrent tasks in the life of a project.

If you execute the `symfony` CLI without any arguments, a list of available
tasks is displayed:

    $ php symfony

By passing the `-V` option, you get some information about the version of
symfony and the path of the symfony libraries used by the CLI:

    $ php symfony -V

The CLI tool takes a task name as its first argument:

    $ php symfony list

A task name can be composed of an optional namespace and a name, separated by
a colon (`:`):

    $ php symfony cache:clear

After the task name, arguments and options can be passed:

    $ php symfony cache:clear --type=template

The CLI tool supports both long options and short ones, with or without
values.

The `-t` option is a global option to ask any task to output more debugging
information.

<div class="pagebreak"></div>

Available Tasks
---------------

 * Global tasks
   * [`help`](16-Tasks#chapter_16_sub_help)
   * [`list`](16-Tasks#chapter_16_sub_list)
 * [`app`](16-Tasks#chapter_16_app)
   * [`app::routes`](16-Tasks#chapter_16_sub_app_routes)
 * [`cache`](16-Tasks#chapter_16_cache)
   * [`cache::clear`](16-Tasks#chapter_16_sub_cache_clear)
 * [`configure`](16-Tasks#chapter_16_configure)
   * [`configure::author`](16-Tasks#chapter_16_sub_configure_author)
   * [`configure::database`](16-Tasks#chapter_16_sub_configure_database)
 * [`doctrine`](16-Tasks#chapter_16_doctrine)
   * [`doctrine::build-all`](16-Tasks#chapter_16_sub_doctrine_build_all)
   * [`doctrine::build-all-load`](16-Tasks#chapter_16_sub_doctrine_build_all_load)
   * [`doctrine::build-all-reload`](16-Tasks#chapter_16_sub_doctrine_build_all_reload)
   * [`doctrine::build-all-reload-test-all`](16-Tasks#chapter_16_sub_doctrine_build_all_reload_test_all)
   * [`doctrine::build-db`](16-Tasks#chapter_16_sub_doctrine_build_db)
   * [`doctrine::build-filters`](16-Tasks#chapter_16_sub_doctrine_build_filters)
   * [`doctrine::build-forms`](16-Tasks#chapter_16_sub_doctrine_build_forms)
   * [`doctrine::build-model`](16-Tasks#chapter_16_sub_doctrine_build_model)
   * [`doctrine::build-schema`](16-Tasks#chapter_16_sub_doctrine_build_schema)
   * [`doctrine::build-sql`](16-Tasks#chapter_16_sub_doctrine_build_sql)
   * [`doctrine::data-dump`](16-Tasks#chapter_16_sub_doctrine_data_dump)
   * [`doctrine::data-load`](16-Tasks#chapter_16_sub_doctrine_data_load)
   * [`doctrine::dql`](16-Tasks#chapter_16_sub_doctrine_dql)
   * [`doctrine::drop-db`](16-Tasks#chapter_16_sub_doctrine_drop_db)
   * [`doctrine::generate-admin`](16-Tasks#chapter_16_sub_doctrine_generate_admin)
   * [`doctrine::generate-migration`](16-Tasks#chapter_16_sub_doctrine_generate_migration)
   * [`doctrine::generate-migrations-db`](16-Tasks#chapter_16_sub_doctrine_generate_migrations_db)
   * [`doctrine::generate-migrations-models`](16-Tasks#chapter_16_sub_doctrine_generate_migrations_models)
   * [`doctrine::generate-module`](16-Tasks#chapter_16_sub_doctrine_generate_module)
   * [`doctrine::generate-module-for-route`](16-Tasks#chapter_16_sub_doctrine_generate_module_for_route)
   * [`doctrine::insert-sql`](16-Tasks#chapter_16_sub_doctrine_insert_sql)
   * [`doctrine::migrate`](16-Tasks#chapter_16_sub_doctrine_migrate)
   * [`doctrine::rebuild-db`](16-Tasks#chapter_16_sub_doctrine_rebuild_db)
 * [`generate`](16-Tasks#chapter_16_generate)
   * [`generate::app`](16-Tasks#chapter_16_sub_generate_app)
   * [`generate::module`](16-Tasks#chapter_16_sub_generate_module)
   * [`generate::project`](16-Tasks#chapter_16_sub_generate_project)
   * [`generate::task`](16-Tasks#chapter_16_sub_generate_task)
 * [`i18n`](16-Tasks#chapter_16_i18n)
   * [`i18n::extract`](16-Tasks#chapter_16_sub_i18n_extract)
   * [`i18n::find`](16-Tasks#chapter_16_sub_i18n_find)
 * [`log`](16-Tasks#chapter_16_log)
   * [`log::clear`](16-Tasks#chapter_16_sub_log_clear)
   * [`log::rotate`](16-Tasks#chapter_16_sub_log_rotate)
 * [`plugin`](16-Tasks#chapter_16_plugin)
   * [`plugin::add-channel`](16-Tasks#chapter_16_sub_plugin_add_channel)
   * [`plugin::install`](16-Tasks#chapter_16_sub_plugin_install)
   * [`plugin::list`](16-Tasks#chapter_16_sub_plugin_list)
   * [`plugin::publish-assets`](16-Tasks#chapter_16_sub_plugin_publish_assets)
   * [`plugin::uninstall`](16-Tasks#chapter_16_sub_plugin_uninstall)
   * [`plugin::upgrade`](16-Tasks#chapter_16_sub_plugin_upgrade)
 * [`project`](16-Tasks#chapter_16_project)
   * [`project::clear-controllers`](16-Tasks#chapter_16_sub_project_clear_controllers)
   * [`project::deploy`](16-Tasks#chapter_16_sub_project_deploy)
   * [`project::disable`](16-Tasks#chapter_16_sub_project_disable)
   * [`project::enable`](16-Tasks#chapter_16_sub_project_enable)
   * [`project::freeze`](16-Tasks#chapter_16_sub_project_freeze)
   * [`project::permissions`](16-Tasks#chapter_16_sub_project_permissions)
   * [`project::unfreeze`](16-Tasks#chapter_16_sub_project_unfreeze)
   * [`project::upgrade1.1`](16-Tasks#chapter_16_sub_project_upgrade1_1)
   * [`project::upgrade1.2`](16-Tasks#chapter_16_sub_project_upgrade1_2)
 * [`propel`](16-Tasks#chapter_16_propel)
   * [`propel::build-all`](16-Tasks#chapter_16_sub_propel_build_all)
   * [`propel::build-all-load`](16-Tasks#chapter_16_sub_propel_build_all_load)
   * [`propel::build-filters`](16-Tasks#chapter_16_sub_propel_build_filters)
   * [`propel::build-forms`](16-Tasks#chapter_16_sub_propel_build_forms)
   * [`propel::build-model`](16-Tasks#chapter_16_sub_propel_build_model)
   * [`propel::build-schema`](16-Tasks#chapter_16_sub_propel_build_schema)
   * [`propel::build-sql`](16-Tasks#chapter_16_sub_propel_build_sql)
   * [`propel::data-dump`](16-Tasks#chapter_16_sub_propel_data_dump)
   * [`propel::data-load`](16-Tasks#chapter_16_sub_propel_data_load)
   * [`propel::generate-admin`](16-Tasks#chapter_16_sub_propel_generate_admin)
   * [`propel::generate-module`](16-Tasks#chapter_16_sub_propel_generate_module)
   * [`propel::generate-module-for-route`](16-Tasks#chapter_16_sub_propel_generate_module_for_route)
   * [`propel::graphviz`](16-Tasks#chapter_16_sub_propel_graphviz)
   * [`propel::init-admin`](16-Tasks#chapter_16_sub_propel_init_admin)
   * [`propel::insert-sql`](16-Tasks#chapter_16_sub_propel_insert_sql)
   * [`propel::schema-to-xml`](16-Tasks#chapter_16_sub_propel_schema_to_xml)
   * [`propel::schema-to-yml`](16-Tasks#chapter_16_sub_propel_schema_to_yml)
 * [`test`](16-Tasks#chapter_16_test)
   * [`test::all`](16-Tasks#chapter_16_sub_test_all)
   * [`test::coverage`](16-Tasks#chapter_16_sub_test_coverage)
   * [`test::functional`](16-Tasks#chapter_16_sub_test_functional)
   * [`test::unit`](16-Tasks#chapter_16_sub_test_unit)


<div class="pagebreak"></div>

### ~`help`~

The `help` task displays help for a task:

    $ php symfony help  [task_name]

*Alias(es)*: `h`

| Argument | Default | Description
| -------- | ------- | -----------
| `task_name` | `help` | The task name






### ~`list`~

The `list` task lists tasks:

    $ php symfony list  [namespace]



| Argument | Default | Description
| -------- | ------- | -----------
| `namespace` | `-` | The namespace name




The `list` task lists all tasks:

    ./symfony list

You can also display the tasks for a specific namespace:

    ./symfony list test

`app`
-----

### ~`app::routes`~

The `app::routes` task displays current routes for an application:

    $ php symfony app:routes  application [name]



| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `name` | `-` | A route name




The `app:routes` displays the current routes for a given application:

    ./symfony app:routes frontend

`cache`
-------

### ~`cache::clear`~

The `cache::clear` task clears the cache:

    $ php symfony cache:clear [--app[="..."]] [--env[="..."]] [--type[="..."]] 

*Alias(es)*: `cc, clear-cache`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--app` | `-` | The application name
| `--env` | `-` | The environment
| `--type` | `all` | The type


The `cache:clear` task clears the symfony cache.

By default, it removes the cache for all available types, all applications,
and all environments.

You can restrict by type, application, or environment:

For example, to clear the `frontend` application cache:

    ./symfony cache:clear --app=frontend

To clear the cache for the `prod` environment for the `frontend` application:

    ./symfony cache:clear --app=frontend --env=prod

To clear the cache for all `prod` environments:

    ./symfony cache:clear --env=prod

To clear the `config` cache for all `prod` environments:

    ./symfony cache:clear --type=config --env=prod

The built-in types are: `config`, `i18n`, `routing`, `module`
and `template`.


`configure`
-----------

### ~`configure::author`~

The `configure::author` task configure project author:

    $ php symfony configure:author  author



| Argument | Default | Description
| -------- | ------- | -----------
| `author` | `-` | The project author




The `configure:author` task configures the author for a project:

    ./symfony configure:author "Fabien Potencier <fabien.potencier@symfony-project.com>"

The author is used by the generates to pre-configure the PHPDoc header for each generated file.

The value is stored in [config/properties.ini].

### ~`configure::database`~

The `configure::database` task configure database DSN:

    $ php symfony configure:database [--env[="..."]] [--name[="..."]] [--class[="..."]] [--app[="..."]] dsn [username] [password]



| Argument | Default | Description
| -------- | ------- | -----------
| `dsn` | `-` | The database dsn
| `username` | `root` | The database username
| `password` | `-` | The database password


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--env` | `all` | The environment
| `--name` | `propel` | The connection name
| `--class` | `sfPropelDatabase` | The database class name
| `--app` | `-` | The application name


The `configure:database` task configures the database DSN
for a project:

    ./symfony configure:database mysql:host=localhost;dbname=example root mYsEcret

By default, the task change the configuration for all environment. If you want
to change the dsn for a specific environment, use the `env` option:

    ./symfony configure:database --env=dev mysql:host=localhost;dbname=example_dev root mYsEcret

To change the configuration for a specific application, use the `app` option:

    ./symfony configure:database --app=frontend mysql:host=localhost;dbname=example root mYsEcret

You can also specify the connection name and the database class name:

    ./symfony configure:database --name=main --class=sfDoctrineDatabase mysql:host=localhost;dbname=example root

WARNING: The `propel.ini` file is also updated when you use a `Propel` database
and configure for `all` environments with no `app`.

`doctrine`
----------

### ~`doctrine::build-all`~

The `doctrine::build-all` task generates Doctrine model, SQL and initializes the database:

    $ php symfony doctrine:build-all [--application[="..."]] [--env="..."] [--no-confirmation] [--skip-forms|-F] 

*Alias(es)*: `doctrine-build-all`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--no-confirmation` | `-` | Do not ask for confirmation
| `--skip-forms`<br />`(-F)` | `-` | Skip generating forms


The `doctrine:build-all` task is a shortcut for three other tasks:

    ./symfony doctrine:build-all

The task is equivalent to:

    ./symfony doctrine:build-model
    ./symfony doctrine:build-sql
    ./symfony doctrine:build-forms
    ./symfony doctrine:insert-sql

See those three tasks help page for more information.

To bypass the confirmation, you can pass the `no-confirmation`
option:

    ./symfony doctrine:buil-all-load --no-confirmation

### ~`doctrine::build-all-load`~

The `doctrine::build-all-load` task generates Doctrine model, SQL, initializes database, and loads fixtures data:

    $ php symfony doctrine:build-all-load [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--skip-forms|-F] [--dir="..."] 

*Alias(es)*: `doctrine-build-all-load`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--connection` | `doctrine` | The connection name
| `--no-confirmation` | `-` | Do not ask for confirmation
| `--skip-forms`<br />`(-F)` | `-` | Skip generating forms
| `--dir` | `-` | The directories to look for fixtures (multiple values allowed)


The `doctrine:build-all-load` task is a shortcut for two other tasks:

    ./symfony doctrine:build-all-load

The task is equivalent to:

    ./symfony doctrine:build-all
    ./symfony doctrine:data-load

The task takes an application argument because of the `doctrine:data-load`
task. See `doctrine:data-load` help page for more information.

To bypass the confirmation, you can pass the `no-confirmation`
option:

    ./symfony doctrine:build-all-load --no-confirmation

### ~`doctrine::build-all-reload`~

The `doctrine::build-all-reload` task generates Doctrine model, SQL, initializes database, and load data:

    $ php symfony doctrine:build-all-reload [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--skip-forms|-F] [--dir="..."] 

*Alias(es)*: `doctrine-build-all-reload`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--connection` | `doctrine` | The connection name
| `--no-confirmation` | `-` | Do not ask for confirmation
| `--skip-forms`<br />`(-F)` | `-` | Skip generating forms
| `--dir` | `-` | The directories to look for fixtures (multiple values allowed)


The `doctrine:build-all-reload` task is a shortcut for five other tasks:

    ./symfony doctrine:build-all-reload

The task is equivalent to:

    ./symfony doctrine:drop-db
    ./symfony doctrine:build-db
    ./symfony doctrine:build-model
    ./symfony doctrine:insert-sql
    ./symfony doctrine:data-load

### ~`doctrine::build-all-reload-test-all`~

The `doctrine::build-all-reload-test-all` task generates Doctrine model, SQL, initializes database, load data and run all tests:

    $ php symfony doctrine:build-all-reload-test-all [--application[="..."]] [--env="..."] [--append] [--dir="..."] [--force] 

*Alias(es)*: `doctrine-build-all-reload-test-all`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--append` | `-` | Don't delete current data in the database
| `--dir` | `-` | The directories to look for fixtures (multiple values allowed)
| `--force` | `-` | Whether to force dropping of the database


The `doctrine:build-all-reload` task is a shortcut for four other tasks:

    ./symfony doctrine:build-all-reload-test-all frontend

The task is equivalent to:
  
    ./symfony doctrine:drop-db
    ./symfony doctrine:build-db
    ./symfony doctrine:build-model
    ./symfony doctrine:insert-sql
    ./symfony doctrine:data-load
    ./symfony test-all

The task takes an application argument because of the `doctrine:data-load`
task. See `doctrine:data-load` help page for more information.

### ~`doctrine::build-db`~

The `doctrine::build-db` task creates database for current model:

    $ php symfony doctrine:build-db [--application[="..."]] [--env="..."] 

*Alias(es)*: `doctrine-build-db`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:build-db` task creates the database:

    ./symfony doctrine:build-db

The task read connection information in `config/doctrine/databases.yml`:

### ~`doctrine::build-filters`~

The `doctrine::build-filters` task creates filter form classes for the current model:

    $ php symfony doctrine:build-filters [--connection="..."] [--model-dir-name="..."] [--filter-dir-name="..."] [--application[="..."]] [--env="..."] 





| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--connection` | `doctrine` | The connection name
| `--model-dir-name` | `model` | The model dir name
| `--filter-dir-name` | `filter` | The filter form dir name
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:build-filters` task creates filter form classes from the schema:

    ./symfony doctrine:build-filters

The task read the schema information in `config/*schema.xml` and/or
`config/*schema.yml` from the project and all installed plugins.

The task use the `doctrine` connection as defined in `config/databases.yml`.
You can use another connection by using the `--connection` option:

    ./symfony doctrine:build-filters --connection="name"

The model filter form classes files are created in `lib/filter`.

This task never overrides custom classes in `lib/filter`.
It only replaces base classes generated in `lib/filter/base`.

### ~`doctrine::build-forms`~

The `doctrine::build-forms` task creates form classes for the current model:

    $ php symfony doctrine:build-forms [--connection="..."] [--model-dir-name="..."] [--form-dir-name="..."] [--application[="..."]] [--env="..."] 





| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--connection` | `doctrine` | The connection name
| `--model-dir-name` | `model` | The model dir name
| `--form-dir-name` | `form` | The form dir name
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:build-forms` task creates form classes from the schema:

    ./symfony doctrine:build-forms

The task read the schema information in `config/*schema.xml` and/or
`config/*schema.yml` from the project and all installed plugins.

The task use the `doctrine` connection as defined in `config/databases.yml`.
You can use another connection by using the `--connection` option:

    ./symfony doctrine:build-forms --connection="name"

The model form classes files are created in `lib/form`.

This task never overrides custom classes in `lib/form`.
It only replaces base classes generated in `lib/form/base`.

### ~`doctrine::build-model`~

The `doctrine::build-model` task creates classes for the current model:

    $ php symfony doctrine:build-model [--application[="..."]] [--env="..."] 

*Alias(es)*: `doctrine-build-model`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:build-model` task creates model classes from the schema:

    ./symfony doctrine:build-model

The task read the schema information in `config/doctrine/*.yml`
from the project and all installed plugins.

The model classes files are created in `lib/model/doctrine`.

This task never overrides custom classes in `lib/model/doctrine`.
It only replaces files in `lib/model/doctrine/base`.

### ~`doctrine::build-schema`~

The `doctrine::build-schema` task creates a schema from an existing database:

    $ php symfony doctrine:build-schema [--application[="..."]] [--env="..."] 

*Alias(es)*: `doctrine-build-schema`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:build-schema` task introspects a database to create a schema:

    ./symfony doctrine:build-schema

The task creates a yml file in `config/doctrine`

### ~`doctrine::build-sql`~

The `doctrine::build-sql` task creates SQL for the current model:

    $ php symfony doctrine:build-sql [--application[="..."]] [--env="..."] 

*Alias(es)*: `doctrine-build-sql`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:build-sql` task creates SQL statements for table creation:

    ./symfony doctrine:build-sql

The generated SQL is optimized for the database configured in `config/databases.yml`:

    doctrine.database = mysql

### ~`doctrine::data-dump`~

The `doctrine::data-dump` task dumps data to the fixtures directory:

    $ php symfony doctrine:data-dump [--application[="..."]] [--env="..."] [target]

*Alias(es)*: `doctrine-dump-data`

| Argument | Default | Description
| -------- | ------- | -----------
| `target` | `-` | The target filename


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:data-dump` task dumps database data:

    ./symfony doctrine:data-dump

The task dumps the database data in `data/fixtures/%target%`.

The dump file is in the YML format and can be reimported by using
the `doctrine:data-load` task.

    ./symfony doctrine:data-load frontend

### ~`doctrine::data-load`~

The `doctrine::data-load` task loads data from fixtures directory:

    $ php symfony doctrine:data-load [--application[="..."]] [--env="..."] [--append] [--connection="..."] [--dir="..."] 

*Alias(es)*: `doctrine-load-data`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--append` | `-` | Don't delete current data in the database
| `--connection` | `doctrine` | The connection name
| `--dir` | `-` | The directories to look for fixtures (multiple values allowed)


The `doctrine:data-load` task loads data fixtures into the database:

    ./symfony doctrine:data-load frontend

The task loads data from all the files found in `data/fixtures/`.

If you want to load data from other directories, you can use
the `--dir` option:

    ./symfony doctrine:data-load --dir="data/fixtures" --dir="data/data" frontend

If you don't want the task to remove existing data in the database,
use the `--append` option:

    ./symfony doctrine:data-load --append frontend

### ~`doctrine::dql`~

The `doctrine::dql` task execute a DQL query and view the results:

    $ php symfony doctrine:dql [--application[="..."]] [--env="..."] [--show-sql] dql_query

*Alias(es)*: `doctrine-dql`

| Argument | Default | Description
| -------- | ------- | -----------
| `dql_query` | `-` | The DQL query to execute


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--show-sql` | `-` | Show the sql that would be executed


The `doctrine:data-dql` task executes a DQL query and display the formatted results:

    ./symfony doctrine:dql "FROM User u"

You can show the SQL that would be executed by using the `--dir` option:

    ./symfony doctrine:dql --show-sql "FROM User u"

### ~`doctrine::drop-db`~

The `doctrine::drop-db` task drops database for current model:

    $ php symfony doctrine:drop-db [--application[="..."]] [--env="..."] [--no-confirmation] 

*Alias(es)*: `doctrine-drop-db`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--no-confirmation` | `-` | Whether to force dropping of the database


The `doctrine:drop-db` task drops the database:

    ./symfony doctrine:drop-db

The task read connection information in `config/doctrine/databases.yml`:

### ~`doctrine::generate-admin`~

The `doctrine::generate-admin` task generates a Doctrine admin module:

    $ php symfony doctrine:generate-admin [--module="..."] [--theme="..."] [--singular="..."] [--plural="..."] [--env="..."] application route_or_model



| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `route_or_model` | `-` | The route name or the model class


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--module` | `-` | The module name
| `--theme` | `admin` | The theme name
| `--singular` | `-` | The singular name
| `--plural` | `-` | The plural name
| `--env` | `dev` | The environment


The `doctrine:generate-admin` task generates a Doctrine admin module:

    ./symfony doctrine:generate-admin frontend Article

The task creates a module in the `%frontend%` application for the
`%Article%` model.

The task creates a route for you in the application `routing.yml`.

You can also generate a Doctrine admin module by passing a route name:

    ./symfony doctrine:generate-admin frontend article

The task creates a module in the `%frontend%` application for the
`%article%` route definition found in `routing.yml`.

For the filters and batch actions to work properly, you need to add
the `wildcard` option to the route:

    article:
      class: sfDoctrineRouteCollection
      options:
        model:              Article
        with_wildcard_routes:   true

### ~`doctrine::generate-migration`~

The `doctrine::generate-migration` task generate migration class:

    $ php symfony doctrine:generate-migration [--application[="..."]] [--env="..."] name

*Alias(es)*: `doctrine-generate-migration`

| Argument | Default | Description
| -------- | ------- | -----------
| `name` | `-` | The name of the migration


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:generate-migration` task generates migration template

    ./symfony doctrine:generate-migration

### ~`doctrine::generate-migrations-db`~

The `doctrine::generate-migrations-db` task generate migration classes from existing database connections:

    $ php symfony doctrine:generate-migrations-db [--application[="..."]] [--env="..."] 

*Alias(es)*: `doctrine-generate-migrations-db, doctrine-gen-migrations-from-db`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:generate-migration` task generates migration classes from existing database connections

    ./symfony doctrine:generate-migration

### ~`doctrine::generate-migrations-models`~

The `doctrine::generate-migrations-models` task generate migration classes from an existing set of models:

    $ php symfony doctrine:generate-migrations-models [--application[="..."]] [--env="..."] 

*Alias(es)*: `doctrine-generate-migrations-models, doctrine-gen-migrations-from-models`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:generate-migration` task generates migration classes from an existing set of models

    ./symfony doctrine:generate-migration

### ~`doctrine::generate-module`~

The `doctrine::generate-module` task generates a Doctrine module:

    $ php symfony doctrine:generate-module [--theme="..."] [--generate-in-cache] [--non-verbose-templates] [--with-show] [--singular="..."] [--plural="..."] [--route-prefix="..."] [--with-doctrine-route] [--env="..."] application module model

*Alias(es)*: `doctrine-generate-crud, doctrine:generate-crud`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `module` | `-` | The module name
| `model` | `-` | The model class name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--theme` | `default` | The theme name
| `--generate-in-cache` | `-` | Generate the module in cache
| `--non-verbose-templates` | `-` | Generate non verbose templates
| `--with-show` | `-` | Generate a show method
| `--singular` | `-` | The singular name
| `--plural` | `-` | The plural name
| `--route-prefix` | `-` | The route prefix
| `--with-doctrine-route` | `-` | Whether you will use a Doctrine route
| `--env` | `dev` | The environment


The `doctrine:generate-module` task generates a Doctrine module:

    ./symfony doctrine:generate-module frontend article Article

The task creates a `%module%` module in the `%application%` application
for the model class `%model%`.

You can also create an empty module that inherits its actions and templates from
a runtime generated module in `%sf_app_cache_dir%/modules/auto%module%` by
using the `--generate-in-cache` option:

    ./symfony doctrine:generate-module --generate-in-cache frontend article Article

The generator can use a customized theme by using the `--theme` option:

    ./symfony doctrine:generate-module --theme="custom" frontend article Article

This way, you can create your very own module generator with your own conventions.

### ~`doctrine::generate-module-for-route`~

The `doctrine::generate-module-for-route` task generates a Doctrine module for a route definition:

    $ php symfony doctrine:generate-module-for-route [--theme="..."] [--non-verbose-templates] [--singular="..."] [--plural="..."] [--env="..."] application route



| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `route` | `-` | The route name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--theme` | `default` | The theme name
| `--non-verbose-templates` | `-` | Generate non verbose templates
| `--singular` | `-` | The singular name
| `--plural` | `-` | The plural name
| `--env` | `dev` | The environment


The `doctrine:generate-module-for-route` task generates a Doctrine module for a route definition:

    ./symfony doctrine:generate-module-for-route frontend article

The task creates a module in the `%frontend%` application for the
`%article%` route definition found in `routing.yml`.

### ~`doctrine::insert-sql`~

The `doctrine::insert-sql` task inserts SQL for current model:

    $ php symfony doctrine:insert-sql [--application[="..."]] [--env="..."] 

*Alias(es)*: `doctrine-insert-sql`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:insert-sql` task creates database tables:

    ./symfony doctrine:insert-sql

The task connects to the database and creates tables for all the
`lib/model/doctrine/*.php` files.

### ~`doctrine::migrate`~

The `doctrine::migrate` task migrates database to current/specified version:

    $ php symfony doctrine:migrate [--application[="..."]] [--env="..."] [version]

*Alias(es)*: `doctrine-migrate`

| Argument | Default | Description
| -------- | ------- | -----------
| `version` | `-` | The version to migrate to


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment


The `doctrine:migrate` task migrates database to current/specified version

    ./symfony doctrine:migrate

### ~`doctrine::rebuild-db`~

The `doctrine::rebuild-db` task creates database for current model:

    $ php symfony doctrine:rebuild-db [--application[="..."]] [--env="..."] [--no-confirmation] 

*Alias(es)*: `doctrine-rebuild-db`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--no-confirmation` | `-` | Whether to no-confirmation dropping of the database


The `doctrine:rebuild-db` task creates the database:

    ./symfony doctrine:rebuild-db

The task read connection information in `config/doctrine/databases.yml`:

`generate`
----------

### ~`generate::app`~

The `generate::app` task generates a new application:

    $ php symfony generate:app [--escaping-strategy="..."] [--csrf-secret="..."] application

*Alias(es)*: `init-app`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--escaping-strategy` | `` | Output escaping strategy
| `--csrf-secret` | `` | Secret to use for CSRF protection


The `generate:app` task creates the basic directory structure
for a new application in the current project:

    ./symfony generate:app frontend

This task also creates two front controller scripts in the
`web/` directory:

    web/%application%.php`     for the production environment
    web/%application%_dev.php` for the development environment

For the first application, the production environment script is named
`index.php`.

If an application with the same name already exists,
it throws a `sfCommandException`.

You can enable output escaping (to prevent XSS) by using the `escaping-strategy` option:

    ./symfony generate:app frontend --escaping-strategy=on

You can enable session token in forms (to prevent CSRF) by defining
a secret with the `csrf-secret` option:

    ./symfony generate:app frontend --csrf-secret=UniqueSecret


### ~`generate::module`~

The `generate::module` task generates a new module:

    $ php symfony generate:module  application module

*Alias(es)*: `init-module`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `module` | `-` | The module name




The `generate:module` task creates the basic directory structure
for a new module in an existing application:

    ./symfony generate:module frontend article

The task can also change the author name found in the `actions.class.php`
if you have configure it in `config/properties.ini`:

    [symfony]
      name=blog
      author=Fabien Potencier <fabien.potencier@sensio.com>

You can customize the default skeleton used by the task by creating a
`%sf_data_dir%/skeleton/module` directory.

The task also creates a functional test stub named
`%sf_test_dir%/functional/%application%/%module%ActionsTest.class.php`
that does not pass by default.

If a module with the same name already exists in the application,
it throws a `sfCommandException`.

### ~`generate::project`~

The `generate::project` task generates a new project:

    $ php symfony generate:project  name

*Alias(es)*: `init-project`

| Argument | Default | Description
| -------- | ------- | -----------
| `name` | `-` | The project name




The `generate:project` task creates the basic directory structure
for a new project in the current directory:

    ./symfony generate:project blog

If the current directory already contains a symfony project,
it throws a `sfCommandException`.

### ~`generate::task`~

The `generate::task` task creates a skeleton class for a new task:

    $ php symfony generate:task [--dir="..."] [--use-database="..."] [--brief-description="..."] task_name



| Argument | Default | Description
| -------- | ------- | -----------
| `task_name` | `-` | The task name (can contain namespace)


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--dir` | `lib/task` | The directory to create the task in
| `--use-database` | `propel` | Whether the task needs model initialization to access database
| `--brief-description` | `-` | A brief task description (appears in task list)


The `generate:task` creates a new sfTask class based on the name passed as
argument:

    ./symfony generate:task namespace:name

The `namespaceNameTask.class.php` skeleton task is created under the `lib/task/`
directory. Note that the namespace is optional.

If you want to create the file in another directory (relative to the project
root folder), pass it in the `--dir` option. This directory will be created
if it does not already exist.

    ./symfony generate:task namespace:name --dir=plugins/myPlugin/lib/task

If you want the task to default to a connection other than `propel`, provide
the name of this connection with the `--use-database` option:

    ./symfony generate:task namespace:name --use-database=main

The `--use-database` option can also be used to disable database
initialization in the generated task:

    ./symfony generate:task namespace:name --use-database=false

You can also specify a description:

    ./symfony generate:task namespace:name --brief-description="Does interesting things"

`i18n`
------

### ~`i18n::extract`~

The `i18n::extract` task extracts i18n strings from php files:

    $ php symfony i18n:extract [--display-new] [--display-old] [--auto-save] [--auto-delete] application culture



| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `culture` | `-` | The target culture


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--display-new` | `-` | Output all new found strings
| `--display-old` | `-` | Output all old strings
| `--auto-save` | `-` | Save the new strings
| `--auto-delete` | `-` | Delete old strings


The `i18n:extract` task extracts i18n strings from your project files
for the given application and target culture:

    ./symfony i18n:extract frontend fr

By default, the task only displays the number of new and old strings
it found in the current project.

If you want to display the new strings, use the `--display-new` option:

    ./symfony i18n:extract --display-new frontend fr

To save them in the i18n message catalogue, use the `--auto-save` option:

    ./symfony i18n:extract --auto-save frontend fr

If you want to display strings that are present in the i18n messages
catalogue but are not found in the application, use the 
`--display-old` option:

    ./symfony i18n:extract --display-old frontend fr

To automatically delete old strings, use the `--auto-delete` but
be careful, especially if you have translations for plugins as they will
appear as old strings but they are not:

    ./symfony i18n:extract --auto-delete frontend fr

### ~`i18n::find`~

The `i18n::find` task finds non "i18n ready" strings in an application:

    $ php symfony i18n:find [--env="..."] application



| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--env` | `dev` | The environment


The `i18n:find` task finds non internationalized strings embedded in templates:

    ./symfony i18n:find frontend

This task is able to find non internationalized strings in pure HTML and in PHP code:

    <p>Non i18n text</p>
    <p><?php echo 'Test' ?></p>

As the task returns all strings embedded in PHP, you can have some false positive (especially
if you use the string syntax for helper arguments).

`log`
-----

### ~`log::clear`~

The `log::clear` task clears log files:

    $ php symfony log:clear  

*Alias(es)*: `log-purge`





The `log:clear` task clears all symfony log files:

    ./symfony log:clear

### ~`log::rotate`~

The `log::rotate` task rotates an application log files:

    $ php symfony log:rotate [--history="..."] [--period="..."] application env

*Alias(es)*: `log-rotate`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `env` | `-` | The environment name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--history` | `10` | The maximum number of old log files to keep
| `--period` | `7` | The period in days


The `log:rotate` task rotates application log files for a given
environment:

    ./symfony log:rotate frontend dev

You can specify a `period` or a `history` option:

    ./symfony --history=10 --period=7 log:rotate frontend dev

`plugin`
--------

### ~`plugin::add-channel`~

The `plugin::add-channel` task add a new PEAR channel:

    $ php symfony plugin:add-channel  name



| Argument | Default | Description
| -------- | ------- | -----------
| `name` | `-` | The channel name




The `plugin:add-channel` task adds a new PEAR channel:

    ./symfony plugin:add-channel symfony.plugins.pear.example.com

### ~`plugin::install`~

The `plugin::install` task installs a plugin:

    $ php symfony plugin:install [--stability|-s="..."] [--release|-r="..."] [--channel|-c="..."] [--install_deps|-d] [--force-license] name

*Alias(es)*: `plugin-install`

| Argument | Default | Description
| -------- | ------- | -----------
| `name` | `-` | The plugin name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--stability`<br />`(-s)` | `-` | The preferred stability (stable, beta, alpha)
| `--release`<br />`(-r)` | `-` | The preferred version
| `--channel`<br />`(-c)` | `-` | The PEAR channel name
| `--install_deps`<br />`(-d)` | `-` | Whether to force installation of required dependencies
| `--force-license` | `-` | Whether to force installation even if the license is not MIT like


The `plugin:install` task installs a plugin:

    ./symfony plugin:install sfGuardPlugin

By default, it installs the latest `stable` release.

If you want to install a plugin that is not stable yet,
use the `stability` option:

    ./symfony plugin:install --stability=beta sfGuardPlugin
    ./symfony plugin:install -s beta sfGuardPlugin

You can also force the installation of a specific version:

    ./symfony plugin:install --release=1.0.0 sfGuardPlugin
    ./symfony plugin:install -r 1.0.0 sfGuardPlugin

To force installation of all required dependencies, use the `install_deps` flag:

    ./symfony plugin:install --install-deps sfGuardPlugin
    ./symfony plugin:install -d sfGuardPlugin

By default, the PEAR channel used is `symfony-plugins`
(plugins.symfony-project.org).

You can specify another channel with the `channel` option:

    ./symfony plugin:install --channel=mypearchannel sfGuardPlugin
    ./symfony plugin:install -c mypearchannel sfGuardPlugin

You can also install PEAR packages hosted on a website:

    ./symfony plugin:install http://somewhere.example.com/sfGuardPlugin-1.0.0.tgz

Or local PEAR packages:

    ./symfony plugin:install /home/fabien/plugins/sfGuardPlugin-1.0.0.tgz

If the plugin contains some web content (images, stylesheets or javascripts),
the task creates a `%name%` symbolic link for those assets under `web/`.
On Windows, the task copy all the files to the `web/%name%` directory.

### ~`plugin::list`~

The `plugin::list` task lists installed plugins:

    $ php symfony plugin:list  

*Alias(es)*: `plugin-list`





The `plugin:list` task lists all installed plugins:

    ./symfony plugin:list

It also gives the channel and version for each plugin.

### ~`plugin::publish-assets`~

The `plugin::publish-assets` task publishes web assets for all plugins:

    $ php symfony plugin:publish-assets [--core-only] [--symfony-lib-dir="..."] 





| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--core-only` | `-` | If set only core plugins will publish their assets
| `--symfony-lib-dir` | `-` | The symfony lib dir


The `plugin:publish-assets` task will publish web assets from all plugins.

    ./symfony plugin:publish-assets

In fact this will send the `plugin.post_install` event to each plugin.


### ~`plugin::uninstall`~

The `plugin::uninstall` task uninstalls a plugin:

    $ php symfony plugin:uninstall [--channel|-c="..."] [--install_deps|-d] name

*Alias(es)*: `plugin-uninstall`

| Argument | Default | Description
| -------- | ------- | -----------
| `name` | `-` | The plugin name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--channel`<br />`(-c)` | `-` | The PEAR channel name
| `--install_deps`<br />`(-d)` | `-` | Whether to force installation of dependencies


The `plugin:uninstall` task uninstalls a plugin:

    ./symfony plugin:uninstall sfGuardPlugin

The default channel is `symfony`.

You can also uninstall a plugin which has a different channel:

    ./symfony plugin:uninstall --channel=mypearchannel sfGuardPlugin

    ./symfony plugin:uninstall -c mypearchannel sfGuardPlugin

Or you can use the `channel/package` notation:

    ./symfony plugin:uninstall mypearchannel/sfGuardPlugin

You can get the PEAR channel name of a plugin by launching the
`plugin:list] task.

If the plugin contains some web content (images, stylesheets or javascripts),
the task also removes the [web/%name%` symbolic link (on *nix)
or directory (on Windows).

### ~`plugin::upgrade`~

The `plugin::upgrade` task upgrades a plugin:

    $ php symfony plugin:upgrade [--stability|-s="..."] [--release|-r="..."] [--channel|-c="..."] name

*Alias(es)*: `plugin-upgrade`

| Argument | Default | Description
| -------- | ------- | -----------
| `name` | `-` | The plugin name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--stability`<br />`(-s)` | `-` | The preferred stability (stable, beta, alpha)
| `--release`<br />`(-r)` | `-` | The preferred version
| `--channel`<br />`(-c)` | `-` | The PEAR channel name


The `plugin:upgrade` task tries to upgrade a plugin:

    ./symfony plugin:upgrade sfGuardPlugin

The default channel is `symfony`.

If the plugin contains some web content (images, stylesheets or javascripts),
the task also updates the `web/%name%` directory content on Windows.

See `plugin:install` for more information about the format of the plugin name and options.

`project`
---------

### ~`project::clear-controllers`~

The `project::clear-controllers` task clears all non production environment controllers:

    $ php symfony project:clear-controllers  

*Alias(es)*: `clear-controllers`





The `project:clear-controllers` task clears all non production environment
controllers:

    ./symfony project:clear-controllers

You can use this task on a production server to remove all front
controller scripts except the production ones.

If you have two applications named `frontend` and `backend`,
you have four default controller scripts in `web/`:

    index.php
    frontend_dev.php
    backend.php
    backend_dev.php

After executing the `project:clear-controllers` task, two front
controller scripts are left in `web/`:

    index.php
    backend.php

Those two controllers are safe because debug mode and the web debug
toolbar are disabled.

### ~`project::deploy`~

The `project::deploy` task deploys a project to another server:

    $ php symfony project:deploy [--go] [--rsync-dir="..."] [--rsync-options[="..."]] server

*Alias(es)*: `sync`

| Argument | Default | Description
| -------- | ------- | -----------
| `server` | `-` | The server name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--go` | `-` | Do the deployment
| `--rsync-dir` | `config` | The directory where to look for rsync*.txt files
| `--rsync-options` | `-azC --force --delete` | To options to pass to the rsync executable


The `project:deploy` task deploys a project on a server:

    ./symfony project:deploy production

The server must be configured in `config/properties.ini`:

    [production]
      host=www.example.com
      port=22
      user=fabien
      dir=/var/www/sfblog/
      type=rsync

To automate the deployment, the task uses rsync over SSH.
You must configure SSH access with a key or configure the password
in `config/properties.ini`.

By default, the task is in dry-mode. To do a real deployment, you
must pass the `--go` option:

    ./symfony project:deploy --go production

Files and directories configured in `config/rsync_exclude.txt` are
not deployed:

    .svn
    /web/uploads/*
    /cache/*
    /log/*

You can also create a `rsync.txt` and `rsync_include.txt` files.

If you need to customize the `rsync*.txt` files based on the server,
you can pass a `rsync-dir` option:

    ./symfony project:deploy --go --rsync-dir=config/production production

Last, you can specify the options passed to the rsync executable, using the 
`rsync-options` option (defaults are `-azC`):

    ./symfony project:deploy --go --rsync-options=avz

### ~`project::disable`~

The `project::disable` task disables an application in a given environment:

    $ php symfony project:disable  application env

*Alias(es)*: `disable`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `env` | `-` | The environment name




The `project:disable` task disables an application for a specific environment:

    ./symfony project:disable frontend prod

### ~`project::enable`~

The `project::enable` task enables an application in a given environment:

    $ php symfony project:enable  application env

*Alias(es)*: `enable`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `env` | `-` | The environment name




The `project:enable` task enables an application for a specific environment:

    ./symfony project:enable frontend prod

### ~`project::freeze`~

The `project::freeze` task freezes symfony libraries:

    $ php symfony project:freeze  symfony_data_dir

*Alias(es)*: `freeze`

| Argument | Default | Description
| -------- | ------- | -----------
| `symfony_data_dir` | `-` | The symfony data directory




The `project:freeze` task copies all the symfony core files to
the current project:

    ./symfony project:freeze /path/to/symfony/data/directory

The task takes a mandatory argument of the path to the symfony data
directory.

The task also changes `config/config.php` to switch to the
embedded symfony files.

### ~`project::permissions`~

The `project::permissions` task fixes symfony directory permissions:

    $ php symfony project:permissions  

*Alias(es)*: `permissions, fix-perms`





The `project:permissions` task fixes directory permissions:

    ./symfony project:permissions

### ~`project::unfreeze`~

The `project::unfreeze` task unfreezes symfony libraries:

    $ php symfony project:unfreeze  

*Alias(es)*: `unfreeze`





The `project:unfreeze` task removes all the symfony core files from
the current project:

    ./symfony project:unfreeze

The task also changes `config/config.php` to switch to the
old symfony files used before the `project:freeze` command was used.

### ~`project::upgrade1.1`~

The `project::upgrade1.1` task upgrade a symfony project to the 1.1 symfony release:

    $ php symfony project:upgrade1.1  







The `project:upgrade1.1` task upgrades a symfony project
based the 1.0 release to the 1.1 symfony release.

    ./symfony project:upgrade1.1

Please read the UPGRADE_TO_1_1 file to have information on what does this task.

### ~`project::upgrade1.2`~

The `project::upgrade1.2` task upgrade a symfony project to the 1.2 symfony release (from 1.1):

    $ php symfony project:upgrade1.2  







The `project:upgrade1.2` task upgrades a symfony project
based on the 1.1 release to the 1.2 symfony release.

    ./symfony project:upgrade1.2

Please read the UPGRADE_TO_1_2 file to have information on what does this task.

`propel`
--------

### ~`propel::build-all`~

The `propel::build-all` task generates Propel model and form classes, SQL and initializes the database:

    $ php symfony propel:build-all [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--skip-forms|-F] [--classes-only|-C] [--phing-arg="..."] 

*Alias(es)*: `propel-build-all`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--connection` | `propel` | The connection name
| `--no-confirmation` | `-` | Do not ask for confirmation
| `--skip-forms`<br />`(-F)` | `-` | Skip generating forms
| `--classes-only`<br />`(-C)` | `-` | Do not initialize the database
| `--phing-arg` | `-` | Arbitrary phing argument (multiple values allowed)


The `propel:build-all` task is a shortcut for five other tasks:

    ./symfony propel:build-all

The task is equivalent to:

    ./symfony propel:build-model
    ./symfony propel:build-forms
    ./symfony propel:build-filters
    ./symfony propel:build-sql
    ./symfony propel:insert-sql

See those tasks' help pages for more information.

To bypass confirmation prompts, you can pass the `no-confirmation` option:

    ./symfony propel:buil-all --no-confirmation

To build all classes but skip initializing the database, use the `classes-only`
option:

    ./symfony propel:build-all --classes-only

### ~`propel::build-all-load`~

The `propel::build-all-load` task generates Propel model and form classes, SQL, initializes the database, and loads data:

    $ php symfony propel:build-all-load [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--skip-forms|-F] [--classes-only|-C] [--phing-arg="..."] [--append] [--dir="..."] 

*Alias(es)*: `propel-build-all-load`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `dev` | The environment
| `--connection` | `propel` | The connection name
| `--no-confirmation` | `-` | Do not ask for confirmation
| `--skip-forms`<br />`(-F)` | `-` | Skip generating forms
| `--classes-only`<br />`(-C)` | `-` | Do not initialize the database
| `--phing-arg` | `-` | Arbitrary phing argument (multiple values allowed)
| `--append` | `-` | Don't delete current data in the database
| `--dir` | `-` | The directories to look for fixtures (multiple values allowed)


The `propel:build-all-load` task is a shortcut for two other tasks:

    ./symfony propel:build-all-load

The task is equivalent to:

    ./symfony propel:build-all
    ./symfony propel:data-load

See those tasks' help pages for more information.

To bypass the confirmation, you can pass the `no-confirmation`
option:

    ./symfony propel:buil-all-load --no-confirmation

### ~`propel::build-filters`~

The `propel::build-filters` task creates filter form classes for the current model:

    $ php symfony propel:build-filters [--connection="..."] [--model-dir-name="..."] [--filter-dir-name="..."] [--application[="..."]] 





| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--connection` | `propel` | The connection name
| `--model-dir-name` | `model` | The model dir name
| `--filter-dir-name` | `filter` | The filter form dir name
| `--application` | `1` | The application name


The `propel:build-filters` task creates filter form classes from the schema:

    ./symfony propel:build-filters

The task read the schema information in `config/*schema.xml` and/or
`config/*schema.yml` from the project and all installed plugins.

The task use the `propel` connection as defined in `config/databases.yml`.
You can use another connection by using the `--connection` option:

    ./symfony propel:build-filters --connection="name"

The model filter form classes files are created in `lib/filter`.

This task never overrides custom classes in `lib/filter`.
It only replaces base classes generated in `lib/filter/base`.

### ~`propel::build-forms`~

The `propel::build-forms` task creates form classes for the current model:

    $ php symfony propel:build-forms [--connection="..."] [--model-dir-name="..."] [--form-dir-name="..."] [--application[="..."]] 





| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--connection` | `propel` | The connection name
| `--model-dir-name` | `model` | The model dir name
| `--form-dir-name` | `form` | The form dir name
| `--application` | `1` | The application name


The `propel:build-forms` task creates form classes from the schema:

    ./symfony propel:build-forms

The task read the schema information in `config/*schema.xml` and/or
`config/*schema.yml` from the project and all installed plugins.

The task use the `propel` connection as defined in `config/databases.yml`.
You can use another connection by using the `--connection` option:

    ./symfony propel:build-forms --connection="name"

The model form classes files are created in `lib/form`.

This task never overrides custom classes in `lib/form`.
It only replaces base classes generated in `lib/form/base`.

### ~`propel::build-model`~

The `propel::build-model` task creates classes for the current model:

    $ php symfony propel:build-model [--phing-arg="..."] 

*Alias(es)*: `propel-build-model`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--phing-arg` | `-` | Arbitrary phing argument (multiple values allowed)


The `propel:build-model` task creates model classes from the schema:

    ./symfony propel:build-model

The task read the schema information in `config/*schema.xml` and/or
`config/*schema.yml` from the project and all installed plugins.

You mix and match YML and XML schema files. The task will convert
YML ones to XML before calling the Propel task.

The model classes files are created in `lib/model`.

This task never overrides custom classes in `lib/model`.
It only replaces files in `lib/model/om` and `lib/model/map`.

### ~`propel::build-schema`~

The `propel::build-schema` task creates a schema from an existing database:

    $ php symfony propel:build-schema [--application[="..."]] [--env="..."] [--connection="..."] [--xml] [--phing-arg="..."] 

*Alias(es)*: `propel-build-schema`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `cli` | The environment
| `--connection` | `-` | The connection name
| `--xml` | `-` | Creates an XML schema instead of a YML one
| `--phing-arg` | `-` | Arbitrary phing argument (multiple values allowed)


The `propel:build-schema` task introspects a database to create a schema:

    ./symfony propel:build-schema

By default, the task creates a YML file, but you can also create a XML file:

    ./symfony --xml propel:build-schema

The XML format contains more information than the YML one.

### ~`propel::build-sql`~

The `propel::build-sql` task creates SQL for the current model:

    $ php symfony propel:build-sql [--phing-arg="..."] 

*Alias(es)*: `propel-build-sql`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--phing-arg` | `-` | Arbitrary phing argument (multiple values allowed)


The `propel:build-sql` task creates SQL statements for table creation:

    ./symfony propel:build-sql

The generated SQL is optimized for the database configured in `config/propel.ini`:

    propel.database = mysql

### ~`propel::data-dump`~

The `propel::data-dump` task dumps data to the fixtures directory:

    $ php symfony propel:data-dump [--application[="..."]] [--env="..."] [--connection="..."] [--classes="..."] [target]

*Alias(es)*: `propel-dump-data`

| Argument | Default | Description
| -------- | ------- | -----------
| `target` | `-` | The target filename


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `cli` | The environement
| `--connection` | `propel` | The connection name
| `--classes` | `-` | The class names to dump (separated by a colon)


The `propel:data-dump` task dumps database data:

    ./symfony propel:data-dump > data/fixtures/dump.yml

By default, the task outputs the data to the standard output,
but you can also pass a filename as a second argument:

    ./symfony propel:data-dump dump.yml

The task will dump data in `data/fixtures/%target%`
(data/fixtures/dump.yml in the example).

The dump file is in the YML format and can be re-imported by using
the `propel:data-load` task.

By default, the task use the `propel` connection as defined in `config/databases.yml`.
You can use another connection by using the `connection` option:

    ./symfony propel:data-dump --connection="name"

If you only want to dump some classes, use the `classes` option:

    ./symfony propel:data-dump --classes="Article,Category"

If you want to use a specific database configuration from an application, you can use
the `application` option:

    ./symfony propel:data-dump --application=frontend

### ~`propel::data-load`~

The `propel::data-load` task loads data from fixtures directory:

    $ php symfony propel:data-load [--application[="..."]] [--env="..."] [--append] [--connection="..."] [--dir="..."] 

*Alias(es)*: `propel-load-data`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `cli` | The environment
| `--append` | `-` | Don't delete current data in the database
| `--connection` | `propel` | The connection name
| `--dir` | `-` | The directories to look for fixtures (multiple values allowed)


The `propel:data-load` task loads data fixtures into the database:

    ./symfony propel:data-load

The task loads data from all the files found in `data/fixtures/`.

If you want to load data from other directories, you can use
the `--dir` option:

    ./symfony propel:data-load --dir="data/fixtures" --dir="data/data"

The task use the `propel` connection as defined in `config/databases.yml`.
You can use another connection by using the `--connection` option:

    ./symfony propel:data-load --connection="name"

If you don't want the task to remove existing data in the database,
use the `--append` option:

    ./symfony propel:data-load --append

If you want to use a specific database configuration from an application, you can use
the `application` option:

    ./symfony propel:data-load --application=frontend

### ~`propel::generate-admin`~

The `propel::generate-admin` task generates a Propel admin module:

    $ php symfony propel:generate-admin [--module="..."] [--theme="..."] [--singular="..."] [--plural="..."] [--env="..."] application route_or_model



| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `route_or_model` | `-` | The route name or the model class


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--module` | `-` | The module name
| `--theme` | `admin` | The theme name
| `--singular` | `-` | The singular name
| `--plural` | `-` | The plural name
| `--env` | `dev` | The environment


The `propel:generate-admin` task generates a Propel admin module:

    ./symfony propel:generate-admin frontend Article

The task creates a module in the `%frontend%` application for the
`%Article%` model.

The task creates a route for you in the application `routing.yml`.

You can also generate a Propel admin module by passing a route name:

    ./symfony propel:generate-admin frontend article

The task creates a module in the `%frontend%` application for the
`%article%` route definition found in `routing.yml`.

For the filters and batch actions to work properly, you need to add
the `wildcard` option to the route:

    article:
      class: sfPropelRouteCollection
      options:
        model:                Article
        with_wildcard_routes: true

### ~`propel::generate-module`~

The `propel::generate-module` task generates a Propel module:

    $ php symfony propel:generate-module [--theme="..."] [--generate-in-cache] [--non-verbose-templates] [--with-show] [--singular="..."] [--plural="..."] [--route-prefix="..."] [--with-propel-route] [--env="..."] application module model

*Alias(es)*: `propel-generate-crud, propel:generate-crud`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `module` | `-` | The module name
| `model` | `-` | The model class name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--theme` | `default` | The theme name
| `--generate-in-cache` | `-` | Generate the module in cache
| `--non-verbose-templates` | `-` | Generate non verbose templates
| `--with-show` | `-` | Generate a show method
| `--singular` | `-` | The singular name
| `--plural` | `-` | The plural name
| `--route-prefix` | `-` | The route prefix
| `--with-propel-route` | `-` | Whether you will use a Propel route
| `--env` | `dev` | The environment


The `propel:generate-module` task generates a Propel module:

    ./symfony propel:generate-module frontend article Article

The task creates a `%module%` module in the `%application%` application
for the model class `%model%`.

You can also create an empty module that inherits its actions and templates from
a runtime generated module in `%sf_app_cache_dir%/modules/auto%module%` by
using the `--generate-in-cache` option:

    ./symfony propel:generate-module --generate-in-cache frontend article Article

The generator can use a customized theme by using the `--theme` option:

    ./symfony propel:generate-module --theme="custom" frontend article Article

This way, you can create your very own module generator with your own conventions.

### ~`propel::generate-module-for-route`~

The `propel::generate-module-for-route` task generates a Propel module for a route definition:

    $ php symfony propel:generate-module-for-route [--theme="..."] [--non-verbose-templates] [--singular="..."] [--plural="..."] [--env="..."] application route



| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `route` | `-` | The route name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--theme` | `default` | The theme name
| `--non-verbose-templates` | `-` | Generate non verbose templates
| `--singular` | `-` | The singular name
| `--plural` | `-` | The plural name
| `--env` | `dev` | The environment


The `propel:generate-module-for-route` task generates a Propel module for a route definition:

    ./symfony propel:generate-module-for-route frontend article

The task creates a module in the `%frontend%` application for the
`%article%` route definition found in `routing.yml`.

### ~`propel::graphviz`~

The `propel::graphviz` task generates a graphviz chart of current object model:

    $ php symfony propel:graphviz [--phing-arg="..."] 





| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--phing-arg` | `-` | Arbitrary phing argument (multiple values allowed)


The `propel:graphviz` task creates a graphviz DOT
visualization for automatic graph drawing of object model:

    ./symfony propel:graphviz

### ~`propel::init-admin`~

The `propel::init-admin` task initializes a Propel admin module:

    $ php symfony propel:init-admin [--theme="..."] application module model

*Alias(es)*: `propel-init-admin`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `module` | `-` | The module name
| `model` | `-` | The model class name


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--theme` | `default` | The theme name


The `propel:init-admin` task generates a Propel admin module:

    ./symfony propel:init-admin frontend article Article

The task creates a `%module%` module in the `%application%` application
for the model class `%model%`.

The created module is an empty one that inherit its actions and templates from
a runtime generated module in `%sf_app_cache_dir%/modules/auto%module%`.

The generator can use a customized theme by using the `--theme` option:

    ./symfony propel:init-admin --theme="custom" frontend article Article

### ~`propel::insert-sql`~

The `propel::insert-sql` task inserts SQL for current model:

    $ php symfony propel:insert-sql [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--phing-arg="..."] 

*Alias(es)*: `propel-insert-sql`



| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--application` | `1` | The application name
| `--env` | `cli` | The environment
| `--connection` | `-` | The connection name
| `--no-confirmation` | `-` | Do not ask for confirmation
| `--phing-arg` | `-` | Arbitrary phing argument (multiple values allowed)


The `propel:insert-sql` task creates database tables:

    ./symfony propel:insert-sql

The task connects to the database and executes all SQL statements
found in `config/sql/*schema.sql` files.

Before execution, the task will ask you to confirm the execution
as it deletes all data in your database.

To bypass the confirmation, you can pass the `--no-confirmation`
option:

    ./symfony propel:insert-sql --no-confirmation

The task read the database configuration from `databases.yml`.
You can use a specific application/environment by passing
an `--application` or `--env` option.

You can also use the `--connection` option if you want to
only load SQL statements for a given connection.

### ~`propel::schema-to-xml`~

The `propel::schema-to-xml` task creates schema.xml from schema.yml:

    $ php symfony propel:schema-to-xml  

*Alias(es)*: `propel-convert-yml-schema`





The `propel:schema-to-xml` task converts YML schemas to XML:

    ./symfony propel:schema-to-xml

### ~`propel::schema-to-yml`~

The `propel::schema-to-yml` task creates schema.yml from schema.xml:

    $ php symfony propel:schema-to-yml  

*Alias(es)*: `propel-convert-xml-schema`





The `propel:schema-to-yml` task converts XML schemas to YML:

    ./symfony propel:schema-to-yml

`test`
------

### ~`test::all`~

The `test::all` task launches all tests:

    $ php symfony test:all  

*Alias(es)*: `test-all`





The `test:all` task launches all unit and functional tests:

    ./symfony test:all

The task launches all tests found in `test/`.

If one or more test fail, you can try to fix the problem by launching
them by hand or with the `test:unit` and `test:functional` task.

### ~`test::coverage`~

The `test::coverage` task outputs test code coverage:

    $ php symfony test:coverage [--detailed] test_name lib_name



| Argument | Default | Description
| -------- | ------- | -----------
| `test_name` | `-` | A test file name or a test directory
| `lib_name` | `-` | A lib file name or a lib directory for wich you want to know the coverage


| Option (Shortcut) | Default | Description
| ----------------- | ------- | -----------
| `--detailed` | `-` | Output detailed information


The `test:coverage` task outputs the code coverage
given a test file or test directory
and a lib file or lib directory for which you want code
coverage:

    ./symfony test:coverage test/unit/model lib/model

To output the lines not covered, pass the `--detailed` option:

    ./symfony test:coverage --detailed test/unit/model lib/model

### ~`test::functional`~

The `test::functional` task launches functional tests:

    $ php symfony test:functional  application [controller1] ... [controllerN]

*Alias(es)*: `test-functional`

| Argument | Default | Description
| -------- | ------- | -----------
| `application` | `-` | The application name
| `controller` | `-` | The controller name




The `test:functional` task launches functional tests for a
given application:

    ./symfony test:functional frontend

The task launches all tests found in `test/functional/%application%`.

You can launch all functional tests for a specific controller by
giving a controller name:

    ./symfony test:functional frontend article

You can also launch all functional tests for several controllers:

    ./symfony test:functional frontend article comment

### ~`test::unit`~

The `test::unit` task launches unit tests:

    $ php symfony test:unit  [name1] ... [nameN]

*Alias(es)*: `test-unit`

| Argument | Default | Description
| -------- | ------- | -----------
| `name` | `-` | The test name




The `test:unit` task launches unit tests:

    ./symfony test:unit

The task launches all tests found in `test/unit`.

You can launch unit tests for a specific name:

    ./symfony test:unit strtolower

You can also launch unit tests for several names:

    ./symfony test:unit strtolower strtoupper



