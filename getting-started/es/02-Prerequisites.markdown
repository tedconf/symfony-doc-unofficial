Prerequisitos
=============

Antes de instalar symfony, necesitas comprobar que tu equipo tiene todo instalado 
y configurado correctamente. Tóma un tiempo para leer este capítulo a conciencia
y sigue todos los pasos necesarios para verificar tu configuración, ya que puede 
salvar tu día en el proceso.

Softwares
---------

En primer lugar, necesitas comprobar que tu equipo tiene un entorno de trabajo 
favorable para el desarrollo web. Como mínimo, necesitas un servidor web (Apache, 
por ejemplo), un motor de base de datos (MySQL, PostgreSQL, SQLite, o cualquier 
motor de base de datos compatible con PDO), y PHP 5.2.4 o posterior.

Interfaz de Linea de Comandos
-----------------------------

El framework Symfony viene con una herramienta de línea de comandos que automatiza 
una gran cantidad de trabajo por tí. Si eres usuario de sistemas operativos tipo 
Unix, te sentirás como en casa. Si ejecutas un sistema Windows, también funcionará 
bien, sólo tendrás que escribir algunos comandos en el prompt `cmd`.

>**Note**
>Los comandos del shell Unix puede ser usados en un entorno Windows.
>Si deseas usar herramientas como like `tar`, `gzip`, o `grep` en Windows 
>puedes instalar [Cygwin](http://cygwin.com/).  La documentación oficial está 
>un poco dispersa, por lo que una buena guía de instalación puede hallarse 
>[aquí](http://www.soe.ucsc.edu/~you/notes/cygwin-install.html).
>Los aventureros pueden también tratar con el 
>[Windows Services for Unix](http://technet.microsoft.com/en-gb/interopmigration/bb380242.aspx).
>de Microsoft.

Configuración PHP
-----------------

Como las configuraciones de PHP pueden variar mucho de un SO a otro, o incluso 
entre las diferentes distribuciones de Linux, tienes que comprobar que tu 
configuración de PHP cumple los requisitos mínimos de symfony. 

En primer lugar, asegúrate de que tienes instalado PHP 5.2.4, como mínimo, 
mediante el uso de la función incorporada `phpinfo()` o ejecutando `php -v` en 
la línea de comandos. Ten en cuenta que en algunas configuraciones, puedes tener 
dos versiones de PHP instalado: una para la línea de comandos, y otro para la web. 

Descarga el script symfony para comprobar la configuración en la siguiente URL:

    http://sf-to.org/1.2/check.php

Guarda el script en algun lugar del directorio web raíz.

Lanza el script de comprobación de configuración en la línea de comandos:

    $ php check_configuration.php

Si hay un problema con la configuración actual de PHP, la salida del comando te 
dará pistas sobre lo que hay para arreglar y cómo solucionarlo. 

También debes ejecutar el comprobador desde un navegador y solucionar los problemas 
que podrías descubrir. Esto se debe a que PHP puede tener un archivo de configuración 
`php.ini` distinto para estos dos entornos, con diferentes configuraciones.

>**NOTE**
>No olvides de eliminar el archivo de tu directorio web raíz después.
