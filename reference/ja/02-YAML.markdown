YAMLフォーマット
===============

[YAML](http://yaml.org/)の公式Webサイトによれば、YAMLは"すべてのプログラミング言語の
ための人間にフレンドリなデータシリアライズ標準"です。

YAMLフォーマットは複雑な入れ子のデータ構造を記述できますが、
この章ではYAMLを設定ファイルのフォーマットとして使うために
必要な最小限の内容のみを説明します。

YAMLはデータを記述するシンプルな言語です。PHPに関して、文字列、論理型、浮動小数点、
もしくは整数のようなシンプルな型のための構文を持ちます。しかしPHPとは異なり、
配列(シーケンス)とハッシュ(マッピング)の間に違いがあります。

スカラー
-------

スカラー用の構文はPHP構文と似ています。

### 文字列

    [yml]
    A string in YAML

-

    [yml]
    'A singled-quoted string in YAML'

>**TIP**
>シングルクォートで囲まれた文字列では、シングルクォートの`'`を2倍にしなければなりません:
>
>     [yml]
>     'A single quote '' in a single-quoted string'

    [yml]
    "A double-quoted string in YAML\n"

文字列が1つ以上の適切なスペースで始まるもしくは終わるときに
シングルクォートスタイルは便利です。

>**TIP**
>ダブルクォートのスタイルは`\`エスケープシーケンスを使用して
>任意の文字列を表現する方法も提供します。
>文字列に`\n`もしくはユニコードを埋め込むことが必要なときにとても便利です。

文字列が改行を含むとき、パイプ(`|`)によって示されるリテラルスタイルを使うことができます。
これは複数行にまたがって空白を置く文字列を示します。 
リテラルにおいて、改行は維持されます:

    [yml]
    |
      \/ /| |\/| |
      / / | |  | |__

代わりの方法として、文字列を`>`で示される折り畳みスタイルで書くことができます。
それぞれの改行はスペースに置き換えられます:

    [yml]
    >
      This is a very long sentence
      that spans several lines in the YAML
      but which will be rendered as a string
      without carriage returns.

>**NOTE**
>以前の例ではそれぞれの行の前にある2つのスペースに注目してください。
>これらは結果のPHP文字列には現れません。

### 数字

    [yml]
    # an integer
    12

-

    [yml]
    # an octal
    014

-

    [yml]
    # an hexadecimal
    0xC

-

    [yml]
    # a float
    13.4

-

    [yml]
    # an exponential number
    1.2e+34

-

    [yml]
    # infinity
    .inf

### null

YAMLでのnullは`null`もしくは`~`で表現されます。

### 論理型

YAMLでの論理型は`true`と`false`で表現されます。

>**NOTE**
>Symfony YAMLパーサーは`on`、`off`、`yes`、と`no`
>も認識しますが1.2 YAMLの仕様から除外されているので
>これらを使うことは非推奨です。

### 日付

YAMLは日付の表現にISO-8601標準を使用します:

    [yml]
    2001-12-14t21:59:43.10-05:00

-

    [yml]
    # simple date
    2002-12-14

コレクション
------------

シンプルなスカラーを記述するためにYAMLファイルが使われることは滅多にありません。
大抵の場合、これはコレクションを記述します。コレクションはシーケンスもしくは要素のマッピングになります。
シーケンスとマッピングは両方ともPHP配列に変換されます。

シーケンスはダッシュと直後のスペース(`- `)を使います:

    [yml]
    - PHP
    - Perl
    - Python

前のYAMLファイルは次のPHPコードと同等です:

    [php]
    array('PHP', 'Perl', 'Python');

マッピングはそれぞれキー/値の組をマークするためにカラムとスペース(`: `)を使います:

    [yml]
    PHP: 5.2
    MySQL: 5.1
    Apache: 2.2.20

上記のコードは次のPHPコードと同等です:

    [php]
    array('PHP' => 5.2, 'MySQL' => 5.1, 'Apache' => '2.2.20');

>**NOTE**
>マッピングにおいて、キーは有効な任意のスカラーになることができます。

コロンと値の間のスペースは問題ありません:

    [yml]
    PHP:    5.2
    MySQL:  5.1
    Apache: 2.2.20

ネストしたコレクションを記述するためにYAMLは1つもしくは複数のスペースでインデントを使います:

    [yml]
    "symfony 1.0":
      PHP:    5.0
      Propel: 1.2
    "symfony 1.2":
      PHP:    5.2
      Propel: 1.3

次のYAMLは次のPHPコードと同等です:

    [php]
    array(
      'symfony 1.0' => array(
        'PHP'    => 5.0,
        'Propel' => 1.2,
      ),
      'symfony 1.2' => array(
        'PHP'    => 5.2,
        'Propel' => 1.3,
      ),
    );

YAMLファイルでインデントを使うとき覚える必要のあることは
1つあります: *インデントは1つか複数のスペースで行い、
タブを使ってはなりません*。

次のようにシーケンスとマッピングをネストできます:

    [yml]
    'Chapter 1':
      - Introduction
      - Event Types
    'Chapter 2':
      - Introduction
      - Helpers

スコープを表現するためにインデントよりも明示的なインディケータを使って、
YAMLはコレクション用にフロースタイルを使うこともできます。

シーケンスは角かっこ(`[]`)の範囲内でカンマで区切られたリスト
として記述できます:

    [yml]
    [PHP, Perl, Python]

マッピングは波かっこ(`{}`)の範囲内のカンマで
区切られたキー/値として記述されます:

    [yml]
    { PHP: 5.2, MySQL: 5.1, Apache: 2.2.20 }

よりよい可読性を実現するためにスタイルを混合させることができます:

    [yml]
    'Chapter 1': [Introduction, Event Types]
    'Chapter 2': [Introduction, Helpers]

-

    [yml]
    "symfony 1.0": { PHP: 5.0, Propel: 1.2 }
    "symfony 1.2": { PHP: 5.2, Propel: 1.3 }

コメント
--------

ハッシュ記号(`#`)をコメントの先頭につけることでYAMLにコメントを追加できます:

    [yml]
    # Comment on a line
    "symfony 1.0": { PHP: 5.0, Propel: 1.2 } # Comment at the end of a line
    "symfony 1.2": { PHP: 5.2, Propel: 1.3 }

>**NOTE**
>コメントはYAMLパーサーによって無視され
>コレクションのネストの現在のレベルにしたがってインデントされます。

動的なYAMLファイル
------------------

symfonyにおいて、解析直前に評価されるYAMLファイルにPHPコードを
含めることができます:

    [php]
    1.0:
      version: <?php echo file_get_contents('1.0/VERSION')."\n" ?>
    1.1:
      version: "<?php echo file_get_contents('1.1/VERSION') ?>"

インデントで散らからないように気をつけてください。PHPコードをYAMLファイルに
追加するとき次のシンプルなティップを思い出してください:

 * `<?php ?>`ステートメントは行で始めるもしくは値に
   埋め込まなければなりません。

 * `<?php ?>`ステートメントが1行で終わるとき、
   改行("\n")を明示的に出力する必要があります。

<div class="pagebreak"></div>

すべての例
----------

次の例はこのドキュメントで説明したほとんどのYAML表記を
記述しています:

    [yml]
    "symfony 1.0":
      end_of_maintainance: 2010-01-01
      is_stable:           true
      release_manager:     "Grégoire Hubert"
      description: >
        This stable version is the right choice for projects
        that need to be maintained for a long period of time.
      latest_beta:         ~
      latest_minor:        1.0.20
      supported_orms:      [Propel]
      archives:            { source: [zip, tgz], sandbox: [zip, tgz] }

    "symfony 1.2":
      end_of_maintainance: 2008-11-01
      is_stable:           true
      release_manager:     'Fabian Lange'
      description: >
        This stable version is the right choice
        if you start a new project today.
      latest_beta:         null
      latest_minor:        1.2.5
      supported_orms:
        - Propel
        - Doctrine
      archives:
        source:
          - zip
          - tgz
        sandbox:
          - zip
          - tgz
