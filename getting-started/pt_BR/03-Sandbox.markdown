Sandbox
=======
Se seu objetivo é dar ao symfony uma chance por algumas horas, continue
lendo este capítulo e iremos lhe mostrar o caminho mais rápido para começar.
Se quiser um projeto mais real, você pode pular este capítulo e ir para o 
[próximo](#chapter_04-Symfony-Installation).

O jeito mais fácil de experimentar o symfony, é instalar o symfony sandbox. O
sandbox é um pré-pacote fácil de instalar do symfony, já configurado com algumas
definições padrões. É uma boa maneira de praticar usando o symfony sem o incômodo
de instalar por completo, respeitando as boas práticas de desenvolvimento.

>**CUIDADO**
>Como o sandbox é pré-configurado para usar o SQLite como banco de dados,
>você precisa checar se o seu PHP suporta ele (veja o capítulo de 
>[Pré-Requisitos](#chapter_02-Prerequisites)). Você pode também ler
>o [Configurando o Banco de Dados](#chapter_05-Project-Setup_sub_configuring_the_database)
>para aprender como mudar o banco de dados usado pelo sandbox.

Você pode fazer o download do sandbox no formato '.tgz' ou '.zip' do
symfony [installation page](http://www.symfony-project.org/installation/1_2)
ou pela seguinte URL:

    http://www.symfony-project.org/get/sf_sandbox_1_2.tgz

    http://www.symfony-project.org/get/sf_sandbox_1_2.zip

Extraia os arquivos em algum lugar sob seu diretório web root, e pronto.
Seu projeto symfony já está acessível, chamando o 'web/index.php' de um
navegador.

>**CUIDADO**
>Colocando todos os arquivos do symfony no diretório root é bom para
>testá-lo no seu computador, mas é uma péssima ideia para o servidor de
>produção pois faz todos as suas aplicações serem visíveis para os usuários
>finais.

Agora, você pode finalizar sua instalação lendo os capítulos 
[Configurando seu Web Server](#chapter_06-Web-Server-Configuration)
e [Ambientes](#chapter_07-Environments).

>**NOTA**
>Como o sandbox é um projeto normal do symfony, onde algumas tarefas
>tem que ser executada por você e algumas configurações são mudadas,
>ser[a fácil de usá-la como ponto inicial para um novo projeto. Entretanto, 
>tenha em mente que você provavelmente necessitará adaptar as configurações.
>Por exemplo: mudando suas definições de segurança (veja a configuração
>de XSS e CSRF no final deste tutorial)
