app.yml設定ファイル
==================

symfonyフレームワークはアプリケーション固有の設定のために
組み込みの設定ファイルの`app.yml`を提供します。

このYAMLファイルには特定のアプリケーションで使いたい任意の設定を格納できます。
コードにおいて、これらの設定はグローバルな`sfConfig`クラスを通して利用可能で、
キーには接頭辞として`app_`文字列がつけられます:

    [php]
    sfConfig::get('app_active_days');

すべての設定には接頭辞の`app_`がつけられます。`sfConfig`クラスは
[symfonyの設定](#chapter_03-Configuration-Files-Principles_sub_configuration_settings)と
[プロジェクトのディレクトリ](#chapter_03-Configuration-Files-Principles_sub_directories)にアクセスする権限を提供するからです。

最初の章で説明したように、`app.yml`ファイルは
[**環境を認識し**](#chapter_03-Configuration-Files-Principles_sub_environment_awareness)、
[**設定カスケードのメカニズム**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade)の恩恵を受けます。

`app.yml`設定ファイルは環境に基づいて変化する設定(たとえばAPIキー)
もしくは時間をかけて進化する可能性のある設定(たとえばEメールアドレス)を定義するのにふさわしい場所です。
このファイルはsymfonyもしくはPHPを必ずしも理解する必要のない人間(たとえばシステム管理者)
によって変更する必要のある設定を定義するのにも最良の場所です。

>**TIP**
>アプリケーションのロジックを搭載するために`app.yml`を使うのは控えてください。

-

>**NOTE**
>`app.yml`設定ファイルはPHPファイルとしてキャッシュされます。
>処理は`sfDefineEnvironmentConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。
