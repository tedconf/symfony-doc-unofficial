symfonyのインストール
====================

### プロジェクトのディレクトリ

symfonyをインストールする前に、最初にプロジェクトに関連するすべてのファイルをホストするディレクトリを作成する必要があります:

    $ mkdir -p /home/sfproject
    $ cd /home/sfproject

Windowsでは:

    c:\> mkdir c:\dev\sfproject
    c:\> cd c:\dev\sfproject

>**NOTE**
>Windowsユーザーの方にはスペースを含まないパスでsymfonyで実行して新しいプロジェクトをセットアップすることをお勧めします。
>`My Documents`の下を含めて`Documents and Settings`ディレクトリを使うのは避けます。

-

>**TIP**
>Webrootディレクトリの元でsymfonyプロジェクトのディレクトリを作る場合、Webサーバーを設定する必要はありません。
>もちろん、運用環境に関して、Webサーバーの設定のセクションで説明されているようにWebサーバーを設定することを強くお勧めします。

### symfonyのインストール

symfonyフレームワークライブラリのファイルをホストするディレクトリを作成します:

    $ mkdir -p lib/vendor

symfonyをインストールする必要があります。
symfonyフレームワークにはいくつかの安定ブランチが存在しsymfony公式サイトの[インストールの手引きのページ](http://www.symfony-project.org/installation)を読みインストールしたいブランチを選ぶ必要があります。


たとえば[symfony 1.2](http://www.symfony-project.org/installation/1_2)など選んだバージョン用のインストールの手引きのページに向かいます。

"**Source Download**"セクションの下で、`.tgz`フォーマットもしくは`.zip`フォーマットでアーカイブが見つかります。
アーカイブをダウンロードし`lib/vendor/`ディレクトリの元に設置し再度解凍します:

    $ cd lib/vendor
    $ tar zxpf symfony-1.2.2.tgz
    $ mv symfony-1.2.2 symfony
    $ rm symfony-1.2.2.tgz

Windowsの環境下ではzipファイルをエクスプローラで展開できます。
ディレクトリを`symfony`にリネームすると、ディレクトリのフルパスは`c:\dev\sfproject\lib\vendor\symfony`になります。

>**TIP**
>Subversionを使う場合、プロジェクトの`lib/vendor/`にsymfonyに埋め込み安定ブランチのバグ修正から自動的に恩恵を受けるためには`svn:externals`プロパティを使うほうがベターです:
>
>     http://svn.symfony-project.com/branches/1.2/

symfonyのバージョンを表示するためにコマンドラインを使うことでsymfonyが正しくインストールされていることを確認します(大文字の`V`に注意):

    $ php ../..
    $ php lib/vendor/symfony/data/bin/symfony -V

Windowsでは:

    c:\> cd ..\..
    c:\> php lib\vendor\symfony\data\bin\symfony -V

>**TIP**
>このコマンドラインツールが何をできるのかご興味があれば、利用可能なオプションとタスクの一覧を表示するために`symfony`を入力します:
>
>     $ php lib/vendor/symfony/data/bin/symfony
>
>Windowsでは:
>
>     c:\> php lib\vendor\symfony\data\bin\symfony
>
>symfonyコマンドは開発者の最良の友です。
>キャッシュのクリア、コードの生成など日常の活動のためのこれはたくさんのユーティリティを提供します。

### symfonyのパス

次のコマンドを入力することでプロジェクトで使われているsymfonyのバージョンを取得できます:

    $ php symfony -V

`-V`オプションは`config/ProjectConfiguration.class.php`に保存されているsymfonyのインストールディレクトリへのパスも表示します:

    [php]
    // config/ProjectConfiguration.class.php
    require_once '/Users/fabien/work/symfony/dev/1.2/lib/autoload/sfCoreAutoload.class.php';

より良いポータビリティのために、symfonyがインストールされている絶対パスを相対パスに変更します:

    [php]
    // config/ProjectConfiguration.class.php
    require_once dirname(__FILE__).'/../lib/vendor/symfony/lib/autoload/sfCoreAutoload.class.php';

この方法では、プロジェクトディレクトリをマシンの他の場所、もしくは別のマシンに移動させて動きます。
