databases.yml設定ファイル
========================

~`databases.yml`~はデータベース接続の設定を可能にします。
これはsymfonyに搭載されているORMであるPropelとDoctrineの両方で使われます。

プロジェクトのメインの`databases.yml`設定ファイルは`config/`ディレクトリで見つかります。

>**NOTE**
>たいていの場合、プロジェクトのすべてのアプリケーションは同じデータベースをを共有します。
>そのことがメインデータベース設定ファイルがプロジェクトの`config/`ディレクトリにある理由です。
>もちろんアプリケーションの設定ディレクトリで`databases.yml`設定ファイルを定義することでデフォルトの設定をオーバーライドできます。

はじめの章で説明したように、`databases.yml`ファイルは[**環境を認識し**](#chapter_03_environment_awareness)、
[**コンフィギュレーションカスケードのメカニズム**](#chapter_03_configuration_cascade)が有効になり、
[**定数**](#chapter_03_constants)を含むことができます。

`databases.yml`で説明されるそれぞれの接続はデータベースオブジェクトを設定するために使用する名前、データベースハンドラクラスの名前、パラメーター(`param`)の設定を格納しなければなりません:

    [yml]
    CONNECTION_NAME:
      class: CLASS_NAME
      param: { ARRAY OF PARAMETERS }

`class`クラスは`sfDatabase`基底クラスを継承します。

データベースハンドラクラスをオートロードできない場合、`file`パスを定義しファクトリが作成される前に自動的に含めることができます:

    [yml]
    CONNECTION_NAME:
      class: CLASS_NAME
      file:  ABSOLUTE_PATH_TO_FILE

>**NOTE**
>`databases.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>プロセスは~`sfDatabaseConfigHandler`~
>[クラス](#chapter_14_config_handlers_yml)によって自動的に管理されます。

-

>**TIP**
>データベースの設定は`database:configure`タスクを使うことでも設定できます。
>このタスクは渡された引数に従って`databases.yml`を更新します。

Propel
------

*デフォルトのコンフィギュレーション*:

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

次のパラメーターは`param`セクションの下で定義できます:

 | キー         | 説明                                      | デフォルトの値|
 | ------------ | ---------------------------------------- | ------------- |
 | `classname`  | Propelのアダプタクラス                    | `PropelPDO`   |
 | `dsn`        | PDOのDSN (必須)                          | -             |
 | `username`   | データベースのユーザー名                  | -             |
 | `password`   | データベースのパスワード                  | -             |
 | `pooling`    | プーリングを有効にするか                  | `true`        |
 | `encoding`   | デフォルトの文字コード                    | `UTF-8`       |
 | `persistent` | 永続的接続を作成するか                   | `false`       |
 | `options`    | Propelオプションの一式                   | -             |

Doctrine
--------

*デフォルトコンフィギュレーション*:

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

次のパラメーターは`param`セクションの下でカスタマイズできます:

 | キー          | 説明                                    | デフォルト値   |
 | ------------ | ---------------------------------------- | ------------- |
 | `dsn`        | PDOのDSN (必須)                          | -             |
 | `username`   | データベースのユーザー名                  | -             |
 | `password`   | データベースのパスワード                  | -             |
 | `encoding`   | デフォルトの文字コード                    | `UTF-8`       |
 | `attributes` | Doctrine属性のセット                     | -             |

次の属性は`attributes`セクションの下でカスタマイズできます:

 | キー                | 説明                                     | デフォルト値   |
 | ------------------- | ---------------------------------------- | ------------- |
 | `quote_identifier`  | クォートで識別子をラップするか            | `false`       |
 | `use_native_enum`   | ネイティブのenumを使うか                  | `false`       |
 | `validate`          | データバリデーションを有効にするかどうか   | `true`        |
 | `idxname_format`    | インデックス名用のフォーマット            | `%s_idx`      |
 | `seqname_format`    | シーケンス名用のフォーマット              | `%s_seq`      |
 | `tblname_format`    | テーブル名用のフォーマット                | `%s`          |
