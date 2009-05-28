Configuración del Servidor Web
==============================

La Manera fea
-------------

En la sección anterior, un directorio se ha creado para alojar el proyecto.
Si lo creaste bajo el directorio web raíz de tu servidor web, ya puedes acceder
al proyecto en un navegador web.

Por supuesto, como no hay ninguna configuración, es muy rápido para establecer,
pero intenta tener acceso al archivo `config/databases.yml` en tu navegador y
comprederás las malas consecuencias de esta actitud perezosa. Si el usuario
conoce que tu sitio web esta desarrollado con Symfony, él tendrá acceso a un
montón de archivos delicados.

**Nunca uses esta configuración en un servidor de producción**, lee la siguiente
sección para aprender cómo configurar su servidor web correctamente.


La Manera segura
----------------

Una buena práctica web es poner bajo el directorio web raíz sólo los archivos a
los que necesita tener acceso el navegador web: las hojas de estilo, JavaScripts,
o imágenes. Te recomendamos almacenar estos archivos en el subdirectorio `web`
de un proyecto symfony.

Si echas un vistazo a este directorio, encontrarás algunos sub-directorios para
los recursos web y los dos archivos de los controladores frontales. Los
controladores frontales son los únicos archivos PHP que necesitan estar bajo el
directorio web raíz. Todos los demás archivos PHP se pueden ocultar del navegador,
la cual es una buena idea en lo que respecta a seguridad.

### Configuración del Servidor Web

Ahora es el momento de cambiar tu configuración de Apache para que el nuevo
proyecto sea accesible para el mundo.

Busca y abre el archivo de configuración `httpd.conf` y añade la siguiente
configuración al final:

    # Asegúrate de tener sólo una vez esta línea en su configuración
    NameVirtualHost 127.0.0.1:8080

    # Esta es la configuración de tu proyecto
    Listen 127.0.0.1:8080

    <VirtualHost 127.0.0.1:8080>
      DocumentRoot "/home/sfproject/web"
      DirectoryIndex index.php
      <Directory "/home/sfproject/web">
        AllowOverride All
        Allow from All
      </Directory>

      Alias /sf /home/sfproject/lib/vendor/symfony/data/web/sf
      <Directory "/home/sfproject/lib/vendor/symfony/data/web/sf">
        AllowOverride All
        Allow from All
      </Directory>
    </VirtualHost>

>**NOTE**:
>El alias `/sf` le da acceso a las imágenes y los archivos JavaScript
>necesarios para adecuadamente mostrar las páginas symfony por defecto y la
>barra de herramientas de depuración web.
>
>En Windows, es necesario sustituir la linea `Alias` con algo como:
>
>     Alias /sf "c:\dev\sfproject\lib\vendor\symfony\data\web\sf"
>
>Y `/home/sfprojects/web` debería ser sustituida por:
>
>     c:\dev\sfproject\web

En esta configuración, Apache escucha en el puerto `8080` de tu máquina, por lo
que el sitio web será accesible en la siguiente URL:

    http://localhost:8080/

Puedes cambiar `8080` por cualquier número mayor que 1024, ya que no se requieren
derechos de administrador en esos puertos.

>**SIDEBAR**
>Configurar un Nombre de Dominio dedicado
>
>Si eres el administrador de tu equipo, es mejor configurar un virtual host en
>lugar de añadir un nuevo puerto cada vez que se inicia un nuevo proyecto.
>En lugar de añadir un puerto y agregar una declaración `Listen`, elige un nombre
>de dominio y añade la declaración `ServerName`:
>
>     # This is the configuration for your project
>     <VirtualHost 127.0.0.1:80>
>       ServerName sfproject.localhost
>       <!-- same configuration as before -->
>     </VirtualHost>
>
>El nombre de dominio `sfproject.localhost` usado en Apache tiene que ser
>declarado localmente. Si ejecutaa un sistema Linux, esto tiene que hacerse en
>el archivo `/etc/hosts`. Si ejecuta Windows XP, este archivo se encuentra en el
>directorio `C:\WINDOWS\system32\drivers\etc\`.
>
>Añade la siguiente línea:
>
>     127.0.0.1 sfproject.localhost

>**Tip**
>**Nota del Traductor**
>Cuando se complican con estos pasos, los usuarios de Distribuciones Linux, como [Ubuntu](http://www.ubuntu.com), pueden usar una herramienta gráfica que simplique aún más esta configuración. [Rapache](http://launchpad.net/rapache), es un software para sistemas Gnome que hace la configuración básica y necesaria, en los archivos `/etc/hosts` y `httpd.conf`, en un solo paso además de permitir el reinicio de Apache con un clic.

### Probar la Nueva Configuración

Reinicia Apache, y comprueba que ahora tienes acceso a la nueva aplicación
abriendo un navegador y escribiendo `http://localhost:8080/index.php/`, o
`http://jobeet.localhost/index.php/` dependiendo de la configuración de Apache
que has elegido en la sección anterior.

![Felicitaciones](http://www.symfony-project.org/images/jobeet/1_2/01/congratulations.png)

>**TIP**
>Si tienes el módulo Apache `mod_rewrite` instalado, puedes remover la parte
>/index.php/ de la URL. Esto es posible gracias a las reglas de reescritura
>configuradas en el archivo `web/.htaccess`.

Deberías tratar de acceder a la aplicación en el entorno de desarrollo. (mira la
seccion siguiente para más información acerca de los entornos). Escribe la
siguiente URL:

    http://sfproject.localhost/frontend_dev.php/

La web debug toolbar o barra de herramientas de depuración web debería mostrarse
en la esquina superior derecha, incluidos los iconos, demostrando que tu
configuración alias `sf/` es correcta.


![web debug toolbar](http://www.symfony-project.org/images/jobeet/1_2/01/web_debug_toolbar.png)

>**Note**
>La configuración es un poco diferente si quieres ejecutar Symfony sobre un
>servidor IIS server en un sistema Windows. Busca la manera de configurarlo en
>el [tutorial](http://www.symfony-project.com/cookbook/1_0/web_server_iis).
