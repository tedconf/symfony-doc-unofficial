Symfony Installation
====================

### Project Directory

Before installing symfony, you first need to create a directory that will host
all the files related to your project:

    $ mkdir -p /home/sfproject
    $ cd /home/sfproject

Or on Windows:

    c:\> mkdir c:\dev\sfproject
    c:\> cd c:\dev\sfproject

>**NOTE**
>Windows users are advised to run symfony and to setup their new
>project in a path which contains no spaces.
>Avoid using the `Documents and Settings` directory, including anywhere
>under `My Documents`.

-

>**TIP**
>If you create the symfony project directory under the web root
>directory, you won't need to configure your web server.  Of course, for
>production environments, we strongly advise you to configure your web
>server as explained in the web server configuration section.

### Symfony Installation

Create a directory to host the symfony framework library files:

    $ mkdir -p lib/vendor

Now, you need to install symfony. As the symfony framework has several stable
branches, you need to choose the one you want to install by reading the
[installation page](http://www.symfony-project.org/installation) on the
symfony website.

Go to the installation page for the version you have just chosen,
[symfony 1.3](http://www.symfony-project.org/installation/1_3) for instance.

Under the "**Source Download**" section, you will find the archive in `.tgz`
or in `.zip` format. Download the archive, put it under the freshly created
`lib/vendor/` directory and un-archive it:

    $ cd lib/vendor
    $ tar zxpf symfony-1.3.0.tgz
    $ mv symfony-1.3.0 symfony
    $ rm symfony-1.3.0.tgz

Under Windows, unzipping the zip file can be achieved using Windows Explorer.
After you rename the directory to `symfony`, there should be a directory
structure similar to `c:\dev\sfproject\lib\vendor\symfony`.

>**TIP**
>If you use Subversion, it is even better to use the `svn:externals`
>property to embed symfony into your project in the `lib/vendor/`
>directory, which benefits from the bug fixes made in the stable branch
>automatically:
>
>     http://svn.symfony-project.com/branches/1.3/

Check that symfony is correctly installed by using the symfony command line to
display the symfony version (note the capital `V`):

    $ cd ../..
    $ php lib/vendor/symfony/data/bin/symfony -V

On Windows:

    c:\> cd ..\..
    c:\> php lib\vendor\symfony\data\bin\symfony -V

>**TIP**
>If you are curious about what this command line tool can do for you, type
>`symfony` to list the available options and tasks:
>
>     $ php lib/vendor/symfony/data/bin/symfony
>
>On Windows:
>
>     c:\> php lib\vendor\symfony\data\bin\symfony
>
>The symfony command line is the developer's best friend. It provides a lot of
>utilities that improve your productivity for day-to-day activities like
>cleaning the cache, generating code, and much more.