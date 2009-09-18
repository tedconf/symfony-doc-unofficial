その他の設定ファイル
===================

この章ではその他のsymfonyの設定ファイルを説明します。
これらを変更する必要性はほとんどありません。

~`autoload.yml`~
----------------

`autoload.yml`設定はsymfonyによってオートロードされる必要のあるディレクトリを決定します。
それぞれのディレクトリはPHPクラスとインターフェイスを見つけるためにスキャンされます。

最初の章で説明したように、`autoload.yml`ファイルは[**コンフィギュレーションカスケードのメカニズム**](#chapter_03_configuration_cascade)が有効で、[**定数**](#chapter_03_constants)を含むことができます。

>**NOTE**
>`autoload.yml`設定ファイルはPHPファイルとしてキャッシュされます; 

たいていのプロジェクトではデフォルトコンフィギュレーションで十分です:

    [yml]
    autoload:
      # project
      project:
        name:           project
        path:           %SF_LIB_DIR%
        recursive:      on
        exclude:        [model, symfony]

      project_model:
        name:           project model
        path:           %SF_LIB_DIR%/model
        recursive:      on

      # application
      application:
        name:           application
        path:           %SF_APP_LIB_DIR%
        recursive:      on

      modules:
        name:           module
        path:           %SF_APP_DIR%/modules/*/lib
        prefix:         1
        recursive:      on

それぞれの設定は名前を持ち、その名前を持つキーの下でセットしなければなりません。
これによってオーバーライドされるデフォルトの設定が可能になります。

>**TIP**
>ご覧の通り、デフォルトでは`lib/vendor/symfony/`ディレクトリは除外されます。
>symfonyはコアクラスには異なるオートロードのメカニズムを利用するからです。

オートロードのふるまいをカスタマイズするためにいくつかのキーを使うことができます:

 * `name`: 説明
 * `path`: オートロードするパス
 * `recursive`: サブディレクトリでPHPクラスを探すか
 * `exclude`: 検索から除外するディレクトリの名前の配列
 * `prefix`: パスで見つかるクラスが指定されたモジュールのみをオートロードする場合`true`にセットする(デフォルトでは`false`)
 * `files`: PHPクラスのために明示的に解析するファイルの配列
 * `ext`: PHPクラスの拡張子(デフォルトは`.php`)

たとえば、`lib/`ディレクトリの下でプロジェクト内部で大きなライブラリを埋め込み、オートロード機能がすでにサポートされている場合、パフォーマンスの向上させるために`project`のオートロード設定を修正することでsymfonyのデフォルトのオートロードシステムからそのライブラリを除外できます:

    [yml]
    autoload:
      project:
        name:           project
        path:           %SF_LIB_DIR%
        recursive:      on
        exclude:        [model, symfony, vendor/large_lib]

~`config_handlers.yml`~
-----------------------

`config_handlers.yml`設定ファイルは他のすべてのYAML設定ファイルを解釈するために使われるコンフィギュレーションハンドラークラスを記述します。
`settings.yml`設定ファイルをロードするために使われるデフォルトコンフィギュレーションは次の通りです:

    [yml]
    config/settings.yml:
      class:    sfDefineEnvironmentConfigHandler
      param:
        prefix: sf_

それぞれの設定ファイルはクラス(`class`エントリ)によって定義し`param`セクションの下でパラメーターを定義することでさらにカスタマイズできます。

デフォルトの`config_handlers.yml`ファイルは次のようにパーサークラスを定義します:

 | 設定ファイル       | コンフィギュレーションハンドラークラス |
 | ------------------ | ----------------------------------- |
 | `autoload.yml`     | `sfAutoloadConfigHandler`           |
 | `databases.yml`    | `sfDatabaseConfigHandler`           |
 | `settings.yml`     | `sfDefineEnvironmentConfigHandler`  |
 | `app.yml`          | `sfDefineEnvironmentConfigHandler`  |
 | `factories.yml`    | `sfFactoryConfigHandler`            |
 | `core_compile.yml` | `sfCompileConfigHandler`            |
 | `filters.yml`      | `sfFilterConfigHandler`             |
 | `routing.yml`      | `sfRoutingConfigHandler`            |
 | `generator.yml`    | `sfGeneratorConfigHandler`          |
 | `view.yml`         | `sfViewConfigHandler`               |
 | `security.yml`     | `sfSecurityConfigHandler`           |
 | `cache.yml`        | `sfCacheConfigHandler`              |
 | `module.yml`       | `sfDefineEnvironmentConfigHandler`  |

~`core_compile.yml`~
--------------------

`core_compile.yml`設定ファイルはsymfonyがロードする時間を加速するために`prod`環境で1つの大きなファイルにマージされるPHPファイルを記述します。
デフォルトでは、symfonyのメインのコアクラスはこの設定ファイルで定義されます。
アプリケーションがそれぞれのリクエストごとにロードする必要のあるいくつかのクラスに依存する場合、プロジェクトもしくはアプリケーションの`core_compile.yml`設定ファイルを作成しこれらのクラスを設定ファイルに追加できます。
デフォルトコンフィギュレーションの抜粋は次の通りです:

    [yml]
    - %SF_SYMFONY_LIB_DIR%/autoload/sfAutoload.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfComponent.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfAction.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfActions.class.php

はじめの章で説明したように、`core_compile.yml`ファイルは[**コンフィギュレーションカスケードのメカニズム**](#chapter_03_configuration_cascade)が有効で、[**定数**](#chapter_03_constants)を含むことができます。

>**NOTE**
>`core_compile.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>処理は~`sfCompileConfigHandler`~[クラス](#chapter_14_config_handlers_yml)によって自動的に管理されます。
