Los Entornos
============

Si echas un vistazo al directorio `web/`, encontrarás dos archivos PHP:
`index.php` y `frontend_dev.php`. Estos archivos se llaman **controladores frontales**;
todas las solicitudes a la aplicación se realizan a través de ellos. Pero,
¿por qué tenemos dos controladores frontales para cada aplicación?

Ambos archivos apuntan a la misma aplicación pero para diferentes **entornos**.
Cuando desarrollas una aplicación, excepto si desarrollas directamente en el
servidor de producción, necesitas  varios entornos:

  * El **entorno de desarrollo**: Este es el ambiente utilizado por
    **desarrolladores web** para añadir nuevas funciones, corregir los errores, ...
  * El **entorno de prueba**: Este entorno se utiliza para probar automáticamente
    la aplicación.
  * El **entorno staging**: Este entorno es utilizado por el **cliente** para
    poner a prueba la aplicación e informar errores o características faltantes.
  * El **entorno de producción**: Este es el entorno donde un **usuario final**
    interactúa.

¿Qué hace que un entorno sea único? En el entorno de desarrollo, la aplicación
necesita registrar todos los detalles de una petición para facilitar la depuración,
debe mostrar la excepción en el navegador, pero la cache debe ser deshabilitada
para que todos los cambios realizados al código se tengan en cuenta de inmediato.
El entorno de desarrollo debe ser optimizado para el desarrollador. El mejor
ejemplo es cuando ocurre una excepción. Para ayudar al desarrollador a depurar
rápidametne, symfony muestra la excepción con toda la información que tenga acerca
de la petición en el navegador:

![An exception in the dev environment](http://www.symfony-project.org/images/jobeet/1_2/01/exception_dev.png)

Pero en el entorno de la producción, la capa del cache debe estar activada, y por
supuesto, la aplicación deberá mostrar los mensajes de error personalizados en
lugar de excepciones. Por eso, el entorno de producción debe ser optimizado para
el rendimiento y la experiencia del usuario final.

![An exception in the prod environment](http://www.symfony-project.org/images/jobeet/1_2/01/exception_prod.png)

>**TIP**
>Si abres los archivos de los controladores frontales, verás que su contenido es
>el mismo salvo por el entorno:
>
>     [php]
>     // web/index.php
>     <?php
>
>     require_once(dirname(__FILE__).'/../config/ProjectConfiguration.class.php');
>
>     $configuration = ProjectConfiguration::getApplicationConfiguration('frontend', 'prod', false);
>     sfContext::createInstance($configuration)->dispatch();

La web debug toolbar o barra de herramientas de depuración web es una gran ejemplo
del uso del entorno. Está presente en todas las páginas en el entorno de desarrollo
y te da acceso a la configuración de la aplicación, los registros logs de la
petición actual, las sentencias SQL ejecutadas en el motor de la base de datos,
información de la memoria, e información de tiempos.
