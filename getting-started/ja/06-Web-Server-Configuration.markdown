Webサーバーの設定
===================

ひどい方法
----------

前の章で、プロジェクトをホストするディレクトリを作成しました。
WebサーバーのWebrootディレクトリのどこかで作成していれば、
Webブラウザでプロジェクトにアクセスできます。

もちろん、設定をしなければ、とても速くセットアップが終わりますが、
ブラウザで`config/databases.yml`ファイルにアクセスすると手抜きした結果がわかります。
Webサイトがsymfonyで開発されていることをユーザーが知ったら、
たくさんの重要なファイルにアクセスされてしまいます。

**本番サーバーでこの設定をけっして使わないでください**。
Webサーバーを適切に設定する方法を学ぶには次のセクションをご覧ください。

セキュアな方法
-------------

WebのグッドプラクティスはWebrootディレクトリの元ではスタイルシート、JavaScript、と
画像のようなWebブラウザがアクセスするのに必要なファイルだけを設置します。
そしてデフォルトでは、symfonyプロジェクトの`web/`サブディレクトリの元で
これらのファイルを保存することをお勧めします。

このディレクトリを見てみると、Webアセット(`css/`と`images/`)と
2つのフロントコントローラファイル用のサブディレクトリが見つかります。
フロントコントローラはPHPファイルのみでWebroot
ディレクトリに置く必要があります。他のすべてのPHPファイルはブラウザから隠され、
セキュリティに関してこれは良いアイディアです。

### Webサーバーの設定

世界中から新しいプロジェクトにアクセスできるように
Apacheの設定を変更してみましょう。

`httpd.conf`設定ファイルを見つけて開き
最後の行に次の設定を追加します:

    # Be sure to only have this line once in your configuration
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

>**NOTE**
>`/sf`エイリアスによってsymfonyのデフォルトページとWebデバッグツールバーを
>適切に表示するために必要な画像とJavaScriptファイルにアクセスできるようになります。
>
>Windowsでは、`Alias`の行を次のように置き換える必要があります:
>
>     Alias /sf "c:\dev\sfproject\lib\vendor\symfony\data\web\sf"
>
>そして`/home/sfproject/web`は次のように置き換えます:
>
>     c:\dev\sfproject\web

この設定によってApacheはマシンのポート番号`8080`をリスニングするようになり、
Webサイトは次のURLからアクセスできるようになります:

    http://localhost:8080/

`8080`は任意の番号に変更できますが、管理者権限が必要ない`1024`より大きな番号が望ましいです。

>**SIDEBAR**
>専用のドメイン名を設定する
>
>マシンの管理者であれば、新しいプロジェクトを始めるたびに新しいポートを追加するよりも
>バーチャルホストをセットアップする方がベターです。
>ポートを追加する代わりに`Listen`ステートメントを追加し、
>ドメイン名を選択し`ServerName`ステートメントを追加します:
>
>     # This is the configuration for your project
>     <VirtualHost 127.0.0.1:80>
>       ServerName sfproject.localhost
>       <!-- same configuration as before -->
>     </VirtualHost>
>
>Apacheで使われるドメイン名である`sfproject.localhost`は
>ローカルで宣言しなければなりません。Linuxシステムを稼働させている場合、
>`/etc/hosts`ファイルで作業を行います。Windows XPを稼働させている場合、このファイルは
>`C:\WINDOWS\system32\drivers\etc\`ディレクトリに設置されています。
>
>次の行を追加します:
>
>     127.0.0.1 sfproject.localhost

### 新しい設定をテストする

Apacheを再起動し、ブラウザを開き`http://localhost:8080/index.php/`、もしくは
`http://sfproject.localhost/index.php/`から新しいアプリケーションにアクセスできることを確認します。
URLは前のセクションで選んだApacheの設定方法によります。

![初期ページ](http://www.symfony-project.org/images/jobeet/1_2/01/congratulations.png)

>**TIP**
>Apacheの`mod_rewrite`モジュールをインストールしていれば、
>URLの`index.php/`の部分を取り除くことができます。`web/.htaccess`ファイルで
>設定されたルールを書き換えることでこれは実現可能です。

開発環境のアプリケーションにもアクセスしてみます
(環境の詳細情報は次のセクションを参照)。
次のURLを入力します:

    http://sfproject.localhost/frontend_dev.php/

Webデバッグツールバーは右上コーナーに表示されます。
小さなアイコンが含まれるのは`sf/`エイリアスが正しいことを証明します。

![Webデバッグツールバー](http://www.symfony-project.org/images/jobeet/1_2/01/web_debug_toolbar.png)

>**Note**
>Windows環境のIISでsymfonyを稼働させたい場合、。
>セットアップ方法は少し異なります。[関連チュートリアル](http://www.symfony-project.org/cookbook/1_0/ja/web_server_iis)
>で設定方法がわかります。
