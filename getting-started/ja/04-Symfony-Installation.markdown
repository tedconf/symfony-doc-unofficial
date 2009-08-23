symfonyのインストール
====================

### プロジェクトのディレクトリ

symfonyをインストールする前に、最初にプロジェクトに関連する
すべてのファイルをホストするディレクトリを作成する必要があります:

    $ mkdir -p /home/sfproject
    $ cd /home/sfproject

Windowsでは:

    c:\> mkdir c:\dev\sfproject
    c:\> cd c:\dev\sfproject

>**NOTE**
>Windowsユーザーの方にはスペースを含まないパスで
>symfonyで実行して新しいプロジェクトをセットアップすることをお勧めします。
>`My Documents`の下を含めて`Documents and Settings`ディレクトリ
>を使うのは避けます。

-

>**TIP**
>Webrootディレクトリの元でsymfonyプロジェクトのディレクトリを作る場合、
>Webサーバーを設定する必要はありません。もちろん、
>本番環境に関して、Webサーバーの設定のセクションで説明されているように
>Webサーバーを設定することを強くお勧めします。

### symfonyのインストール

symfonyフレームワークライブラリのファイルをホストするディレクトリを作成します:

    $ mkdir -p lib/vendor

symfonyをインストールする必要があります。symfonyフレームワークにはいくつかの安定ブランチが存在し
symfony公式サイトの[インストールの手引きのページ](http://www.symfony-project.org/installation)を読み
インストールしたいブランチを選ぶ必要があります。


たとえば[symfony 1.2](http://www.symfony-project.org/installation/1_2)など選んだバージョン用の
インストールの手引きのページに向かいます。

"**Source Download**"セクションの元で、`.tgz`フォーマットもしくは`.zip`フォーマットで
アーカイブが見つかります。アーカイブをダウンロードし
`lib/vendor/`ディレクトリの元に設置し再度解凍します:

    $ cd lib/vendor
    $ tar zxpf symfony-1.2.2.tgz
    $ mv symfony-1.2.2 symfony
    $ rm symfony-1.2.2.tgz

Windowsの環境下ではzipファイルをエクスプローラで解凍できます。
ディレクトリを`symfony`にリネームすると、ディレクトリのフルパスは
`c:\dev\sfproject\lib\vendor\symfony`になります。

>**TIP**
>Subversionを使う場合、プロジェクトの`lib/vendor/`に
>symfonyに埋め込み安定ブランチのバグ修正から
>自動的に恩恵を受けるためには`svn:externals`プロパティを
>使う方がベターです:
>
>     http://svn.symfony-project.com/branches/1.2/

symfonyのバージョンを表示するためにコマンドラインを使用することで
symfonyが正しくインストールされていることを確認します(大文字の`V`に注意):

    $ php ../..
    $ php lib/vendor/symfony/data/bin/symfony -V

On Windows:

    c:\> cd ..\..
    c:\> php lib\vendor\symfony\data\bin\symfony -V

>**TIP**
>このコマンドラインツールが何をできるのかご興味があれば、
>利用可能なオプションとタスクの一覧を表示するために`symfony`を入力します:
>
>     $ php lib/vendor/symfony/data/bin/symfony
>
>Windowsでは:
>
>     c:\> php lib\vendor\symfony\data\bin\symfony
>
>symfonyコマンドは開発者の最良の友です。
>キャッシュのクリア、コードの生成など日常の活動のための
>これはたくさんのユーティリティを提供します。

### symfonyのパス

次のコマンドを入力することでプロジェクトで使われているsymfonyのバージョンを取得できます:

    $ php symfony -V

`-V`オプションは`config/ProjectConfiguration.class.php`に保存されている
symfonyのインストールディレクトリへのパスも表示します:

    [php]
    // config/ProjectConfiguration.class.php
    require_once '/Users/fabien/work/symfony/dev/1.2/lib/autoload/sfCoreAutoload.class.php';

より良いポータビリティのために、symfonyがインストールされている
絶対パスを相対パスに変更します:

    [php]
    // config/ProjectConfiguration.class.php
    require_once dirname(__FILE__).'/../lib/vendor/symfony/lib/autoload/sfCoreAutoload.class.php';

この方法では、プロジェクトディレクトリをマシンの他の場所、もしくは別のマシンに移動させて
動きます。
