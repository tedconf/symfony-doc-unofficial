Pré-Requisitos
==============

Antes de instalar o symfony, você precisa checar se seu computador tem tudo
instalado e configurado corretamente. Leia conscientemente este capítulo e
siga todos os passos requeridos para checar sua configuração, pois poderá te
poupar bastante tempo.

Software
--------
	
Primeiro de tudo, você tem que checar se seu computador tem um ambiente de trabalho
adequado para desenvolvimento web. Você precisa no mínimo de um Web Server (Apache, 
por exemplo), um banco de dados (MySQL, PostgreSQL, SQLite o qualquer compatível com
o PDO) e o PHP 5.2.4 ou maior.


Interface de Linha de Comando
-----------------------------

O symfony vem embutido com uma ferramenta de linha de comando (Command Line 
Interface) que automatiza muito o seu trabalho. Se você é um adorador de Unix,
se sentirá em casa. Se usa o sistema Windows, também vai rodar bem,	mas você 
terá que executar alguns comandos no prompt do `cmd` .

>**Nota**
>Comandos de Unix pode ser útil em ambiente Windows.
>Se quiser usar ferramentas como 'tar', 'gzip' ou 'grep' no Windows,
>tente instalar o [Cygwin](http://cygwin.com/). A documentação oficial é
>um pouco esparsa, então um bom guia de instalação pode ser achado 
>[aqui](http://www.soe.ucsc.edu/~you/notes/cygwin-install.html).
>Os aventureiros podem tentar também o Microsoft 
>[Windows Services for Unix](http://technet.microsoft.com/en-gb/interopmigration/bb380242.aspx).


Configuração do PHP
-------------------
Como a configuração do PHP pode variar muito de um SO para outro, ou mesmo
entre diferentes distribuições do Linux, você precisa checar se as elas
atendendem aos requerimentos mínimos do symfony.

Primeiro, assegure que você tenha o PHP 5.2.4 com a instalação mínima, 
usando 'phpinfo()' como função ou executando o comando 'php -v' na linha
de comando. Esteja alerta que em algumas configurações, você pode ter duas
versões diferentes do PHP instaladas: uma para a linha de comando, e a outra
para web.

Então, faça o download do script que checa as configurações do symfony na URL:

   http://sf-to.org/1.2/check.php
   
Execute o script na linha de comando:

   $ php check_configuration.php

Se houver um problema com a configuração do PHP, o output do comando lhe 
dará uma informação de como consertar.

Você também deve executar o script em um navegador e corrigir os problemas 
que possam ser descobertos. Isso porque o PHP pode ter a configuração do 
'php.ini' distintas para esses dois ambientes, com diferentes definições.

>**NOTA**
>Não esqueça de remover o arquivo do seu diretório root depois.
