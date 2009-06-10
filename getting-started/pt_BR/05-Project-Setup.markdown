Configuração do Projeto
=======================

No symfony, as **aplicações** que compartilham o mesmo modelo de dados são reagrupadas em **projetos**. Para a maioria dos projetos, você vai precisar de duas aplicações diferentes: um frontend e backend.

### Criação do Projeto

A partir da pasta `sfproject/`, execute o comando symfony `generate:project` para criar o novo projeto symfony.

    $ php lib/vendor/symfony/data/bin/symfony generate:project NOME_DO_PROJETO

No Windows:

    c:\> php lib\vendor\symfony\data\bin\symfony generate:project NOME_DO_PROJETO

A tarefa `generate:project` gera uma estrutura de diretórios padrão com os arquivos necessários para o projeto:

 | Diretório   | Descrição
 | ----------- | ----------------------------------
 | `apps/`     | Onde fica as aplicações do projeto
 | `cache/`    | Arquivos cache do framework
 | `config/`   | Arquivos de configuração do projeto
 | `lib/`      | Bibliotecas e classes do projeto
 | `log/`      | Arquivos de log do framework
 | `plugins/`  | Plugins instalados
 | `test/`     | Arquivos de teste unitário e funcional
 | `web/`      | O diretório raiz (veja abaixo)

>**NOTA**
>Por que o symfony gera tantos arquivos? Um dos maiores benefícios de utilizar um framework "full-stack" (framework que oferece tudo que você precisa para desenvolver um portal em um único lugar) é padronizar o desenvolvimento. Graças a estrutura padrão de arquivos e diretórios que qualquer desenvolvedor, com pouco conhecimento do symfony, consegue dar manutenção em qualquer projeto do symfony. Em questão de minutos ele vai conseguir navegar pelo cógido, promover correções e adicionar novas funcionalidades.

A tarefa `generate:project` também gerou um atalho `symfony` no diretório raiz do projeto visando diminuir o número de caracteres que você digita ao executar as tarefas.

Então, a partir de agora, ao invés de utilizar o caminho inteiro para o "binário" do symfony, você pode simplesmente utilizar o atalho `symfony`.

### Criação da Aplicação

Agora, vamos criar a aplicação frontend utilizando a tarefa `generate:app`:

    $ php symfony generate:app --escaping-strategy=on
      ➥ --csrf-secret=UniqueSecret frontend

>**DICA**
>Já que o atalho `symfony` é um binário, usuários do Unix podem substituir todas as ocorrências de '`php symfony`' por '`./symfony`' de agora em diante.
>
>No windows você pode copiar o '`symfony.bat`' para o seu projeto e utilizar '`symfony`' ao invés de '`php symfony`':
>
>     c:\> copy lib\vendor\symfony\data\bin\symfony.bat .

Baseado no nome da aplicação que passamos como *argumento*, a tarefa `generate:app` cria a estrutura de diretório padrão que será utilizada dentro do diretório `apps/frontend/`:

 | Diretório   | Descrição
 | ------------ | -------------------------------------
 | `config/`    | Arquivos de configuração do projeto
 | `lib/`       | Bibliotecas e classes do projeto   
 | `modules/`   | Código da aplicação (MVC)
 | `templates/` | Arquivos globais do template

>**DICA**
>Quando chamamos a tarefa `generate:app`, você pode opcionalmente passar duas opções relacionadas à segunça:
>
>  * `--escaping-strategy`: Escapa as saídas evitando atacques XSS
>  * `--csrf-secret`: Habilita "tokens de sessão" nos formulários evitando ataques CSRF
>
>Passando esses dois parâmetros opcionais você certifica a segurança contra as vulnerabilidades mais conhecidas na web. É claro, o symfony vai automaticamente tomar medidas de segurança em seu nome.
>
>Se você souber algo sobre
>[XSS](http://en.wikipedia.org/wiki/Cross-site_scripting) ou
>[CSRF](http://en.wikipedia.org/wiki/CSRF), tire um tempo para aprender um pouco sobre vulnerabilidades e segurança.


### Permissão das estruturas de diretórios

Antes de tentar acessar o projeto criado, você precisa setar a permissão dos diretórios `cache/` e `log/` para que seu webserver possa escrever:

    $ chmod 777 cache/ log/

>**SIDEBAR**
>Dica para os que usam a ferramenta SCM
>
>symfony só escreve dentro de dois diretórios diferentes do projeto, `cache/` e `log/`. O conteúdo desses diretórios podem ser ignorados pelo SCM (editando a propriedade `svn:ignore` do Subversion por exemplo).

### Configurando o banco de dados

Uma das primeiras coisas que você vai querer configurar é a conexão do banco de dados com seu projeto. O symfony suporta todos os bancos de dados baseados no [PDO]((http://www.php.net/PDO)). Dessa maneira, (MySQL, PostgreSQL, SQLite, Oracle, MSSQL, ...). O symfony vem com duas ferramentas ORM: Propel e Doctrine. Propel é o padrão, mas trocar para o Doctrine é muito fácil (veja a próxima seção).

Configurando o banco de dados utilizando a tarefa `configure:database`:

    $ php symfony configure:database "mysql:host=localhost;dbname=dbname" root mYsEcret

A tarefa `configure:database` utiliza tres argumentos: [~PDO DSN~](http://www.php.net/manual/en/pdo.drivers.php). o usuário e a senha. Se você não precisa de senha para acessar o banco no servidor de desenvolvimento, simplesmente omita o terceiro argumento.

### Trocando o ORM para Doctrine

Caso você decida utilizar o Doctrine ao invés do Propel, você precisa habilitar o plugin `sfDoctrinePlugin` e desabilitar o `sfPropelPlugin`. Isso pode ser feito trocando a seguinte linha no arquivo `config/ProjectConfiguration.class.php`:

    [php]
    public function setup()
    {
      $this->enableAllPluginsExcept(array('sfPropelPlugin', 'sfCompat10Plugin'));
    }

Depois de fazer estas alterações, execute os seguintes comandos:

    $ php symfony plugin:publish-assets
    $ php symfony cc
    $ rm web/sfPropelPlugin
    $ rm config/propel.ini
    $ rm config/schema.yml
    $ rm config/databases.yml

Agora, execute o seguinte comando para configura o banco de dados com Doctrine:

    $ php symfony configure:database --name=doctrine --class=sfDoctrineDatabase "mysql:host=localhost;dbname=jobeet" root mYsEcret
