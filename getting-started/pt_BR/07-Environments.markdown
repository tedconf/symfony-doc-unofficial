Os ambientes
============

Se você olhar o diretório `web/`, você encontrará dois arquivos PHP: `index.php` e `frontend_dev.php`. Esses arquivos são chamados de **front controllers**; todas as chamadas para a aplicação são feitas através deles. Mas, por que nós temos dois "front controllers" diferentes para cada aplicação?

Ambos os arquivos apontam para a mesmo aplicação, mas para diferentes **ambientes**. Quando vocÊ desenvolve uma aplicação, ao não ser que você desenvolva direto no servidor de produção, você precisa de alguns ambientes:

  * O **ambiente de desenvolvimento**: Esse é o ambiente utilizado pelos **desenvolvedores** quando eles estão trabalhando numa aplicação adicionando novas funcionalidades, corrigindo bugs, etc.
  * O **ambiente de teste**: Esse ambiente é utilizado para testar a aplicação automaticamente
  * O **ambiente de homologação**: Esse ambiente é utilizado pelo **cliente** para testar a aplicação e reportar bugs ou funcionalidades que estão faltando.
  * O **ambiente de produção**: Esse ambiente é para os **usuários finais** interagirem.
  

O que torna um ambiente único? No ambiente de desenvolvimento, por exemplo, a aplicação deve logar todos os detalhes de uma chamada visando facilitar o debug. O cache precisa estar desabilitado por completo para todas as alterações surtirem efeito em tempo real. Entao, um ambiente de desenvolvimento precisa ser otimizado para o desenvolvedor. O melhor exemplo é quando ocorre algum erro. Para ajudar o desenvolvedor a corrigir os erros mais rápido, o symfony mostra a "exception" com todas as informações detalhadas da chamada no browser:

![An exception in the dev environment](http://www.symfony-project.org/images/jobeet/1_2/01/exception_dev.png)

Mas, no ambiente de produção, o cache deve estar ativado e , é claro, a aplicação deve mostrar erros personalizados ao invés de "exceptions". Entao, o ambiente de produção deve ser otimizado para performance e ao usuário final.

![An exception in the prod environment](http://www.symfony-project.org/images/jobeet/1_2/01/exception_prod.png)

>**DICA**
>Se você abrir um arquivo "front controller", você poderá ver que a única diferença entre eles é a configuração que determina o ambiente:
>
>     [php]
>     // web/index.php
>     <?php
>
>     require_once(dirname(__FILE__).'/../config/ProjectConfiguration.class.php');
>
>     $configuration = ProjectConfiguration::getApplicationConfiguration('frontend', 'prod', false);
>     sfContext::createInstance($configuration)->dispatch();

A barra de debug é um bom exemplo da utilização de ambientes. Ela está presente em todas as páginas do ambiente de desenvolvimento e fornece acesso para todas as informações do framework. Há diversas abas que permitem esta navegação: configuração da aplicação, logs, SQL executados no banco de dados, alocação de memória e informações de tempo.
