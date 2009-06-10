Configuração do Web Server
==========================

O modo feio
-----------

In the previous chapters, you have created a directory that hosts the project.
If you have created it somewhere under the web root directory of your web
server, you can already access the project in a web browser.

Nos capítulos ateriores você criou o diretório que hospeda seu projeto. Se você criou o projeto em algum lugar dentro do diretório raíz do seu servidor web, você já deve conseguir acessar a pasta web/ seu projeto pelo browser.

Como não há nenhuma configuração, é muito rápido de visualizar o projeto. Tente para acessar o arquivo `config/databases.yml` no seu navegador para compreender a más consequências dessa atitude preguiçosa. 


**Nunca utilize esse tipo de configuração num servidor de produção**, e leia a próxima seção para aprender a melhor maneira de configurar seu servidor.

O modo seguro
-------------

Uma boa prática é colocar dentro do diretório raíz somente os arquivos que são acessados pelo browser, como CSS`s, JavaScripts e imagens. Por padrão recomendamos guardar todos esses arquivos dentro do subdiretório `web/`.

Se você olhar esse diretório, verá que alguns subdiretórios (`css/` and `images/`) e dois arquivos dos controladores. Esses dois arquivos PHP são os únicos necessários de estar no diretório web/. Todos os outros arquivos PHP podem estar escondidos do browser (o que é uma boa idéia quando falamos em segurança). 

### Configuração do Web Server

Agogra é hora de mudarmos a configuração do Apache para tornar nosso projeto acessível.

Procure e abra o arquivo `httpd.conf` e adicione as seguintes configurações ao final:

    # Essa linha só pode aparecer uma vez na sua configuração
    NameVirtualHost 127.0.0.1:8080

    # This is the configuration for your project
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

>**NOTA**
>O alias `/sf` dará acesso para as imagens e javascripts necessários pelas páginas do symfony e da barra de debug.
>
>No Windows, você precisa substituir a linha do `Alias` para algo do tipo:
>
>     Alias /sf "c:\dev\sfproject\lib\vendor\symfony\data\web\sf"
>
>e `/home/sfproject/web` deve ser substituído por:
>
>     c:\dev\sfproject\web

Essa configuração faz com que o Apache escute na porta `8080` da sua máquina. Desse modo, o site estará acessível na seguinte URL:

    http://localhost:8080/

Você pode trocar a porta `8080` por qualquer número maior que `1024` para que não seja necessário de permissões administrativas.

>**SIDEBAR**
>Configurar um domínio
>
>Sendo o administrador da sua máquina, temos maior liberdade para configurar o virtual host ao invés de adicionar uma nova porta a cada vez que criarmos um novo projeto. Ao invés de escolher a porta e adicionar a declaração de `Listen`, você pode declarar o `ServerName`:
>
>     # This is the configuration for your project
>     <VirtualHost 127.0.0.1:80>
>       ServerName sfproject.localhost
>       <!-- same configuration as before -->
>     </VirtualHost>
>
>
>O domínio `sfproject.localhost` utilizado na configuração do Apache precisa ser declarado localmente. Se você está utilizando Linux, isso deve ser feito no arquivo `/etc/hosts`. Se você está utilizando Windows XP, esse arquivo está em `C:\WINDOWS\system32\drivers\etc\`.
>Adicione as seguintes linhas:
>
>     127.0.0.1 sfproject.localhost

### Testando a nova configuração

Reinicie o Apache e acesse sua aplicação pelo browser utilizando `http://localhost:8080/index.php/`, ou `http://sfproject.localhost/index.php/` dependendo do tipo de configuração que você escolheu anteriormente.

![Parabéns](http://www.symfony-project.org/images/jobeet/1_2/01/congratulations.png)

>**DICA**
Se você possui o módulo `mod_rewrite` instalado no Apache, você pode remover o `index.php/` da URL. Isso é possível graças as regras (rewriting rules) configuradas no arquivo `web/.htaccess`.

Você deve também tentar acessar a aplicação pelo ambiente de desenvolvimento (veja na próxima seção para mais informações sobre ambientes). Utilize a seguinte URL:

    http://sfproject.localhost/frontend_dev.php/

A barra de debug deve aparecer no topo direito. Isso quer dizer que o Alias no virtual host foi configurado corretamente.

![web debug toolbar](http://www.symfony-project.org/images/jobeet/1_2/01/web_debug_toolbar.png)

>**Nota**
>A configuração é diferente caso você queira rodar o symfony em um servidor IIS debaixo do Windows. Estes passos estão relacionados neste [tutorial](http://www.symfony-project.com/cookbook/1_0/web_server_iis).
