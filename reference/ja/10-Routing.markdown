routing.yml設定ファイル
======================

`routing.yml`設定ファイルはルートの定義を可能にします。

アプリケーション用のメインの`routing.yml`設定ファイルは
`apps/APP_NAME/config/`ディレクトリで見つかります。

`routing.yml`設定ファイルは名前付きのルートの定義の
リストを含みます:

    [yml]
    ROUTE_1:
      # definition of route 1

    ROUTE_2:
      # definition of route 2

    # ...

リクエストがやってくるとき、ルーティングシステムはやってくるURLのルートがマッチするか試します。
最初にマッチするルートが優先されるので、
`routing.yml`設定ファイルで定義されたルートの順序は大切です。

`routing.yml`設定ファイルが読み込まれるとき、それぞれのルートが
`class`クラスのオブジェクトに変換されます:

    [yml]
    ROUTE_NAME:
      class: CLASS_NAME
      # configuration if the route

`class`の名前は`sfRoute`基底クラスを継承します。提供されない場合、
`sfRoute`基底クラスはフォールバックとして使われます。

>**NOTE**
>`routing.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>処理は`sfRoutingConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。

<div class="pagebreak"></div>

ルートクラス
------------

 * [メインの設定](#chapter_10-Routing_sub_route_configuration)

   * [`class`](#chapter_10-Routing_sub_class)
   * [`options`](#chapter_10-Routing_sub_options)
   * [`param`](#chapter_10-Routing_sub_param)
   * [`params`](#chapter_10-Routing_sub_params)
   * [`requirements`](#chapter_10-Routing_sub_requirements)
   * [`type`](#chapter_10-Routing_sub_type)
   * [`url`](#chapter_10-Routing_sub_url)

 * [`sfRoute`](#chapter_10-Routing_sub_sfroute)
 * [`sfRequestRoute`](#chapter_10-Routing_sub_sfrequestroute)

   * [`sf_method`](#chapter_10-Routing_sub_sf_method)

 * [`sfObjectRoute`](#chapter_10-Routing_sub_sfobjectroute)

   * [`allow_empty`](#chapter_10-Routing_sub_allow_empty)
   * [`convert`](#chapter_10-Routing_sub_convert)
   * [`method`](#chapter_10-Routing_sub_method)
   * [`model`](#chapter_10-Routing_sub_model)
   * [`type`](#chapter_10-Routing_sub_type)

 * [`sfPropelRoute`](#chapter_10-Routing_sub_sfpropelroute)

   * [`method_for_criteria`](#chapter_10-Routing_sub_method_for_criteria)

 * [`sfDoctrineRoute`](#chapter_10-Routing_sub_sfdoctrineroute)

   * [`method_for_query`](#chapter_10-Routing_sub_method_for_query)

 * [`sfRouteCollection`](#chapter_10-Routing_sub_sfroutecollection)
 * [`sfObjectRouteCollection`](#chapter_10-Routing_sub_sfobjectroutecollection)

   * [`actions`](#chapter_10-Routing_sub_actions)
   * [`collection_actions`](#chapter_10-Routing_sub_collection_actions)
   * [`column`](#chapter_10-Routing_sub_column)
   * [`model`](#chapter_10-Routing_sub_model)
   * [`model_methods`](#chapter_10-Routing_sub_model_methods)
   * [`module`](#chapter_10-Routing_sub_module)
   * [`object_actions`](#chapter_10-Routing_sub_object_actions)
   * [`prefix_path`](#chapter_10-Routing_sub_prefix_path)
   * [`requirements`](#chapter_10-Routing_sub_requirements)
   * [`route_class`](#chapter_10-Routing_sub_route_class)
   * [`segment_names`](#chapter_10-Routing_sub_segment_names)
   * [`with_show`](#chapter_10-Routing_sub_with_show)
   * [`with_wildcard_routes`](#chapter_10-Routing_sub_with_wildcard_routes)

 * [`sfPropelRouteCollection`](#chapter_10-Routing_sub_sfpropelroutecollection)
 * [`sfDoctrineRouteCollection`](#chapter_10-Routing_sub_sfdoctrineroutecollection)

<div class="pagebreak"></div>

ルートの設定
------------

`routing.yml`設定ファイルはルートをさらに設定するための複数の設定をサポートします。
これらの設定はそれぞれのルートをオブジェクトに変換するために
`sfRoutingConfigHandler`クラスによって使われます。

### `class`

*デフォルト*: `sfRoute` (もしくは`type`が`collection`である場合`sfRouteCollection`、下記を参照)

`class`設定によってルートのために使用するルートクラスを変更できるようになります。

### `url`

*デフォルト*: `/`

`url`設定は現在のリクエストのために使われるルートに対して
やってくるURLがマッチしなければならないパターンです。

パターンは複数のセグメントで構成されます:

 * 変数([コロン `:`](#chapter_05-Factories_sub_variable_prefixes)を接頭辞とする単語)
 * 定数
 * キー/値の組のシーケンスにマッチするワイルドカード(`*`)

それぞれのセグメントはあらかじめ定義された区切り文字の1つで区切らなければなりません
([デフォルトでは`/`もしくは`.`](#chapter_05-Factories_sub_segment_separators))。

### `params`

*デフォルト*: 空の配列

`params`設定はルートに関連付けされたパラメータの配列を定義します。
これらは`url`に含まれる変数、もしくはこのルートに関連する変数
のためのデフォルト値になることができます。

### `param`

*デフォルト*: 空の配列

この設定は`params`設定と同等です。

### `options`

`*デフォルト*: 空の配列

`options`設定はさらに振る舞いをカスタマイズするためにルートオブジェクトに渡すオプションの配列です。
次のセクションでそれぞれのルートクラスで利用可能なオプションを説明します。

### `requirements`

*デフォルト*: 空の配列

`requirements`設定は`url`変数で満たさなければならない要件の配列です。
キーはurl変数で変数は変数の値が
マッチしなければならない正規表現です。

>**TIP**
>正規表現は別の正規表現に含められ、
>そういうものとして、区切り文字でこれらを囲んだり、
>値全体をマッチするように`^`もしくは`$`をくっつける
>必要はありません。

### `type`

*デフォルト*: `null`

`collection`にセットされている場合、ルートはルートコレクションとして読み込まれます。

>**NOTE**
>`class`の名前に`Collection`の単語が含まれる場合
>この設定は設定ハンドラによって自動的に`collection`にセットされます。
>このことは大抵の場合この設定を使う必要のないことを意味します。

`sfRoute`
---------

すべてのルートクラスは`sfRoute`基底クラスを継承します。
この基底クラスはルートを設定するために必須の設定を提供します。

`sfRequestRoute`
------------------

### `sf_method`

*デフォルト*: `get`

`sf_method`オプションは`requirements`配列で使われます。
これはルートにマッチする処理でHTTPリクエストを強制します。

`sfObjectRoute`
-----------------

`sfObjectRoute`の次のすべてのオプションは
`routing.yml`設定ファイルの`options`設定内部になければなりません。

### `model`

`model`オプションは必須で現在のルートに関連する
モデルクラスの名前です。

### `type`

`type`オプションは必須でモデルのために必要なルートの種類です; 
これは`object`もしくは`list`のどちらかになります。`object`型のルートは
単独のモデルオブジェクトを表し、`list`型のルートは
モデルオブジェクトのコレクションを表します。

### `method`

`type`オプションは必須です。このルートに関連するオブジェクトを読み取るために
モデルクラスで呼び出すメソッドはこれです。これはstaticメソッドでなければなりません。
このメソッドは引数として解析されるルートのパラメータで呼び出されます。

### `allow_empty`

*デフォルト*: `true`

`allow_empty`オプションが`false`にセットされる場合、
`model` `method`への呼び出しによってオブジェクトが返されない場合、ルートは404の例外を投げます。

### `convert`

*デフォルト*: `toParams`

`convert`オプションはこのモデルオブジェクトに基づいてルートを生成するために
モデルを適切なパラメータの配列に変換するために呼び出すメソッドです。 
これは少なくともルートパターンの必須パラメータを持つ配列を
返さなければなりません(`url`設定で定義される)。

`sfPropelRoute`
-----------------

### `method_for_criteria`

*デフォルト*: コレクションには`doSelect`、単独のオブジェクトには`doSelectOne`

`method_for_criteria`オプションは現在のリクエストに関連するオブジェクトを
読み取るためにPeerクラスで呼び出されるメソッドを定義します。
メソッドは引数として解析されるルートのパラメータで呼び出されます。

`sfDoctrineRoute`
-------------------

### `method_for_query`

*デフォルト*: none

`method_for_query`オプションは現在のリクエストに
関連するオブジェクトを読み取るために
モデルを呼び出すメソッドを定義します。 
現在のクエリオブジェクトは引数として渡されます。

オプションがセットされていない場合、クエリは`execute()`メソッドで
"実行"されるだけです。

`sfRouteCollection`
---------------------

`sfRouteCollection`基底クラスはルートのコレクションを表します。

`sfObjectRouteCollection`
---------------------------

### `model`

`model`オプションは必須で
現在のルートに関連するモデルクラスの名前です。

### `actions`

*デフォルト*: `false`

`actions`オプションはルート用に認可されるアクションの配列を定義します。
アクションはすべての利用可能なアクションのサブセット: `list`、`new`、`create`、
`edit`、`update`、`delete`、と`show`でなければなりません。

オプションが`false`にセットされている場合、デフォルトでは、`with_show`オプションが
`false`にセットされている場合`show`アクション以外のすべてのアクションが
利用可能になります(下記を参照)。

### `module`

*デフォルト*: ルートの名前

`module`オプションはモジュールの名前を定義します。

### `prefix_path`

`*デフォルト*: ルートの名前の後に続く`/`

`prefix_path`オプションはすべての`url`パターンの先頭に追加される接頭辞を追加します。
これは任意の有効なパターンになり変数と複数のセグメントを含むことができます。

### `column`

*デフォルト*: `id`

`column`オプションはモデルオブジェクト用のユニークな
識別子として使用するモデルのカラムを定義します。

### `with_show`

*デフォルト*: `true`

`show`アクションはルート用に認可されたアクションに含めなければならないか決定するために
`with_show`オプションは`actions`オプションが
`false`にセットされるときに使われます。

### `segment_names`

*デフォルト*: `array('edit' => 'edit', 'new' => 'new'),`

`segment_names`は`edit`と`new`アクション用に
`url`パターンで使う単語を定義します。

### `model_methods`

*デフォルト*: 空の配列

`model_methods`オプションはモデルからオブジェクトを読み取るために
呼び出すメソッドを定義します(`sfObjectRoute`の`method`オプションを参照)。
これは実際には`list`と`object`メソッドを定義する配列です:

    [yml]
    model_methods:
      list:   getObjects
      object: getObject

### `requirements`

*デフォルト*: `column`に対して`\d+`

`requirements`オプションはルート変数に適用する
要件の配列を定義します。

### `with_wildcard_routes`

*デフォルト*: `false`

`with_wildcard_routes`オプションは2つのワイルドカードのルートを通して
アクションにアクセスできるようにします: 1つは単独のオブジェクトに、もう1つはオブジェクトコレクションに。

### `route_class`

*デフォルト*: `sfObjectRoute`

`route_class`オプションはコレクション用に使われるデフォルトのルートオブジェクトを
オーバーライドできます。

### `collection_actions`

*デフォルト*: 空の配列

`collection_actions`オプションはコレクションルートに利用可能な
追加アクションの配列を定義します。

### `object_actions`

*デフォルト*: 空の配列

`object_actions`オプションはオブジェクトルートに利用可能な
追加アクションの配列を定義します。

`sfPropelRouteCollection`
---------------------------

`sfPropelRouteCollection`ルートクラスは`sfRouteCollection`を継承し、
デフォルトのルートクラスを`sfPropelRoute`に変更します(上記の`route_class`
オプションを参照)。

`sfDoctrineRouteCollection`
-----------------------------

`sfDoctrineRouteCollection`ルートクラスは`sfRouteCollection`を継承し、
デフォルトのルートクラスを`sfDoctrineRoute`に変更します(上記の
`route_class`オプションを参照)。
