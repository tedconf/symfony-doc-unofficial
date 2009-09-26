設定ファイルの原則
=================

symfonyの設定ファイルは一連の共通原則にしたがい共通のプロパティを共有します。
このセクションでは、YAMLの設定ファイルを説明する他のセクション用のリファレンスとしてこれらを詳しく説明します。

キャッシュ
----------

symfonyのすべての設定ファイルはコンフィギュレーションハンドラークラスによってPHPファイルとしてキャッシュされます。
`is_debug`設定が`false`のとき(たとえば`prod`環境)、YAMLファイルは初回のリクエスト時のみ
アクセスされます;初回以降のアクセスはPHPキャッシュが使われます。前述のような仕様は
初回時のみYAMLファイルの解析と解釈を行うことで、"重い"作業は一度しか行われないことを意味します。

>**TIP**
>デフォルトで`is_debug`が`true`にセットされている`dev`環境においては、設定ファイルが変更されるたびにコンパイルが行われます(symfonyはファイルの修正時刻をチェックします)。

それぞれの設定ファイルの解析とキャッシュは特別なコンフィギュレーションハンドラークラスで行われます。
コンフィギュレーションハンドラーは[`config_handler.yml`](#chapter_14_config_handlers_yml)で設定されます。

次のセクションで"コンパイル"について話をしますが、コンパイルとは初回アクセス時にYAMLファイルがPHPファイルに変換されキャッシュに保存されることを意味します。

>**TIP**
>コンフィギュレーションキャッシュのリロードを強制するには、`cache:clear`タスクを使います:
>
>     $ php symfony cache:clear --type=config

定数
----

*設定ファイル*: `core_compile.yml`、`factories.yml`、`generator.yml`、`databases.yml`、`filters.yml`、`view.yml`、`autoload.yml`

いくつかの設定ファイルはあらかじめ定義されている定数の使用を許容します。
定数は`%XXX%`(XXXは大文字のキー)で表記されるプレースホルダーで宣言され"コンパイル"の時に実際の値に置き換えられます。

### コンフィギュレーション設定

定数は`settings.yml`設定ファイルで定義される任意の値を設定することができます。
プレースホルダーのキーは`SF_`のプレフィックスがつけられた大文字の設定キーの名前です:

    [yml]
    logging: %SF_LOGGING_ENABLED%

symfonyが設定ファイルをコンパイルするとき、存在するすべての`%SF_XXX%`プレースホルダーは`settings.yml`からの値に置き換えられます。
上記の例では、`SF_LOGGING_ENABLED`プレースホルダーを`settings.yml`内の`logging_enabled`で定義されている値に置き換えます。

### アプリケーションの設定

キーの名前にプレフィックスとして`APP_`をつけることで`app.yml`設定ファイルで定義される設定も使うことができます。

### 特別な定数

デフォルトでは、現在のフロントコントローラーに従ってsymfonyは4つの定数を定義します:

 | 定数                 | 説明                            | 設定メソッド         |
 | -------------------- | ------------------------------ | -------------------- |
 | `SF_APP`             | 現在のアプリケーションの名前    | `getApplication()`   |
 | `SF_ENVIRONMENT`     | 現在の環境の名前               | `getEnvironment()`   |
 | `SF_DEBUG`           | デバッグモードが有効であるか    | `isDebug()`          |
 | `SF_SYMFONY_LIB_DIR` | symfonyライブラリのディレクトリ | `getSymfonyLibDir()` |

### ディレクトリ

ハードコードせずにディレクトリもしくはファイルパスを参照する際に定数はとても便利です。
symfonyは共通のプロジェクトとアプリケーションディレクトリ用に多くの定数を定義します。

階層の基点となるのはプロジェクトのrootディレクトリである`SF_ROOT_DIR`です。
他のすべての定数はこのrootディレクトリから派生します。

プロジェクトのディレクトリ構造は次のように定義されます:

 | 定数             | デフォルトの値        |
 | ---------------- | -------------------- |
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

アプリケーションのディレクトリ構造は`SF_APPS_DIR/APP_NAME`ディレクトリの元で定義されます:

 | 定数                  | デフォルトの値          |
 | --------------------- | ---------------------- |
 | `SF_APP_CONFIG_DIR`   | `SF_APP_DIR/config`    |
 | `SF_APP_LIB_DIR`      | `SF_APP_DIR/lib`       |
 | `SF_APP_MODULE_DIR`   | `SF_APP_DIR/modules`   |
 | `SF_APP_TEMPLATE_DIR` | `SF_APP_DIR/templates` |
 | `SF_APP_I18N_DIR`     | `SF_APP_DIR/i18n`      |

最後に、アプリケーションのキャッシュディレクトリ構造は次のように定義されます:

 | 定数                    | デフォルトの値                    |
 | ------------------------| -------------------------------- |
 | `SF_APP_BASE_CACHE_DIR` | `SF_CACHE_DIR/APP_NAME`          |
 | `SF_APP_CACHE_DIR`      | `SF_CACHE_DIR/APP_NAME/ENV_NAME` |
 | `SF_TEMPLATE_CACHE_DIR` | `SF_APP_CACHE_DIR/template`      |
 | `SF_I18N_CACHE_DIR`     | `SF_APP_CACHE_DIR/i18n`          |
 | `SF_CONFIG_CACHE_DIR`   | `SF_APP_CACHE_DIR/config`        |
 | `SF_TEST_CACHE_DIR`     | `SF_APP_CACHE_DIR/test`          |
 | `SF_MODULE_CACHE_DIR`   | `SF_APP_CACHE_DIR/modules`       |

環境の認識
----------

*設定ファイル*: `settings.yml`、`factories.yml`、`databases.yml`、`app.yml`

symfonyの設定ファイルの中には環境を認識するものがあります。
環境の解釈は現在symfonyが動作している環境に依存します。これら設定ファイルはそれぞれの環境ごとに変化するべき設定を定義するための異なるセクションを持ちます。
新しいアプリケーションを作成するとき、symfonyは3つのデフォルト環境: `prod`、`test`と`dev`を含む適切な設定ファイルを作成します:

    [yml]
    prod:
      # prod環境用の設定

    test:
      # test環境用の設定

    dev:
      # dev環境用の設定

    all:
      # すべての環境用のデフォルト設定

symfonyが設定ファイルから値を必要とするとき、現在の環境セクションで見つかる設定と`all`セクションの設定をマージします。
特別な`all`セクションはすべての環境用のデフォルト設定を定義します。
環境セクションが定義されていない場合、symfonyは`all`設定を代わりに使います。

コンフィギュレーションカスケード
------------------------------

*設定ファイル*: `core_compile.yml`、`autoload.yml`、`settings.yml`、`factories.yml`、`databases.yml`、`security.yml`、`cache.yml`、`app.yml`、`filters.yml`、`view.yml`

設定ファイルはプロジェクトのディレクトリ構造に格納されるいくつかの`config/`サブディレクトリで定義できます。

コンフィギュレーションがコンパイルされるとき、すべての異なるファイルからの値は次の優先順位に従ってマージされます

  * モジュールのコンフィギュレーション(`PROJECT_ROOT_DIR/apps/APP_NAME/modules/MODULE_NAME/config/XXX.yml`)
  * アプリケーションのコンフィギュレーション(`PROJECT_ROOT_DIR/apps/APP_NAME/config/XXX.yml`)
  * プロジェクトのコンフィギュレーション(`PROJECT_ROOT_DIR/config/XXX.yml`)
  * プラグインで定義されるコンフィギュレーション(`PROJECT_ROOT_DIR/plugins/*/config/XXX.yml`)
  * symfonyライブラリで定義されるデフォルトのコンフィギュレーション(`SF_LIB_DIR/config/XXX.yml`)

たとえば、アプリケーションディレクトリで定義される`settings.yml`はプロジェクトのメインの`config/`ディレクトリのコンフィギュレーションのセット、およびフレームワーク自身に格納されるデフォルトコンフィギュレーション(`lib/config/config/settings.yml`)を継承します。

>**TIP**
>設定ファイルが環境を認識し複数のディレクトリで定義できる場合、次の優先順位リストが適用されます:
>
> 1. モジュール
> 2. アプリケーション
> 3. プロジェクト
> 4. 特定の環境
> 5. すべての環境
> 6. デフォルト
