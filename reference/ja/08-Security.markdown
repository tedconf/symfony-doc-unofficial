security.yml設定ファイル
========================

`security.yml`設定ファイルは
symfonyアプリケーション用の認証と認可を記述します。

>**TIP**
>`security.yml`ファイルからの設定情報は
>[`user`](#chapter_05_user)ファクトリクラス(デフォルトは`sfBasicSecurityUser`)によって使われます。
>認証と認可の強制は`security` [フィルタ](12-Filters#chapter_12_security)によって行われます。

アプリケーションが作成されるとき、symfonyはアプリケーションの`config/`ディレクトリで
デフォルトの`security.yml`ファイルを生成します。これはアプリケーション全体のセキュリティ
を記述します(`default`キーの下):

    [yml]
    default:
      is_secure: off

はじめの章で説明したように、`security.yml`ファイルは
[**設定カスケードのメカニズム**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade)から恩恵を受け、
[**定数**](#chapter_03-Configuration-Files-Principles_sub_constants)を含むことができます。

アプリケーションのデフォルト設定はモジュールの`config/`ディレクトリで
`security.yml`ファイルを作成することでオーバーライドできます。
メインキーは接頭辞の`execute`を伴わないアクションの名前です(例えば`index`では
`executeIndex`メソッド)。

アクションがセキュアかどうかを判断するために、symfonyは次の順序で
情報を探します:

  * 存在するのであればモジュールの設定ファイルの
    固有のアクションのための設定;

  * 存在するのであればモジュールん設定ファイルの
    モジュール全体のための設定(`all`キーの下);

  * アプリケーションのデフォルト設定(`default`キーの下)。

アクションにアクセスするために必要なクレデンシャルを決定するために
同じ優先ルールが使われます。

>**NOTE**
>`security.yml`設定ファイルはPHPファイルとしてキャッシュできます; 
>処理は`sfSecurityConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。

認証
----

`security.yml`のデフォルト設定は、それぞれのアプリケーションごとにインストールされ、
だれでもアクセスを許可します:

    [yml]
    default:
      is_secure: off

アプリケーションの`security.yml`ファイルで`is_secure`キーを`on`にセットすることで、
アプリケーション全体ですべてのユーザーの認証が必須になります。

>**NOTE**
>認証されていないユーザーがセキュアなアクションにアクセスしようとすると、
>symfonyはリクエストを`settings.yml`で設定された`login`アクションに転送します。

モジュールの認証要件を修正するには、
`config/`ディレクトリで`security.yml`を作成し`all`キーを定義します:

    [yml]
    all:
      is_secure: on

モジュールの単独のアクションの認証要件を修正するには、
モジュールの`config/`ディレクトリで`security.yml`ファイルを作成します。
アクションの名前の後でキーを定義します:

    [yml]
    index:
      is_secure: off

>**TIP**
>loginアクションをセキュアにすることはできません。
>これは無限ループを避けるためです。

認可
----

ユーザーが認証されるとき、*クレデンシャル*を定義することで
一部のアクションへのアクセスをさらに制限できます。クレデンシャルが定義されたとき、
ユーザーはアクションにアクセスする必須クレデンシャルを持たなければなりません:

    [yml]
    all:
      is_secure:   on
      credentials: admin

symfonyのクレデンシャルのシステムはシンプルで強力です。クレデンシャルは
アプリケーションのセキュリティモデルを記述するために必要なものを表現できる
文字列です(グループもしくはパーミッション)。

`credentials`キーは配列記法を使用することで複雑なクレデンシャル要件を記述するための
ブール演算をサポートします。

ユーザーがクレデンシャルA**かつ**クレデンシャルBを持たなければならない場合、
角かっこでクレデンシャルを囲みます:

    [yml]
    index:
      credentials: [A, B]

ユーザーがクレデンシャルA **または**クレデンシャルBを持たなければならないとき、
2つのペアの角かっこでこれらを囲みます:

    [yml]
    index:
      credentials: [[A, B]]

任意の数のクレデンシャルで任意の種類のブール式を記述するために
かっこを混ぜることもできます。
