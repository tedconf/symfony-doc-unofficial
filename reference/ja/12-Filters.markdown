filters.yml設定ファイル
======================

`filters.yml`設定ファイルはすべてのリクエストで実行される
フィルタチェーンを記述します。

アプリケーション用のメインの`filters.yml`設定ファイルは
`apps/APP_NAME/config/`ディレクトリで見つかります。

最初の章で説明したように、`filters.yml`ファイルは
[**設定カスケードのメカニズム**](#chapter_03-Configuration-Files-Principles_sub_configuration_cascade)が有効で、
[**定数**](#chapter_03-Configuration-Files-Principles_sub_constants)を含むことができます。

`filters.yml`設定ファイルは名前つきフィルタ定義のリストを
格納できます:

    [yml]
    FILTER_1:
      # definition of filter 1

    FILTER_2:
      # definition of filter 2

    # ...

コントローラがリクエストに対してフィルタを初期化するとき、
`filters.yml`ファイルを読み込みフィルタオブジェクトを設定するために使われる
フィルタ(`class`)とパラメータ(`param`)のクラス名を探すことで
フィルタを登録します:

    [yml]
    FILTER_NAME:
      class: CLASS_NAME
      param: { ARRAY OF PARAMETERS }

フィルタは設定ファイルで記述されている順序で実行されます。
symfonyは複数のフィルタを1つのチェーンとして実行するので、
最初に登録されたフィルタは最初と最後に実行されます。

`class`クラスは`sfFilter`基底クラスを継承します。

フィルタクラスがオートロードできない場合、`file`パスを定義することが可能で
フィルタオブジェクトが作成される前に自動的にインクルードされます:

    [yml]
    FACTORY_NAME:
      class: CLASS_NAME
      file:  ABSOLUTE_PATH_TO_FILE

`filters.yml`ファイルをオーバーライドするとき、継承する設定ファイルから
すべてのフィルタを維持しなければなりません:

    [yml]
    rendering: ~
    security:  ~
    cache:     ~
    common:    ~
    execution: ~

フィルタを除外するには、`enabled`キーを`false`にセットすることで
これを無効にする必要があります:

    [yml]
    FACTORY_NAME:
      enabled: false

2つの特別な名前のフィルタ: `rendering`と`execution`があります。
これらは両方とも必須で`type`パラメータで識別されます。
`rendering`フィルタは常に最初に登録されフィルタリングされ
`execution`フィルタは最後のものになります:

    [yml]
    rendering:
      class: sfRenderingFilter
      param:
        type: rendering

    # ...

    execution:
      class:  sfExecutionFilter
      param:
        type: execution

>**NOTE**
>`filters.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>処理は`sfFilterConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。

<div class="pagebreak"></div>

フィルタ
--------

 * [`rendering`](#chapter_12-Filters_sub_rendering)
 * [`security`](#chapter_12-Filters_sub_security)
 * [`cache`](#chapter_12-Filters_sub_cache)
 * [`common`](#chapter_12-Filters_sub_common)
 * [`execution`](#chapter_12-Filters_sub_execution)

`rendering`
-----------

*デフォルト構成*:

    [yml]
    rendering:
      class: sfRenderingFilter
      param:
        type: rendering

`rendering`フィルタはブラウザへのレスポンス出力の責務を担います。
これは最初に登録されるフィルタになるので、
リクエストを管理する機会を持つ最後のフィルタにもなります。

`security`
----------

*デフォルト構成*:

    [yml]
     security:
       class: sfBasicSecurityFilter
       param:
         type: security

`security`フィルタはアクションの`getCredential()`メソッドを呼び出すことで
セキュリティをチェックします。いったんクレデンシャルが得られたら、 
ユーザーオブジェクトの`hasCredential()`メソッドを呼び出すことで
ユーザーが同じクレデンシャルを持つことを確認します。

`security`フィルタは`security`型を持たなければなりません。

`security`フィルタのきめ細かい設定は
`security.yml`設定[ファイル](#chapter_08-Security)を通して行われます。

>**TIP**
>必須のアクションが`security.yml`でセキュアなものとして設定されていない場合、
>`security`フィルタは実行されません。

`cache`
-------

*デフォルト構成*:

    [yml]
    cache:
      class: sfCacheFilter
      param:
        condition: %SF_CACHE%

`cache`フィルタはアクションとページを管理します。
これは必要とされるHTTPキャッシュヘッダーを
レスポンスに追加するための責務も担います
(`Last-Modified`、`ETag`、`Cache-Control`、`Expires`、・・・)。

`common`
--------

*デフォルト構成*:

    [yml]
    common:
      class: sfCommonFilter

`common`フィルタはJavaScriptとスタイルシートがすでにインクルードされていない場合
メインのレスポンスにこれらを追加します。

>**TIP**
>レイアウトで`include_stylesheets()`と`include_javascripts()`ヘルパーを使う場合、
>安全にこのフィルタを無効にすることが可能で、
>小さなパフォーマンスの加速の恩恵を受けます。

`execution`
-----------

*デフォルト構成*:

    [yml]
    execution:
      class:  sfExecutionFilter
      param:
        type: execution

`execution`フィルタはフィルタチェーンの真ん中にあり、
すべてのアクションとビューの実行を行います。

`execution`フィルタは最後に登録されたフィルタになります。
