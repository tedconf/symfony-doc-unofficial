El Sandbox
===========

Si tu objetivo es probar symfony por unas horas, sigue leyendo este capítulo,
ya que te mostraremos la manera más rápida de empezar. Si desea arrancar un
proyecto en el mundo real, puedes saltarte este capítulo, y [pasar](#chapter_04-Symfony-Installation)
al siguiente ya mismo.

La forma más rápida de experimentar con symfony es instalar el sandbox symfony
(la caja de arena). El sandbox es la forma fácil de instalar un proyecto symfony
pre-empaquetado, ya configurado con algunos valores predeterminados razonables.
Es una gran manera de experimentar con symfony sin la molestia de hacer una
instalación adecuada que respete las mejores prácticas de la web.

>**CAUTION**
>Ya que el sandbox está pre-configurado para usar SQLite como motor de base de
>datos, necesitas comprobar que PHP tiene soporte SQLite (mira el capítulo de
>[Prerequisitos](#chapter_02-Prerequisites) ). También puedes leer la sección
>[Configurando la Base de Datos](#chapter_05-Project-Setup_sub_configuring_the_database)
> para aprender como se cambia la base de datos usada por el sandbox.

Puedes descargar el sandbox symfony en formato `.tgz` o `.zip` de la
[página de instalación](http://www.symfony-project.org/installation/1_2) de
symfony o de la siguiente URL:

    http://www.symfony-project.org/get/sf_sandbox_1_2.tgz

    http://www.symfony-project.org/get/sf_sandbox_1_2.zip

Descomprime los archivos en algún lugar de tu directorio web raíz, y listo.
Tu proyecto symfony está ahora accesible mediante la petición del navegador al
script `web/index.php`.

>**CAUTION**
>Tener todos los archivos de symfony en el directorio web raís está bien para
>probarlos en tu equipo local, pero es realmente una mala idea para un servidor
>de producción ya que potencialmente hace visible a los usuarios finales toda
>la aplicación interna.

Puedes ahora finalizar tu instalación leyendo los capítulos
[Configuración del Servidor Web](#chapter_06-Web-Server-Configuration)
y [Entornos](#chapter_07-Environments).

>**NOTE**
>Ya que el sandbox es solo un proyecto symfony normal y corriente donde algunas
>tareas han sido ejecutadas por tí y se modificaron algunas configuraciones,
>es bastante fácil de usar ya que comienza desde un nuevo proyecto. Pero ten en
>mente que probablemente necesitarás adaptar la configuración; por ejemplo
>cambiar la configuraciónes de seguridad (leer sobre la configuración de XSS
>y CSRF más tarde en este tutorial).
