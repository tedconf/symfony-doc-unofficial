factories.yml設定ファイル
========================

ファクトリはリクエストが存続する間にフレームワークが必要とするコアオブジェクトです。
これらは`factories.yml`設定ファイルで設定され
`sfContext`オブジェクトを通して常にアクセス可能です:

    [php]
    // ユーザーファクトリを取得する
    sfContext::getInstance()->getUser();

アプリケーション用のメインの`factories.yml`設定ファイルは
`apps/APP_NAME/config/`ディレクトリで見つかります。

はじめの章で説明したように、`factories.yml`ファイルは
[**環境を認識し**](#chapter_03-Configuration-Files-Principles_sub_environment_awareness)、
[**設定カスケードのメカニズム**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade)が有効になり、[**定数**](#chapter_03-Configuration-Files-Principles_sub_constants)を含むことができます。

`factories.yml`設定ファイルは名前つきのファクトリのリストを格納します:

    [yml]
    FACTORY_1:
      # definition of factory 1

    FACTORY_2:
      # definition of factory 2

    # ...

サポートされるファクトリの名前は次の通りです: `controller`、`logger`、`i18n`、`request`、
`response`、`routing`、`storage`、`user`、`view_cache`、と
`view_cache_manager`

`sfContext`がファクトリを初期化するとき、ファクトリオブジェクトを設定するために
使われるファクトリ(`class`)とパラメータ(`param`)の
クラス名用の`factories.yml`ファイルを読み取ります:

    [yml]
    FACTORY_NAME:
      class: CLASS_NAME
      param: { ARRAY OF PARAMETERS }

ファクトリをカスタマイズできることはsymfonyのコアオブジェクト用の
デフォルトクラスの代わりにカスタムクラスを使うことができることを意味します。
これらに送信するパラメータをカスタマイズすることで
これらのクラスのデフォルトのふるまいを変更することもできます。

ファクトリクラスがオートロードできないとき、`file`パスが定義され
ファクトリが作成される前に自動的にインクルードできます:

    [yml]
    FACTORY_NAME:
      class: CLASS_NAME
      file:  ABSOLUTE_PATH_TO_FILE

>**NOTE**
>`factories.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>プロセスは`sfFactoryConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。

<div class="pagebreak"></div>

ファクトリ
---------

 * [`request`](#chapter_05-Factories_sub_request)

   * [`formats`](#chapter_05-Factories_sub_formats)
   * [`path_info_array`](#chapter_05-Factories_sub_path_info_array)
   * [`path_info_key`](#chapter_05-Factories_sub_path_info_key)
   * [`relative_url_root`](#chapter_05-Factories_sub_relative_url_root)

 * [`response`](#chapter_05-Factories_response)

   * [`charset`](#chapter_05-Factories_sub_charset)
   * [`http_protocol`](#chapter_05-Factories_sub_http_protocol)
   * [`send_http_headers`](#chapter_05-Factories_sub_send_http_headers)

 * [`user`](#chapter_05-Factories_sub_user)

   * [`default_culture`](#chapter_05-Factories_sub_default_culture)
   * [`timeout`](#chapter_05-Factories_sub_timeout)
   * [`use_flash`](#chapter_05-Factories_sub_use_flash)

 * [`storage`](#chapter_05-Factories_sub_storage)

   * [`auto_start`](#chapter_05-Factories_sub_auto_start)
   * [`database`](#chapter_05-Factories_sub_database_storage_specific_options)
   * [`db_table`](#chapter_05-Factories_sub_database_storage_specific_options)
   * [`db_id_col`](#chapter_05-Factories_sub_database_storage_specific_options)
   * [`db_data_col`](#chapter_05-Factories_sub_database_storage_specific_options)
   * [`db_time_col`](#chapter_05-Factories_sub_database_storage_specific_options)
   * [`session_cache_limiter`](#chapter_05-Factories_sub_session_cache_limiter)
   * [`session_cookie_domain`](#chapter_05-Factories_sub_session_set_cookie_params_parameters)
   * [`session_cookie_httponly`](#chapter_05-Factories_sub_session_set_cookie_params_parameters)
   * [`session_cookie_lifetime`](#chapter_05-Factories_sub_session_set_cookie_params_parameters)
   * [`session_cookie_path`](#chapter_05-Factories_sub_session_set_cookie_params_parameters)
   * [`session_cookie_secure`](#chapter_05-Factories_sub_session_set_cookie_params_parameters)
   * [`session_name`](#chapter_05-Factories_sub_session_name)

 * [`view_cache_manager`](#chapter_05-Factories_sub_view_cache_manager)
 * [`view_cache`](#chapter_05-Factories_sub_view_cache)
 * [`i18n`](#chapter_05-Factories_sub_i18n)

   * [`cache`](#chapter_05-Factories_sub_cache)
   * [`debug`](#chapter_05-Factories_sub_debug)
   * [`source`](#chapter_05-Factories_sub_source)
   * [`untranslated_prefix`](#chapter_05-Factories_sub_untranslated_prefix)
   * [`untranslated_suffix`](#chapter_05-Factories_sub_untranslated_suffix)

 * [`routing`](#chapter_05-Factories_sub_routing)

   * [`cache`](#chapter_05-Factories_sub_cache)
   * [`extra_parameters_as_query_string`](#chapter_05-Factories_sub_extra_parameters_as_query_string)
   * [`generate_shortest_url`](#chapter_05-Factories_sub_generate_shortest_url)
   * [`lazy_routes_deserialize`](#chapter_05-Factories_sub_lazy_routes_deserialize)
   * [`lookup_cache_dedicated_keys`](#chapter_05-Factories_sub_lookup_cache_dedicated_keys)
   * [`load_configuration`](#chapter_05-Factories_sub_load_configuration)
   * [`segment_separators`](#chapter_05-Factories_sub_segment_separators)
   * [`suffix`](#chapter_05-Factories_sub_suffix)
   * [`variable_prefixes`](#chapter_05-Factories_sub_variable_prefixes)

 * [`logger`](#chapter_05-Factories_sub_logger)

   * [`level`](#chapter_05-Factories_sub_level)
   * [`loggers`](#chapter_05-Factories_sub_loggers)

 * [`controller`](#chapter_05-Factories_sub_controller)

<div class="pagebreak"></div>

`request`
---------

*sfContextアクセサ*: `$context->getRequest()`

*デフォルト構成*:

    [yml]
    request:
      class: sfWebRequest
      param:
        logging:           %SF_LOGGING_ENABLED%
        path_info_array:   SERVER
        path_info_key:     PATH_INFO
        relative_url_root: ~
        formats:
          txt:  text/plain
          js:   [application/javascript, application/x-javascript, text/javascript]
          css:  text/css
          json: [application/json, application/x-json]
          xml:  [text/xml, application/xml, application/x-xml]
          rdf:  application/rdf+xml
          atom: application/atom+xml

### `path_info_array`

`path_info_array`オプションは情報を読み取るために使われるグローバルなPHP配列を定義します。
設定によってはデフォルトの`SERVER`の値を`ENV`に変更するとよいでしょう。

### `path_info_key`

`path_info_key`オプションは`PATH_INFO`の情報が見つかる
キーを定義します。

`IIFR`もしくは`ISAPI`のようなrewriteモジュールつきのIISを使う場合、
この値を`HTTP_X_REWRITE_URL`に変更するとよいでしょう。

### `formats`

`formats`オプションはファイル拡張子と`Content-Type`の配列です。
リクエストURIの拡張子に基づいて、レスポンスの`Content-Type`を自動的に管理するために
symfonyによって使われます。

### `relative_url_root`

`relative_url_root`オプションはフロントコントローラ前のURLの部分を定義します。
たいていの場合、これはフレームワークによって自動的に検出されるので
変更する必要はありません。

`response`
----------

*sfContextアクセサ*: `$context->getResponse()`

*デフォルト構成*:

    [yml]
    response:
      class: sfWebResponse
      param:
        logging:           %SF_LOGGING_ENABLED%
        charset:           %SF_CHARSET%
        send_http_headers: true

*`test`環境用のデフォルト構成*:

    [yml]
    response:
      class: sfWebResponse
      param:
        send_http_headers: false

### `send_http_headers`

`send_http_headers`オプションはレスポンスがレスポンスのコンテンツに沿って
HTTPレスポンスヘッダーを送信するかを指定します。この設定は
出力の後でヘッダーを送信しようとすると警告を発する
PHPの`header()`関数でヘッダーが送信されるので、テストの際に便利です。

### `charset`

`charset`オプションはレスポンスに使う文字集合を定義します。
デフォルトでは、`settings.yml`の`charset`設定が使われます。 

### `http_protocol`

`http_protocol`オプションはレスポンスに使うHTTPプロトコルのバージョンを定義します。
デフォルトでは、利用可能であれば`$_SERVER['SERVER_PROTOCOL']`の値をチェックします。
デフォルトは`HTTP/1.0`です。

`user`
------

*sfContextのアクセサ*: `$context->getUser()`

*デフォルト構成*:

    [yml]
    user:
      class: myUser
      param:
        timeout:         1800
        logging:         %SF_LOGGING_ENABLED%
        use_flash:       true
        default_culture: %SF_DEFAULT_CULTURE%

>**NOTE**
>デフォルトでは、`myUser`クラスは`sfBasicSecurityUser`を継承します。
>これは[`security.yml`](#chapter_08-Security)設定ファイルで
>設定できます。

### `timeout`

`timeout`オプションはユーザー認証のタイムアウトを定義します。
これはセッションのタイムアウトとは関係ありません。
デフォルトの設定は30分間何もしていない
ユーザーの認証を自動的に解除します。

`sfBasicSecurityUser`基底クラスを継承する
ユーザークラスのみがこの設定を使います。
これは`myUser`生成クラスに
当てはまります。

>**NOTE**
>予期していないふるまいを避けるために、ユーザークラスはセッションガーベッジコレクタ用の
>最大限の期限(`session.gc_maxlifetime`)をタイムアウトよりも長くなるように
>強制します。

### `use_flash`

`use_flash`オプションはflashコンポーネントを有効もしくは無効にします。

### `default_culture`

`default_culture`オプションはサイトに始めて訪問したユーザーのために
デフォルトのcultureを定義します。デフォルトでは、
`settings.yml`からの`default_culture`が使用され
たいていの場合これで十分です。

>**CAUTION**
>`factories.yml`もしくは`settings.yml`の`default_culture`設定を変更する場合、
>結果を確認するためにブラウザのCookieを
>クリアする必要があります。

`storage`
---------

ストレージファクトリはHTTPリクエスト間のユーザーデータを一貫させるために
ユーザーファクトリによって使われます。

*sfContextアクセサ*: `$context->getStorage()`

*デフォルト構成*:

    [yml]
    storage:
      class: sfSessionStorage
      param:
        session_name: symfony

*`test`環境用のデフォルト構成*:

    [yml]
    storage:
      class: sfSessionTestStorage
      param:
        session_path: %SF_TEST_CACHE_DIR%/sessions

### `auto_start`

`auto_start`オプションは(`session_start()`関数を通して)PHPのセッション自動開始機能を
有効もしくは無効にします。

### `session_name`

`session_name`オプションはユーザーセッションを保存するために
symfonyによって使用されるCookieの名前を定義します。デフォルトの名前は`symfony`で、
すべてのアプリケーションが同じCookieを共有することを意味します
(そして対応する認証と権限付与も)。

### `session_set_cookie_params()`パラメータ

`storage`ファクトリは
次のオプションの値で[`session_set_cookie_params()`](http://www.php.net/session_set_cookie_params)
関数を呼び出します:

 * `session_cookie_lifetime`: セッションCookieの寿命、秒単位で
                                定義。
 * `session_cookie_path`:   Cookieが機能するドメイン上のパス。
                              ドメインのすべてのパスに対して単独のスラッシュ(`/`)
                              を使う。
 * `session_cookie_domain`: Cookieのドメイン、たとえば`www.php.net`。
                              すべてのサブドメインにCookieを見えるようにするためには
                              `.php.net`のように接頭辞としてドットをドメインにつけなければなりません。
 * `session_cookie_secure`: `true`の場合Cookieはセキュアなコネクションを通してのみ
                              送信されます。
 * `session_cookie_httponly`: `true`にセットされている場合、セッションCookieを設定する際に
                                PHPは`httponly`フラグを送信しようとします。

>**NOTE**
>それぞれのオプションの説明は`session_set_cookie_params()`関数の説明は
>PHPの公式サイトに説明に由来しています。

### `session_cache_limiter`

`session_cache_limiter`オプションがセットされている場合、PHPの
[`session_cache_limiter()`](http://www.php.net/session_cache_limiter)
関数が呼び出され引数としてオプションの値が渡されます。

### データベースストレージ固有のオプション

`sfDatabaseSessionStorage`クラスを継承するストレージを使うとき、
いくつかの追加オプションが利用可能です:

 * `database`:     データベースの名前(必須)
 * `db_table`:     テーブルの名前(必須)
 * `db_id_col`:    主キーのカラムの名前(デフォルトは`sess_id`)
 * `db_data_col`:  データカラムの名前(デフォルトは`sess_data`)
 * `db_time_col`:  時間カラムの名前(デフォルトは`sess_time`)

`view_cache_manager`
--------------------

*sfContextアクセサ*: `$context->getViewCacheManager()`

*デフォルト構成*:

    [yml]
    view_cache_manager:
      class: sfViewCacheManager

>**CAUTION**
>[`cache`](#chapter_04-Settings_sub_cache)設定が
>`on`にセットされている場合にのみこのファクトリは作成されます。

ビューキャッシュマネージャーの設定は`param`キーを含みません。
この設定は`view_cache`ファクトリを通して行われます。
これはビューキャッシュマネージャーによって使われる
内部のキャッシュオブジェクトを定義します。

`view_cache`
------------

*sfContextアクセサ*: 無し(`view_cache_manager`ファクトリによって直接使われる)

*デフォルト構成*:

    [yml]
    view_cache:
      class: sfFileCache
      param:
        automatic_cleaning_factor: 0
        cache_dir:                 %SF_TEMPLATE_CACHE_DIR%
        lifetime:                  86400
        prefix:                    %SF_APP_DIR%/template

>**CAUTION**
>[`cache`](#chapter_04-Settings_sub_cache)設定が
>`on`にセットされている場合のみこのファクトリが定義されます。

`view_cache`ファクトリは`sfCache`を継承するキャッシュクラスを
定義します(詳細な情報はキャッシュのセクションを参照)。

`i18n`
------

*sfContextアクセサ*: `$context->getI18N()`

*デフォルト構成*:

    [yml]
    i18n:
      class: sfI18N
      param:
        source:               XLIFF
        debug:                off
        untranslated_prefix:  "[T]"
        untranslated_suffix:  "[/T]"
        cache:
          class: sfFileCache
          param:
            automatic_cleaning_factor: 0
            cache_dir:                 %SF_I18N_CACHE_DIR%
            lifetime:                  31556926
            prefix:                    %SF_APP_DIR%/i18n

>**CAUTION**
>[`i18n`](#chapter_04-Settings_sub_i18n)設定が
>`on`にセットされている場合のみこのファクトリが定義されます。

### `source`

`source`オプションは翻訳用コンテナの種類を定義します。

*組み込みのコンテナ*: `XLIFF`、`SQLite`、`MySQL`、と`gettext`

### `debug`

`debug`オプションはデバッグモードをセットします。`on`にセットされる場合、
未翻訳のメッセージは接頭辞と接尾辞によってデコレートされます(下記を参照)。

### `untranslated_prefix`

`untranslated_prefix`は未翻訳のメッセージに使われる接頭辞を定義します。

### `untranslated_suffix`

`untranslated_suffix`は未翻訳のメッセージに使われる接尾辞を定義します。

### `cache`

`cache`オプションは国際化データのキャッシュに使われる匿名キャッシュファクトリ
を定義します(詳細な情報はキャッシュのセクションを参照)。

`routing`
---------

*sfContextアクセサ*: `$context->getRouting()`

*デフォルト構成*:

    [yml]
    routing:
      class: sfPatternRouting
      param:
        load_configuration:               true
        suffix:                           ''
        default_module:                   default
        default_action:                   index
        debug:                            %SF_DEBUG%
        logging:                          %SF_LOGGING_ENABLED%
        generate_shortest_url:            true
        extra_parameters_as_query_string: true
        cache:
          class: sfFileCache
          param:
            automatic_cleaning_factor: 0
            cache_dir:                 %SF_CONFIG_CACHE_DIR%/routing
            lifetime:                  31556926
            prefix:                    %SF_APP_DIR%/routing

### `variable_prefixes`

*デフォルト*: `:`

`variable_prefixes`オプションはルートのパターンの変数名を始める
文字のリストを定義します。

### `segment_separators`

*デフォルト*: `/`と`.`

`segment_separators`オプションはルートセグメントの区切り文字のリストを定義します。
たいていの場合、特定のルート以外、ルーティング全体に対してこのオプションを
オーバーライドすることはないでしょう。

### `generate_shortest_url`

*デフォルト*: 新しいプロジェクトでは`true`、アップグレードしたプロジェクトには`false`

`true`にセットされる場合、`generate_shortest_url`オプションは
ルーティングシステムに実現可能な最短のルートを生成するよう伝えます。
symfony 1.0と1.1との後方互換性のあるルートが欲しい場合は、
`false`にセットします。

### `extra_parameters_as_query_string`

*デフォルト*: 新しいプロジェクトには`true`、アップグレードしたプロジェクトには`false`

パラメータがルートの生成に使われるとき、
`extra_parameters_as_query_string`は
追加パラメータをクエリ文字列に変換することも可能にします。
symfony 1.0もしくは1.1のふるまいを
フォールバックするには`false`にセットします。このバージョンでは、
追加パラメータはルーティングシステムによって無視されるだけでした。

### `cache`

`cache`オプションはルーティングの設定とデータに使われる
匿名キャッシュファクトリを定義します(詳細な情報はキャッシュセクションを参照)。

### `suffix`

*デフォルト*: none

すべてのルートに対して使用するデフォルトの接尾辞。このオプションは
非推奨でもはや役に立ちません。

### `load_configuration`

*デフォルト*: `true`

`load_configuration`オプションは`routing.yml`ファイルが
自動的にロードされ解析されなければならないのかを定義します。
symfonyプロジェクトの外部でルーティングシステムを
使いたい場合`false`にセットします。

### `lazy_routes_deserialize`

*デフォルト*: `false`

`true`にセットする場合、`lazy_routes_deserialize`設定はルーティングキャッシュの遅延
デシリアライズの有効にします。たくさんのルートを抱えており、もっともマッチするのが最初のものである場合
この設定はアプリケーションのパフォーマンスを改善できます。
特定の状況ではパフォーマンスに悪い影響を与える可能性があるので
本番サーバーにデプロイする前に設定をテストすることを
強くお勧めします。

>**CAUTION**
>symfony 1.2.7とそれ以降でのみこの設定は利用できます。

### `lookup_cache_dedicated_keys`

*デフォルト*: `false`

`lookup_cache_dedicated_keys`設定はルーティングキャッシュをコンストラクタする方法を決定します。
`false`にセットされている場合、キャッシュは1つの大きな値として保存されます; `true`に
セットされている場合それぞれのルートは独自のキャッシュストアを持ちます。
この設定はパフォーマンス最適化設定です。

経験則として、ファイルベースのキャッシュクラス(たとえば`sfFileCache`)を使う際には
この設定を`false`に、メモリベースのキャッシュクラス(たとえば`sfAPCCache`)を使う際には`true`に
すると良いです。

>**CAUTION**
>symfony 1.2.7とそれ以降でのみこの設定は利用可能です。

`logger`
--------

*sfContextアクセサ*: `$context->getLogger()`

*デフォルト構成*:

    [yml]
    logger:
      class: sfAggregateLogger
      param:
        level: debug
        loggers:
          sf_web_debug:
            class: sfWebDebugLogger
            param:
              level: debug
              condition:       %SF_WEB_DEBUG%
              xdebug_logging:  true
              web_debug_class: sfWebDebug
          sf_file_debug:
            class: sfFileLogger
            param:
              level: debug
              file: %SF_LOG_DIR%/%SF_APP%_%SF_ENVIRONMENT%.log

*`prod`環境用のデフォルト構成*:

    [yml]
    logger:
      class:   sfNoLogger
      param:
        level:   err
        loggers: ~

>**CAUTION**
>このファクトリは常に定義されますが、`logging_enabled`設定が`on`にセットされている
>場合のみロギングが行われます。

### `level`

`level`オプションはロガーのレベルを定義します。

*可能な値*: `EMERG`、`ALERT`、`CRIT`、`ERR`、`WARNING`、`NOTICE`、
`INFO`、もしくは`DEBUG`

### `loggers`

`loggers`オプションは使用するロガーのリストを定義します。リストは
匿名ロガーファクトリの配列です。

*組み込みのロガークラス*: `sfConsoleLogger`、`sfFileLogger`、`sfNoLogger`、
`sfStreamLogger`、と`sfVarLogger`

`controller`
------------

*sfContextアクセサ*: `$context->getController()`

*デフォルト構成*:

    [yml]
    controller:
      class: sfFrontWebController

匿名キャッシュファクトリ
-----------------------

いくつかのファクトリ(`view_cache`、`i18n`、と`routing`)はキャッシュオブジェクトを利用できます。
キャッシュオブジェクトの設定はすべてのファクトリと似ています。
`cache`キーは匿名キャッシュファクトリを定義します。
他のファクトリと同じように、これは`class`と
`param`エントリを取ります。`param`エントリは任意のキャッシュクラスで
利用可能な任意のオプションを取ります。

最も重要なのは`prefix`オプションで異なる環境/アプリケーション/プロジェクトの間で
キャッシュを共有するもしくは分離できるようにします。

*組み込みのキャッシュクラス*: `sfAPCCache`、`sfEAcceleratorCache`、`sfFileCache`、
`sfMemcacheCache`、`sfNoCache`、`sfSQLiteCache`、と`sfXCachCache`。
