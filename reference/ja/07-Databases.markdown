databases.yml設定ファイル
========================

`databases.yml`はデータベース接続の設定を可能にします。
これはsymfonyに搭載される両方のORM: PropelとDoctrineで
使われます。

プロジェクトのメインの`databases.yml`設定ファイルは
`config/`ディレクトリで見つかります。

>**NOTE**
>大抵の場合、プロジェクトのすべてのアプリケーションは同じデータベースを
>を共有します。これがメインのデータベース設定ファイルが
>プロジェクトの`config/`ディレクトリにある理由です。もちろん
>アプリケーションの設定ディレクトリで`databases.yml`設定ファイルを定義することで
>デフォルトの設定をオーバーライドできます。

最初の章で説明したように、`databases.yml`ファイルは
[**環境を認識し**](#chapter_03-Configuration-File-Principles_sub_environment_awareness)、
[**設定カスケードのメカニズム**](#chapter_03-Configuration-File-Principles_sub_configuration_cascade)の恩恵を受け、
[**定数**](#chapter_03-Configuration-Files-Principles_sub_constants)を格納します。

`databases.yml`で説明したそれぞれの接続はデータベースオブジェクトを設定するために
使用する、名前、データベースハンドラクラスの名前、
パラメータ(`param`)の設定を含まなければなりません:

    [yml]
    CONNECTION_NAME:
      class: CLASS_NAME
      param: { ARRAY OF PARAMETERS }

`class`の名前は`sfDatabase`基底クラスを継承します。

データベースハンドラクラスをオートロードできない場合、`file`パスを定義しファクトリが
作成される前に自動的に含めることができます:

    [yml]
    CONNECTION_NAME:
      class: CLASS_NAME
      file:  ABSOLUTE_PATH_TO_FILE

>**NOTE**
>`databases.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>プロセスは`sfDatabaseConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。

-

>**TIP**
>データベースの設定は`database:configure`タスクを
>使うことでも設定できます。このタスクは渡された引数に従って
>`databases.yml`を更新します。

Propel
------

*デフォルトの構成*:

    [yml]
    dev:
      propel:
        param:
          classname:  DebugPDO

    test:
      propel:
        param:
          classname:  DebugPDO

    all:
      propel:
        class:        sfPropelDatabase
        param:
          classname:  PropelPDO
          dsn:        mysql:dbname=##PROJECT_NAME##;host=localhost
          username:   root
          password:   
          encoding:   utf8
          persistent: true
          pooling:    true

次のパラメータは`param`セクションの元で定義できます:

 | キー         | 説明                                      | デフォルトの値|
 | ------------ | ---------------------------------------- | ------------- |
 | `classname`  | Propelのアダプタクラス                    | `PropelPDO`   |
 | `dsn`        | PDOのDSN (必須)                          | -             |
 | `username`   | データベースのユーザー名                  | -             |
 | `password`   | データベースのパスワード                  | -             |
 | `pooling`    | プーリングを有効にするか                  | `true`        |
 | `encoding`   | デフォルトの文字集合                      | `UTF-8`       |
 | `persistent` | 永続的接続を作成するか                   | `false`       |
 | `options`    | Propelオプションの一式                   | -             |

Doctrine
--------

*デフォルト構成*:

    [yml]
    all:
      doctrine:
        class:        sfDoctrineDatabase
        param:
          dsn:        mysql:dbname=##PROJECT_NAME##;host=localhost
          username:   root
          password:   
          attributes:
            quote_identifier: false
            use_native_enum: false
            validate: all
            idxname_format: %s_idx
            seqname_format: %s_seq
            tblname_format: %s

次のパラメータは`param`セクションの元でカスタマイズできます:

 | キー          | 説明                                    | デフォルト値   |
 | ------------ | ---------------------------------------- | ------------- |
 | `dsn`        | PDOのDSN (必須)                          | -             |
 | `username`   | データベースのユーザー名                  | -             |
 | `password`   | データベースのパスワード                  | -             |
 | `encoding`   | デフォルトの文字集合                      | `UTF-8`       |
 | `attributes` | Doctrine属性のセット                     | -             |

次の属性は`attributes`セクションの元でカスタマイズできます:

 | キー                | 説明                                     | デフォルト値   |
 | ------------------- | ---------------------------------------- | ------------- |
 | `quote_identifier`  | クォートで識別子をラップするか            | `false`       |
 | `use_native_enum`   | ネイティブのenumを使うか                  | `false`       |
 | `validate`          | データバリデーションを有効にするかどうか   | `true`        |
 | `idxname_format`    | インデックス名用のフォーマット            | `%s_idx`      |
 | `seqname_format`    | シーケンス名用のフォーマット              | `%s_seq`      |
 | `tblname_format`    | テーブル名用のフォーマット                | `%s`          |
