プロジェクトのセットアップ
=========================

symfonyでは、同じデータモデルを共有する**アプリケーション**(application)は**プロジェクト**(project)に再分類されます。多くのプロジェクトでは、2つの異なるアプリケーション: フロントエンド(frontend)とバックエンド(backend)が用意されます。

### プロジェクトの作成

`sfproject/`ディレクトリから、symfonyプロジェクトを実際に作成するために`generate:project`タスクを実行します:

    $ php lib/vendor/symfony/data/bin/symfony generate:project PROJECT_NAME

Windowsでは:

    c:\> php lib\vendor\symfony\data\bin\symfony generate:project PROJECT_NAME

`generate:project`タスクはsymfonyプロジェクトに必要なデフォルトのディレクトリ構造を生成します:

 | ディレクトリ | 説明
 | ----------- | ----------------------------------
 | `apps/`     | すべてのプロジェクトのアプリケーションをホストする
 | `cache/`    | フレームワークによってキャッシュされるファイル
 | `config/`   | プロジェクトの設定ファイル
 | `lib/`      | プロジェクトのライブラリとクラス
 | `log/`      | フレームワークのログファイル
 | `plugins/`  | インストールされたプラグイン
 | `test/`     | ユニットテストと機能テストのファイル
 | `web/`      | ウェブのrootディレクトリ(下記を参照)

>**NOTE**
>なぜsymfonyはこんなに多くのファイルを生成するのか？
>フルスタックフレームワークを使う主な利点の1つは開発プロセスを標準化することです。
>symfonyのファイルとディレクトリのデフォルト構造のおかげで、
>symfonyの知識がある開発者であればsymfonyプロジェクトのメンテナンスを引き継ぐことができます。
>数分のうちに、開発者はコードに飛び込み、バグを修正し、
>新しい機能を追加できるようになります。

タスクを実行するときに書かなければならない文字数を短くするために
`generate:project`タスクはプロジェクトのrootディレクトリで
`symfony`ショートカットも作成しました。

ですので今後は、symfonyプログラムへのフルパスを使う代わりに、
`symfony`ショートカットを使うことができます。

### アプリケーションの作成

`generate:app`タスクを実行してfrontendアプリケーションを作成します:

    $ php symfony generate:app --escaping-strategy=on
      ➥ --csrf-secret=UniqueSecret frontend

>**TIP**
>symfonyショートカットは実行可能なので、Unixユーザーは
>symfonyを'`./symfony`'に置き換えることができます。
>
>Windowsでは'`symfony.bat`'ファイルをプロジェクトにコピーして
>'`php symfony`'の代わりに'`symfony`'を使うことができます:
>
>     c:\> copy lib\vendor\symfony\data\bin\symfony.bat .

*引数*として渡されるアプリケーションの名前に基づいて、`generate:app`タスク
は`apps/frontend/`ディレクトリ:の下のアプリケーションに必要な
デフォルトのディレクトリ構造を作成します:

 | ディレクトリ | 説明
 | ------------ | -------------------------------------
 | `config/`    | アプリケーションの設定ファイル
 | `lib/`       | アプリケーションのライブラリとクラス
 | `modules/`   | アプリケーションのコード(MVC)
 | `templates/` | グローバルテンプレートファイル

>**TIP**
>`generate:app`タスクを呼び出すとき、2つのセキュリティ関連の*オプション*も
>渡すことができます:
>
>  * `--escaping-strategy`: XSS攻撃を阻止する出力エスケーピングを有効にする
>  * `--csrf-secret`: CSRF攻撃を阻止するセッショントークンを有効にする
>
>タスクにこれらの2つのオプションを渡すことで、
>ウェブで最もよく知られる2つの脆弱性から将来の開発をセキュアにしました。
>これで作業はお終いです。symfonyはあなたに代わって自動的に
>セキュリティ対策を行います。
>
>[XSS](http://en.wikipedia.org/wiki/Cross-site_scripting)もしくは
>[CSRF](http://en.wikipedia.org/wiki/CSRF)を
>知らなければ、
>これらのセキュリティの脆弱性を学ぶために時間をかけてください。

### ディレクトリ構造

新しく作成したプロジェクトにアクセスしてみる前に、
ウェブサーバーが書き込みできるように`cache/`と`log/`ディレクトリに書き込みできるように
これらのディレクトリの書き込みパーミッションを
適切なレベルに設定する必要があります:

    $ chmod 777 cache/ log/

>**SIDEBAR**
>SCMツールを使う方々のためのティップス
>
>symfonyはsymfonyプロジェクトの2つのディレクトリ、
>`cache/`と`log/`に対してのみ書き込みます。SCMはこれらのディレクトリの内容を
>無視します(例えばSubveresionの場合
>`svn:ignore`プロパティを編集します)。

### データベースを設定する

最初になさりたいことの1つはプロジェクト用のデータベース接続を設定することでしょう。
symfonyフレームワークは[PDO]((http://www.php.net/PDO))が
サポートするデータベース(MySQL、PostgreSQL、
SQLite、Oracle、MSSQL、...)をすべてサポートします。
PDOの上で、symfonyは2つのORMツール: PropelとDoctrineを
搭載しています。PropelはデフォルトのORMですが、
Doctrineに切り替えるのはきわめて簡単です(詳細な情報は次のセクションを参照)。

`configure:database`タスクを使えばデータベースの設定は簡単にできます:

    $ php symfony configure:database "mysql:host=localhost;dbname=dbname" root mYsEcret

`configure:database`タスクは3つの引数: [PDO DSN](http://www.php.net/manual/pdo.drivers.php)、
データベースにアクセスするユーザー名、とパスワードを受け取ります。
開発サーバーのデータベースにアクセスする必要がない場合、
3番目の引数を省略します。

### Doctrineに切り替える

Propelの代わりにDoctrineを使うことを決めた場合、
`sfDoctrinePlugin`を有効にして`sfPropelPlugin`を無効にする必要があります。これは`config/ProjectConfiguration.class.php`で次のコードを変更することでこれを実現できます:

    [php]
    public function setup()
    {
      $this->enableAllPluginsExcept(array('sf#PropelPlugin', 'sfCompat10Plugin'));
    }

これらの変更を行った後で、これらのコマンドを立ち上げます:

    $ php symfony plugin:publish-assets
    $ php symfony cc
    $ rm web/sfPropelPlugin
    $ rm config/propel.ini
    $ rm config/schema.yml
    $ rm config/databases.yml

Doctrine用のデータベースを設定するために次のコマンドを実行します:

    $ php symfony configure:database --name=doctrine --class=sfDoctrineDatabase "mysql:host=localhost;dbname=jobeet" root mYsEcret
