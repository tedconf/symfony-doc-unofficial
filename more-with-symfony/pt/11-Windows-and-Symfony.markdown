Windows e symfony
=================

*por Laurent Bonnet*

Vis�o Geral
-----------

Este documento � um novo tutorial passo-a-passo cobrindo a instala��o,
implanta��o e teste de funcionamento do framework symfony no Windows Server
2008.

A fim de preparar o desenvolvimento para Internet, o tutorial pode ser
executado em um ambiente de servidor dedicado, hospedado na Internet.

Naturalmente, � poss�vel concluir o tutorial em um servidor local, ou uma
m�quina virtual na esta��o de trabalho do leitor.

### A raz�o para um novo tutorial

Atualmente, existem duas fontes de informa��o relacionadas com o Microsoft Internet
Information Server (IIS) no symfony
[website](http://trac.symfony-project.org/wiki/symfonyOnIIS)
  [](http://www.symfony-project.org/cookbook/1_2/en/web_server_iis),
mas eles se referem �s vers�es anteriores que n�o evolu�ram com novas vers�es
de sistemas operacionais Microsoft Windows, especialmente Windows Server 2008
(lan�ado em fevereiro de 2008), que inclui muitas mudan�as de interesse para desenvolvedores PHP:

* IIS vers�o 7, a vers�o embutida no Windows Server 2008, foi inteiramente
   reescrita para um design totalmente modular.

* IIS 7 provou ser muito confi�vel, com muito poucas corre��es necess�rias de
   Windows Update, desde o lan�amento do produto.

* O IIS 7 tamb�m inclui o acelerador FastCGI, um pool de aplicativos multi-threaded
   que tira proveito do modelo de segmenta��o nativo do sistemas operacionais Windows.

* A implementa��o do PHP FastCGI equivale a um desempenho 5x a 10x
   melhor na execu��o, sem cache, quando comparado ao tradicional ISAPI
   ou implanta��es CGI do PHP em Windows e IIS.

* Mais recentemente, a Microsoft mostrou um acelerador de cache para o PHP,
   que est� em status de lan�amento *Release Candidate* no momento da reda��o deste texto (02/11/2009).

>**SIDEBAR**
>Planejamento de Extens�o para este tutorial
>
>Uma se��o complementar deste cap�tulo est� em constru��o e ser� liberada
>no site do symfony projeto web logo ap�s a publica��o 
>deste livro. Abrange a conex�o com MS SQL Server atrav�s do PDO, algo que a
>Microsoft planeja melhorias em breve.
>
>[PHP_PDO_MSSQL]
>extension=php_pdo_mssql.dll
>
>Atualmente, o melhor desempenho na execu��o de c�digo � obtido pelo 
>driver nativo do Microsoft SQL Server para PHP 5, um driver open-source dispon�vel no Windows
>e atualmente dispon�vel na vers�o 1.1. Isso � implementado como uma
>nova extens�o DLL do PHP:
>
>[PHP_SQLSRV]
>extension=php_sqlsrv.dll
>
>� poss�vel usar o Microsoft SQL Server 2005 ou 2008 como
>banco de dados. A extens�o de tutorial planejada ir� cobrir o uso da
>edi��o que est� dispon�vel de gra�a: SQL Server Express.

### Como utilizar este tutorial em diferentes sistemas Windows, incluindo 32-bit

Este documento foi escrito especificamente para edi��es do Windows Server 2008 de 64-bits 
. Entretanto, voc� deve ser capaz de usar as outras vers�es, sem quaisquer complica��es.

>**NOTE**
>A vers�o exata do software operacional utilizado na tela �
>Windows Server 2008 Enterprise Edition com Service Pack 2, 64-bit edition.

#### Vers�es do Windows 32-bit

O tutorial � facilmente transport�vel para vers�es do Windows 32-bit, substituindo
as seguintes refer�ncias no texto:

* Em edi��es 64-bit: `C:\Program Files (x86)\` e `C:\Windows\SysWOW64\`

* Em edi��es de 32-bit: `C:\Program Files\` e `C:\Windows\System32\`

#### Sobre outras edi��es do Enterprise

Al�m disso, se voc� n�o tiver Enterprise Edition, este n�o � um problema. Essa
documenta��o � diretamente port�vel para outras edi��es do Windows Server:
Windows Server 2008 Web, Standard ou Datacenter Windows Server 2008 Web
Standard ou Datacenter, com Service Pack 2 do Windows Server 2008 R2 Web
Edi��es Standard, Enterprise ou Datacenter.

Por favor, note que todas as edi��es do Windows Server 2008 R2 est�o dispon�veis apenas como
sistemas operacionais 64-bit.

#### Sobre a Edi��es Internacionais

As configura��es regionais usadas nas imagens s�o `en-US`. N�s tamb�m
instalamos um pacote de linguagem internacional para a Fran�a.

� poss�vel executar o tutorial sobre sistemas operacionais Windows clientes:
Windows XP, Windows Vista e Windows Seven, tanto em modos x64 e x86.

### Servidor Web utilizado em todo o documento

O servidor web utilizado � o Microsoft Internet Information Server vers�o 7.0,
que � inclu�do em todas as edi��es como parte do Windows Server 2008. Come�amos o
tutorial com um servidor Windows Server 2008 totalmente funcional e instalamos o IIS
a partir do zero. As etapas de instala��o usam as op��es padr�es, bastando adicionar dois
m�dulos espec�ficos que vem com o design modular do IIS 7.0: **FastCGI** e 
**URL Rewrite**.

### Bancos de Dados

SQLite � o banco de dados pr�-configurado para no sandbox do symfony. No Windows,
n�o h� nada espec�fico para instalar: SQLite � diretamente implementado na
extens�o PDO do PHP para o SQLite, que � instalado no momento da instala��o do PHP.

Assim, n�o h� necessidade de baixar e executar uma inst�ncia separada do SQLITE.EXE:

      [PHP_PDO_SQLITE]
      extension=php_pdo_sqlite.dll

### Configura��o do Windows Server

� melhor usar uma nova instala��o do Windows Server, a fim de corresponder
as capturas de telas (*screenshots*) do passo-a-passo neste cap�tulo.

Claro que voc� pode trabalhar diretamente em uma m�quina existente, mas voc� pode encontrar
diferen�as devido aos softwares instalados, tempo de execu��o, e as configura��es regionais.

A fim de obter as mesmas telas que aparecem no
tutorial, � recomend�vel a obten��o de um Windows Server dedicado em um 
ambiente virtual, dispon�vel gratuitamente na Internet por um per�odo de 30 dias.

>**SIDEBAR** 
>Como obter um Windows Server Trial gratuito?
>
>� claro que � poss�vel utilizar qualquer servidor dedicado com acesso � Internet. Um
> servidor f�sico ou mesmo servidor virtual dedicado (VDS) vai funcionar perfeitamente.
>
>Um servidor dispon�vel para avalia��o por 30 dias com o Windows est� no Ikoula, um provedor franc�s, 
>que oferece uma lista abrangente de servi�os para os desenvolvedores e
>designers. Esta avalia��o come�a em 0 � / m�s para uma m�quina virtual Windows
>sendo executado em um ambiente Microsoft Hyper-V. Sim, voc� pode obter uma
> m�quina virtual com o Windows Server 2008 Web plenamente funcional, Standard, Enterprise
>ou mesmo a edi��o Datacenter gratuitamente durante um per�odo de 30 dias.
>
>Para solicitar, basta abrir no navegador http://www.ikoula.com/flex_server e
>clique no bot�o "Testez gratuitement".
>
>A fim de obter as mesmas mensagens descritas neste documento, o sistema operacional 
>que solicitamos ao servidor Flex �: "Windows Server 2008 Enterprise Edition
>64 bits". Esta � uma distribui��o x64, entregues com as l�nguas fr-FR
>e en-US. � f�cil mudar de `fr-FR` para `en-US` e vice-versa
>a partir do Painel de Controle do Windows. Especificamente, esta configura��o pode ser encontrada
>em "Regional and Language Options", que fica sob a guia
>"Keyboards and Languages". Basta clicar em "Install/uninstall languages".

� necess�rio ter acesso de Administrador no servidor.

Se estiver trabalhando em uma esta��o de trabalho remota, o leitor deve executar Remote Desktop
Services (anteriormente conhecido como cliente do Terminal Server) e garantir que ele tem
acesso de Administrador.

A distribui��o usada aqui �: Windows Server 2008 com Service Pack 2.

![Verifique o seu ambiente de in�cio, com o comando Winver - aqui em Ingl�s](http://www.symfony-project.org/images/more-with-symfony/windows_01.png)

Windows Server 2008 foi instalado com o ambiente gr�fico, que
utiliza visual do Windows Vista. Tamb�m � poss�vel usar uma linha de comando
apenas para a vers�o do Windows Server 2008 com os mesmos servi�os, a fim de reduzir o tamanho da distribui��o (1,5 GB em vez de 6,5 GB). Isto tamb�m reduz a �rea de ataque e o n�mero de patches do Windows Update, que ter�o de ser aplicados.

Verifica��es preliminares - Servidor Dedicado na Internet
-----------------------------------------------------

Uma vez que o servidor est� diretamente acess�vel na Internet, � sempre uma boa
id�ia verificar se o Firewall do Windows est� fornecendo prote��o ativa. As �nicas
exce��es que devem ser verificadas s�o:

* Core Networking
* Remote Desktop (se acessado remotamente)
* Secure World Wide Web Services (HTTPS)
* World Wide Web Services (HTTP)

![Verifique as configura��es de firewall, diretamente no painel de controle.](http://www.symfony-project.org/images/more-with-symfony/windows_02.png)

Ent�o, � sempre bom para executar o Windows Update para garantir que todos os pacotes do software 
est�o instalados com as �ltimas corre��es, patches e documenta��o.

![Verificar status do Windows Update, diretamente no Painel de controle.](Http://www.symfony-project.org/images/more-with-symfony/windows_03.png)

Como �ltima etapa de prepara��o, e por uma quest�o de eliminar quaisquer potenciais
par�metros conflitantes na distribui��o existente no Windows ou configura��o do IIS,
recomendamos que voc� desinstale o Servi�o Web do servidor Windows, se previamente
instalado.

![Retire o servi�o Servidor Web (*Web Server role*), a partir do Server Manager.](http://www.symfony-project.org/images/more-with-symfony/windows_04.png)

Instalando PHP - Apenas alguns cliques de dist�ncia
---------------------------------------

Agora, podemos instalar o IIS e o PHP em uma opera��o simples.

PHP n�o � uma parte da distribui��o do Windows Server 2008, portanto, n�s precisamos
instalar primeiro o Microsoft Web Platform Installer 2.0, denominada Web PI
nas se��es seguintes.

Web PI toma cuidado de instalar todas as depend�ncias necess�rias para a execu��o de PHP
em qualquer Windows / sistema do IIS. Ent�o, ele instala o IIS com o m�nimo de Role Services 
para o servidor Web, e tamb�m oferece op��es m�nimas para o PHP runtime.

![http://www.microsoft.com/web - Fa�a o download agora.](http://www.symfony-project.org/images/more-with-symfony/windows_05.png)

A instala��o do Microsoft Web Platform Installer 2.0 cont�m um
analisador de configura��o, verifica m�dulos existentes, prop�e as atualiza��es de m�dulos necess�rias, 
e ainda permite que voc� fa�a um beta-teste de extens�es ainda n�o lan�adas da
Microsoft Web Platform.

![Web PI 2.0 - Primeira Vis�o.](http://www.symfony-project.org/images/more-with-symfony/windows_06.png)

Web PI 2.0 oferece a instala��o do PHP runtime em um clique. A sele��o
instala a implementa��o Win32 "non-thread safe" do PHP, que �
melhor associada ao IIS 7 e FastCGI. Tamb�m oferece o mais recente
runtime testado, aqui 5.2.11. Para encontr�-lo, basta selecionar a guia "Frameworks and 
Runtimes" � esquerda:

![PI Web 2.0 - Guia Frameworks e Runtimes.](http://www.symfony-project.org/images/more-with-symfony/windows_07.png)

Depois de selecionar o PHP, Web PI 2,0 automaticamente s�o selecionadas todas as depend�ncias necess�rias
para servir p�ginas web `.php` armazenadas no servidor, incluindo o m�nimo
IIS 7.0 roles services:

![PI Web 2.0 - Depend�ncias automaticamente adicionadas - 1 / 3.] (http://www.symfony-project.org/images/more-with-symfony/windows_08.png)

![PI Web 2.0 - Depend�ncias automaticamente adicionadas - 2 / 3.](http://www.symfony-project.org/images/more-with-symfony/windows_09.png)

![PI Web 2.0 - Depend�ncias automaticamente adicionadas - 3 / 3.](http://www.symfony-project.org/images/more-with-symfony/windows_10.png)

Em seguida, clique em Install, em seguida, no bot�o "I Accept". A instala��o dos componentes IIS 
come�ar� enquanto, paralelamente, o PHP � transferido
[runtime](http://windows.php.net) e alguns m�dulos s�o atualizados (atualiza��o para um
IIS FastCGI 7,0 por exemplo).

![PI Web 2.0 - Instala��o dos componentes do IIS enquanto as atualiza��es s�o baixadas da web.](http://www.symfony-project.org/images/more-with-symfony/windows_11.png)

Finalmente, o programa de instala��o do PHP � executado, e, ap�s alguns minutos, dever� apresentar:

![PI Web 2.0 - Instala��o do PHP foi finalizada.](http://www.symfony-project.org/images/more-with-symfony/windows_12.png)

Clique em "Finish".

O Windows Server est� escutando e agora � capaz de ouvir e responder na porta 80.

Vamos verificar isso no navegador:

![Firefox - IIS 7.0 est� respondendo na porta 80.](http://www.symfony-project.org/images/more-with-symfony/windows_13.png)

Agora, para verificar que o PHP est� instalado corretamente, e dispon�vel a partir do IIS, n�s
criamos um pequeno arquivo `phpinfo.php` a ser acessado pelo servidor web padr�o
na porta 80, em `C:\inetpub\wwwroot`.

Antes de fazer isso, garantimos que, no Windows Explorer, podemos ver as 
extens�es corretas dos arquivos. Selecione "Unhide Extensions for Known Files Types".

![Windows Explorer - Unhide Extensions for Known Files Types.](http://www.symfony-project.org/images/more-with-symfony/windows_14.png)

Abra o Windows Explorer e v� para "C:\inetpub\wwwroot`. Clique com o bot�o direito do mouse e selecione
"New Text Document". Renomeie para `phpinfo.php` e copie a chamada de fun��o 
usual:

![Windows Explorer - Criar phpinfo.php.] (Http://www.symfony-project.org/images/more-with-symfony/windows_15.png)

Em seguida, reabrir o navegador web, e colocar `/phpinfo.php` no final da
URL do servidor:

![Firefox - Execu��o do phpinfo.php est� OK](http://www.symfony-project.org/images/more-with-symfony/windows_16.png)

Finalmente, para garantir que o symfony ir� instalar sem problemas, fa�a o download
[http://sf-to.org/1.3/check.php] ( `check_configuration.php`).

![PHP - Onde baixar check.php.](http://www.symfony-project.org/images/more-with-symfony/windows_17.png)

Copie para o mesmo diret�rio como `phpinfo.php` ( `C:\inetpub\wwwroot`) e
renomeie para `check_configuration.php` se necess�rio.

![PHP - Copie e renomeie o check_configuration.php.] (Http://www.symfony-project.org/images/more-with-symfony/windows_18.png)

Por fim, reabra o navegador web uma �ltima vez para agora, e coloque
`/check_configuration.php` no final da URL do servidor:

![Firefox - Execu��o check_configuration.php est� OK.] Http://www.symfony-project.org/images/more-with-symfony/windows_19.png ()

Executando o PHP na interface de linha de comando
---------------------------------------------

Para depois executar as tarefas da linha de comando com o symfony, precisamos assegurar que o 
PHP.EXE � acess�vel a partir do prompt de comando e � executad corretamente.

Abra um prompt de comando para `C:\inetpub\wwwroot` e digite 

    PHP phpinfo.php

A seguinte mensagem de erro deve aparecer:

![PHP - MSVCR71.DLL n�o foi encontrado.](http://www.symfony-project.org/images/more-with-symfony/windows_20.png)

Se n�o fizermos nada, a execu��o de PHP.EXE trava na aus�ncia do 
MSVCR71.DLL. Ent�o, temos de encontrar o arquivo DLL e instal�-lo no local 
correto.

Este `MSVCR71.DLL` � uma vers�o do Microsoft Visual C + + runtime, que
remonta � �poca de 2003. Ele est� contido no pacote redistribu�vel 
.NET Framework 1.1.

O pacote redistribu�vel .NET Framework 1.1, pode ser baixado em
[MSDN](http://msdn.microsoft.com/en-us/netframework/aa569264.aspx)

O arquivo que estamos procurando � instalado no seguinte diret�rio:
`C:\Windows\Microsoft.NET\Framework\v1.1.4322`

Basta copiar o para `MSVCR71.DLL` para o seguinte destino:

* Em sistemas x64: o diret�rio `C:\windows\syswow64` 
* Em sistemas x86: o diret�rio `C:\windows\system32`

Podemos agora desisntalar o .Net Framework 1.1.

O execut�vel PHP.EXE agora pode ser executado no prompt de comando sem erro.
Por exemplo:

    PHP phpinfo.php
    PHP check_configuration.php

Mais tarde, n�s vamos verificar que symfony.bat (a partir da distribui��o Sandbox) tamb�m
d� a resposta esperada, que � a sintaxe do comando symfony.

Instala��o e Uso do Sandbox do symfony 
--------------------------------------

O par�grafo seguinte � um trecho do "Guia de Introdu��o ao symfony",
["The Sandbox"](http://www.symfony-project.org/getting-started/1_3/en/A-Sandbox):
p�gina: "O sandbox � um projeto symfony pr�-empacotado super f�cil de instalar,
j� configurado com alguns padr�es razo�veis. � uma �tima forma de praticar o
symfony, sem o inc�modo de uma instala��o adequada que respeite a
melhores pr�ticas da web".

O sandbox � pr�-configurado para usar o SQLite como banco de dados. No Windows,
n�o h� nada espec�fico para instalar: O SQLite � diretamente implementado na
extens�o PDO do PHP para o SQLite, que � instalado juntamente com
o PHP. N�s j� realizamos isto antes, quando o PHP runtime 
foi instalado atrav�s do Microsoft Web PI.

Basta verificar se a extens�o SQLite est� corretamente referida no arquivo 
PHP.INI, que reside no diret�rio `C:\Program Files (x86)\PHP`, e que
a DLL que implementa o suporte PDO para SQLite est� definida como 
`C:\Program Files (x86)\PHP\ext\php_pdo_sqlite.dll`.

![PHP - Localiza��o do arquivo de configura��o php.ini.](http://www.symfony-project.org/images/more-with-symfony/windows_21.png)

### Baixar, criar Diret�rio, copiar todos os Arquivos

O projeto sandbox do symfony est� "pronto para instalar e executar", e vem em um 
arquivo `.zip`.

Baixe o [arquivo](http://www.symfony-project.org/get/sf_sandbox_1_3.zip)
e extraia-o para em um local tempor�rio, como o diret�rio "downloads",
que est� dispon�vel para leitura/escrita no diret�rio `C:\User\administrador`.

![sandbox - Baixe e descompacte o arquivo.](http://www.symfony-project.org/images/more-with-symfony/windows_22.png)

Crie um diret�rio para o destino final do sandbox, como `F:\dev\sfsandbox`:

![sandbox - Criar sfsandbox Directory.](http://www.symfony-project.org/images/more-with-symfony/windows_23.png)

Selecione todos os arquivos - `CTRL-A` no Windows Explorer - a partir do seu local de download
(fonte), e os copie para o diret�rio `F:\dev\sfsandbox`.

Voc� dever� ver 2599 itens copiados para o diret�rio de destino:

![sandbox - C�pia de 2599 itens.](http://www.symfony-project.org/images/more-with-symfony/windows_24.png)

### Teste de Execu��o

Abra o prompt de comando. V� para `F:\dev\sfsandbox` e execute o seguinte comando:

    PHP symfony -V

Isso deve retornar:

    symfony vers�o 1.3.0 (F:\dev\sfsandbox\lib\symfony)

A partir do mesmo prompt de comando, execute:

    SYMFONY.BAT -V

Isso deve retornar o mesmo resultado:

    symfony version 1.3.0 (F:\dev\sfsandbox\lib\symfony)

![Sandbox - Teste de linha de comando - Sucesso.] (http://www.symfony-project.org/images/more-with-symfony/windows_25.png)

### A cria��o de aplicativos Web

Para criar uma aplica��o Web no servidor local, utilize o gerenciador do IIS7,
que � o painel de controle da interface gr�fica do usu�rio para todas as atividades relacionadas com 
o IIS. Todas as a��es realizadas a partir da interface do usu�rio que s�o efetivamente realizadas por tr�s dos bastidores
atrav�s da interface de linha de comando.

O Gerenciador do IIS � acess�vel a partir do Menu *Start* em *Programs*,
*Administrative Tools*, *Internet Information Server (IIS) Manager*.

#### Reconfigurando o "Default Web Site" de modo a n�o interferir na Porta 80

Queremos garantir que apenas o nosso symfony sandbox est� respondendo na porta 80
(HTTP). Para fazer isso, altere a porta atual do "Default Web Site porta" para 8080.

![*IIS Manager* - Editar *Binding* para "Default Web Site".](Http://www.symfony-project.org/images/more-with-symfony/windows_26.png)

Observe que, se o Firewall do Windows estiver ativo, voc� poder� ter de criar uma 
exce��o para a porta 8080 para continuar a ser capaz de atingir o "Default Web Site". Para
esse efeito, v� para Windows Control Panel, selecione Windows Firewall, clique em
*"Allow a program through Windows Firewall"* e clique em *"Add port"* para criar
esta exce��o. Marque a caixa para ativ�-lo ap�s a cria��o.

![Firewall do Windows - Criar Uma Exce��o para a Porta 8080.] (Http://www.symfony-project.org/images/more-with-symfony/windows_27.png)

#### Adicionar um Novo Site para a Sandbox

Abra o *IIS Manager* a partir *Administration Tools*. No painel esquerdo, selecione o �cone "Sites"
e clique com o bot�o direito. Selecione *Add Web Site* a partir do menu de contexto. Digite, por
exemplo, "symfony Sandbox" como o nome do site, `D:\dev\sfsandbox` para a Physical 
Path, e deixe os outros campos inalterados. Voc� ver� esta caixa de di�logo:

![IIS Manager - Adicionando o Web Site.](Http://www.symfony-project.org/images/more-with-symfony/windows_28.png)

Clique em OK. Se um pequeno `X` aparece no �cone do site (em Features View /
Sites), n�o deixe de clicar em "Restart" no painel direito para faz�-lo 
desaparecer.

#### Verifique se o Site est� Respondendo

Do IIS Manager, selecione o site "symfony Sandbox", e, no painel da direita,
Clique em "Browse *. 80 (http)".

![IIS Manager - Clique em Browse port 80.](http://www.symfony-project.org/images/more-with-symfony/windows_29.png)

Voc� dever� receber uma mensagem de erro expl�cita, isso n�o � inesperado:
`HTTP Error 403.14 - Forbidden`.
O servidor Web est� configurado para n�o listar o conte�do deste diret�rio.

Isto � originado a partir da configura��o padr�o do servidor web, que especifica
que o conte�do deste diret�rio n�o deve ser listado. Uma vez que nenhum arquivo 
padr�o como `index.php` ou `index.html` existe em `D:\dev\sfsandbox`, o
servidor retorna corretamente a mensagem de erro "Forbidden". N�o tenha medo.

![Internet Explorer - Erro Normal.](http://www.symfony-project.org/images/more-with-symfony/windows_30.png)

Digite `http://localhost/web` na barra de endere�os do seu navegador, em vez de apenas
http://localhost `. Agora voc� deve ver o seu navegador, por padr�o o Internet
Explorer, exibindo "symfony Project Created":

![IIS Manager - Digite http://localhost/web na URL. Sucesso!](http://www.symfony-project.org/images/more-with-symfony/windows_31.png)

A prop�sito, voc� pode ver uma faixa amarela no topo dizendo 
"Intranet settings are now turned off by default". Configura��es de Intranet s�o menos seguras do que
Configura��es de Internet. Clique para ver op��es. N�o tenha medo desta mensagem.

Para fech�-lo permanentemente, clique com o bot�o direito do mouse na faixa amarela, e selecione a
op��o apropriada.

Esta tela confirma que a p�gina padr�o `index.php` foi corretamente carregado
a partir de `D:\dev\sfsandbox\web\index.php`, executado corretamente, e que as bibliotecas symfony
est�o corretamente configuradas.

Temos que realizar uma �ltima tarefa antes de come�ar a brincar com o symfony
Sandbox: configurar a p�gina web do front-end importando as regras de reescrita de URL.
Estas regras s�o implementadas como arquivos `.htaccess` e podem ser controladas em
apenas alguns cliques no Gerenciador do IIS.

### Sandbox: Configura��o do Front-end Web

N�s queremos configurar aplica��o sandbox a fim de come�ar a
brincar realmente com as coisas do symfony. Por padr�o, a primeira p�gina final pode ser
alcan�ada e executa corretamente quando solicitada a partir da m�quina local
(isto �, o nome localhost ou o endere�o `127.0.0.1`).

![Internet Explorer - p�gina frontend_dev.php est� OK a partir de localhost.](http://www.symfony-project.org/images/more-with-symfony/windows_32.png)

Vamos explorar as "configuration", "logs" e "timers" os pain�is de debug web para
garantir que o sandbox esteja totalmente funcional no Windows Server 2008.

![uso do sandbox: configuration.](http://www.symfony-project.org/images/more-with-symfony/windows_33.png)

![uso do sandbox: logs.](http://www.symfony-project.org/images/more-with-symfony/windows_34.png)

![uso do sandbox: timers.](http://www.symfony-project.org/images/more-with-symfony/windows_35.png)

Enquanto n�s poder�amos tentar acessar a aplica��o sandbox da Internet ou
a partir de um endere�o IP remoto, o sandbox � mais concebido como uma ferramenta para aprender o
framework symfony na m�quina local. Portanto, n�s vamos cobrir detalhes relacionados
ao acesso remoto na �ltima se��o: Projeto: Configura��o do Front-end Web.

Cria��o de um novo Projeto symfony
---------------------------------

Criar um ambiente de projeto symfony para fins de desenvolvimento real � quase
t�o simples como a instala��o do sandbox. Vamos ver todo o
processo de instala��o de um procedimento simplificado, que � equivalente a
instala��o e implanta��o do sandbox.

A diferen�a � que, neste se��o "projeto", vamos nos concentrar na
configura��o da aplica��o Web para fazer funcionar de qualquer lugar
Internet.

Como o sandbox, o projeto symfony vem pr�-configurado para usar o SQLite como 
motor de base de dados. Este foi instalado e configurado no in�cio deste cap�tulo.

### Baixar, criar um diret�rio e copiar os arquivos

Cada vers�o do symfony pode ser baixada como um arquivo zip e ent�o usada para
criar um projeto do zero.

Baixe o arquivo contendo a biblioteca do
[website symfony](http://www.symfony-project.org/get/symfony-1.3.0.zip).
Em seguida, extraia o diret�rio contido em um local tempor�rio, como a
diret�rio de "downloads".

![Windows Explorer - Baixe e descompacte o arquivo do projeto.] (Http://www.symfony-project.org/images/more-with-symfony/windows_37.png)

Agora precisamos criar uma �rvore de diret�rios para o destino final do
projeto. Isto � um pouco mais complicado do que o sandbox.

### �rvore de diret�rios de instala��o

Vamos criar uma �rvore de diret�rios para o projeto. Iniciar a partir da raiz do volume,
`D:` por exemplo.

Criar um diret�rio `\dev` em `D:`, e criar outro diret�rio chamado
`sfproject` l�:

    D:
    MD dev
    CD dev
    MD sfproject
    CD sfproject

Estamos agora em: `D:\dev\sfproject`

A partir da�, criar uma �rvore de subdiret�rios, criando os diret�rios `lib`, `vendor`
e `symfony` em cascata:

    MD lib
    CD lib
    MD vendor
    CD vendor
    CD vendor
    CD symfony

Estamos agora em: `D:\dev\sfproject\lib\vendor\symfony`

![Windows Explorer - a �rvore de diret�rios do projeto.](http://www.symfony-project.org/images/more-with-symfony/windows_38.png)

Selecione todos os arquivos (CTRL + `A` no Windows Explorer) a partir do seu local de download
(fonte), e copie de Downloads para `D:\dev\sfproject\lib\vendor\symfony`.
Voc� dever� ver 3819 itens copiados para o diret�rio de destino:

![Windows Explorer - C�pia de 3819 itens.] (Http://www.symfony-project.org/images/more-with-symfony/windows_39.png)

### Cria��o e inicializa��o

Abra o prompt de comando. Mude para o diret�rio `D:\dev\sfproject` e execute
o seguinte comando:

    PHP lib\vendor\symfony\data\bin\symfony -V

Isso deve retornar:

    symfony version 1.3.0 (D:\dev\sfproject\lib\vendor\symfony\lib)

Para iniciar o projeto, basta executar o seguinte linha de comando PHP:

    PHP lib\vendor\symfony\data\bin\symfony generate:project sfproject

Isso deve retornar uma lista de opera��es de arquivo, incluindo alguns comandos 
`chmod 777`:

![Windows Explorer - Inicializa��o do Projeto OK.](http://www.symfony-project.org/images/more-with-symfony/windows_40.png)

Ainda no prompt de comando, crie uma aplica��o symfony, executando o
seguinte comando:

    PHP lib\vendor\symfony\data\bin\symfony generate:app sfapp

Novamente, este deve retornar uma lista de opera��es de arquivo, incluindo alguns 
comandos `chmod 777`.

A partir deste ponto, ao inv�s de digitar `PHP lib\vendor\symfony\data\bin\symfony`
cada vez que for necess�rio, copie o arquivo `symfony.bat` desde a sua origem:

    copy lib\vendor\symfony\data\bin\symfony.bat

Temos agora um comando conveniente para ser executado na linha de comando no prompt
`D:\dev\sfproject`.

Ainda em `D:\dev\sfproject`, podemos agora executar o comando cl�ssico:

    symfony -V

para obter a resposta cl�ssica:

    symfony version 1.3.0 (D:\dev\sfproject\lib\vendor\symfony\lib)

### A cria��o de aplicativos Web

Nas linhas que se segue, vamos supor que voc� j� leu em "Sandbox: Cria��o do Front-end Web" 
os passos preliminares para reconfigurar o" Default Web Site" para 
que n�o interfira na porta 80.

#### Adicione um novo Web Site para o Projeto

Abra o IIS Manager a partir do Administration Tools. No painel esquerdo, selecione o icone "Sites"
e clique com o bot�o direito do mouse. Selecione "Add Web Site" do menu popup. Digite, por
exemplo, "symfony Project" como o nome do site, `D:\dev\sfproject` para a
"Physical Path", e deixar os outros campos inalterados; voc� ver� esta caixa 
de di�logo:

![IIS Manager - Add Web Site.](http://www.symfony-project.org/images/more-with-symfony/windows_41.png)

Clique em OK. Se um pequeno `x` aparece no �cone do site (em Features View / 
Sites), n�o deixe de clicar em "Restart" no painel direito para faze-lo
desaparecer.

#### Verifique se o Web Site est� respondendo

A partir do *IIS Manager*, selecione o site "Symfony Project", e, no painel da direita,
clique em "Browse *. 80 (http)".

Voc� deve obter a mesma mensagem de erro expl�cita como voc� tinha quando se testava o sandbox:

    *HTTP Error 403.14 - Forbidden*

O servidor Web est� configurado para n�o listar o conte�do deste diret�rio.

Digite `http://localhost/web` na barra de endere�os do seu navegador, voc� deve agora
ver a p�gina "*Symfony Project Created*", mas com uma discreta diferen�a da 
mesma p�gina resultante de inicializa��o do sandbox: n�o existem imagens:

![Internet Explorer - *symfony Project Created* - sem imagens.] (Http://www.symfony-project.org/images/more-with-symfony/windows_42.png)

As imagens n�o est�o aqui por enquanto, por�m elas est�o localizadas em um diret�rio `sf`
na biblioteca symfony. � f�cil lig�-los ao
diret�rio `/` web, adicionando um diret�rio virtual em `/web, nomeado
`sf`, e apontando para `D:\dev\sfproject\lib\vendor\symfony\data\web\sf`.

![IIS Manager - Adicionar o Diret�rio Virtual sf.](Http://www.symfony-project.org/images/more-with-symfony/windows_43.png)

Agora temos a p�gina "symfony Project Created" regular com imagens como
esperado:

![Internet Explorer - symfony Project Created- com imagens.](http://www.symfony-project.org/images/more-with-symfony/windows_44.png)

E, finalmente, toda a aplica��o symfony est� funcionando. A partir do navegador da Web,
digite o endere�o da aplica��o web, i.e. `http://localhost/web/sfapp_dev.php`:

![Internet Explorer - p�gina sfapp_dev.php est� OK a partir de localhost.](http://www.symfony-project.org/images/more-with-symfony/windows_45.png)

Vamos realizar um �ltimo teste no modo de local: verificar os pain�is do web debug "configuration",
"logs" e "timers" para garantir que o projeto est� totalmente
funcional.

![Internet Explorer - P�gina de logs est� OK a partir de localhost.](http://www.symfony-project.org/images/more-with-symfony/windows_46.png)

### Configura��o da Aplica��o para Aplica��es Prontas para Internet 

Nosso projeto symfony gen�rico est� agora trabalhando localmente, como o sandbox, a partir da
servidor host local, localizado em `http://localhost` ou `http://127.0.0.1`.

Agora, n�s gostar�amos de ser capazes de acessar o aplicativo da Internet.

A configura��o padr�o do projeto protege a aplica��o de 
ser executada de um local remoto, embora, na realidade, tudo deve estar ok
para acessar os arquivos `index.php` e `sfapp_dev.php`. Vamos executar o
projeto a partir do navegador da Web, usando o endere�o IP do servidor externo
(por exemplo `94.125.163.150`) e o FQDN do nosso Servidor Dedicado Virtual
(por exemplo, `12543hpv163150.ikoula.com`). Voc� ainda pode usar os dois endere�os
a partir de dentro do servidor, uma vez que eles n�o est�o mapeadas ao `127.0.0.1`:

![Internet Explorer - Acesso a index.php pela Internet est� OK.](http://www.symfony-project.org/images/more-with-symfony/windows_47.png)

![Internet Explorer - A execu��o de sfapp_dev.php da Internet n�o est� OK.](http://www.symfony-project.org/images/more-with-symfony/windows_48.png)

Como dissemos antes, o acesso a `index.php` e `sfapp_dev.php` de um
localiza��o remota est� ok. A execu��o do `sfapp_dev.php` entretanto falha, pois
n�o � permitida por padr�o. Isso impede usu�rios maliciosos de
acessarem seu ambiente de desenvolvimento, que cont�m informa��es potencialmente sens�veis
sobre o projeto. Voc� pode editar o arquivo `sfapp_dev.php` para fazer
o trabalho, mas isto � fortemente desencorajado.

Finalmente, podemos simular um dom�nio real, editando o arquivo "hosts".

Este arquivo executa a resolu��o de nomes FQDN local, sem necessidade de instalar o
Servi�o de DNS no Windows. O servi�o de DNS est� dispon�vel em todas as edi��es do
Windows Server 2008 R2, e tamb�m no Windows Server 2008 Standard, Enterprise
e Datacenter.

Em sistemas operacionais Windows x64, o arquivo "hosts" est� localizado por padr�o em:
`C:\Windows\SysWOW64\Drivers\etc`

O arquivo "hosts" � pr�-preenchido para a m�quina poder resolver `localhost` para
`C:\Windows\SysWOW64\Drivers\etc`

Vamos adicionar um nome real de dom�nio falso, como o `sfwebapp.local`, e poder
resolv�-lo localmente.

![Altera��es aplicadas ao arquivo "hosts".](http://www.symfony-project.org/images/more-with-symfony/windows_50.png)

Seu projeto symfony agora roda na Internet, sem DNS, a partir de uma sess�o de navegador web
executada de dentro do servidor web.