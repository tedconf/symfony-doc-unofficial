Configurar el Proyecto
======================

En symfony, las **aplicaciones** que comparten el mismo modelo de datos se
agrupa en **proyectos**. Para la mayoría de los proyectos, tendrás dos diferentes
aplicaciones: un frontend y un backend.

### Creación del Proyecto

Desde el directorio `sfproject/`, ejecuta la tarea symfony `generate:project`
para inmediatamente crear el proyecto symfony:

    $ php lib/vendor/symfony/data/bin/symfony generate:project PROJECT_NAME

En Windows:

    c:\> php lib\vendor\symfony\data\bin\symfony generate:project PROJECT_NAME

La tarea `generate:project` genera la estructura predeterminada de directorios y
los archivos necesarios para un proyecto symfony:

 | Directorio   | Descripción
 | ----------- | ---------------------------------------------------
 | `apps/`     | Contiene todas las aplicaciones del proyecto
 | `cache/`    | Los archivos en guardados en cache por el framework
 | `config/`   | Los archivos de configuración del proyecto
 | `lib/`      | Las librerías y clases del proyecto
 | `log/`      | Los archivos de log del framework
 | `plugins/`  | Los plugins instalados
 | `test/`     | Los archivos de pruebas unitarias funcionales
 | `web/`      | El directorio web raíz (ver más abajo)

>**NOTE**
>¿Porquées symfony genera tantos archivos? Uno de los beneficios principales de
>usar un completo framework es la estandarización de tus desarrollos. Gracias a
>la estructura predeterminada de archivos y directorios de symfony, cualquier
>desarrollador con algún conocimiento de symfony puede mantener cualquier proyecto
>En cuestión de minutos, él será capaz de entrar al código, arreglar errores,
>y agregar características nuevas.

La tarea `generate:project` ha creado también un acceso directo `symfony` en el
directorio raíz del proyecto para acortar el número de carácteres que tiene que
tipear cuando ejecutas una tarea.

Entoces, de ahora en más, en lugar de usar la ruta completa al programa symfony
puedes usar el acceso/atajo `symfony`.

### Creación de la Aplicación

Ahota, crea la aplicación frontend mediante la tarea `generate:app`:

    $ php symfony generate:app --escaping-strategy=on
      ➥ --csrf-secret=UniqueSecret frontend

>**TIP**
>Debido a que el archivo atajo de symfony es ejecutable, los usuarios de Unix
>reemplazar todas las ocurrencias de '`php symfony`' por '`./symfony`' desde ahora.
>
>En Windows puedes copiar el archivo '`symfony.bat`' a tu proyecto y usar
>'`symfony`' en lugar de '`php symfony`':
>
>     c:\> copy lib\vendor\symfony\data\bin\symfony.bat .

Basado en el nombre de la aplicación dado como *argumento*, la tarea `generate:app`
crea una estructura de directorio predeterminada necesaria para la aplicación bajo
el directorio `apps/frontend/`:

 | Directorio   | Descripción
 | ------------ | ----------------------------------------------
 | `config/`    | Los archivos de configuración de la aplicación
 | `lib/`       | Las librerías y clases de la aplicación
 | `modules/`   | El código de la aplicación (MVC)
 | `templates/` | Los archivos de la plantilla general

>**TIP**
>Cuando llamas a la tarea `generate:app`, debes también pasar dos *opciones*
>relacionadas a la securidad:
>
>  * `--escaping-strategy`: Previene de ataques XSS
>  * `--csrf-secret`: Previene de ataques CSRF con tokens en formularios
>
>Pasando estas dos opciones a la tarea, aseguras tu desarrollo de futuras
>vulnerabilidades que se puedan encontrar en la web.
>Así es, symfony automaticamente tomará las medidas de seguridad.
>
>Si no sabes nada acerca de
>[XSS](http://en.wikipedia.org/wiki/Cross-site_scripting) o
>[CSRF](http://en.wikipedia.org/wiki/CSRF), tómate un tiempo para aprender más
>acercca de estas vulnerabilidades.

### Permisos en la Estructura de Directorios

Antes de tratar de acceder a tu reciente proyecto creado, necesitas establecer
los permisos de escritura sobre los directorios `cache/` y `log/` a los niveles
apropiados para que el servidor web pueda escribir en ellos:

    $ chmod 777 cache/ log/

>**SIDEBAR**
>Consejos para Personas que usen una herramienta SCM
>
>symfony solo escribe en dos directorios de proyeco symfony, `cache/` and `log/`.
>El contenido de estos directorios debería ser ignorado por tu SCM
>(editando la propiedad `svn:ignore` si usas Subversion por ejemplo).

### Configurando la Base de Datos

una de las primeras cosas que podrías querer hacer es configurar la conexión a
la base de datos de tu proyecto. El framework symfony da spporte a todos las
bases-[PDO]((http://www.php.net/PDO)) (MySQL, PostgreSQL,
SQLite, Oracle, MSSQL, ...). Arriba del PDO, symfony viene con dos tecnos ORM:
Propel y Doctrine. Propel es el predeterminado, pero cambiar a
Doctrine es bastante fácil (mira la sección siguiente para más información).

Configurara la base de datos es tan simple como usar la tarea `configure:database`:

    $ php symfony configure:database "mysql:host=localhost;dbname=dbname" root mYsEcret

La tarea `configure:database` toma tres argumentos: el
[~PDO DSN~](http://www.php.net/manual/en/pdo.drivers.php), el username, y
el password de acceso a la base de datos. Si no necesitas de un password para
acceder a tu base de datos en el servidor de desarrollo, solo omite el tercer argumento.

### Cambiando a Doctrine

Si decides usar Doctrine en lugar de Propel, necesitas primero habilitar
`sfDoctrinePlugin` y desactivar `sfPropelPlugin`. Esto se hace modificando
el siguiente código en tu `config/ProjectConfiguration.class.php`:

    [php]
    public function setup()
    {
      $this->enableAllPluginsExcept(array('sf#PropelPlugin', 'sfCompat10Plugin'));
    }

Despues de hacer estos cambios, ejecuta estos comandos:

    $ php symfony plugin:publish-assets
    $ php symfony cc
    $ rm web/sfPropelPlugin
    $ rm config/propel.ini
    $ rm config/schema.yml
    $ rm config/databases.yml

Entoces, ejecuta el siguiente comando para configurar tu base de datos para Doctrine:

    $ php symfony configure:database --name=doctrine --class=sfDoctrineDatabase "mysql:host=localhost;dbname=jobeet" root mYsEcret
