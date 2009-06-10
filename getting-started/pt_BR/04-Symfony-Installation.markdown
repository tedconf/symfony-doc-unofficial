Instalação do Symfony
=====================

### Diretório do Projeto

Antes de instalar o symfony, você primeiro vai precisar criar um diretório
que hospedará todos os arquivos relacionados ao seu projeto:

  $ mkdir -p /home/sfproject
  $ cd /home/sfproject

Ou no Windows:

    c:\> mkdir c:\dev\sfproject
    c:\> cd c:\dev\sfproject

>**NOTA**
>Usuários do Windows são aconselhados a executar o symfony e
>configurar o novo projeto em um caminho onde não haja espaços
>em branco.
>Evite usar o diretório `Documents and Settings`, incluindo qualquer um
>abaixo dos `Meus Documentos`

-

>**DICA**
>Se você criar o projeto abaixo do diretório root da web, não precisará
>configurar o seu web server. Claro que em ambiente de produção, nós
>recomendamos sériamente que configure seu web server como explicado
>na seção `Configurando o Web Server`.

### Instalação do Symfony

Crie um diretório para hospedar os arquivos de biblioteca do symfony:

    $ mkdir -p lib/vendor

Agora, vamos instalar o symfony. Como o symfony possui algumas versões
estáveis, você precisa escolher um, lendo a 
[Página de instalação](http://www.symfony-project.org/installation) no
site do symfony.

Vá para ela e escolha a versão.
[symfony 1.2](http://www.symfony-project.org/installation/1_2) por exemplo.

Na seção "**Source Download**", você pode achar o arquivo no formato
`.tgz` ou em `.zip`. Faça o download e extraia no diretório que acabara de
criar `lib/vendor`:

    $ cd lib/vendor
    $ tar zxpf symfony-1.2.2.tgz
    $ mv symfony-1.2.2 symfony
    $ rm symfony-1.2.2.tgz

No Windows, deszipe o arquivo zip usando o Windows Explorer. Depois renomeie
o diretório para 'symfony',lá, haverá um diretório com a estrutura similar a
'c:\dev\sfproject\lib\vendor\symfony'.

>**DICA**
>Se você usa o Subversion, é melhor usar a propriedade
>'svn:externals' pra embutir o symfony no seu projeto no diretório
>'lib/vendor/', que beneficia de correções feitas nas versões estáveis
>automaticamente:
>     http://svn.symfony-project.com/branches/1.2/

Verifique se o symfony está corretamente instalando usando a linha de comando
para visualizar sua versão:

    $ cd ../..
    $ php lib/vendor/symfony/data/bin/symfony -V

No Windows:

    c:\> cd ..\..
    c:\> php lib\vendor\symfony\data\bin\symfony -V

>**DICA**
>Se você está curioso sobre a ferramenta da linha de comando do symfony
>pode fazer, digite `symfony` pra listar as opções e tarefas disponíveis:
>
>     $ php lib/vendor/symfony/data/bin/symfony
>
>No Windows:
>
>     c:\> php lib\vendor\symfony\data\bin\symfony
>
>Este comando é o melhor amigo do desenvolvedor. Ele fornece muitas utilidades
>que melhora sua produtividade pras atividades do dia-a-dia, como limpando o 
>cache, gerando códigos e muito mais.

### The symfony Path

Você pode verificar a versão do symfony de seu projeto digitando:

    $ php symfony -V

A opção -V também mostra o caminho para o diretório onde o symfony está instalado,
que está armazenado em `config/ProjectConfiguration.class.php`:

    [php]
    // config/ProjectConfiguration.class.php
    require_once '/Users/fabien/work/symfony/dev/1.2/lib/autoload/sfCoreAutoload.class.php';

Para melhor portabilidade, mude o caminho absoluto da instalação do symfony
para um relativo:

    [php]
    // config/ProjectConfiguration.class.php
    require_once dirname(__FILE__).'/../lib/vendor/symfony/lib/autoload/sfCoreAutoload.class.php';

Sendo assim, você pode mover o diretório do projeto para onde quiser 
em sua máquina, que irá funcionar normalmente.
