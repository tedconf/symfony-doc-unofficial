その他の設定ファイル
===================

この章ではその他のsymfonyの設定ファイルを説明します。
これらは変更する必要はほとんどありません。

`autoload.yml`
--------------

`autoload.yml`設定はsymfonyによってオートロードする必要のあるディレクトリを決定します。
それぞれのディレクトリはPHPクラスとインターフェイスを見つけるために
スキャンされます。

最初の章で説明したように、`autoload.yml`ファイルは
[**設定カスケードのメカニズム**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade)から恩恵を受け、[**定数**](#chapter_03-Configuration-Files-Principles_sub_constants)を格納できます。

>**NOTE**
>`autoload.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>処理は`sfAutoloadConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。

大抵のプロジェクトではデフォルト構成で十分です:

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

オートロードの振る舞いをカスタマイズするためにいくつかのキーを使うことができます:

 * `name`: 説明
 * `path`: オートロードするパス
 * `recursive`: サブディレクトリでPHPクラスを探すか
 * `exclude`: 検索から除外するディレクトリの名前の配列
 * `prefix`: パスで見つかるクラスが指定されたモジュールのみをオートロードする場合`true`にセットする(デフォルトでは`false`)
 * `files`: PHPクラスのために明示的に解析するファイルの配列
 * `ext`: PHPクラスの拡張子(デフォルトは`.php`)

例えば、`lib/`ディレクトリの元でプロジェクト内部で大きなライブラリを埋め込み、
オートロード機能が既にサポートされている場合、パフォーマンスの押し上げの恩恵を受けるために
`project`のオートロード設定を修正することでsymfonyのデフォルトのオートロードシステムから
そのライブラリを除外できます:

    [yml]
    autoload:
      project:
        name:           project
        path:           %SF_LIB_DIR%
        recursive:      on
        exclude:        [model, symfony, vendor/large_lib]

`config_handlers.yml`
---------------------

`config_handlers.yml`設定ファイルは他のすべてのYAML設定ファイルを解析して
解釈するために使われる設定ハンドラクラスを記述します。
`settings.yml`設定ファイルをロードするために使われる
デフォルト構成は次の通りです:

    [yml]
    config/settings.yml:
      class:    sfDefineEnvironmentConfigHandler
      param:
        prefix: sf_

それぞれの設定ファイルはクラス(`class`エントリ)によって定義し
`param`セクションの下でパラメータを定義することでさらにカスタマイズできます。

デフォルトの`config_handlers.yml`ファイルは次のようにパーサークラスを定義します:

 | 設定ファイル       | 設定ハンドラクラス                  |
 | ------------------ | ---------------------------------- |
 | `autoload.yml`     | `sfAutoloadConfigHandler`          |
 | `databases.yml`    | `sfDatabaseConfigHandler`          |
 | `settings.yml`     | `sfDefineEnvironmentConfigHandler` |
 | `app.yml`          | `sfDefineEnvironmentConfigHandler` |
 | `factories.yml`    | `sfFactoryConfigHandler`           |
 | `core_compile.yml` | `sfCompileConfigHandler`           |
 | `filters.yml`      | `sfFilterConfigHandler`            |
 | `routing.yml`      | `sfRoutingConfigHandler`           |
 | `generator.yml`    | `sfGeneratorConfigHandler`         |
 | `view.yml`         | `sfViewConfigHandler`              |
 | `security.yml`     | `sfSecurityConfigHandler`          |
 | `cache.yml`        | `sfCacheConfigHandler`             |
 | `module.yml`       | `sfDefineEnvironmentConfigHandler` |

`core_compile.yml`
------------------

`core_compile.yml`設定ファイルはsymfonyがロードする時間を加速するために
`prod`環境で1つの大きなファイルにマージされるPHPファイルを記述します。
デフォルトでは、symfonyのメインのコアクラスはこの設定ファイルで定義されます。
アプリケーションがそれぞれのリクエストごとにロードする必要のあるいくつかのクラスに依存する場合、
プロジェクトもしくはアプリケーションの`core_compile.yml`
設定ファイルを作成しこれらのクラスを設定ファイルに追加できます。
デフォルト構成の抜粋は次の通りです:

    [yml]
    - %SF_SYMFONY_LIB_DIR%/autoload/sfAutoload.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfComponent.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfAction.class.php
    - %SF_SYMFONY_LIB_DIR%/action/sfActions.class.php

最初の章で説明したように、`core_compile.yml`ファイルは
[**設定カスケードのメカニズム**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade)の恩恵を受け、
[**定数**](#chapter_03-Configuration-Files-Principles_sub_constants)を格納できます。

>**NOTE**
>`core_compile.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>処理は`sfCompileConfigHandler`
>[クラス](#chapter_14_config_handlers_yml)によって自動的に管理されます。
