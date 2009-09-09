generator.yml設定ファイル
========================

symfonyのadminジェネレータはモデルクラス用のバックエンドインターフェイスの作成を可能にします。
PropelもしくはDoctrineをORMとして使うことでこれは機能します。

### 作成

adminジェネレータモジュールは`propel:generate-admin`もしくは
`doctrine:generate-admin`タスクによって作成されます:

    $ php symfony propel:generate-admin backend Article

    $ php symfony doctrine:generate-admin backend Article

上記のコマンドは`Article`モデルクラス用の`article` adminジェネレータモジュールを作成します。

>**NOTE**
>`generator.yml`設定ファイルはPHPファイルとしてキャッシュされます; 
>処理は`sfGeneratorConfigHandler`
>[クラス](#chapter_14-Other-Configuration-Files_config_handlers_yml)によって自動的に管理されます。

### 設定ファイル

次のようなモジュールの設定は
`apps/backend/modules/job/article/generator.yml`ファイルで行います:

    [yml]
    generator:
      class: sfPropelGenerator
      param:
        # パラメータの配列

ファイルは2つのメインエントリ: `class`と`param`を格納します。
クラスはPropelに対して`sfPropelGenerator`でDoctrineに対しては`sfDoctrineGenerator`です。

`param`エントリは生成モジュール用の設定オプションを格納します。
`model_class`はこのモジュールにバインドされたモデルクラスを定義し、
`theme`オプションはデフォルトで使用するテーマを定義します。

しかしメインの構成は`config`エントリの下にあります。
これは7つのセクションに編成されます:

  * `actions`: リストとフォームで見つかるアクションのデフォルト設定
  * `fields`:  フィールド用のデフォルト設定
  * `list`:    リスト用の設定
  * `filter`:  フィルタ用の設定
  * `form`:    新規ページ/編集フォーム用の設定
  * `edit`:    編集ページ用固有の設定
  * `new`:     新規ページ用固有の設定

最初に生成されるとき、すべてのセクションは空なものとして定義されます。
adminジェネレータはすべての実現可能なオプションに対して適切なデフォルトを定義します:

    [yml]
    generator:
      param:
        config:
          actions: ~
          fields:  ~
          list:    ~
          filter:  ~
          form:    ~
          edit:    ~
          new:     ~

`config`エントリを通してadminジェネレータをカスタマイズするために使うことのできる
このドキュメントはすべての利用可能なオプションを説明します。

>**NOTE**
>すべてのオプションはPropelとDoctrineの両方で利用可能で
>言及していなければ同じ動作をします。

### フィールド

多くのオプションが引数としてフィールドのリストを受け取ります。フィールドは実際のカラム名もしくは
仮想的な名前になります。両方のケースにおいてゲッターは
モデルクラスで定義しなければなりません(`get`の後にキャメルケースのフィールド名が続く)。

adminジェネレータは賢いのでコンテキストに基づいてフィールドをレンダリングする方法を知っています。
レンダリングをカスタマイズするには、パーシャルもしくはコンポーネントを作ります。
慣習では、パーシャルには接頭辞として(`_`)を、
コンポーネントには接頭辞としてチルダ(``)をつけます:

    [yml]
    display: [_title, ~content]

上記の例において、`title`フィールドは`title`パーシャルによってレンダリングされ、
`content`フィールドは`content`コンポーネントによってレンダリングされます。

adminジェネレータはパーシャルとコンポーネントに対していくつかのパラメータを渡します:

  * `new`と`edit`ページに対して:

    * `form`:       現在のモデルオブジェクトに関連づけされているフォーム
    * `attributes`: ウィジェットに適用されるHTML属性の配列

  * `list`ページに対して:

    * `type`:       `list`
    * `MODEL_NAME`: 現在のオブジェクトのインスタンス、`MODEL_NAME`は
                    は子文字版のモデルクラスの名前。

`edit`もしくは`new`ページにおいて、2つのカラムレイアウトを維持したい場合(フィールドラベル
とウィジェット)、パーシャルもしくはコンポーネントテンプレートは
次のテンプレートに従います:

    [php]
    <div class="sf_admin_form_row">
      <label>
        <!-- Field label or content to be displayed in the first column -->
      </label>
      <!-- Field widget or content to be displayed in the second column -->
    </div>

### オブジェクトプレースホルダー

オプションの中にはモデルオブジェクトプレースホルダーを受け取ることができるものがあります。
プレースホルダーは`%%NAME%%`のパターンに従う文字列です。 
`NAME`の文字列は妥当なオブジェクトのゲッターメソッドの名前に使われる文字列になります。
(`get`の後にキャメルケースバージョンの`NAME`文字列が続く)。 
たとえば、`%%title%%`は`$article->getTitle()`の値に置き換えられます。
現在のコンテキストに関連するオブジェクトに従ってプレースホルダーの値は実行時に
動的に置き換えられます。

>**TIP**
>モデルが別のモデルへの外部キーを持つとき、PropelとDoctrineは
>関連オブジェクト用のゲッターを定義します。他のゲッターに関して、
>オブジェクトを文字列に変換する意味のある`__toString()`メソッドを
>定義していればプレースホルダーとして使うことができます。

### 設定の継承

adminジェネレータの設定は設定カスケードの原則に
基づきます。継承ルールは次の通りです:

 * `new`と`edit`は`form`を継承し`form`は`fields`を継承する
 * `list`は`fields`を継承する
 * `filter`は`fields`を継承する

### クレデンシャル

`credential`オプション(下記を参照)を使用するユーザークレデンシャルに基づいて、
(リストとフォーム上の)adminジェネレータのアクションを隠すことができます。
しかしながら、リンクもしくはボタンが現れない場合、違法なアクセスからアクションが
適切にセキュアな状態でなければなりません。adminジェネレータの
クレデンシャル管理機能は表示のみを考慮します。

`credential`オプションはlistページのカラムを隠すためにも使うことができます。

### アクションのカスタマイズ

設定が十分ではないとき、生成メソッドを上書きできます:

 | メソッド               | 説明
 | ---------------------- | -------------------------------------
 | `executeIndex()`       | `list`ビューアクション
 | `executeFilter()`      | フィルタを更新する
 | `executeNew()`         | `new`ビューアクション
 | `executeCreate()`      | 新しいJobを作成する
 | `executeEdit()`        | `edit`ビューアクション
 | `executeUpdate()`      | Jobを更新する
 | `executeDelete()`      | Jobを削除する
 | `executeBatch()`       | バッチアクションを実行する
 | `executeBatchDelete()` | `_delete`バッチアクションを実行する
 | `processForm()`        | Jobフォームを処理する
 | `getFilters()`         | 現在のフィルタを返す
 | `setFilters()`         | フィルタをセットする
 | `getPager()`           | listページャを返す
 | `getPage()`            | ページャページを取得する
 | `setPage()`            | ページャページをセットする
 | `buildCriteria()`      | list用に`Criteria`をビルドする
 | `addSortCriteria()`    | list用にソートの`Criteria`を追加する
 | `getSort()`            | 現在のソートカラムを返す
 | `setSort()`            | 現在のソートカラムをセットする

### テンプレートのカスタマイズ

それぞれの生成テンプレートを上書きできます:

 | テンプレート                  | 説明
 | ---------------------------- | -------------------------------------
 | `_assets.php`                | テンプレートに使うCSSとJSをレンダリングする
 | `_filters.php`               | フィルタボックスをレンダリングする
 | `_filters_field.php`         | 単独のフィルタフィールドをレンダリングする
 | `_flashes.php`               | flashメッセージをレンダリングする
 | `_form.php`                  | フォームを表示する
 | `_form_actions.php`          | フォームのアクションを表示する
 | `_form_field.php`            | 単独のフォームフィールドを表示する
 | `_form_fieldset.php`         | フォームのフィールドセットを表示する
 | `_form_footer.php`           | フォームのフッターを表示する
 | `_form_header.php`           | フォームのヘッダーを表示する
 | `_list.php`                  | listを表示する
 | `_list_actions.php`          | listアクションを表示する
 | `_list_batch_actions.php`    | listバッチアクションを表示する
 | `_list_field_boolean.php`    | listの単独の論理型フィールドを表示する
 | `_list_footer.php`           | listのフッターを表示する
 | `_list_header.php`           | listのヘッダーを表示する
 | `_list_td_actions.php`       | 列用のオブジェクトアクションを表示する
 | `_list_td_batch_actions.php` | 列用のチェックボックスを表示する
 | `_list_td_stacked.php`       | 列用のstackedレイアウトを表示する
 | `_list_td_tabular.php`       | list用の単独フィールドを表示する
 | `_list_th_stacked.php`       | ヘッダー用の単独のカラム名を表示する
 | `_list_th_tabular.php`       | ヘッダー用の単独のカラム名を表示する
 | `_pagination.php`            | listページ分割を表示する
 | `editSuccess.php`            | `edit`ビューを表示する
 | `indexSuccess.php`           | `list`ビューを表示する
 | `newSuccess.php`             | `new`ビューを表示する

### 外見のカスタマイズ

生成テンプレートは多くの`class`と`id`属性を定義するので
adminジェネレータの外見はとても簡単に調整できます。

`edit`もしくは`new`ページにおいて、それぞれのフィールドのHTMLコンテナは
次のクラスを持ちます:

  * `sf_admin_form_row`
  * フィールドの型に依存するクラス: `sf_admin_text`、`sf_admin_boolean`、
    `sf_admin_date`、`sf_admin_time`、もしくは`sf_admin_foreignkey`。
  * `sf_admin_form_field_COLUMN`。`COLUMN`がカラムの名前です。

`list`ページにおいて、それぞれのフィールドのHTMLコンテナは次のクラスを持ちます:

  * フィールドの型に依存するクラス: `sf_admin_text`、`sf_admin_boolean`、
    `sf_admin_date`、`sf_admin_time`、もしくは`sf_admin_foreignkey`。
  * `sf_admin_form_field_COLUMN`。`COLUMN`がカラムの名前です。

<div class="pagebreak"></div>

利用可能な設定オプション
-----------------------

 * [`actions`](#chapter_06-Admin-Generator_sub_actions)

   * [`name`](#chapter_06-Admin-Generator_sub_name)
   * [`action`](#chapter_06-Admin-Generator_sub_action)
   * [`credentials`](#chapter_06-Admin-Generator_sub_credentials)

 * [`fields`](#chapter_06-Admin-Generator_sub_fields)

   * [`label`](#chapter_06-Admin-Generator_sub_label)
   * [`help`](#chapter_06-Admin-Generator_sub_help)
   * [`attributes`](#chapter_06-Admin-Generator_sub_attributes)
   * [`credentials`](#chapter_06-Admin-Generator_sub_credentials)
   * [`renderer`](#chapter_06-Admin-Generator_sub_renderer)
   * [`renderer_arguments`](#chapter_06-Admin-Generator_sub_renderer_arguments)

 * [`list`](#chapter_06-Admin-Generator_sub_list)

   * [`title`](#chapter_06-Admin-Generator_sub_title)
   * [`display`](#chapter_06-Admin-Generator_sub_display)
   * [`hide`](#chapter_06-Admin-Generator_sub_hide)
   * [`layout`](#chapter_06-Admin-Generator_sub_layout)
   * [`params`](#chapter_06-Admin-Generator_sub_params)
   * [`sort`](#chapter_06-Admin-Generator_sub_sort)
   * [`max_per_page`](#chapter_06-Admin-Generator_sub_max_per_page)
   * [`pager_class`](#chapter_06-Admin-Generator_sub_pager_class)
   * [`batch_actions`](#chapter_06-Admin-Generator_sub_batch_actions)
   * [`object_actions`](#chapter_06-Admin-Generator_sub_object_actions)
   * [`actions`](#chapter_06-Admin-Generator_sub_actions)
   * [`peer_method`](#chapter_06-Admin-Generator_sub_peer_method)
   * [`peer_count_method`](#chapter_06-Admin-Generator_sub_peer_count_method)
   * [`table_method`](#chapter_06-Admin-Generator_sub_table_method)
   * [`table_count_method`](#chapter_06-Admin-Generator_sub_table_count_method)

 * [`filter`](#chapter_06-Admin-Generator_sub_filter)

   * [`display`](#chapter_06-Admin-Generator_sub_display)
   * [`class`](#chapter_06-Admin-Generator_sub_class)

 * [`form`](#chapter_06-Admin-Generator_form)

   * [`display`](#chapter_06-Admin-Generator_sub_display)
   * [`class`](#chapter_06-Admin-Generator_sub_class)

 * [`edit`](#chapter_06-Admin-Generator_sub_edit)

   * [`title`](#chapter_06-Admin-Generator_sub_title)
   * [`actions`](#chapter_06-Admin-Generator_sub_actions)

 * [`new`](#chapter_06-Admin-Generator_sub_new)

   * [`title`](#chapter_06-Admin-Generator_sub_title)
   * [`actions`](#chapter_06-Admin-Generator_sub_actions)

<div class="pagebreak"></div>

`fields`
--------

`fields`セクションはそれぞれのフィールドに対するデフォルト設定を定義します。
この設定はすべてのページに対して定義されページごとに
オーバーライドできます(`list`、`filter`、`form`、`edit`、と`new`)。

### `label`

*デフォルト*: 人間にわかりやすいカラムの名前

`label`オプションはフィールドに使うラベルを定義します:

    [yml]
    config:
      fields:
        slug: { label: "URL shortcut" }

### `help`

*デフォルト*: none

`help`オプションはフィールド用に表示するヘルプテキストを定義します。

### `attributes`

*デフォルト*: `array()`

`attributes`オプションはウィジェットに渡すHTML属性を定義します:

    [yml]
    config:
      fields:
        slug: { attributes: { class: foo } }

### `credentials`

*デフォルト*: none

`credentials`オプションは表示するフィールドに対してユーザーが持たなければならない
クレデンシャルを定義します。クレデンシャルはオブジェクトのリストに対してのみ強制されます。

    [yml]
    config:
      fields:
        slug:      { credentials: [admin] }
        is_online: { credentials: [[admin, moderator]] }

>**NOTE**
>クレデンシャルは
>`security.yml`設定ファイルと同じルールで定義されます。

### `renderer`

*デフォルト*: none

`renderer`オプションはフィールドをレンダリングするために呼び出すPHPのコールバックを定義します。
定義されていれば、パーシャルもしくはコンポーネントのように他のフラグをオーバーライドします。

コールバックは`renderer_arguments`オプションで
定義されたフィールドと引数の値で呼び出されます。

### `renderer_arguments`

*デフォルト*: `array()`

`renderer_arguments`オプションはフィールドをレンダリングする際に
PHPの`renderer`コールバックに渡す引数を定義します。 
`renderer`オプションが定義される場合のみ使われます。

`actions`
---------

フレームワークはいくつかの組み込みのアクションを定義します。
これらすべてに接頭辞としてアンダースコア(`_`)がつけられます。 
それぞれのアクションはこのセクションで説明されているオプションでカスタマイズできます。 
同じオプションは`list`、`edit`、もしくは`new`エントリでアクションを
定義する際に使うことができます。

### `name`

*デフォルト*: アクションのキー

`name`オプションはアクションに使うラベルを定義します。

### `action`

*デフォルト*: アクションの名前に基づいて定義されます。

`action`オプションは接頭辞の`execute`無しで実行するアクションの名前を定義します。

### `credentials`

*デフォルト*: none

`credentials`オプションは表示するアクションに対してユーザーが
持たなければならないクレデンシャルを定義します。

>**NOTE**
>クレデンシャルは
>`security.yml`設定ファイルと同じルールで定義されます。

`list`
------

### `title`

*デフォルト*: 接尾辞の"List"がつけられた人間にわかりやすいモデルクラスの名前

`title`オプションはlistページのタイトルを定義します。

### `display`

*デフォルト*: すべてのモデルのカラム、スキーマファイルでの定義順

`display`オプションはlistで表示する順序付きカラムの配列を
定義します。

カラム前の等号(`=`)は文字列を現在のオブジェクトの`edit`ページに向かうリンクに
変換する規約です。

    [yml]
    config:
      list:
        display: [=name, slug]

>**NOTE**
>カラムを隠す`hide`オプションもご覧ください。

### `hide`

*デフォルト*: none

`hide`オプションはlistから隠すカラムを定義します。
カラムを隠すのに`display`オプションで表示されるカラムを指定するよりも、 
こちらの方が速いことがあります:

    [php]
    config:
      list:
        hide: [created_at, updated_at]

>**NOTE**
>`display`と`hide`オプションが両方とも提供される場合、`hide`
>オプションが無視されます。

### `layout`

*デフォルト*: `tabular`

*可能な値*: `tabular`もしくは`stacked`

`layout`オプションはlistを表示するのに使うレイアウトを定義します。

`tabular`レイアウトでは、それぞれのカラムの値は独自テーブルのカラムにあります。

`stacked`レイアウトでは、それぞれのオブジェクトは`params`オプション(下記を参照)で
定義される単独文字列で表現されます。

>**NOTE**
>`display`オプションは`stacked`レイアウトを使う際にも必要です。
>これはユーザーによってソート可能になるカラムを定義するからです。

### `params`

*デフォルト値*: none

`params`オプションは`stacked`レイアウトを使用する際に使うHTML文字列のパターンを定義するために使われます。
この文字列はモデルオブジェクトプレースホルダーを含むことができます:

    [yml]
    config:
      list:
        params:  |
          %%title%% written by %%author%% and published on %%published_at%%.

カラムの前の等号(`=`)は文字列を
現在のオブジェクトの`edit`ページに向かうリンクに変換する規約です。

### `sort`

*デフォルト値*: none

`sort`オプションはデフォルトのsortカラムを定義します。これは2つのコンポーネント
から構成されます: カラムの名前とソートの順序: `asc`もしくは`desc`:

    [yml]
    config:
      list:
        sort: [published_at, desc]

### `max_per_page`

*デフォルト値*: `20`

`max_per_page`オプションは1つのページを表示するオブジェクトの
最大数を定義します。

### `pager_class`

*デフォルト値*: Propelでは`sfPropelPager`、Doctrineでは`sfDoctrinePager`

`pager_class`オプションはlistを表示する際に使用する
ページャクラスを定義します。

### `batch_actions`

*デフォルト値*: `{ _delete: ~ }`

`batch_actions`オプションはlistのオブジェクト選択用に実行できるアクションのリスト
を定義します。

`action`を定義しない場合、adminジェネレータは
接頭辞が`executeBatch`であるキャメルケースの名前のメソッドを探します。

実行されるメソッドは`ids`リクエストパラメータを通して
選択されたオブジェクトの主キーを受け取ります。

>**TIP**
>バッチアクションの機能はオプションを
>空の配列: `{}`にセットすることで無効にできます。

### `object_actions`

*デフォルト値*: `{ _edit: ~, _delete: ~ }`

`object_actions`オプションはlistのそれぞれのオブジェクトで実行可能な
アクションのリストを定義します。

`action`を定義しない場合、adminジェネレータは
接頭辞が`executeList`であるキャメルケースの名前のメソッドを探します。

>**TIP**
>オブジェクトアクションの機能はオプションを
>空の配列: `{}`にセットすることで無効にできます。

### `actions`

*デフォルト値*: `{ _new: ~ }`

新しいオブジェクトの作成のように、`actions`オプションは
オブジェクトを受け取らないアクションを定義します。

`action`を定義しない場合、adminジェネレータは`executeListを接頭辞とするキャメルケースの名前
のメソッドを探します。

>**TIP**
>オブジェクトアクション機能は
>オプションを空の配列: `{}`にセットすることで無効にできます。

### `peer_method`

*デフォルト値*: `doSelect`

`peer_method`オプションはlistで表示するオブジェクトを読み取るために
呼び出すメソッドを定義します。

>**CAUTION**
>このオプションはPropelに対してのみ存在します。Doctrineに対しては、`table_method`
>オプションを使います。

### `table_method`

*デフォルト値*: `doSelect`

`table_method`オプションはlistで表示するオブジェクトを読み取るために
呼び出すメソッドを定義します。

>**CAUTION**
>このオプションはDoctrineに対してのみ存在します。Propelに対しては、`peer_method`
>オプションを使います。

### `peer_count_method`

*デフォルト値*: `doCount`

`peer_count_method`オプションは現在のフィルタ用のオブジェクトの個数を
算出するために呼び出すメソッドを定義します。

>**CAUTION**
>このオプションはPropelに対してのみ存在します。Doctrineに対しては、
>`table_count_method`オプションを使います。

### `table_count_method`

*デフォルト値*: `doCount`

`table_count_method`オプションは現在のフィルタ用のオブジェクトの個数を算出するために
呼び出すメソッドを定義します。

>**CAUTION**
>このオプションはDoctrineに対してのみ存在します。Propelに対しては、
>`peer_count_method`オプションを使います。

`filter`
--------

`filter`セクションはlistページに表示されるフォームをフィルタリングするための設定を
定義します。

### `display`

*デフォルト値*: 定義の順序で、フィルタフォームクラスで定義されたすべてのフィールド。

`display`オプションは表示するフィールドの順序付きリストを定義します。

>**TIP**
>フィルタフィールドは常にオプションで、表示するフィールドを設定するために
>フィルタフォームクラスをオーバーライドする必要はありません。

### `class`

*デフォルト値*: 接尾辞が`FormFilter`であるモデルクラスの名前

`class`オプションは`filter`フォームに使用するフォームクラスを定義します。

>**TIP**
>フィルタリング機能を完全に除外するには、`class`を`false`にセットします。

`form`
------

`form`セクションは`edit`と`new`セクションのためのフォールバックとしてのみ存在します
(最初の継承ルールを参照)。

>**NOTE**
>フォームセクション(`form`、`edit`、と`new`)に関して、`label`と`help`オプションは
>フォームクラスで定義されたものをオーバーライドします。

### `display`

*デフォルト値*: フォームクラスで定義されたすべてのクラス。順序は定義された順序と同じ。

`display`オプションは表示するフィールドの順序付きリストを定義します。

このオプションはフィールドをグループに分類するためにも使うことができます:

    [yml]
    # apps/backend/modules/job/config/generator.yml
    config:
      form:
        display:
          Content: [title, body, author]
          Admin:   [is_published, expires_at]

上記の構成は2つのグループ(`Content`と`Admin`)を定義します。
それぞれがフォームフィールドのサブセットを含みます。

>**CAUTION**
>モデルフォームで定義されるすべてのフィールドは`display`オプションに存在しなければなりません。
>そうではない場合、予期しないバリデーションエラーになる可能性があります。

### `class`

*デフォルト値*: 接尾辞が`Form`であるモデルクラスの名前

`class`オプションは`edit`と`new`ページに使うフォームクラスを定義します。

>**TIP**
>`new`と`edit`セクションの両方で`class`オプションを定義できますが、
>1つのクラスを使用し条件ロジックを使用し違いを
>考慮する方がベターです。

`edit`
------

`edit`セクションは`form`セクションと同じオプションを受け取ります。

### `title`

*デフォルト*: 接尾辞が"Edit"である人間にわかりやすいモデルクラスの名前

`title`オプションはeditページのタイトルの見出しを定義します。
これはモデルオブジェクトのプレースホルダーを格納できます。

### `actions`

*デフォルト値*: `{ _delete: ~, _list: ~, _save: ~ }`

`actions`オプションはフォームを投稿する際に利用可能なアクションを定義します。

`new`
-----

`new`セクションは`form`セクションと同じオプションを受け取ります。

### `title`

*デフォルト*: 接尾辞が"New"である人間にわかりやすいモデルクラスの名前

`title`オプションは新しいページのタイトルを定義します。
これはモデルオブジェクトのプレースホルダーを格納できます。

>**TIP**
>オブジェクトが新しい場合でも、タイトルの一部として出力したい
>デフォルトの値を格納できます。

### `actions`

*デフォルト値*: `{ _delete: ~, _list: ~, _save: ~, _save_and_add: ~ }`

`actions`オプションはフォームを投稿する際に利用可能なアクションを定義します。
