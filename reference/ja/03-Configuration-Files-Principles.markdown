設定ファイルの原則
=================

symfonyの設定ファイルは一連の共通原則にしたがい共通のプロパティを共有します。
このセクションでは、YAMLの設定ファイルを記述する他のセクション用のリファレンスとして
これらを詳しく説明します。

キャッシュ
----------

symfonyのすべての設定ファイルは設定ハンドラクラスによってPHPファイルとしてキャッシュされます。
`is_debug`設定が`false`のとき(例えば`prod`環境)、YAMLファイルは一番最初のリクエスト用に
のみアクセスされます; PHPキャッシュは後に来るリクエストのために使われます。このことは
最初のときにYAMLファイルが解析され解釈されるとき、
"重い"作業は一度のみしか行われないことを意味します。

>**TIP**
>デフォルトでは`is_debug`が`true`にセットされている`dev`環境において、
>設定ファイルが変更されるたびにコンパイルが行われます(symfony
>はファイルの修正時刻をチェックします)。

それぞれの設定ファイルの解析とキャッシュは特別な設定ハンドラクラスで行われます。
設定ハンドラは[`config_handler.yml`](#chapter_14-Other-Configuration-Files_config_handlers_yml)で設定されます。

次のセクションで、"コンパイル"の話をするとき、
これは最初にYAMLファイルがPHPファイルに変換されキャッシュに
保存されることを意味します。

>**TIP**
>設定キャッシュのリロードを強制するには、
>`cache:clear`タスクを使います:
>
>     $ php symfony cache:clear --type=config

定数
----

*設定ファイル*: `core_compile.yml`、`factories.yml`、`generator.yml`,
`databases.yml`、`filters.yml`、`view.yml`、`autoload.yml`

設定ファイルの中にはあらかじめ定義された定数の使用を許可するものがあります。
定数は`%XXX%`(XXXは大文字のキー)で表記されるプレースホルダーで宣言され
"コンパイル時"に実際の値に置き換えられます。

### 設定の構成

定数は`settings.yml`設定ファイルて定義される任意の設定になります。
プレースホルダーのキーは
`SF_`の接頭辞がつけられた大文字の設定キーの名前です:

    [yml]
    logging: %SF_LOGGING_ENABLED%

symfonyが設定ファイルをコンパイルするとき、存在するすべての`%SF_XXX%`プレースホルダーは
`settings.yml`からの値に置き換えられます。上記の例では、
`SF_LOGGING_ENABLED`プレースホルダーが
`settings.yml`で定義された`logging_enabled`設定の値に置き換えます。

### アプリケーションの設定

キーの名前に接頭辞として`APP_`をつけることで`app.yml`設定ファイルで
定義される設定も使うことができます。

### 特別な定数

デフォルトでは、現在のフロントコントローラに従ってsymfonyは
4つの定数を定義します:

 | 定数                   | 説明                            | 設定メソッド          |
 | ---------------------- | ------------------------------- | -------------------- |
 | `SF_APP`               | 現在のアプリケーションの名前     | `getApplication()`   |
 | `SF_ENVIRONMENT`       | 現在の環境の名前                | `getEnvironment()`   |
 | `SF_DEBUG`             | デバッグモードが有効であるか     | `isDebug()`          |
 | `SF_SYMFONY_LIB_DIR`   | symfonyライブラリのディレクトリ | `getSymfonyLibDir()` |

### ディレクトリ

ハードコードせずにディレクトリもしくはファイルパスを参照する際に定数はとても便利です。
symfonyは共通のプロジェクトとアプリケーションディレクトリ用に
多くの定数を定義します。

階層のrootはプロジェクトのrootディレクトリである`SF_ROOT_DIR`です。
他のすべての定数はこのrootディレクトリから派生します。

プロジェクトのディレクトリ構造は次のように定義されます:

 | 定数               | デフォルトの値        |
 | ------------------ | -------------------- |
 | `SF_APPS_DIR`    | `SF_ROOT_DIR/apps`   |
 | `SF_CONFIG_DIR`  | `SF_ROOT_DIR/config` |
 | `SF_CACHE_DIR`   | `SF_ROOT_DIR/cache`  |
 | `SF_DATA_DIR`    | `SF_ROOT_DIR/data`   |
 | `SF_DOC_DIR`     | `SF_ROOT_DIR/doc`    |
 | `SF_LIB_DIR`     | `SF_ROOT_DIR/lib`    |
 | `SF_LOG_DIR`     | `SF_ROOT_DIR/log`    |
 | `SF_PLUGINS_DIR` | `SF_ROOT_DIR/plugins`|
 | `SF_TEST_DIR`    | `SF_ROOT_DIR/test`   |
 | `SF_WEB_DIR`     | `SF_ROOT_DIR/web`    |
 | `SF_UPLOAD_DIR`  | `SF_WEB_DIR/uploads` |

アプリケーションのディレクトリ構造は
`SF_APPS_DIR/APP_NAME`ディレクトリの元で定義されます:

 | 定数                    | デフォルトの値          |
 | ----------------------- | ---------------------- |
 | `SF_APP_CONFIG_DIR`   | `SF_APP_DIR/config`    |
 | `SF_APP_LIB_DIR`      | `SF_APP_DIR/lib`       |
 | `SF_APP_MODULE_DIR`   | `SF_APP_DIR/modules`   |
 | `SF_APP_TEMPLATE_DIR` | `SF_APP_DIR/templates` |
 | `SF_APP_I18N_DIR`     | `SF_APP_DIR/i18n`      |

最後に、アプリケーションのキャッシュディレクトリ構造は次のように定義されます:

 | 定数                      | デフォルトの値                    |
 | ------------------------- | -------------------------------- |
 | `SF_APP_BASE_CACHE_DIR` | `SF_CACHE_DIR/APP_NAME`          |
 | `SF_APP_CACHE_DIR`      | `SF_CACHE_DIR/APP_NAME/ENV_NAME` |
 | `SF_TEMPLATE_CACHE_DIR` | `SF_APP_CACHE_DIR/template`      |
 | `SF_I18N_CACHE_DIR`     | `SF_APP_CACHE_DIR/i18n`          |
 | `SF_CONFIG_CACHE_DIR`   | `SF_APP_CACHE_DIR/config`        |
 | `SF_TEST_CACHE_DIR`     | `SF_APP_CACHE_DIR/test`          |
 | `SF_MODULE_CACHE_DIR`   | `SF_APP_CACHE_DIR/modules`       |

環境の認識
----------

*設定ファイル*: `settings.yml`、`factories.yml`、`databases.yml`、
`app.yml`

symfonyの設定ファイルの中には環境を認識するものがあります。
これらの解釈はsymfonyの現在の環境に依存します。これらのファイルは
それぞれの環境ごとに変わる設定を定義する異なるセクションを持ちます。
新しいアプリケーションを作成するとき、symfonyは
3つのデフォルト環境: `prod`、`test`、と`dev`を考慮する設定を作成します:

    [yml]
    prod:
      # prod環境用の設定

    test:
      # test環境用の設定

    dev:
      # dev環境用の設定

    all:
      # すべての環境用のデフォルト設定

symfonyが設定ファイルから値を必要とするとき、
現在の環境セクションで見つかる設定を`all`設定にマージします。
特別な`all`セクションはすべての環境用のデフォルト設定を定義します。
環境セクションが定義されていない場合、symfonyは
`all`設定に戻ります。

設定カスケード
-------------

*設定ファイル*: `core_compile.yml`、`autoload.yml`、`settings.yml`、
`factories.yml`、`databases.yml`、`security.yml`、`cache.yml`、`app.yml`、
`filters.yml`、`view.yml`

設定ファイルはプロジェクトのディレクトリ構造に格納される
いくつかの`config/`サブディレクトリで定義できます。

設定がコンパイルされるとき、すべての異なるファイルからの値は
次の優先順位に従ってマージされます

  * モジュールの設定(`PROJECT_ROOT_DIR/apps/APP_NAME/modules/MODULE_NAME/config/XXX.yml`)
  * アプリケーションの設定(`PROJECT_ROOT_DIR/apps/APP_NAME/config/XXX.yml`)
  * プロジェクトの設定(`PROJECT_ROOT_DIR/config/XXX.yml`)
  * プラグインで定義された設定(`PROJECT_ROOT_DIR/plugins/*/config/XXX.yml`)
  * symfonyライブラリで定義されたデフォルト設定(`SF_LIB_DIR/config/XXX.yml`)

例えば、アプリケーションディレクトリで定義される`settings.yml`は
プロジェクトのメインの`config/`ディレクトリの設定の一式、および
フレームワーク自身に格納されるデフォルト設定
(`lib/config/config/settings.yml`)を継承します。

>**TIP**
>When 設定ファイルが環境を認識し複数のディレクトリで定義できる場合、
>次の優先順位リストが適用されます:
>
> 1. モジュール
> 2. アプリケーション
> 3. プロジェクト
> 4. 特定の環境
> 5. すべての環境
> 6. デフォルト
