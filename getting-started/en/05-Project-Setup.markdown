Project Setup
=============

In symfony, **applications** sharing the same data model are regrouped into
**projects**. For most projects, you will have two different applications: a
frontend and a backend.

### Project Creation

From the `sfproject/` directory, run the symfony `generate:project` task to
actually create the symfony project:

    $ php lib/vendor/symfony/data/bin/symfony generate:project PROJECT_NAME

On Windows:

    c:\> php lib\vendor\symfony\data\bin\symfony generate:project PROJECT_NAME

The `generate:project` task generates the default structure of directories and
files needed for a symfony project:

 | Directory   | Description
 | ----------- | ----------------------------------
 | `apps/`     | Hosts all project applications
 | `cache/`    | The files cached by the framework
 | `config/`   | The project configuration files
 | `lib/`      | The project libraries and classes
 | `log/`      | The framework log files
 | `plugins/`  | The installed plugins
 | `test/`     | The unit and functional test files
 | `web/`      | The web root directory (see below)

>**NOTE**
>Why does symfony generate so many files? One of the main benefits of using
>a full-stack framework is to standardize your developments. Thanks to
>symfony's default structure of files and directories, any developer with
>some symfony knowledge can take over the maintenance of any symfony project.
>In a matter of minutes, he will be able to dive into the code, fix bugs,
>and add new features.

The `generate:project` task has also created a `symfony` shortcut in the
project root directory to shorten the number of characters you have to write
when running a task.

So, from now on, instead of using the fully qualified path to the symfony
program, you can use the `symfony` shortcut.

### Application Creation

Now, create the frontend application by running the `generate:app` task:

    $ php symfony generate:app --escaping-strategy=on
      ➥ --csrf-secret=UniqueSecret frontend

>**TIP**
>Because the symfony shortcut file is executable, Unix users can replace all
>occurrences of '`php symfony`' by '`./symfony`' from now on.
>
>On Windows you can copy the '`symfony.bat`' file to your project and use
>'`symfony`' instead of '`php symfony`':
>
>     c:\> copy lib\vendor\symfony\data\bin\symfony.bat .

Based on the application name given as an *argument*, the `generate:app` task
creates the default directory structure needed for the application under the
`apps/frontend/` directory:

 | Directory    | Description
 | ------------ | -------------------------------------
 | `config/`    | The application configuration files
 | `lib/`       | The application libraries and classes
 | `modules/`   | The application code (MVC)
 | `templates/` | The global template files

>**TIP**
>When calling the `generate:app` task, you have also passed two security-related
>*options*:
>
>  * `--escaping-strategy`: Enables output escaping to prevent XSS attacks
>  * `--csrf-secret`: Enables session tokens in forms to prevent CSRF attacks
>
>By passing these two optional options to the task, you have secured your
>future development from the two most widespread vulnerabilities found on the
>web. That's right, symfony will automatically take security measures on your
>behalf.
>
>If you know nothing about
>[XSS](http://en.wikipedia.org/wiki/Cross-site_scripting) or
>[CSRF](http://en.wikipedia.org/wiki/CSRF), take the time to learn more about
>these security vulnerabilities.

### Directory Structure Rights

Before trying to access your newly created project, you need to set the write
permissions on the `cache/` and `log/` directories to the appropriate levels,
so that your web server can write to them:

    $ chmod 777 cache/ log/

>**SIDEBAR**
>Tips for People using a SCM Tool
>
>symfony only ever writes in two directories of a symfony project,
>`cache/` and `log/`. The content of these directories should be ignored
>by your SCM (by editing the `svn:ignore` property if you use Subversion
>for instance).

### The symfony Path

You can get the symfony version used by your project by typing:

    $ php symfony -V

The `-V` option also displays the path to the symfony installation directory,
which is stored in `config/ProjectConfiguration.class.php`:

    [php]
    // config/ProjectConfiguration.class.php
    require_once '/Users/fabien/symfony-1.2/lib/autoload/sfCoreAutoload.class.php';

For better portability, change the absolute path to the symfony installation
to a relative one:

    [php]
    // config/ProjectConfiguration.class.php
    require_once dirname(__FILE__).'/../lib/vendor/symfony/lib/autoload/sfCoreAutoload.class.php';

That way, you can move the project directory anywhere on your machine or
another one, and it will just work.

### Configuring the Database

One of the first things you might want to do is to configure the database
connection for your project. The symfony framework supports all
[PDO](http://www.php.net/PDO)-supported databases (MySQL, PostgreSQL, SQLite,
Oracle, MSSQL, ...). On top of PDO, symfony comes bundled with two ORM tools:
Propel and Doctrine. Propel is the default one, but switching to Doctrine is
quite easy (see the next section for more information).

Configuring the database is as simple as using the `configure:database` task:

    $ php symfony configure:database "mysql:host=localhost;dbname=dbname" root mYsEcret

The `configure:database` task takes three arguments: the
[~PDO DSN~](http://www.php.net/manual/en/pdo.drivers.php), the username, and
the password to access the database. If you don't need a password to access
your database on the development server, just omit the third argument.

### Switching to Doctrine

If you decide to use Doctrine instead of Propel, you need to first enable
`sfDoctrinePlugin` and disable `sfPropelPlugin`. This can be done by changing
the following code in your `config/ProjectConfiguration.class.php`:

    [php]
    public function setup()
    {
      $this->enableAllPluginsExcept(array('sfPropelPlugin', 'sfCompat10Plugin'));
    }

After making these changes, launch these commands:

    $ php symfony plugin:publish-assets
    $ php symfony cc
    $ rm web/sfPropelPlugin
    $ rm config/propel.ini
    $ rm config/schema.yml
    $ rm config/databases.yml

Then, run the following command to configure your database for Doctrine:

    $ php symfony configure:database --name=doctrine --class=sfDoctrineDatabase "mysql:host=localhost;dbname=jobeet" root mYsEcret
