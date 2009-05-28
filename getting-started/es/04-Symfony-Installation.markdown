Instalación de Symfony
======================

### Directorio del Proyecto

Antes de instalar symfony, primero debes crear un directorio que será anfitrión
de todos los archivos relacionados a tu proyecto:

    $ mkdir -p /home/sfproject
    $ cd /home/sfproject

O en Windows:

    c:\> mkdir c:\dev\sfproject
    c:\> cd c:\dev\sfproject

>**NOTE**
>Para los usuarios de Windows se recomienda ejecutar symfony y configurar sus
>proyectos nuevos en una ruta que no tenga espacios en blanco.
>Evita usar el directorio `Documents and Settings`, como los incluidos bajo
>`My Documents`.

-

>**TIP**
>Si creas el directorio del proyecto symfony bajo el directorio web raíz,
>no necesitas configurar tu servidor web. Por supuesto que, para entornos de
>producción, te recomendamos energicamente configurar tu servidor web como se
>explico en la sección de configuración del servidor web.

### Instalación de Symfony

Crea un directorio para hospedar los archivos de las librerias del framework
symfony:

    $ mkdir -p lib/vendor

Ahora, tendrás que instalar symfony. Como el framework symfony tiene varias
ramas estables, es necesario elegir la que deseas instalar usando la
[página de instalación](http://www.symfony-project.org/installation) deñ sitio
web de symfony.

Ve a la página de instalación por la versión que acabas de elegir,
[symfony 1.2](http://www.symfony-project.org/installation/1_2) por ejemplo.

Bajo la sección "**Source Download**", encontrarás el archivo en formato
`.tgz` o `.zip`. Descarga el archivo, colócalo bajo el nuevo directorio creado
`lib/vendor/` y descomprimelo ahi:

    $ cd lib/vendor
    $ tar zxpf symfony-1.2.2.tgz
    $ mv symfony-1.2.2 symfony
    $ rm symfony-1.2.2.tgz

En Windows descomprimir el archivo zip file puede ser hecho con el explorer.
Luego renombra el directorio a `symfony`, deberia quedar 
`c:\dev\sfproject\lib\vendor\symfony`.

>**TIP**
>Si usas Subversion, esto es auún mejor para usar la propiedad `svn:externals`
>para incluir symfony dentro de tu proyecto en el directorio `lib/vendor/`
>y recibir asi los arreglos a errores hechos en la rama estable automatically:
>
>     http://svn.symfony-project.com/branches/1.2/

Comprueba que symfony está correctamente instalado mediante la linea de comandos
de symfony para mostrar la versión de symfony (nota la mayúscula `V`):

    $ cd ../..
    $ php lib/vendor/symfony/data/bin/symfony -V

En Windows:

    c:\> cd ..\..
    c:\> php lib\vendor\symfony\data\bin\symfony -V

>**TIP**
>Si eres curioso acerca de que puede hacer la linea de comandos por tí, tipea
>`symfony` para listar las opciones y las tareas tasks disponibles:
>
>     $ php lib/vendor/symfony/data/bin/symfony
>
>En Windows:
>
>     c:\> php lib\vendor\symfony\data\bin\symfony
>
>La linea de comandos de symfony es el mejor amigo del desarrollador. Brinda un
>montón de utilidades que mejoran tu productividad en las acitivades del día-a-día
>como borrar la the cache, generando códigoe, y mucho más.

### La Ruta de symfony

Puedes saber la versión symfony usada en tu proyecto tipeando:

    $ php symfony -V

La opción `-V` también muestra la ruta a al directorio de instalación de symfony,
el cual esta guardado en `config/ProjectConfiguration.class.php`:

    [php]
    // config/ProjectConfiguration.class.php
    require_once '/Users/fabien/work/symfony/dev/1.2/lib/autoload/sfCoreAutoload.class.php';

Para una mejor portabilidad, cambia la ruta absoluta a la instalación de symfony
a una relativa:

    [php]
    // config/ProjectConfiguration.class.php
    require_once dirname(__FILE__).'/../lib/vendor/symfony/lib/autoload/sfCoreAutoload.class.php';

De esta manera, puedes mover el directorio del proyecto a cualquier lugar de tu
equipo o a otro equipo, y éste funcionará.