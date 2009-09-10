タスク
======

symfonyフレームワークはコマンドラインインターフェイスツールを搭載しています。
組み込みのタスクによって開発者はプロジェクトの間に繰り返される
たくさんのタスクを実行できるようになります。

引数なしで`symfony` CLIを実行すると、
利用可能なタスクの一覧が表示されます:

    $ php symfony

`-V`オプションを渡すことで、symfonyのバージョンと
CLIで使われるsymfonyライブラリのパスの情報が得られます:

    $ php symfony -V

CLIツールは最初の引数としてタスクの名前を受け取ります:

    $ php symfony list

タスクの名前はコロン(`:`)で区切られる、
オプションの名前空間と名前でコンフィギュレーションされます:

    $ php symfony cache:clear

タスクの名前の後で、引数とオプションが渡されます:

    $ php symfony cache:clear --type=template

CLIツールは値の有無とオプションの長短の表記をそれぞれサポートします。

デバッグ情報を出力するようにタスクに指示する`-t`オプションはグローバルオプションです。

<div class="pagebreak"></div>

利用可能なタスク
---------------

 * グローバルタスク
   * [`help`](#chapter_16_sub_help)
   * [`list`](#chapter_16_sub_list)
 * [`app`](#chapter_16_app)
   * [`app::routes`](#chapter_16_sub_app_routes)
 * [`cache`](#chapter_16_cache)
   * [`cache::clear`](#chapter_16_sub_cache_clear)
 * [`configure`](#chapter_16_configure)
   * [`configure::author`](#chapter_16_sub_configure_author)
   * [`configure::database`](#chapter_16_sub_configure_database)
 * [`doctrine`](#chapter_16_doctrine)
   * [`doctrine::build-all`](#chapter_16_sub_doctrine_build_all)
   * [`doctrine::build-all-load`](#chapter_16_sub_doctrine_build_all_load)
   * [`doctrine::build-all-reload`](#chapter_16_sub_doctrine_build_all_reload)
   * [`doctrine::build-all-reload-test-all`](#chapter_16_sub_doctrine_build_all_reload_test_all)
   * [`doctrine::build-db`](#chapter_16_sub_doctrine_build_db)
   * [`doctrine::build-filters`](#chapter_16_sub_doctrine_build_filters)
   * [`doctrine::build-forms`](#chapter_16_sub_doctrine_build_forms)
   * [`doctrine::build-model`](#chapter_16_sub_doctrine_build_model)
   * [`doctrine::build-schema`](#chapter_16_sub_doctrine_build_schema)
   * [`doctrine::build-sql`](#chapter_16_sub_doctrine_build_sql)
   * [`doctrine::data-dump`](#chapter_16_sub_doctrine_data_dump)
   * [`doctrine::data-load`](#chapter_16_sub_doctrine_data_load)
   * [`doctrine::dql`](#chapter_16_sub_doctrine_dql)
   * [`doctrine::drop-db`](#chapter_16_sub_doctrine_drop_db)
   * [`doctrine::generate-admin`](#chapter_16_sub_doctrine_generate_admin)
   * [`doctrine::generate-migration`](#chapter_16_sub_doctrine_generate_migration)
   * [`doctrine::generate-migrations-db`](#chapter_16_sub_doctrine_generate_migrations_db)
   * [`doctrine::generate-migrations-models`](#chapter_16_sub_doctrine_generate_migrations_models)
   * [`doctrine::generate-module`](#chapter_16_sub_doctrine_generate_module)
   * [`doctrine::generate-module-for-route`](#chapter_16_sub_doctrine_generate_module_for_route)
   * [`doctrine::insert-sql`](#chapter_16_sub_doctrine_insert_sql)
   * [`doctrine::migrate`](#chapter_16_sub_doctrine_migrate)
   * [`doctrine::rebuild-db`](#chapter_16_sub_doctrine_rebuild_db)
 * [`generate`](#chapter_16_generate)
   * [`generate::app`](#chapter_16_sub_generate_app)
   * [`generate::module`](#chapter_16_sub_generate_module)
   * [`generate::project`](#chapter_16_sub_generate_project)
   * [`generate::task`](#chapter_16_sub_generate_task)
 * [`i18n`](#chapter_16_i18n)
   * [`i18n::extract`](#chapter_16_sub_i18n_extract)
   * [`i18n::find`](#chapter_16_sub_i18n_find)
 * [`log`](#chapter_16_log)
   * [`log::clear`](#chapter_16_sub_log_clear)
   * [`log::rotate`](#chapter_16_sub_log_rotate)
 * [`plugin`](#chapter_16_plugin)
   * [`plugin::add-channel`](#chapter_16_sub_plugin_add_channel)
   * [`plugin::install`](#chapter_16_sub_plugin_install)
   * [`plugin::list`](#chapter_16_sub_plugin_list)
   * [`plugin::publish-assets`](#chapter_16_sub_plugin_publish_assets)
   * [`plugin::uninstall`](#chapter_16_sub_plugin_uninstall)
   * [`plugin::upgrade`](#chapter_16_sub_plugin_upgrade)
 * [`project`](#chapter_16_project)
   * [`project::clear-controllers`](#chapter_16_sub_project_clear_controllers)
   * [`project::deploy`](#chapter_16_sub_project_deploy)
   * [`project::disable`](#chapter_16_sub_project_disable)
   * [`project::enable`](#chapter_16_sub_project_enable)
   * [`project::freeze`](#chapter_16_sub_project_freeze)
   * [`project::permissions`](#chapter_16_sub_project_permissions)
   * [`project::unfreeze`](#chapter_16_sub_project_unfreeze)
   * [`project::upgrade1.1`](#chapter_16_sub_project_upgrade1_1)
   * [`project::upgrade1.2`](#chapter_16_sub_project_upgrade1_2)
 * [`propel`](#chapter_16_propel)
   * [`propel::build-all`](#chapter_16_sub_propel_build_all)
   * [`propel::build-all-load`](#chapter_16_sub_propel_build_all_load)
   * [`propel::build-filters`](#chapter_16_sub_propel_build_filters)
   * [`propel::build-forms`](#chapter_16_sub_propel_build_forms)
   * [`propel::build-model`](#chapter_16_sub_propel_build_model)
   * [`propel::build-schema`](#chapter_16_sub_propel_build_schema)
   * [`propel::build-sql`](#chapter_16_sub_propel_build_sql)
   * [`propel::data-dump`](#chapter_16_sub_propel_data_dump)
   * [`propel::data-load`](#chapter_16_sub_propel_data_load)
   * [`propel::generate-admin`](#chapter_16_sub_propel_generate_admin)
   * [`propel::generate-module`](#chapter_16_sub_propel_generate_module)
   * [`propel::generate-module-for-route`](#chapter_16_sub_propel_generate_module_for_route)
   * [`propel::graphviz`](#chapter_16_sub_propel_graphviz)
   * [`propel::init-admin`](#chapter_16_sub_propel_init_admin)
   * [`propel::insert-sql`](#chapter_16_sub_propel_insert_sql)
   * [`propel::schema-to-xml`](#chapter_16_sub_propel_schema_to_xml)
   * [`propel::schema-to-yml`](#chapter_16_sub_propel_schema_to_yml)
 * [`test`](#chapter_16_test)
   * [`test::all`](#chapter_16_sub_test_all)
   * [`test::coverage`](#chapter_16_sub_test_coverage)
   * [`test::functional`](#chapter_16_sub_test_functional)
   * [`test::unit`](#chapter_16_sub_test_unit)


<div class="pagebreak"></div>

### ~`help`~

`help`タスクはタスク用のヘルプメッセージを表示します:

    $ php symfony help  [task_name]

*エイリアス*: `h`

| 引数        | デフォルト| 説明
| ----------- | -------- | -----------
| `task_name` | `help`   | タスクの名前



### ~`list`~

`list`タスクはタスクの一覧を表示します:

    $ php symfony list  [namespace]



| 引数        | デフォルト | 説明
| ----------- | --------- | -----------
| `namespace` | `-`       | 名前空間の名前




`list`タスクはすべてのタスクの一覧を表示します:

    ./symfony list

特定の名前空間用のタスクを表示することもできます:

    ./symfony list test

`app`
-----

### ~`app::routes`~

`app::routes`タスクはアプリケーション用の現在のルートを表示します:

    $ php symfony app:routes  application [name]



| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `name`        | `-`       | ルートの名前




`app:routes`はアプリケーションの現在のルートを表示します:

    ./symfony app:routes frontend

`cache`
-------

### ~`cache::clear`~

`cache::clear`タスクはキャッシュをクリアします:

    $ php symfony cache:clear [--app[="..."]] [--env[="..."]] [--type[="..."]] 

*エイリアス*: `cc、clear-cache`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | ---
| `--app`                   | `-`        | アプリケーションの名前
| `--env`                   | `-`        | 環境
| `--type`                  | `all`      | 種類


`cache:clear`タスクはsymfonyのキャッシュをクリアします。

デフォルトでは、これはすべての利用可能な種類、すべてのアプリケーションと
すべての環境用のキャッシュを削除します。

種類、アプリケーションもしくは環境で制限できます:

たとえば、`frontend`アプリケーションのキャッシュをクリアするには:

    ./symfony cache:clear --app=frontend

`frontend`アプリケーションの`prod`環境用のキャッシュをクリアするには:

    ./symfony cache:clear --app=frontend --env=prod

すべての`prod`環境用のキャッシュをクリアするには:

    ./symfony cache:clear --env=prod

`prod`環境用の`config`キャッシュをクリアするには:

    ./symfony cache:clear --type=config --env=prod

組み込みの種類は次の通りです: `config`、`i18n`、`routing`、`module`
と`template`。


`configure`
-----------

### ~`configure::author`~

`configure::author`タスクはプロジェクトの著者を設定します:

    $ php symfony configure:author  author



| 引数     | デフォルト | 説明
| -------- | --------- | -----------
| `author` | `-`       | プロジェクトの著者




`configure:author`タスクはプロジェクトの著者を設定します:

    ./symfony configure:author "Fabien Potencier <fabien.potencier@symfony-project.com>"

それぞれの生成ファイルでPHPDocヘッダーをあらかじめ設定するために著者の名前はジェネレーターによって使われます。

値は[config/properties.ini]に保存されます。

### ~`configure::database`~

`configure::database`タスクはデータベースのDSNを設定します:

    $ php symfony configure:database [--env[="..."]] [--name[="..."]] [--class[="..."]] [--app[="..."]] dsn [username] [password]



| 引数       | デフォルト | 説明
| ---------- | --------- | -----------
| `dsn`      | `-`       | データベースのDSN
| `username` | `root`    | データベースのユーザー名
| `password` | `-`       | データベースのパスワード


| オプション(ショートカット)    | デフォルト          | 説明
| ---------------------------- | ------------------ | -----------
| `--env`                      | `all`              | 環境
| `--name`                     | `propel`           | 接続名
| `--class`                    | `sfPropelDatabase` | データベースクラスの名前
| `--app`                      | `-`                | アプリケーションの名前


`configure:database`タスクはプロジェクト用の
データベースのDSNを設定します:

    ./symfony configure:database mysql:host=localhost;dbname=example root mYsEcret

デフォルトでは、タスクはすべての環境用の設定を変更します。
特定の環境のためにDSNを変更したい場合、`env`オプションを使います:

    ./symfony configure:database --env=dev mysql:host=localhost;dbname=example_dev root mYsEcret

特定のアプリケーション用の設定を変更するには、`app`オプションを使います:

    ./symfony configure:database --app=frontend mysql:host=localhost;dbname=example root mYsEcret

接続名とデータベースのクラス名を指定することもできます:

    ./symfony configure:database --name=main --class=sfDoctrineDatabase mysql:host=localhost;dbname=example root mYsEcret

WARNING: `Propel`データベースを使い`app`なしで`all`環境用に設定する際に
`propel.ini`ファイルも更新されます。

`doctrine`
----------

### ~`doctrine::build-all`~

`doctrine::build-all`タスクはDoctrineモデル、SQLを生成しデータベースを初期化します:

    $ php symfony doctrine:build-all [--application[="..."]] [--env="..."] [--no-confirmation] [--skip-forms|-F] 

*エイリアス*: `doctrine-build-all`



| オプション(ショートカット)  | デフォルト | 説明
| -------------------------- | ---------- | -----------
| `--application`            | `1`        | アプリケーションの名前
| `--env`                    | `dev`      | 環境
| `--no-confirmation`        | `-`        | 確認の質問をしない
| `--skip-forms`<br />`(-F)` | `-`        | フォーム生成をスキップする


`doctrine:build-all`タスクは6つのタスクのショートカットです:

    ./symfony doctrine:build-all

タスクは次のものと同等です:

    ./symfony doctrine:build-db
    ./symfony doctrine:build-model
    ./symfony doctrine:build-sql
    ./symfony doctrine:build-forms
    ./symfony doctrine:build-filters
    ./symfony doctrine:insert-sql

詳細な情報はこれらのタスクのヘルプページを参照してください。

確認を回避するには、`no-confirmation`
オプションを渡します:

    ./symfony doctrine:buil-all-load --no-confirmation

### ~`doctrine::build-all-load`~

`doctrine::build-all-load`タスクはDoctrineモデル、SQLを生成し、データベースを初期化し、フィクスチャデータをロードします:

    $ php symfony doctrine:build-all-load [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--skip-forms|-F] [--dir="..."]

*エイリアス*: `doctrine-build-all-load`



| オプション(ショートカット)  | デフォルト  | 説明
| -------------------------- | ---------- | -----------
| `--application`            | `1`        | アプリケーションの名前
| `--env`                    | `dev`      | 環境
| `--connection`             | `doctrine` | 接続名
| `--no-confirmation`        | `-`        | 確認の質問をしない
| `--skip-forms`<br />`(-F)` | `-`        | フォーム生成をスキップする
| `--dir`                    | `-`        | フィクスチャを探すディレクトリ(複数の値が許可される)


`doctrine:build-all-load`タスクは7つのタスクのショートカットです:

    ./symfony doctrine:build-all-load

タスクは次のものと同等です:

    ./symfony doctrine:build-db
    ./symfony doctrine:build-model
    ./symfony doctrine:build-sql
    ./symfony doctrine:build-forms
    ./symfony doctrine:build-filters
    ./symfony doctrine:insert-sql
    ./symfony doctrine:data-load

`doctrine:data-load`タスクなのでアプリケーションの引数を受け取ります。
詳細な情報は`doctrine:data-load`のヘルプページを参照してください。

確認を回避するには、`no-confirmation`オプションを
渡します　:

    ./symfony doctrine:build-all-load --no-confirmation

### ~`doctrine::build-all-reload`~

`doctrine::build-all-reload`タスクはDoctrineモデル、SQLを生成し、データベースを初期化し、データをロードします:

    $ php symfony doctrine:build-all-reload [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--skip-forms|-F] [--dir="..."] 

*エイリアス*: `doctrine-build-all-reload`



| オプション(ショートカット)  | デフォルト | 説明
| -------------------------- | ---------- | -----------
| `--application`            | `1`        | アプリケーションの名前
| `--env`                    | `dev`      | 環境
| `--connection`             | `doctrine` | 接続名
| `--no-confirmation`        | `-`        | 確認をしない
| `--skip-forms`<br />`(-F)` | `-`        | フォーム生成をスキップする
| `--dir`                    | `-`        | フィクスチャを探すディレクトリ(複数の値が許可される)


`doctrine:build-all-reload`タスクは8つのタスクのショートカットです:

    ./symfony doctrine:build-all-reload

タスクは次のものと同等です:

    ./symfony doctrine:drop-db
    ./symfony doctrine:build-db
    ./symfony doctrine:build-model
    ./symfony doctrine:build-sql
    ./symfony doctrine:build-forms
    ./symfony doctrine:build-filters
    ./symfony doctrine:insert-sql
    ./symfony doctrine:data-load

### ~`doctrine::build-all-reload-test-all`~

`doctrine::build-all-reload-test-all`タスクはDoctrineモデル、SQLを生成し、データベースを初期化し、データをロードしてすべてのテストを実施します:

    $ php symfony doctrine:build-all-reload-test-all [--application[="..."]] [--env="..."] [--append] [--dir="..."] [--force] 

*エイリアス*: `doctrine-build-all-reload-test-all`



| オプション(ショートカット)   | デフォルト | 説明
| --------------------------- | --------- | -----------
| `--application`             | `1`       | アプリケーションの名前
| `--env`                     | `dev`     | 環境
| `--append`                  | `-`       | データベースの現在の値を削除しない
| `--dir`                     | `-`       | フィクスチャを探すディレクトリ(複数の値が許可される)
| `--force`                   | `-`       | データベースの削除を強制するか


`doctrine:build-all-reload`タスクは9つのタスクのショートカットです:

    ./symfony doctrine:build-all-reload-test-all frontend

タスクは次のものと同等です:
  
    ./symfony doctrine:drop-db
    ./symfony doctrine:build-db
    ./symfony doctrine:build-model
    ./symfony doctrine:build-sql
    ./symfony doctrine:build-forms
    ./symfony doctrine:build-filters
    ./symfony doctrine:insert-sql
    ./symfony doctrine:data-load
    ./symfony test-all

`doctrine:data-load`タスクなのでアプリケーションの引数を受け取ります。
詳細な情報は`doctrine:data-load`のヘルプページを参照してください。

### ~`doctrine::build-db`~

`doctrine::build-db`タスクは現在のモデル用のデータベースを作成します:

    $ php symfony doctrine:build-db [--application[="..."]] [--env="..."] 

*エイリアス*: `doctrine-build-db`



| オプション(ショートカット)| デフォルト | 説明
| ------------------------ | --------- | -----------
| `--application`          | `1`       | アプリケーションの名前
| `--env`                  | `dev`     | 環境


`doctrine:build-db`タスクはデータベースを作成します:

    ./symfony doctrine:build-db

タスクは`config/doctrine/databases.yml`の接続情報を読み込みます:

### ~`doctrine::build-filters`~

`doctrine::build-filters`タスクは現在のモデル用のフィルターフォームクラスを作成します:

    $ php symfony doctrine:build-filters [--connection="..."] [--model-dir-name="..."] [--filter-dir-name="..."] [--application[="..."]] [--env="..."] 





| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--connection`            | `doctrine` | 接続名
| `--model-dir-name`        | `model`    | モデルディレクトリの名前
| `--filter-dir-name`       | `filter`   | フィルターフォームディレクトリの名前
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `dev`      | 環境


`doctrine:build-filters`タスクはスキーマからフィルターフォームクラスを作成します:

    ./symfony doctrine:build-filters

タスクはプロジェクトとすべてのインストールしたプラグインから`config/*schema.xml` かつ/または
`config/*schema.yml`のスキーマ情報を読み込みます。

タスクは`config/databases.yml`で定義された`doctrine`接続を使います。
`--connection`オプションを指定することで別の接続を使うことができます:

    ./symfony doctrine:build-filters --connection="name"

モデルフィルターフォームクラスのファイルは`lib/filter`で作成されます。

このタスクは`lib/filter`のカスタムタスクをオーバーライドしません。
これは`lib/filter/base`で生成された基底クラスのみを置き換えます。

### ~`doctrine::build-forms`~

`doctrine::build-forms`タスクは現在のモデル用のフォームクラスを作成します:

    $ php symfony doctrine:build-forms [--connection="..."] [--model-dir-name="..."] [--form-dir-name="..."] [--application[="..."]] [--env="..."] 





| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--connection`            | `doctrine` | 接続名
| `--model-dir-name`        | `model`    | モデルディレクトリの名前
| `--form-dir-name`         | `form`     | フォームディレクトリの名前
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `dev`      | 環境


`doctrine:build-forms`タスクはスキーマからフォームクラスを作成します:

    ./symfony doctrine:build-forms

タスクは`プロジェクトとすべてのインストールしたプラグインからconfig/*schema.xml`かつ/もしくは
`config/*schema.yml`のスキーマ情報を読み取ります。

タスクは`config/databases.yml`で定義された`doctrine`接続を使います。
`--connection`オプションを指定することで別の接続を使うことができます:

    ./symfony doctrine:build-forms --connection="name"

モデルフォームクラスのファイルは`lib/form`で作成されます。

このタスクは`lib/form`のカスタムクラスをけっしてオーバーライドしません。
これは`lib/form/base`で生成された基底クラスのみを置き換えます。

### ~`doctrine::build-model`~

`doctrine::build-model`タスクは現在のモデル用のクラスを作成します:

    $ php symfony doctrine:build-model [--application[="..."]] [--env="..."] 

*エイリアス*: `doctrine-build-model`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--application`           | `1`       | アプリケーションの名前
| `--env`                   | `dev`     | 環境


`doctrine:build-model`タスクはスキーマからモデルクラスを作成します:

    ./symfony doctrine:build-model

タスクはプロジェクトとインストールしたすべてのプラグインから
`config/doctrine/*.yml`のスキーマ情報を読み込みます。

モデルクラスファイルは`lib/model/doctrine`で作成されます。

このタスクは`lib/model/doctrine`のカスタムタスクをけして上書きしません。
これは`lib/model/doctrine/base`のファイルのみを置き換えます。

### ~`doctrine::build-schema`~

`doctrine::build-schema` タスクは既存のデータベースからスキーマを作成します:

    $ php symfony doctrine:build-schema [--application[="..."]] [--env="..."] 

*エイリアス*: `doctrine-build-schema`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--application`           | `1`       | アプリケーションの名前
| `--env`                   | `dev`     | 環境


`doctrine:build-schema`タスクはスキーマを作成するデータベースをイントロスペクトします:

    ./symfony doctrine:build-schema

タスクは`config/doctrine`にYAMLファイルを作成します。

### ~`doctrine::build-sql`~

`doctrine::build-sql`タスクは現在のモデル用のSQLを作成します:

    $ php symfony doctrine:build-sql [--application[="..."]] [--env="..."] 

*エイリアス*: `doctrine-build-sql`



| オプション(ショートカット)   | デフォルト | 説明
| --------------------------- | --------- | -----------
| `--application`             | `1`       | アプリケーションの名前
| `--env`                     | `dev`     | 環境


`doctrine:build-sql`タスクはテーブル作成用のSQLステートメントを作成します:

    ./symfony doctrine:build-sql

生成SQLは`config/databases.yml`で設定されたデータベース用に最適化されます:

    doctrine.database = mysql

### ~`doctrine::data-dump`~

`doctrine::data-dump`タスクはフィクスチャディレクトリにデータをダンプします:

    $ php symfony doctrine:data-dump [--application[="..."]] [--env="..."] [target]

*エイリアス*: `doctrine-dump-data`

| 引数     | デフォルト | 説明
| -------- | --------- | -----------
| `target` | `-`       | ターゲットのファイル名


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--application`           | `1`       | アプリケーションの名前
| `--env`                   | `dev`     | 環境


`doctrine:data-dump`タスクはデータベースデータをダンプします:

    ./symfony doctrine:data-dump

タスクは`data/fixtures/%target%`にデータベースのデータをダンプします。

ダンプファイルはYAMLフォーマットで
`doctrine:data-load`タスクを使用して再インポートできます。

    ./symfony doctrine:data-load frontend

### ~`doctrine::data-load`~

`doctrine::data-load`タスクはフィクスチャディレクトリからデータをロードします:

    $ php symfony doctrine:data-load [--application[="..."]] [--env="..."] [--append] [--connection="..."] [--dir="..."] 

*エイリアス*: `doctrine-load-data`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----
| `--application`           | `1`       | アプリケーションの名前
| `--env`                   | `dev`     | 環境
| `--append`                | `-`       | データベースの現在の値を削除しない
| `--connection`            | `doctrine`| 接続名
| `--dir`                   | `-`       | フィクスチャを探すディレクトリ(複数の値が許可される)


`doctrine:data-load`タスクはデータフィクスチャをデータベースにロードします:

    ./symfony doctrine:data-load frontend

タスクは`data/fixtures/`で見つかるすべてのファイルからデータをロードします。

他のディレクトリからデータをロードしたい場合、
`--dir`オプションを使うことができます:

    ./symfony doctrine:data-load --dir="data/fixtures" --dir="data/data" frontend

タスクにデータベースの既存のデータを削除させたくない場合、
`--append`オプションを使います:

    ./symfony doctrine:data-load --append frontend

### ~`doctrine::dql`~

`doctrine::dql`タスクはDQLクエリを実行し結果を表示します:

    $ php symfony doctrine:dql [--application[="..."]] [--env="..."] [--show-sql] dql_query

*エイリアス*: `doctrine-dql`

| 引数        | デフォルト | 説明
| ----------- | --------- | -----------
| `dql_query` | `-`       | 実行するDQLクエリ


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--application`           | `1`       | アプリケーションの名前
| `--env`                   | `dev`     | 環境
| `--show-sql`              | `-`       | 実行されるSQLを表示する


`doctrine:data-dql`タスクはDQLクエリを実行しフォーマットされた結果を表示します:

    ./symfony doctrine:dql "FROM User u"

`--dir`オプションを使用して実行されるSQLを表示できます:

    ./symfony doctrine:dql --show-sql "FROM User u"

### ~`doctrine::drop-db`~

`doctrine::drop-db`タスクは現在のモデル用のデータベースを削除します:

    $ php symfony doctrine:drop-db [--application[="..."]] [--env="..."] [--no-confirmation] 

*エイリアス*: `doctrine-drop-db`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | ----
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `dev`      | 環境
| `--no-confirmation`       | `-`        | データベースの削除を強制するか


`doctrine:drop-db`タスクはデータベースをドロップします:

    ./symfony doctrine:drop-db

タスクは`config/doctrine/databases.yml`の接続情報を読み込みます:

### ~`doctrine::generate-admin`~

`doctrine::generate-admin`タスクはDoctrineのadminモジュールを生成します:

    $ php symfony doctrine:generate-admin [--module="..."] [--theme="..."] [--singular="..."] [--plural="..."] [--env="..."] application route_or_model



| 引数             | デフォルト | 説明
| ---------------- | ---------- | -----------
| `application`    | `-`        | アプリケーションの名前
| `route_or_model` | `-`        | ルートの名前もしくはモデルクラス


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--module`                | `-`       | モジュールの名前
| `--theme`                 | `admin`   | テーマの名前
| `--singular`              | `-`       | 単数形の名前
| `--plural`                | `-`       | 複数形の名前
| `--env`                   | `dev`     | 環境


`doctrine:generate-admin`タスクはDoctrineのadminモジュールを生成します:

    ./symfony doctrine:generate-admin frontend Article

タスクは`%Article%`モデル用の`%frontend%`アプリケーションで
モジュールを作成します。

タスクはアプリケーションの`routing.yml`用のルートを作成します。

ルートの名前を渡すことでDoctrineのadminモジュールを生成することもできます:

    ./symfony doctrine:generate-admin frontend article

タスクは`routing.yml`で見つかる`%article%`ルート用の`%frontend%`アプリケーションで
モジュールを作成します。

フィルターとバッチアクションを適切に動作させるために、
ルートに`wildcard`オプションを追加する必要があります:

    article:
      class: sfDoctrineRouteCollection
      options:
        model:              Article
        with_wildcard_routes:   true

### ~`doctrine::generate-migration`~

`doctrine::generate-migration`タスクはマイグレーションクラスを生成します:

    $ php symfony doctrine:generate-migration [--application[="..."]] [--env="..."] name

*エイリアス*: `doctrine-generate-migration`

| 引数     | デフォルト | 説明
| -------- | --------- | -----------
| `name`   | `-`       | マイグレーションの名前


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--application`           | `1`       | アプリケーションの名前
| `--env`                   | `dev`     | 環境


`doctrine:generate-migration`タスクはマイグレーションテンプレートを生成します。

    ./symfony doctrine:generate-migration

### ~`doctrine::generate-migrations-db`~

`doctrine::generate-migrations-db`タスクは既存のデータベース接続からマイグレーションクラスを生成します:

    $ php symfony doctrine:generate-migrations-db [--application[="..."]] [--env="..."] 

*エイリアス*: `doctrine-generate-migrations-db`、`doctrine-gen-migrations-from-db`



| オプション(ショートカット) | デフォルト  | 説明
| ------------------------- | ---------- | -----------
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `dev`      | 環境


`doctrine:generate-migration`タスクは既存のデータベース接続からマイグレーションクラスを生成します。

    ./symfony doctrine:generate-migration

### ~`doctrine::generate-migrations-models`~

`doctrine::generate-migrations-models`タスクは既存のモデルのセットからマイグレーションクラスを生成します:

    $ php symfony doctrine:generate-migrations-models [--application[="..."]] [--env="..."] 

*エイリアス*: `doctrine-generate-migrations-models`、`doctrine-gen-migrations-from-models`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `dev`      | 環境


`doctrine:generate-migration`タスクは既存のモデルのセットからマイグレーションクラスを生成します。

    ./symfony doctrine:generate-migration

### ~`doctrine::generate-module`~

`doctrine::generate-module`タスクはDoctrineモジュールを生成します:

    $ php symfony doctrine:generate-module [--theme="..."] [--generate-in-cache] [--non-verbose-templates] [--with-show] [--singular="..."] [--plural="..."] [--route-prefix="..."] [--with-doctrine-route] [--env="..."] application module model

*エイリアス*: `doctrine-generate-crud`、`doctrine:generate-crud`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `module`      | `-`       | モジュールの名前
| `model`       | `-`       | モデルクラスの名前


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--theme`                 | `default` | テーマの名前
| `--generate-in-cache`     | `-`       | キャッシュでモジュールを生成する
| `--non-verbose-templates` | `-`       | 冗長ではないテンプレートを生成する
| `--with-show`             | `-`       | showメソッドを生成する
| `--singular`              | `-`       | 単数形の名前
| `--plural`                | `-`       | 複数形の名前
| `--route-prefix`          | `-`       | ルートの接頭辞
| `--with-doctrine-route`   | `-`       | Doctrineのルートを使うかどうか
| `--env`                   | `dev`     | 環境


`doctrine:generate-module`タスクはDoctrineのモジュールを生成します:

    ./symfony doctrine:generate-module frontend article Article

モデルクラスの`%model%`用の`%application%`アプリケーションで
`%module%`モジュールを作成します。

`--generate-in-cache`オプションを指定することで`%sf_app_cache_dir%/modules/auto%module%`で
実行時に生成されたモジュールからアクションとテンプレートを継承する空のモジュールを作成することもできます:

    ./symfony doctrine:generate-module --generate-in-cache frontend article Article

`--theme`オプションを指定することでジェネレーターはカスタマイズされたテーマを使うことができます:

    ./symfony doctrine:generate-module --theme="custom" frontend article Article

この方法では、独自仕様に合わせてモジュールジェネレーターを作成できます。

### ~`doctrine::generate-module-for-route`~

`doctrine::generate-module-for-route`タスクはルートの定義用のDoctrineモジュールを生成します:

    $ php symfony doctrine:generate-module-for-route [--theme="..."] [--non-verbose-templates] [--singular="..."] [--plural="..."] [--env="..."] application route



| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `route`       | `-`       | ルートの名前


| オプション(ショートカット)     | デフォルト | 説明
| ----------------------------- | --------- | -----------
| `--theme`                     | `default` | テーマの名前
| `--non-verbose-templates`     | `-`       | 冗長ではないテンプレートを生成する
| `--singular`                  | `-`       | 単数形の名前
| `--plural`                    | `-`       | 複数形の複数形の名前
| `--env`                       | `dev`     | 環境


`doctrine:generate-module-for-route`タスクはルートの定義用のDoctrineモジュールを生成します:

    ./symfony doctrine:generate-module-for-route frontend article

タスクは`routing.yml`で見つかる`%article%`のルート定義用の`%frontend%`アプリケーションで
モジュールを作成します。

### ~`doctrine::insert-sql`~

`doctrine::insert-sql`タスクは現在のモデルにSQLをinsertします:

    $ php symfony doctrine:insert-sql [--application[="..."]] [--env="..."] 

*エイリアス*: `doctrine-insert-sql`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `dev`      | 環境


`doctrine:insert-sql`タスクはデータベースのテーブルを作成します:

    ./symfony doctrine:insert-sql

タスクはデータベースに接続しすべての`lib/model/doctrine/*.php`ファイル用の
テーブルを作成します。

### ~`doctrine::migrate`~

`doctrine::migrate`タスクは現在/指定されたバージョンのデータベースにマイグレートします:

    $ php symfony doctrine:migrate [--application[="..."]] [--env="..."] [version]

*エイリアス*: `doctrine-migrate`

| 引数      | デフォルト | 説明
| --------- | --------- | -----------
| `version` | `-`       | マイグレートするバージョン


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--application`           | `1`       | アプリケーションの名前
| `--env`                   | `dev`     | 環境


`doctrine:migrate`タスクは現在/指定されたバージョンにデータベースをマイグレートします。

   ./symfony doctrine:migrate

### ~`doctrine::rebuild-db`~

`doctrine::rebuild-db`タスクは現在のモデル用のデータベースを作成します:

    $ php symfony doctrine:rebuild-db [--application[="..."]] [--env="..."] [--no-confirmation] 

*エイリアス*: `doctrine-rebuild-db`



| オプション(ショートカット)   | デフォルト | 説明
| --------------------------- | --------- | -----------
| `--application`             | `1`       | アプリケーションの名前
| `--env`                     | `dev`     | 環境
| `--no-confirmation`         | `-`       | 確認をせずにデータベースをドロップするか


`doctrine:rebuild-db`タスクはデータベースを作成します:

    ./symfony doctrine:rebuild-db

タスクは`config/doctrine/databases.yml`の接続情報を読み込みます:

`generate`
----------

### ~`generate::app`~

`generate::app`タスクは新しいアプリケーションを生成します:

    $ php symfony generate:app [--escaping-strategy="..."] [--csrf-secret="..."] application

*エイリアス*: `init-app`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--escaping-strategy`     | ``        | エスケーピング戦略を出力する
| `--csrf-secret`           | ``        | CSRF保護に使う秘密の文字列


`generate:app`タスクは現在のプロジェクトの新しいアプリケーションのための
基本的なディレクトリ構造を作成します:

    ./symfony generate:app frontend

このタスクは`web/`ディレクトリに
2つのフロントコントローラーも作成します:

    web/%application%.php`     運用環境用
    web/%application%_dev.php` 開発環境用

最初のアプリケーションに関しては、運用環境のスクリプトの名前は
`index.php`です。

同じ名前のアプリケーションがすでに存在する場合、
これは`sfCommandException`を投げます。

(XSSを防止するために)`escaping-strategy`オプションを指定することで出力エスケーピングを有効にできます:

    ./symfony generate:app frontend --escaping-strategy=on

(CSRFを防止するために)`csrf-secret`オプションで秘密の文字列を定義することで
フォームのセッショントークンを有効にできます:

    ./symfony generate:app frontend --csrf-secret=UniqueSecret


### ~`generate::module`~

`generate::module`タスクは新しいモジュールを生成します:

    $ php symfony generate:module  application module

*エイリアス*: `init-module`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `module`      | `-`       | モジュールの名前




`generate:module`タスクは既存のアプリケーションの
新しいモジュール用の基本ディレクトリ構造を作成します:

    ./symfony generate:module frontend article

`config/properties.ini`で設定した場合
タスクは`actions.class.php`で見つかる著者の名前を変更することもできます:

    [symfony]
      name=blog
      author=Fabien Potencier <fabien.potencier@sensio.com>

`%sf_data_dir%/skeleton/module`ディレクトリを作成することで
タスクによって使われるデフォルトのスケルトンをカスタマイズできます。

`%sf_test_dir%/functional/%application%/%module%ActionsTest.class.php`
という名前の機能テストのスタブも作成します。これはデフォルトではパスしません。

アプリケーションに同じ名前のモジュールがすでにある場合、
`sfCommandException`を投げます。

### ~`generate::project`~

`generate::project`タスクは新しいプロジェクトを生成します:

    $ php symfony generate:project  name

*エイリアス*: `init-project`

| 引数     | デフォルト | 説明
| -------- | --------- | -----------
| `name`   | `-`       | プロジェクトの名前




`generate:project`タスクは現在のディレクトリで
新しいプロジェクト用の基本ディレクトリ構造を作成します:

    ./symfony generate:project blog

現在のディレクトリがすでにsymfonyのプロジェクトを含む場合、
これは`sfCommandException`を投げます。

### ~`generate::task`~

`generate::task`タスクは新しいタスク用のスケルトンクラスを作成します:

    $ php symfony generate:task [--dir="..."] [--use-database="..."] [--brief-description="..."] task_name



| 引数        | デフォルト | 説明
| ----------- | --------- | -----------
| `task_name` | `-`       | タスクの名前(名前空間を含むことができる)


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--dir`                   | `lib/task` | タスクを作成するディレクトリ
| `--use-database`          | `propel`   | タスクがデータベースにアクセスするモデルの初期化を必要とするか
| `--brief-description`     | `-`        | 短いタスクの説明(タスクのリストに表示される)


`generate:task`は引数として渡された名前に基づいて
新しい`sfTask`クラスを作成します:

    ./symfony generate:task namespace:name

`namespaceNameTask.class.php`スケルトンタスクは
`lib/task/`ディレクトリの元で作成されます。名前空間はオプションであることに留意してください。

別のディレクトリ(プロジェクトのrootフォルダに相対的)でファイルを作成したい場合、
これを`--dir`オプションに渡します。このディレクトリはまだ存在しなければ作成されます。

    ./symfony generate:task namespace:name --dir=plugins/myPlugin/lib/task

デフォルトの`propel`以外の接続を使いたい場合、
`--use-database`オプションでこの接続の名前を提供します:

    ./symfony generate:task namespace:name --use-database=main

`--use-database`オプションは生成されたタスクでデータベースの初期化を
無効化するためにも使うことができます:

    ./symfony generate:task namespace:name --use-database=false

説明を指定することもできます:

    ./symfony generate:task namespace:name --brief-description="Does interesting things"

`i18n`
------

### ~`i18n::extract`~

`i18n::extract`タスクはPHPファイルから国際化された文字列を抽出します:

    $ php symfony i18n:extract [--display-new] [--display-old] [--auto-save] [--auto-delete] application culture



| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `culture`     | `-`       | ターゲットのculture


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--display-new`           | `-`        | 新しく見つかるすべての文字列を出力する
| `--display-old`           | `-`        | すべての古い文字列を出力する
| `--auto-save`             | `-`        | 新しい文字列を保存する
| `--auto-delete`           | `-`        | 古い文字列を削除します


`i18n:extract`タスクはアプリケーションとターゲットのculture用の
プロジェクトファイルから国際化文字列を抽出します:

    ./symfony i18n:extract frontend fr

デフォルトでは、タスクは現在のプロジェクトで見つかる
新旧の文字列の数のみを表示します。

新しい文字列を表示したい場合、`--display-new`オプションを使います:

    ./symfony i18n:extract --display-new frontend fr

i18nメッセージカタログでこれらを保存するには、`--auto-save`オプションを使います:

    ./symfony i18n:extract --auto-save frontend fr

国際化メッセージカタログで見つかるが
アプリケーションで見つからない文字列を表示したい場合、 
`--display-old`オプションを使います:

    ./symfony i18n:extract --display-old frontend fr

古い文字列を自動的に削除するには、`--auto-delete`を使いますが、
とりわけプラグインの翻訳がある場合、これらは現在のではなく
古い文字列として現れるので、注意してください:

    ./symfony i18n:extract --auto-delete frontend fr

### ~`i18n::find`~

`i18n::find`タスクはアプリケーションで"i18n ready"ではない文字列を見つけます:

    $ php symfony i18n:find [--env="..."] application



| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--env`                   | `dev`     | 環境


`i18n:find`タスクはテンプレートに埋め込まれる非国際化文字列を見つけます:

    ./symfony i18n:find frontend

このタスクは純粋なHTMLとPHPコードで非国際化文字列を見つけることができます:

    <p>Non i18n text</p>
    <p><?php echo 'Test' ?></p>

タスクはPHPに埋め込まれたすべての文字列を返しますが、誤検出(false positive)がある場合があります
(とりわけヘルパーの引数用の文字列構文を使う場合)。

`log`
-----

### ~`log::clear`~

`log::clear`タスクはログファイルをクリアします:

    $ php symfony log:clear  

*エイリアス*: `log-purge`





`log:clear`タスクはすべてのsymfonyログをクリアします:

    ./symfony log:clear

### ~`log::rotate`~

`log::rotate`タスクはアプリケーションのログファイルのローテーションを行います:

    $ php symfony log:rotate [--history="..."] [--period="..."] application env

*エイリアス*: `log-rotate`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `env`         | `-`       | 環境名


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--history`               | `10`      | 維持する古いログファイルの最大数
| `--period`                | `7`       | 日にち単位の期間


`log:rotate`タスクは与えられた環境用のアプリケーションの
ログファイルのローテーションを行います:

    ./symfony log:rotate frontend dev

`period`もしくは`history`オプションを指定できます:

    ./symfony --history=10 --period=7 log:rotate frontend dev

`plugin`
--------

### ~`plugin::add-channel`~

`plugin::add-channel`タスクは新しいPEARチャンネルを追加します:

    $ php symfony plugin:add-channel  name



| 引数     | デフォルト | 説明
| -------- | --------- | -----------
| `name`   | `-`       | チャンネル名




`plugin:add-channel`タスクは新しいPEARチャンネルを追加します:

    ./symfony plugin:add-channel symfony.plugins.pear.example.com

### ~`plugin::install`~

`plugin::install`タスクはプラグインをインストールします:

    $ php symfony plugin:install [--stability|-s="..."] [--release|-r="..."] [--channel|-c="..."] [--install_deps|-d] [--force-license] name

*エイリアス*: `plugin-install`

| 引数     | デフォルト | 説明
| -------- | --------- | ----------------
| `name`   | `-`       | プラグインの名前


| オプション(ショートカット)    | デフォルト | 説明
| ---------------------------- | ---------- | -----------
| `--stability`<br />`(-s)`    | `-`        | 安定性(stable、beta、alpha)
| `--release`<br />`(-r)`      | `-`        | 優先バージョン
| `--channel`<br />`(-c)`      | `-`        | PEARチャンネルの名前
| `--install_deps`<br />`(-d)` | `-`        | 必須の依存パッケージのインストールを強制するか
| `--force-license`            | `-`        | MITのようなライセンスではない場合にインストールを強制するか


`plugin:install`タスクはプラグインをインストールします:

    ./symfony plugin:install sfGuardPlugin

デフォルトでは、これは最新の`stable`リリースをインストールします。

まだ安定版ではないプラグインをインストールしたい場合、
`stability`オプションを使います:

    ./symfony plugin:install --stability=beta sfGuardPlugin
    ./symfony plugin:install -s beta sfGuardPlugin

特定のバージョンのインストールを強制することもできます:

    ./symfony plugin:install --release=1.0.0 sfGuardPlugin
    ./symfony plugin:install -r 1.0.0 sfGuardPlugin

依存の必須パッケージをすべて強制的にインストールするには、`install_deps`フラグを使います:

    ./symfony plugin:install --install-deps sfGuardPlugin
    ./symfony plugin:install -d sfGuardPlugin

デフォルトでは、使用されるPEARチャンネルは`symfony-plugins`
(plugins.symfony-project.org)です。

`channel`オプションで別のチャンネルを指定できます:

    ./symfony plugin:install --channel=mypearchannel sfGuardPlugin
    ./symfony plugin:install -c mypearchannel sfGuardPlugin

WebサイトでホストされているPEARパッケージをインストールすることもできます:

    ./symfony plugin:install http://somewhere.example.com/sfGuardPlugin-1.0.0.tgz

もしくはローカルなPEARパッケージ:

    ./symfony plugin:install /home/fabien/plugins/sfGuardPlugin-1.0.0.tgz

プラグインがWebコンテンツ(画像、スタイルシートもしくはJavaScript)を収納する場合、
タスクはこれらのアセットのために`web/`の元で`%name%`シンボリックリンクを作成します。
Windowsでは、タスクはこれらすべてのファイルを`web/%name%`ディレクトリにコピーします。

### ~`plugin::list`~

`plugin::list`タスクはインストールされたプラグインの一覧を表示します:

    $ php symfony plugin:list  

*エイリアス*: `plugin-list`





`plugin:list`タスクはインストールされたプラグインの一覧を表示します:

    ./symfony plugin:list

これはそれぞれのプラグインのチャンネルとバージョンも表示します。

### ~`plugin::publish-assets`~

`plugin::publish-assets`タスクはすべてのプラグイン用のWebアセットを公開します:

    $ php symfony plugin:publish-assets [--core-only] [--symfony-lib-dir="..."] 





| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--core-only`             | `-`       | セットされている場合コアプラグインのみがアセットを公開します
| `--symfony-lib-dir`       | `-`       | symfonyのlibディレクトリ


`plugin:publish-assets`タスクはすべてのプラグインからWebアセットを公開します。

    ./symfony plugin:publish-assets

実際これはそれぞれのプラグインに`plugin.post_install`イベントを送信します。


### ~`plugin::uninstall`~

`plugin::uninstall`タスクはプラグインをアンインストールします:

    $ php symfony plugin:uninstall [--channel|-c="..."] [--install_deps|-d] name

*エイリアス*: `plugin-uninstall`

| 引数     | デフォルト | 説明
| -------- | ---- ----- | -----------
| `name`   | `-`        | プラグインの名前


| オプション(ショートカット)    | デフォルト | 説明
| ---------------------------- | ---------- | -----------
| `--channel`<br />`(-c)`      | `-`        | PEARチャンネル名
| `--install_deps`<br />`(-d)` | `-`        | 依存パッケージをインストールするかどうか


`plugin:uninstall`タスクはプラグインをアンインストールします:

    ./symfony plugin:uninstall sfGuardPlugin

デフォルトのチャンネルは`symfony`です。

異なるチャンネルを持つプラグインをアンインストールすることもできます:

    ./symfony plugin:uninstall --channel=mypearchannel sfGuardPlugin

    ./symfony plugin:uninstall -c mypearchannel sfGuardPlugin

もしくは`channel/package`の表記を使うことができます:

    ./symfony plugin:uninstall mypearchannel/sfGuardPlugin

`plugin:list`タスクを立ち上げて
プラグインのPEARチャンネルを取得できます。

プラグインがWebコンテンツを格納する場合(画像、スタイルシートもしくはJavaScript)、
タスクは`web/%name%`シンボリックリンク(Unix系)
もしくはディレクトリ(Windows)も削除します。

### ~`plugin::upgrade`~

`plugin::upgrade`タスクはプラグインをアップグレードします:

    $ php symfony plugin:upgrade [--stability|-s="..."] [--release|-r="..."] [--channel|-c="..."] name

*エイリアス*: `plugin-upgrade`

| 引数     | デフォルト | 説明
| -------- | --------- | -----------
| `name`   | `-`       | プラグインの名前


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--stability`<br />`(-s)` | `-`       | 安定性(stable、beta、alpha)
| `--release`<br />`(-r)`   | `-`       | 優先バージョン
| `--channel`<br />`(-c)`   | `-`       | PEARチャンネルの名前


`plugin:upgrade`タスクはプラグインをアップグレードしようとします:

    ./symfony plugin:upgrade sfGuardPlugin

デフォルトのチャンネルは`symfony`です。

プラグインがWebコンテンツを格納する場合(画像、スタイルシートもしくはJavaScript)、
Windowsではタスクは`web/%name%`ディレクトリのコンテンツもアップグレードします。

プラグインの名前とオプションのフォーマットの詳細情報に関しては`plugin:install`を参照してください。

`project`
---------

### ~`project::clear-controllers`~

`project::clear-controllers`タスクは運用環境以外のコントローラーをクリアします:

    $ php symfony project:clear-controllers  

*エイリアス*: `clear-controllers`





`project:clear-controllers`タスクは運用環境以外の
コントローラーをクリアします:

    ./symfony project:clear-controllers

運用環境以外のすべてのフロントコントローラーを削除するために
運用サーバーでこのタスクを使うことができます。

`frontend`と`backend`という名前の2つのアプリケーションがある場合、
`web/`にはデフォルトのコントローラーは4つあります:

    index.php
    frontend_dev.php
    backend.php
    backend_dev.php

`project:clear-controllers`タスクを実行した後で、
`web/`には2つのフロントコントローラーが残ります:

    index.php
    backend.php

デバッグモードとWebデバッグツールバーが無効なので
これら2つのコントローラーは安全です。

### ~`project:deploy`~

`project:deploy`タスクはプロジェクトを別のサーバーにデプロイします:

    $ php symfony project:deploy [--go] [--rsync-dir="..."] [--rsync-options[="..."]] server

*エイリアス*: `sync`

| 引数     | デフォルト | 説明
| -------- | --------- | -----------
| `server` | `-`       | サーバーの名前


| オプション(ショートカット) | デフォルト               | 説明
| ------------------------- | ----------------------- | -----------
| `--go`                    | `-`                     | デプロイを行う
| `--rsync-dir`             | `config`                | rsync*.txtファイルを探すディレクトリ
| `--rsync-options`         | `-azC --force --delete` | rsync実行ファイルに渡すオプション


`project:deploy`タスクはプロジェクトをサーバーにデプロイします:

    ./symfony project:deploy production

サーバーは`config/properties.ini`で設定しなければなりません:

    [production]
      host=www.example.com
      port=22
      user=fabien
      dir=/var/www/sfblog/
      type=rsync

デプロイを自動化するために、タスクはSSH越しにrsyncを使います。
キーでSSHのアクセス権限を設定するか
`config/properties.ini`でパスワードを設定しなければなりません。

デフォルトでは、タスクはdry-modeです。本当にデプロイを行うには、
`--go`オプションを渡さなければなりません:

    ./symfony project:deploy --go production

`config/rsync_exclude.txt`で設定されたファイルとディレクトリは
デプロイされません:

    .svn
    /web/uploads/*
    /cache/*
    /log/*

`rsync.txt`と`rsync_include.txt`ファイルも作成できます。

サーバーに基づいて`rsync*.txt`ファイルをカスタマイズする必要がある場合、
`rsync-dir`オプションを渡すことができます:

    ./symfony project:deploy --go --rsync-dir=config/production production

最後に、`rsync-options`オプションを使うことで、
rsync実行ファイルに渡されるオプションを指定することができます(デフォルトは`-azC`):

    ./symfony project:deploy --go --rsync-options=avz

### ~`project::disable`~

`project::disable`タスクは与えられた環境のアプリケーションを無効にします:

    $ php symfony project:disable  application env

*エイリアス*: `disable`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `env`         | `-`       | 環境名




`project:disable`タスクは特定の環境のアプリケーションを無効にします:

    ./symfony project:disable frontend prod

### ~`project::enable`~

`project::enable`タスクは与えられた環境のアプリケーションを有効にします:

    $ php symfony project:enable  application env

*エイリアス*: `enable`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `env`         | `-`       | 環境名




`project:enable`タスクは特定の環境のアプリケーションを有効にします:

    ./symfony project:enable frontend prod

### ~`project::freeze`~

`project::freeze`タスクはsymfonyライブラリを凍結します:

    $ php symfony project:freeze  symfony_data_dir

*エイリアス*: `freeze`

| 引数               | デフォルト | 説明
| ------------------ | --------- | -----------
| `symfony_data_dir` | `-`       | symfonyのデータディレクトリ




`project:freeze`タスクはsymfonyのすべてのコアファイルを
現在のプロジェクトにコピーします:

    ./symfony project:freeze /path/to/symfony/data/directory

タスクは必須の引数としてsymfonyのデータディレクトリへのパス
を受け取ります。

タスクはsymfonyの埋め込みファイルに切り替えるために
`config/config.php`も変更します。

### ~`project::permissions`~

`project::permissions`タスクはsymfonyのディレクトリのパーミッションを修正します:

    $ php symfony project:permissions  

*エイリアス*: `permissions, fix-perms`




`project:permissions`タスクはディレクトリのパーミッションを修正します:

    ./symfony project:permissions

### ~`project::unfreeze`~

`project::unfreeze`タスクはsymfonyライブラリの凍結を解除します:

    $ php symfony project:unfreeze  

*エイリアス*: `unfreeze`





`project:unfreeze`タスクは現在のプロジェクトから
symfonyのすべてのコアファイルを取り除きます:

    ./symfony project:unfreeze

タスクは`project:freeze`コマンドが使われる前に使われていたsymfonyの古いファイルに
切り替えるために`config/config.php`も変更します。

### ~`project::upgrade1.1`~

`project::upgrade1.1`タスクはsymfonyのプロジェクトをsymfony 1.1のリリースにアップグレードします:

    $ php symfony project:upgrade1.1  



`project:upgrade1.1`タスクは1.0のリリースに基づくsymfonyのプロジェクトを
symfony 1.1のリリースにアップグレードします。

    ./symfony project:upgrade1.1

このタスクが行うことに関する情報を得るにはUPGRADE_TO_1_1ファイルを参照するようお願いします。

### ~`project::upgrade1.2`~

`project::upgrade1.2`タスクはsymfonyプロジェクトを(1.1)からsymfony 1.2リリースにアップグレードします:

    $ php symfony project:upgrade1.2  







`project:upgrade1.2`タスクは1.1リリースに基づくsymfonyプロジェクトを
1.2リリースにアップグレードします。

    ./symfony project:upgrade1.2

このタスクが行うことに関する情報を得るにはUPGRADE_TO_1_2ファイルをご覧ください。

`propel`
--------

### ~`propel::build-all`~

`propel::build-all`タスクはPropelモデルとフォームクラス、SQLを生成しデータベースを初期化します:

    $ php symfony propel:build-all [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--skip-forms|-F] [--classes-only|-C] [--phing-arg="..."] 

*エイリアス*: `propel-build-all`



| オプション(ショートカット)    | デフォルト | 説明
| ---------------------------- | --------- | -----------
| `--application`              | `1`       | アプリケーションの名前
| `--env`                      | `dev`     | 環境
| `--connection`               | `propel`  | 接続名
| `--no-confirmation`          | `-`       | 確認の質問をしない
| `--skip-forms`<br />`(-F)`   | `-`       | フォーム生成をスキップする
| `--classes-only`<br />`(-C)` | `-`       | データベースを初期化しない
| `--phing-arg`                | `-`       | Phingの任意の引数(複数の値が可)


`propel:build-all`タスクは他の5つのタスクのショートカットです:

    ./symfony propel:build-all

このタスクは次のものと同等です:

    ./symfony propel:build-model
    ./symfony propel:build-forms
    ./symfony propel:build-filters
    ./symfony propel:build-sql
    ./symfony propel:insert-sql

詳細な情報はこれらのタスクのヘルプページを参照してください。

確認のプロンプトを回避するには、`no-confirmation`オプションを渡します:

    ./symfony propel:buil-all --no-confirmation

すべてのクラスをビルドするがデータベースの初期化をスキップするには、
`classes-only`オプションを使います:

    ./symfony propel:build-all --classes-only

### ~`propel::build-all-load`~

`propel::build-all-load`タスクはPropelモデルとフォームクラス、SQLを生成し、
データベースを初期化し、データをロードします:

    $ php symfony propel:build-all-load [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--skip-forms|-F] [--classes-only|-C] [--phing-arg="..."] [--append] [--dir="..."] 

*エイリアス*: `propel-build-all-load`



| オプション(ショートカット)    | デフォルト | 説明
| ---------------------------- | --------- | -----------
| `--application`              | `1`       | アプリケーションの名前
| `--env`                      | `dev`     | 環境
| `--connection`               | `propel`  | 接続名
| `--no-confirmation`          | `-`       | 確認の質問をしない
| `--skip-forms`<br />`(-F)`   | `-`       | フォーム生成をスキップする
| `--classes-only`<br />`(-C)` | `-`       | データベースを初期化しない
| `--phing-arg`                | `-`       | Phingの任意の引数(複数の値が可)
| `--append`                   | `-`       | データベースの現在の値を削除しない
| `--dir`                      | `-`       | フィクスチャを探すディレクトリ(複数の値が許可される)


`propel:build-all-load`タスクは他の2つのタスクのショートカットです:

    ./symfony propel:build-all-load

このタスクは次のものと同等です:

    ./symfony propel:build-all
    ./symfony propel:data-load

詳細な情報はこれらのタスクのヘルプページを参照してください。

確認を回避するには、`no-confirmation`オプション
を渡します:

    ./symfony propel:buil-all-load --no-confirmation

### ~`propel::build-filters`~

`propel::build-filters`タスクは現在のモデル用のフィルターフォームクラスを作成します:

    $ php symfony propel:build-filters [--connection="..."] [--model-dir-name="..."] [--filter-dir-name="..."] [--application[="..."]] 





| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--connection`            | `propel`  | 接続名
| `--model-dir-name`        | `model`   | モデルディレクトリの名前
| `--filter-dir-name`       | `filter`  | フィルターフォームディレクトリの名前
| `--application`           | `1`       | アプリケーションの名前


`propel:build-filters`タスクはスキーマからフィルターフォームクラスを作成します:

    ./symfony propel:build-filters

タスクはプロジェクトとすべてのインストールされたプラグインから`config/*schema.xml`かつ/もしくは
`config/*schema.yml`のスキーマ情報を読み込みます。

タスクは`config/databases.yml`で定義された`propel`接続を使います。
`--connection`オプションを指定することで別の接続が使えます:

    ./symfony propel:build-filters --connection="name"

モデルのフィルターフォームクラスは`lib/filter`に作成されます。

このタスクは`lib/filter`のカスタムクラスをけっして上書きしません。
これは`lib/filter/base`で生成された既定クラスを置き換えます。

### ~`propel::build-forms`~

`propel::build-forms`タスクは現在のモデル用のフォームクラスを作成します:

    $ php symfony propel:build-forms [--connection="..."] [--model-dir-name="..."] [--form-dir-name="..."] [--application[="..."]] 





| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--connection`            | `propel`  | 接続名
| `--model-dir-name`        | `model`   | モデルディレクトリの名前
| `--form-dir-name`         | `form`    | フォームディレクトリの名前
| `--application`           | `1`       | アプリケーションの名前


`propel:build-forms`タスクはスキーマからフォームクラスを作成します:

    ./symfony propel:build-forms

タスクはプロジェクトとインストールしたすべてのプラグインから`config/*schema.xml`かつ/もしくは
`config/*schema.yml`のスキーマ情報を読み取ります。

タスクは`config/databases.yml`で定義された`propel`接続を使います。
`--connection`オプションを指定することで別の接続を使うことができます:

    ./symfony propel:build-forms --connection="name"

モデルのフォームクラスは`lib/form`に作成されます。

このタスクは`lib/form`のカスタムクラスをけっして上書きしません。
これは`lib/form/base`に生成された基底クラスのみを置き換えます。

### ~`propel::build-model`~

`propel::build-model`タスクは現在のモデル用のクラスを作成します:

    $ php symfony propel:build-model [--phing-arg="..."] 

*エイリアス*: `propel-build-model`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--phing-arg`             | `-`       | Phingの任意の引数(複数の値が可)


`propel:build-model`タスクはスキーマからモデルクラスを作成します:

    ./symfony propel:build-model

タスクはプロジェクトとインストールされたすべてのプラグインから`config/*schema.xml`かつ/もしくは
`config/*schema.yml`のスキーマ情報を読み込みます。

YAMLとXMLスキーマファイルを混ぜることができます。
タスクはPropelタスクを呼び出す前にYAMLをXMLに変換します。

モデルクラスのファイルは`lib/model`に作成されます。

このタスクは`lib/model`のカスタムクラスをけっして置き換えません。
`lib/model/om`と`lib/model/map`のファイルのみを置き換えます。

### ~`propel::build-schema`~

`propel::build-schema`タスクは既存のデータベースからスキーマを作成します:

    $ php symfony propel:build-schema [--application[="..."]] [--env="..."] [--connection="..."] [--xml] [--phing-arg="..."] 

*エイリアス*: `propel-build-schema`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--application`           | `1`       | アプリケーションの名前
| `--env`                   | `cli`     | 環境
| `--connection`            | `-`       | 接続名
| `--xml`                   | `-`       | YAMLの代わりにXMLスキーマを作成する
| `--phing-arg`             | `-`       | Phingの任意の引数(複数の値が可)


`propel:build-schema`タスクはスキーマを作成するためにデータベースをイントロスペクトします:

    ./symfony propel:build-schema

デフォルトでは、タスクはYAMLファイルを作成しますが、XMLファイルを作成することもできます:

    ./symfony --xml propel:build-schema

XMLフォーマットはYAMLよりも多くの情報を格納できます。

### ~`propel::build-sql`~

`propel::build-sql`タスクは現在のモデル用のSQLを作成します:

    $ php symfony propel:build-sql [--phing-arg="..."] 

*エイリアス*: `propel-build-sql`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--phing-arg`             | `-`        | Phingの任意の引数(複数の値が可)


`propel:build-sql`タスクはテーブル作成用のSQLステートメントを作成します:

    ./symfony propel:build-sql

生成されるSQLは`config/propel.ini`で設定されたデータベース用に最適化されます:

    propel.database = mysql

### ~`propel::data-dump`~

`propel::data-dump`タスクはデータをフィクスチャディレクトリにダンプします:

    $ php symfony propel:data-dump [--application[="..."]] [--env="..."] [--connection="..."] [--classes="..."] [target]

*エイリアス*: `propel-dump-data`

| 引数     | デフォルト | 説明
| -------- | ---------- | -----------
| `target` | `-`        | ターゲットのファイル名


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `cli`      | 環境
| `--connection`            | `propel`   | 接続名
| `--classes`               | `-`        | ダンプするクラスの名前(コロンで区切られる)


`propel:data-dump`タスクはデータベースデータをダンプします:

    ./symfony propel:data-dump > data/fixtures/dump.yml

デフォルトでは、タスクはデータを標準出力しますが、
2番目の引数としてファイルの名前を渡すこともできます:

    ./symfony propel:data-dump dump.yml

タスクはデータを`data/fixtures/%target%`にダンプします
(この例ではdata/fixtures/dump.yml)。

ダンプファイルはYAMLフォーマットで
`propel:data-load`タスクを使うことで再インポートできます。

デフォルトでは、`config/databases.yml`で定義された`propel`接続を使います。
`connection`オプションを指定することで別の接続を使うことができます:

    ./symfony propel:data-dump --connection="name"

クラスをダンプしたいだけなら、`classes`オプションを使います:

    ./symfony propel:data-dump --classes="Article,Category"

アプリケーションから特定のデータベース接続を使いたい場合、
`application`オプションを指定します:

    ./symfony propel:data-dump --application=frontend

### ~`propel::data-load`~

`propel::data-load`タスクはフィクスチャディレクトリからデータをロードします:

    $ php symfony propel:data-load [--application[="..."]] [--env="..."] [--append] [--connection="..."] [--dir="..."] 

*エイリアス*: `propel-load-data`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `cli`      | 環境
| `--append`                | `-`        | データベースの現在の値を削除しない
| `--connection`            | `propel`   | 接続名
| `--dir`                   | `-`        | フィクスチャを探すディレクトリ(複数の値が許可される)


`propel:data-load`タスクはデータフィクスチャをデータベースにロードします:

    ./symfony propel:data-load

タスクは`data/fixtures/`で見つかるすべてのファイルからデータをロードします。

他のディレクトリからデータをロードしたい場合、
`--dir`オプションを使います:

    ./symfony propel:data-load --dir="data/fixtures" --dir="data/data"

タスクは`config/databases.yml`で定義された`propel`接続を使います。
`--connection`オプションを指定することで別の接続を使うことができます:

    ./symfony propel:data-load --connection="name"

タスクにデータベースの既存のデータを削除させたくない場合、
`--append`オプションを使います:

    ./symfony propel:data-load --append

アプリケーションからデータベース接続を使いたい場合、 
`application`オプションを使います:

    ./symfony propel:data-load --application=frontend

### ~`propel::generate-admin`~

`propel::generate-admin`タスクはPropelのadminモジュールを生成します:

    $ php symfony propel:generate-admin [--module="..."] [--theme="..."] [--singular="..."] [--plural="..."] [--env="..."] application route_or_model



| 引数             | デフォルト | 説明
| ---------------- | --------- | -----------
| `application`    | `-`       | アプリケーションの名前
| `route_or_model` | `-`       | ルートの名前もしくはモデルクラス


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--module`                | `-`        | モジュールの名前
| `--theme`                 | `admin`    | テーマの名前
| `--singular`              | `-`        | 単数形の名前
| `--plural`                | `-`        | 複数形の名前
| `--env`                   | `dev`      | 環境


`propel:generate-admin`タスクはPropelのadminモジュールを生成します:

    ./symfony propel:generate-admin frontend Article

タスクは`%Article%`モデルの`%frontend%`アプリケーションの
モジュールを作成します。

このタスクはアプリケーションの`routing.yml`でルートを作成します。

ルートの名前を渡すことでもPropelのadminモジュールを生成することもできます:

    ./symfony propel:generate-admin frontend article

タスクは`routing.yml`で見つかる`%article%`のルート定義用の
`%frontend%`アプリケーションのモジュールを作成します。


フィルターとバッチアクションを適切に動作させるために、 
`wildcard`オプションをルートに追加する必要があります:

    article:
      class: sfPropelRouteCollection
      options:
        model:                Article
        with_wildcard_routes: true

### ~`propel::generate-module`~

`propel::generate-module`タスクはPropelモジュールを生成します:

    $ php symfony propel:generate-module [--theme="..."] [--generate-in-cache] [--non-verbose-templates] [--with-show] [--singular="..."] [--plural="..."] [--route-prefix="..."] [--with-propel-route] [--env="..."] application module model

*エイリアス*: `propel-generate-crud`、`propel:generate-crud`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `module`      | `-`       | モジュールの名前
| `model`       | `-`       | モデルクラスの名前


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--theme`                 | `default` | テーマの名前
| `--generate-in-cache`     | `-`       | キャッシュでモジュールを生成する
| `--non-verbose-templates` | `-`       | 冗長ではないテンプレートを生成する
| `--with-show`             | `-`       | showメソッドを生成する
| `--singular`              | `-`       | 単数形の名前
| `--plural`                | `-`       | 複数形の名前
| `--route-prefix`          | `-`       | ルートの接頭辞
| `--with-propel-route`     | `-`       | Propelのルートを使うかどうか
| `--env`                   | `dev`     | 環境


`propel:generate-module`タスクはPropelモジュールを生成します:

    ./symfony propel:generate-module frontend article Article

タスクは`%model%`モデルクラス用の`%application%`アプリケーションで
`%module%`モジュールを作成します。

`--generate-in-cache`オプションを指定することで`%sf_app_cache_dir%/modules/auto%module%`の実行時に
生成されるモジュールからアクションとテンプレートを継承する空のモジュールを作成することもできます:

    ./symfony propel:generate-module --generate-in-cache frontend article Article

`--theme`オプションを指定することでジェネレーターはカスタマイズされたテーマを使うことができます:

    ./symfony propel:generate-module --theme="custom" frontend article Article

この方法では、独自仕様に合わせてモジュールジェネレーターを作ることができます。

### ~`propel::generate-module-for-route`~

`propel::generate-module-for-route`タスクはルート定義用のPropelモジュールを生成します:

    $ php symfony propel:generate-module-for-route [--theme="..."] [--non-verbose-templates] [--singular="..."] [--plural="..."] [--env="..."] application route



| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `route`       | `-`       | ルートの名前


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--theme`                 | `default`  | テーマの名前
| `--non-verbose-templates` | `-`        | 冗長ではないテンプレートを生成する
| `--singular`              | `-`        | 単数形の名前
| `--plural`                | `-`        | 複数形の名前
| `--env`                   | `dev`      | 環境


`propel:generate-module-for-route`タスクはルートの定義用のPropelモジュールを生成します:

    ./symfony propel:generate-module-for-route frontend article

タスクは`routing.yml`で見つかる`%article%`ルート定義用の
`%frontend%`アプリケーションでモジュールを作成します。

### ~`propel::graphviz`~

`propel::graphviz`タスクは現在のオブジェクトモデルのgraphvizによるチャートを生成します:

    $ php symfony propel:graphviz [--phing-arg="..."] 





| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | ------------------------------
| `--phing-arg`             | `-`       | Phingの任意の引数(複数の値が可)


`propel:graphviz`タスクはオブジェクトモデルのグラフの自動描画にgraphvizのDOTを作成します:

    ./symfony propel:graphviz

### ~`propel::init-admin`~

`propel::init-admin`タスクはPropelのadminモジュールを初期化します:

    $ php symfony propel:init-admin [--theme="..."] application module model

*エイリアス*: `propel-init-admin`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `module`      | `-`       | モジュールの名前
| `model`       | `-`       | モデルクラスの名前


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--theme`                 | `default` | テーマの名前


`propel:init-admin`タスクはPropelのadminモジュールを生成します:

    ./symfony propel:init-admin frontend article Article

`%model%`モデルクラス用の`%application%`アプリケーションで`%module%`モジュールを作成します。

作成されるモジュールは`%sf_app_cache_dir%/modules/auto%module%`の実行時に生成されるモジュールから
アクションとテンプレートを継承する空のモジュールです。

ジェネレーターは`--theme`オプションを指定することでカスタマイズされたテーマを使うことができます:

    ./symfony propel:init-admin --theme="custom" frontend article Article

### ~`propel::insert-sql`~

`propel::insert-sql`タスクは現在のモデル用のSQLをinsertします:

    $ php symfony propel:insert-sql [--application[="..."]] [--env="..."] [--connection="..."] [--no-confirmation] [--phing-arg="..."] 

*エイリアス*: `propel-insert-sql`



| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | ---------- | -----------
| `--application`           | `1`        | アプリケーションの名前
| `--env`                   | `cli`      | 環境
| `--connection`            | `-`        | 接続名
| `--no-confirmation`       | `-`        | 確認の質問をしない
| `--phing-arg`             | `-`        | Phingの任意の引数(複数の値が可)


`propel:insert-sql`タスクはデータベースのテーブルを作成します:

    ./symfony propel:insert-sql

タスクはデータベースに接続して`config/sql/*schema.sql`ファイルで
見つかるすべてのSQLステートメントを実行します。

実行する前に、データベースのすべてのデータが削除されるのでタスクは本当に実行するか確認します。

確認を回避するには、`--no-confirmation`
オプションを渡します:

    ./symfony propel:insert-sql --no-confirmation

タスクは`databases.yml`からデータベース設定を読み込みます。
`--application`もしくは`--env`オプションを渡せば
特定のアプリケーションの環境を使うことができます。

与えられた接続でSQLステートメントをロードしたいだけなら
 `--connection`オプションを指定することもできます。

### ~`propel::schema-to-xml`~

`propel::schema-to-xml`タスクは`schema.yml`から`schema.xml`を作成します:

    $ php symfony propel:schema-to-xml  

*エイリアス*: `propel-convert-yml-schema`





`propel:schema-to-xml`タスクはYAMLスキーマをXMLスキーマに変換します:

    ./symfony propel:schema-to-xml

### ~`propel::schema-to-yml`~

`propel::schema-to-yml`タスクは`schema.xml`から`schema.yml`を作成します:

    $ php symfony propel:schema-to-yml  

*エイリアス*: `propel-convert-xml-schema`





`propel:schema-to-yml`タスクはXMLスキーマをYAMLに変換します:

    ./symfony propel:schema-to-yml

`test`
------

### ~`test::all`~

`test::all`タスクはすべてのテストを立ち上げます:

    $ php symfony test:all  

*エイリアス*: `test-all`





`test:all`タスクはすべてのユニットテストと機能テストを立ち上げます:

    ./symfony test:all

タスクは`test/`で見つかるすべてのテストを立ち上げます。

1つもしくは複数のテストが失敗する場合、手動もしくは`test:unit`と`test:functional`タスクで
これらを立ち上げることで問題の修正に取り組むことができます。

### ~`test::coverage`~

`test::coverage`タスクはテストのコードカバレッジを出力します:

    $ php symfony test:coverage [--detailed] test_name lib_name



| 引数        | デフォルト | 説明
| ----------- | --------- | -----------
| `test_name` | `-`       | テストファイルの名前もしくはテストのディレクトリ
| `lib_name`  | `-`       | カバレージを知りたいlibファイルの名前もしくはlibディレクトリ


| オプション(ショートカット) | デフォルト | 説明
| ------------------------- | --------- | -----------
| `--detailed`              | `-`       | 詳細な情報を出力する


`test:coverage`タスクはテストディレクトリもしくはテストディレクトリと
libファイルもしくはlibディレクトリの欲しいコードカバレージを出力します:

    ./symfony test:coverage test/unit/model lib/model

カバーされていない行を出力するには、`--detailed`オプションを渡します:

    ./symfony test:coverage --detailed test/unit/model lib/model

### ~`test::functional`~

`test::functional`タスクは機能テストを立ち上げます:

    $ php symfony test:functional  application [controller1] ... [controllerN]

*エイリアス*: `test-functional`

| 引数          | デフォルト | 説明
| ------------- | --------- | -----------
| `application` | `-`       | アプリケーションの名前
| `controller`  | `-`       | コントローラーの名前




`test:functional`タスクは与えられたアプリケーション用の機能テストを立ち上げます:

    ./symfony test:functional frontend

タスクは`test/functional/%application%`で見つかるすべてのテストを立ち上げます。

コントローラーの名前を渡すことで特定のコントローラー用の
すべての機能テストを立ち上げることができます:

    ./symfony test:functional frontend article

複数のコントローラー用の機能テストをすべて立ち上げることもできます:

    ./symfony test:functional frontend article comment

### ~`test::unit`~

`test::unit`タスクはユニットテストを立ち上げます:

    $ php symfony test:unit  [name1] ... [nameN]

*エイリアス*: `test-unit`

| 引数     | デフォルト | 説明
| -------- | --------- | -----------
| `name`   | `-`       | テストの名前




`test:unit`タスクはユニットテストを立ち上げます:

    ./symfony test:unit

タスクは`test/unit`で見つかるすべてのテストを立ち上げます。

特定の名前のユニットテストを立ち上げることができます:

    ./symfony test:unit strtolower

複数の名前のユニットテストを立ち上げることもできます:

    ./symfony test:unit strtolower strtoupper



