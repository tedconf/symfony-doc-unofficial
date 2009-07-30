settings.yml設定ファイル
========================

symfonyの多くの面はYAMLもしくはプレーンなPHPで書かれた設定ファイルを通して
設定できます。このセクションでは、
アプリケーション用のメインの設定ファイルである、`settings.yml`を説明します。

アプリケーション用のメインの`settings.yml`設定ファイルは
`apps/APP_NAME/config/`ディレクトリで見つかります。

はじめの章で検討されたように、`settings.yml`ファイルは
[**環境を認識し**](#chapter_03-Configuration-File-Principles_sub_environment_awareness)、
[**設定カスケードのメカニズム**](#chapter_03-Configuration-File-Principles_sub_configuration_cascade)から恩恵を受けます。

それぞれの環境は2つのサブセクション: `.actions`と`.settings`を持ちます。
共通ページ用にレンダリングされるデフォルトのアクション以外は、
すべての設定ディレクティブは`.settings`サブセクションの元で動作します。

>**NOTE**
>`settings.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>プロセスは`sfDefineEnvironmentConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。

<div class="pagebreak"></div>

設定
----

  * `.actions`

    * [`error_404`](#chapter_04-Settings_sub_error_404)
    * [`login`](#chapter_04-Settings_sub_error_404)
    * [`secure`](#chapter_04-Settings_sub_secure)
    * [`module_disabled`](#chapter_04-Settings_sub_module_disabled)

  * `.settings`

    * [`cache`](#chapter_04-Settings_sub_cache)
    * [`charset`](#chapter_04-Settings_sub_charset)
    * [`check_lock`](#chapter_04-Settings_sub_check_lock)
    * [`check_symfony_version`](#chapter_04-Settings_sub_check_symfony_version)
    * [`compressed`](#chapter_04-Settings_sub_compressed)
    * [`csrf_secret`](#chapter_04-Settings_sub_csrf_secret)
    * [`default_culture`](#chapter_04-Settings_sub_default_culture)
    * [`default_timezone`](#chapter_04-Settings_sub_default_timezone)
    * [`enabled_modules`](#chapter_04-Settings_sub_enabled_modules)
    * [`error_reporting`](#chapter_04-Settings_sub_error_reporting)
    * [`escaping_strategy`](#chapter_04-Settings_sub_escaping_strategy)
    * [`escaping_method`](#chapter_04-Settings_sub_escaping_method)
    * [`etag`](#chapter_04-Settings_sub_etag)
    * [`i18n`](#chapter_04-Settings_sub_i18n)
    * [`lazy_cache_key`](#chapter_04-Settings_sub_lazy_cache_key)
    * [`logging_enabled`](#chapter_04-Settings_sub_logging_enabled)
    * [`no_script_name`](#chapter_04-Settings_sub_no_script_name)
    * [`max_forwards`](#chapter_04-Settings_sub_max_forwards)
    * [`standard_helpers`](#chapter_04-Settings_sub_standard_helpers)
    * [`strip_comments`](#chapter_04-Settings_sub_strip_comments)
    * [`use_database`](#chapter_04-Settings_sub_use_database)
    * [`web_debug`](#chapter_04-Settings_sub_web_debug)
    * [`web_debug_web_dir`](#chapter_04-Settings_sub_web_debug_web_dir)

<div class="pagebreak"></div>

`.actions`サブセクション
-----------------------

*デフォルトの構成*:

    [yml]
    default:
      .actions:
        error_404_module:       default
        error_404_action:       error404

        login_module:           default
        login_action:           login

        secure_module:          default
        secure_action:          secure

        module_disabled_module: default
        module_disabled_action: disabled

`.actions`サブセクションは共通のページがレンダリングされなければならないとき
に実行するアクションを定義します。それぞれの定義は2つのコンポーネントを持ちます: 1つはモジュール用
(接尾辞は`_module`)、もう1つはアクション用です(接尾辞は`_action`)。

### `error_404`

`error_404`アクションは404ページがレンダリングされなければならないときに実行されます。

### `login`

`login`アクションは認証されていないユーザーがセキュアなページにアクセスしようとするときに
実行されます。

### `secure`

`secure`アクションはユーザーが求められるクレデンシャルを持たないときに
実行されます。

### `module_disabled`

`module_disabled`アクションはユーザーが無効なモジュールをリクエストされるときに
実行されます。

`.settings`サブセクション
-------------------------

`.settings`サブセクションはフレームワークの設定が行われる所です。
下記のパラグラフはすべてのありえる設定を説明し、
重要度順におおまかに並べています。

`.settings`セクションで定義されたすべての設定は`sfConfig`オブジェクトを使用し
`sf_`の接頭辞をつけることで.任意の場所で利用可能です。
例えば、`charset`設定の値を取得するには、次を使います:

    [php]
    sfConfig::get('sf_charset');

### `escaping_strategy`

*デフォルト*: `off`

`escaping_strategy`設定は出力エスケーパーサブフレームワークが有効であるかどうかを決定する論理値の設定です。
有効なとき、テンプレートの中で利用可能なすべての変数は 
`escaping_method`設定で定義されたヘルパー関数を呼び出すことで
自動的にエスケープされます(下記を参照)。

`escaping_method`はsymfonyによって使われるデフォルトのヘルパーであることに注意してください。
しかしこれは例えばJavaScriptスクリプトのタグで変数を出力するときなど、
ケースバイケースでこれをオーバーライドできます。

出力エスケーパーサブフレームワークはエスケープ用に`charset`設定を使います。

デフォルトの値を`on`に変更することを大いにお勧めします。

>**TIP**
>`--escaping-strategy`オプションを使用することで`generate:app`タスクで
>アプリケーションを作成するときにこの設定をセットできます。

### `escaping_method`

*デフォルト*: `ESC_SPECIALCHARS`

`escaping_method`はテンプレートでエスケープするために
使うデフォルト関数を定義します(上記の`escaping_strategy`設定を参照)。

組み込み関数の1つ: `ESC_SPECIALCHARS`、`ESC_RAW`、
`ESC_ENTITIES`、`ESC_JS`、`ESC_JS_NO_ENTITIES`、と
`ESC_SPECIALCHARS`を選ぶ、もしくは独自関数を作ることができます。

大抵の場合、デフォルトの値で十分です。
英語もしくはヨーロッパの言語のみ扱う場合のみ
`ESC_ENTITIES`ヘルパーも使うことができます。

### `csrf_secret`

*デフォルト*: `false`

`csrf_secret`はアプリケーション用のunique secretです。
`false`に設定されていない場合、これはフォームフレームワークで定義された
すべてのフォーム用のCSRF保護を有効にします。
リンクをフォームに変換することが必要なとき(例えばHTTP `DELETE`メソッドをシミュレートする)
この設定は`link_to()`ヘルパーにも使われます。

デフォルトの値をunique secretに変更することを多いにお勧めします。

>**TIP**
>`--csrf-secret`オプションを使用して`generate:app`で
>アプリケーションを作成した際にこの設定は自動的にセットされます

### `charset`

*デフォルト*: `utf-8`

`charset`設定はフレームワークのすべての場所: レスポンスの`Content-Type`ヘッダー
から出力エスケーピング機能まで使われる文字集合です。

大抵の場合、デフォルトで十分です。

### `enabled_modules`

*デフォルト*: `[default]`

`enabled_modules`はこのアプリケーションのために有効なモジュール名の配列です。
デフォルトでは、プラグインもしくはsymfonyコアで定義されたモジュールは有効ではなく、
受け入れできるようにこの設定のリストに含めなければなりません。

モジュールの追加方法はシンプルで名前をリストに追加するだけです(
モジュールの順序は問題にはなりません):

    [yml]
    enabled_modules: [default, sfGuardAuth]

フレームワークで定義された`default`モジュールはサブセクションの`settings.yml`の`.actions`で
セットされたすべてのデフォルトのアクションを格納します。これらすべてをカスタマイズし、
この設定から`default`モジュールを
削除することをお勧めします。

### `default_timezone`

*デフォルト*: none

`default_timezone`設定はPHPで使われるデフォルトのタイムゾーンを定義します。
PHPで認識される任意の[タイムゾーン](http://www.php.net/manual/class.datetimezone.php)になります。


>**NOTE**
>タイムゾーンが定義されていない場合、
>`php.ini`ファイルで定義することをお勧めします。
>そうでなければ、
>PHPの[`date_default_timezone_get()`](http://www.php.net/date_default_timezone_get)関数を呼び出すことで
>symfonyはベストのタイムゾーンを推測しようとします。

### `cache`

*デフォルト*: `off`

`cache`設定はテンプレートキャッシュを有効もしくは無効にする。

>**TIP**
>キャッシュシステムの一般設定は`factories.yml`設定ファイルの
>[`view_cache_manager`](#chapter_05_view_cache_manager)と
>[`view_cache`](#chapter_05-Factories_sub_view_cache)セクションで行われます。
>きめ細かい設定は
>[`cache.yml`](#chapter_09-Cache)設定ファイルで行われます。

### `etag`

*デフォルト*: `dev`と`test`環境を除いて、デフォルトでは`on`

`etag`設定はHTTPの`ETag`ヘッダーの自動生成を有効もしくは無効にします。
symfonyによって生成されたETagはレスポンスのコンテンツの
単純なmd5です。

### `i18n`

*デフォルト*: `off`

`i18n`設定は国際化サブフレームワークを有効もしくは無効にする論理値です。
アプリケーションが国際化されている場合、`on`にセットします。

>**TIP**
>国際化システムの一般設定は
>`factories.yml`設定ファイルの[`i18n`](#chapter_05-Factories_sub_i18n)セクションで
>行われます。

### `default_culture`

*デフォルト*: `en`

`default_culture`設定は国際化サブフレームワークで使われるデフォルトのcultureを定義します。
これは任意の有効なcultureになります。

### `standard_helpers`

*デフォルト*: `[Partial, Cache, Form]`

`standard_helpers`設定はすべてのテンプレート用にロードするヘルパーグループの配列です
(`Helper`の接尾辞を持たないgroupヘルパーの名前)。

### `no_script_name`

*デフォルト*: `on`は最初に作成されたアプリケーションの`prod`環境用に、
`off` for all others

`no_script_name`設定はフロントコントローラスクリプトの名前が
生成URLに追加されるかどうかを決定します。デフォルトでは、
最初に作成されるアプリケーションの`prod`環境のために`generate:app`タスク
によってこれは`on`にセットされます。

すべてのフロントコントローラが同じディレクトリ(`web/`)にある場合、
あきらかに、1つのアプリケーションと環境のみがこの設定を`on`にセットできます。
`no_script_name`を`on`にセットしたアプリケーションが複数欲しい場合、
対応するフロントコントローラをWeb公開ディレクトリのサブディレクトリの元に
移動させます。

### `lazy_cache_key`

*デフォルト*: 新しいプロジェクトには`on`、アップグレードしたプロジェクトには`off`

これが有効なとき、`lazy_cache_key`設定はアクションもしくはパーシャルがキャッシュ可能になるまで
キャッシュキーの作成を遅延させます。テンプレートパーシャルの使い方によって、
これは大きなパフォーマンスの改善になります。

以前の1.2リリースとの後方互換性を壊さずにパフォーマンスを改善するために、
この設定はsymfony 1.2.7で導入されました。
symfony 1.3では最適化は常に有効になるのでこの設定は削除されます。

>**CAUTION**
>この設定はsymfony 1.2.7とそれ以降でのみ利用可能です。

### `logging_enabled`

*デフォルト*: `prod`以外のすべての環境に`on`

`logging_enabled`設定はロギングサブフレームワークを有効にします。
これを`false`に設定することでロギングメカニズムを回避し
小さなパフォーマンスのゲインを提供します。

>**TIP**
>ロギングのきめ細かい設定は
>`factories.yml`設定ファイルで行われます。

### `web_debug`

*デフォルト*: `dev`以外のすべての環境に対して`off`

`web_debug`設定はWebデバッグツールバーを有効にします。レスポンスの
Content-TypeがHTMLであるときにWebデバッグツールバーはページに注入されます。

### `error_reporting`

*デフォルト*:

  * `prod`:  E_PARSE | E_COMPILE_ERROR | E_ERROR | E_CORE_ERROR | E_USER_ERROR
  * `dev`:   E_ALL | E_STRICT
  * `test`:  (E_ALL | E_STRICT) ^ E_NOTICE
  * default: E_PARSE | E_COMPILE_ERROR | E_ERROR | E_CORE_ERROR | E_USER_ERROR

`error_reporting`設定は(ブラウザに表示されログに書き込まれる)
PHPのエラーレポートのレベルをコントロールします。

>**TIP**
>PHPの公式サイトには
>[ビット演算子](http://www.php.net/language.operators.bitwise)の使い方に関する情報があります。

デフォルトの設定は最も利にかなったものであり、変えるべきではありません。

>**NOTE**
>`prod`環境のフロントコントローラは
>`debug`を無効にしており
>ブラウザのエラーの表示は自動的に無効になります。

### `compressed`

*デフォルト*: `off`

`compressed`設定はPHPのネイティブなレスポンス圧縮を有効にします。
`on`に設定されている場合、symfonyは`ob_start()`用のコールバック関数として
[`ob_gzhandler`](http://www.php.net/ob_gzhandler)を使います。

これは`off`の状態にしておいて、
代わりにWebサーバーのネイティブな圧縮メカニズムを
使うことをお勧めします。

### `use_database`

*デフォルト*: `on`

`use_database`はアプリケーションがデータベースを使うかどうかを決定します。

### `check_lock`

*デフォルト*: `off`

`check_lock`設定は`cache:clear`と`project:disable`のようなタスクに
よって実行されるアプリケーションのロックシステムを有効もしくは無効にします。

`on`に設定される場合、無効なアプリケーションへのすべてのリクエストは
symfonyコアの`lib/exception/data/unavailable.php`ページに自動的にリダイレクトされます。

>**TIP**
>`config/unavailable.php`ファイルをプロジェクトもしくはアプリケーションに
>追加することで無効なページ用のデフォルトテンプレートをオーバーライドできます。

### `check_symfony_version`

*デフォルト*: `off`

`check_symfony_version`はリクエストごとにsymfonyの現在のバージョンを
チェックするのを有効もしくは無効にします。有効な場合、symfonyがアップグレードされた際に
キャッシュは自動的にクリアされます。

これを`on`にしないことが大いに推奨されます。小さなオーバーヘッドを追加するのと
新しいバージョンのプロジェクトをデプロイする際にキャッシュをクリアするだけで済むからです。
この設定は複数のプロジェクトが同じsymfonyのコードを共有する場合
のみ役に立ちますが、推奨されません。

### `web_debug_web_dir`

*デフォルト*: `/sf/sf_web_debug`

`web_debug_web_dir`はWebデバッグツールバーのアセットへのWebパスをセットします
(画像、スタイルシート、とJavaScriptファイル)。

### `strip_comments`

*デフォルト*: `on`

`strip_comments`はコアクラスをコンパイルする際にsymfonyがコメントをはぎとるべきか決定します。
アプリケーションの`debug`設定が`off`にセットされている場合のみこの設定が使われます。

本番環境でのみ空白のページを用意する場合、 
この設定を`off`にセットしてみてください。

### `max_forwards`

*デフォルト*: `5`

`max_forwards`設定はsymfonyが例外を投げる前に許容される内部転送の最大回数をセットします。
これは無限ループを避けるためにあります。
