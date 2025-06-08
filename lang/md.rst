========
Markdown
========

はじめに
========

この文書は Markdown のリファレンスマニュアルです。

Thothglyph 方言
---------------

Thothglyph で使用可能な Markdown は `MyST <https://myst-parser.readthedocs.io/en/latest/>`_ をベースに独自の構文を加えた方言です。
MyST は Sphinx で使用可能な Markdown の方言の一つです。

マニュアルの中で Thothglyph 独自の構文は "(Thothglyph 方言)" と表記してあります。

Preprocess
==========

Thothglyph は Markdown ドキュメントを構文解析する前に簡単なプリプロセスを実行します。

Config
------

(Thothglyph 方言)

``---``\ のみの行で囲まれたテキストは Config ブロックとなります。
ドキュメントの最初のみ記述可能な構文です。
ドキュメントの設定を YAML 言語で記述します。

.. code-block:: none

    ---
    title : 'Document Title'
    version : '1.2.3'
    author : Foo Bar
    attrs : {author : 'Foo Bar'}
    templatedir : './my_template'
    theme : 'preview'  # or 'default'
    ---

先頭の\ ``---``\ に続いて\ ``python``\ を記述すると Python 言語で設定を記述できます。

.. code-block:: none

    ---python
    title = 'Document Title'
    version = '1.2.3'
    author = cmd("git config user.name")
    attrs = {'author': 'Foo Bar'}
    templatedir = './my_template'
    theme = 'preview'  # or 'default'
    ---

以下のように記述すると、外部ファイルをインクルードできます。

.. code-block:: none

    ---{include} ./conf.py python
    ---

ControlFlow
-----------

(Thothglyph 方言)

``%#``\ の後に特定のキーワードを記入すると ControlFlow となります。
ドキュメントの一部分を条件により表示/非表示できます。
サポートしているキーワードは if, elif, end です。
条件には Python 構文が使用でき、Config ブロックで定義した attrs の値を使用できます。

.. code-block:: none

    %#if author == 'Smith'
    Profile about Smith.
    %#elif author == 'Tanaka'
    Profile about Tanaka.
    %#end

Comment
-------

``%//``\ で始まる行はコメント行となります。
コメント行はプリプロセスの段階で文中から削除されます。

.. code-block:: none

    This is main text.
    % This is a comment.

Blocks
======

ドキュメントはブロックの木構造で構成されています。
ブロックは基本的に行単位でまとまっています。

Section
-------

1文字以上の\ ``#``\ の後に空白とテキストを記入すると Section (見出し) となります。
ドキュメントの最も大枠となるブロックです。
``#``\ の文字数が見出しレベルに相当します。

.. code-block:: none

    # Section Lv.1 Title
    ## Section Lv.2 Title
    ### Section Lv.3 Title
    #### Section Lv.4 Title
    ##### Section Lv.5 Title
    ###### Section Lv.6 Title

各見出しにはラベルを付けられます。ラベルは後述の Cross Reference で利用できます。

.. code-block:: none

    (sect1)=
    # Section Title

notoc 属性を記入すると目次に表示されなくなります。
nonum 属性を記入すると見出しの番号付けをスキップします。

.. code-block:: none

    # まえがき (1. まえがき)

    {notoc=1 nonum=1}
    # 目次 (目次)

    # XXとは (2. XXとは)

    # YYとは (3. YYとは)

Paragraph
---------

通常の文字から始まる行は Paragraph (段落) となります。
Paragraph は空行が出現するまで継続します。

.. code-block:: none

    これは段落1のテキストです。
    改行しても段落は継続します。

    これは段落2のテキストです。

Bullet List
-----------

\ ``*``\ または\ ``-``\ と空白から始まるブロックは Bullet List (箇条書きリスト) となります。

.. code-block:: none

    * apple
    * orange
    * grape

行頭に2文字以上の空白を挿入するとリストのレベルを上げられます。

.. code-block:: none

    * List item 1
      * List item 1-1
        * List item 1-1-1
        * List item 1-1-2
      * List item 1-2
        * List item 1-2-1
        * List item 1-2-2
    * List item 2

リストの各アイテムの本文には複数ブロックを記入できます。
行頭のインデントを揃える必要があります。

.. code-block:: none

    * Item 1 paragraph 1.
      new line.

      paragraph 2.

    * Item 2 paragraph 1.
      new line.

      paragraph 2.

リストを終了して別のリストを開始するには、リストと同じ階層で HTML コメント\ ``<!-- -->``\ を記入します。

.. code-block:: none

    * My favorite foods

      * apple
      * orange
      * grape
      <!-- -->
      * sushi
      * tempura
    <!-- -->
    * My favorite sports

Ordered List
------------

数字の後に\ ``.``\ と空白から始まるブロックは Ordered List (順序付きリスト) となります。
数字の値に意味はありません。

.. code-block:: none

    1. List item 1
       1. List item 1-1
          1. List item 1-1-1
          1. List item 1-1-2
       1. List item 1-2
          1. List item 1-2-1
          1. List item 1-2-2
    1. List item 2
    <!-- -->
    1. List item new 1

Description List
----------------

テキストの次の行が\ ``:``\ と空白から始まる行はDescription List (説明リスト) となります。
最初の行は用語、\ ``:``\ で始まる行以降は本文です。

.. code-block:: none

    Term 1
    : List item 1

      Term 1-1
      : List item 1-1

        Term 1-1-1
        : List item 1-1-1
        Term 1-1-2
        : List item 1-1-2

      Term 1-2
      : List item 1-2
    
        Term 1-2-1
        : List item 1-2-1
        Term 1-2-2
        : List item 1-2-2

    Term 2
    : List item 2
    <!-- -->
    Term 1
    : List item new 1

Field List
----------

テキストの先頭が\ ``:``\ で囲まれている行が複数行ある場合 Field List (フィールドリスト) となります。
Field List は入れ子できません。

.. code-block:: none

    :Term 1: List item1
    :Term 2: List item2
    :Term 3: List item3

Check List
----------

Bullet List の先頭が\ ``[ ]``\ と空白から始まるブロックは Check List (チェックリスト) となります。
チェックボックスの状態は\ ``[ ]``\ , \ ``[x]``\ , \ ``[-]``\ の3つを選択できます。
本家 MyST では Ordered List も Check List として使用できますが、Thothglyph 方言では Bullet List のみ使用できます。

.. code-block:: none

    * [ ] List item 1
      * [-] List item 1-1
        * [x] List item 1-1-1
        * [ ] List item 1-1-2
      * [x] List item 1-2
        * [x] List item 1-2-1
        * [x] List item 1-2-2
    * [ ] List item 2
    <!-- -->
    * [x] List item new 1

複合リスト
----------

これまで説明したリストは別種のリストを入れ子にできます。

.. code-block:: none

    * List item 1
      1. List item 1-1
         A
         : List item 1-1-1
         B
         : List item 1-1-2
      1. List item 1-2
         * [x] List item 1-2-1
         * [ ] List item 1-2-2
    * List item 2

Fence
-----

Markdown の Fence 構文は主に Code Block として利用されます。
開始行の\ ```````\ に続けて\ ``{keyword}``\ と記入すると、MyST でいうところの Directive となり、特殊なブロックとして機能します。

Footnote List
-------------

(Thothglyph 方言)

Fence の開始行で\ ``{footnote}``\ と指定すると Footnote List (脚注リスト) となります。
Field List ブロックの書き方で脚注の内容を記入します。
文中の脚注の書き方は :ref:`footnote` 参照。

.. code-block:: none

    ```{footnote}
    :1: This is footnote.
    :2: This is footnote too.
    ```

Reference List
--------------

(Thothglyph 方言)

Fence の開始行で\ ``{reference}``\ と指定すると Reference List (参照リスト) となります。
Field List ブロックの書き方で脚注の内容を記入します。
文中の参照の書き方は :ref:`reference` 参照。

.. code-block:: none

    ```{reference}
    :1: The Awesome Document, 1990, Anonymous.
    :2: The theory of theory, 2000-01-01, Anonymous.
    ```

Basic Table
-----------

Markdown の Table 構文を利用できます。

``:-:``\ で構成された行はヘッダ部とデータ部を分割し、セル内のテキストアライメントを設定します。
ヘッダ部は1行のみ指定可能です。またヘッダ部のないテーブルは作成できません。

.. code-block:: none

    | head11 | head12 | head13 |
    |:-------|:------:|-------:|
    | data11 | data12 | data13 |
    | data21 | data22 | data23 |

(Thothglyph 方言)

Thothglyph 専用の構文として、セルの内容を\ ``:<``\ もしくは\ ``:^``\ で開始することで、セルを結合できます。
\ ``:<``\ は水平方向、\ ``:^``\ は垂直方向に結合します。

.. code-block:: none

    | head11 | head12  | :<      | :<      |
    |--------|---------|---------|---------|
    | data11 | data12  | data13  | data14  |
    | data21 | data22  | :<      | data24  |
    | data31 | :^      | :<      | :^      |
    | data41 | data42  | :<      | data44  |
    | data51 | data52  | data53  |:<data54 |
    | data61 |:<data62 |:<data63 |:<data64 |

Basic Table (Directive)
-----------------------

(Thothglyph 方言)

標準の Markdown の表に関するパースはいくつかの制限があります。

* 複数行のヘッダを記述できない
* ヘッダなしでアライメントの指定ができない

``{table}`` Directive を用いてこれらの問題を解決できます。

.. code-block:: none

    ```{table}
    | head11 | head12 | head13 |
    | head21 | head22 | head23 |
    |:-------|:------:|-------:|
    | data11 | data12 | data13 |
    | data`21` | data22 | data23 |
    | a | b | c |
    ```

.. code-block:: none

    ```{table}
    |:------:|-------:|+-------|
    | data11 | data12 | data13 |
    | A | B | C |
    ```

先頭でオプションを記述できます。

.. code-block:: none

    ```{table}
    :type: long
    :w: 100%
    :widths: 1,2,3
    :fontsize: small
    | data11 | data12 | data13 |
    | data21 | data22 | data23 |
    ```

指定できるオプションは次の通りです。

type
    表のタイプを指定します。long のみ指定できます。
    long を指定すると PDF 形式の出力時にページをまたがる表を生成できます。

w
    表の幅を指定します。単位として pt と % が指定できます。

widths
    表の各列の幅の相対サイズを指定します。

align
    表の各列のアライメントを指定します。l c r x xc xr から指定できます。
    それぞれ左、中央、右、左(幅調整)、中央(幅調整)、右(幅調整)を表します。

colspec
    widths と align を同時に指定できます。
    5l,3c,1r のように指定します。

fontsize
    表全体のフォントサイズを指定します。mediam small x-small から指定できます。

List Table
----------

Fence の開始行で\ ``{list-table}``\ と指定すると List Table となります。
List Table 内はレベル2以上の Bullet List で構成されます。
レベル1の文は無視され、レベル2のリストアイテムが各セルの内容になります。
レベル3のリストは表内のレベル1のリストに置き換わります。

.. code-block:: none

    ```{list-table}
    * - data11
      - data12
        * item1
        * item2
        * item3
      - data13
    * - data21
      - data22
      - data23
    ```

``header-rows``\ オプションでヘッダ部の行数を指定できます。

.. code-block:: none

    ```{list-table}
    :header-rows: 1

    * - head1
      - head2
      - head3
    * - data11
      - data12
      - data13
    * - data23
      - data22
      - data23
    ```

Basic Tableと 同様にセルの内容を\ ``:<``\ もしくは\ ``:^``\ で開始することで、セルを結合できます。

.. code-block:: none

    ```{list-table}
    :header-rows: 1

    * - head1
      - head2
      - :<
    * - data11
      - data12
      - data13
    * - data23
      - :^data22
      - data23
    ```

``align``\ オプションで各列のアライメントを指定できます。

.. code-block:: none

    ```{list-table}
    :align: lcr

    * - data11
      - data12
      - data13
    * - A
      - B
      - C
    ```

Figure
------

Fence の開始行で\ ``{figure}``\ と指定すると Figure ブロックとなります。
図や表にキャプションを付けられます。
実際にキャプションが表示される位置は出力形式やテンプレートに依存します。

.. code-block:: none

    ```{figure} caption
    ![](./tglyph_64.png)
    ```

.. code-block:: none

    ```{figure} caption
    | head11 | head12 | head13 |
    |--------|--------|--------|
    | data11 | data12 | data13 |
    | data21 | data22 | data23 |
    ```

.. code-block:: none

    ```{figure} caption
    Not Image.
    ```

Quote Block
-----------

未対応

Literal Include Block
---------------------

Fence の開始行で\ ``{literalinclude}``\ と指定すると Literal Include ブロックとなります。
指定したファイルをそのままコードとして展開します。
ファイルのパスは最初の入力ファイルを基点とした相対パスで指定します。

.. code-block:: none

    ```{literalinclude} ./example.c
    :language: c
    ```

Include Block
-------------

Fence の開始行で\ ``{include}``\ と指定すると Include ブロックとなります。
指定したファイルを Thothglyph で解釈して展開します。
ファイルのパスは最初の入力ファイルを基点とした相対パスで指定します。

.. code-block:: none

    ```{include} ./sub.md
    ```

Custom Block
------------

Fence の開始行でその他の\ ``{keyword}``\ を指定すると Custom ブロックとなります。
``keyword``\ には ``math``, ``graphviz`` , ``blockdiag`` , ``wavedrom`` を使用できます。

Code Block
----------

前述以外の Fence ブロックは Code Block となります。
始めの\ ```````\ に続き言語名を記入することでシンタックスハイライトのヒントを与えます。

.. code-block:: none

    ```c
    #include <stdio.h>
    # include <stdlib.h>
    int main()
    {
    printf("Hello World!!\n");
    exit(0);
    }
    ```

Horizontal Line
---------------

4文字以上の\ ``=``\ もしくは\ ``-``\ で始まる1行は Horizontal Line (水平線) となります。

.. code-block:: none

    paragraph

    ====

    paragraph

Inline markup
=============

ブロック内のいくつかのテキストにはインラインマークアップを適用できます。

Decoration
----------

特定のシンボルでテキストを囲むことで、テキストを装飾できます。

.. code-block:: none

    装飾の種類は *強調* **重要** ***強調かつ重要*** があります。
    `コード` も記入できます。

Color Decoration
----------------

(Thothglyph 方言)

色を表すシンボルと終了シンボルでテキストを囲むことで、テキストの色を指定できます。
指定できる色は5種類です。

.. code-block:: none

    text `🔴color1` `🟡color2` `🟢color3` `🔵color4` `🟣color5` color text.

ネストした場合内側の色が反映されます。

.. code-block:: none

    ``🔵Color `🟣decoration` can`` be nested.

Image
-----

画像を挿入します。

.. code-block:: none

    Thothglyph のアイコンはこちら: ![](./tglyph_64.png)

オプションで画像の幅を設定できます。縦横比は固定です。

.. code-block:: none

    画像サイズ変更(ピクセルサイズで指定): ![](./tglyph_64.png){w=150px}

    画像サイズ変更(最大幅からの%で指定): ![](./tglyph_64.png){w="20%"}

Keyboard / Button / Menu
------------------------

テキストの装飾の一種です。

.. code-block:: none

    Type {kbd}`Ctrl A` right now.

    Click {btn}`OK` or {btn}`Cancel`.

    Select {menu}`File > Quit` to exit application.

Hyper Link
----------

``[テキスト](URL)``\ という構文は Hyper Link となります。

.. code-block:: none

    Search [](https://www.yahoo.com/) !

    For more information, check [here](https://www.google.com/) !

Cross Reference
---------------

Hyper Link と同じ構文でURLの代わりに文書中のラベル名を指定すると Cross Reference となります。
テキストを指定しない場合、ラベルの参照先から取得します。

.. code-block:: none

    First section: [](sect1)!

    [Here](sect1) is the same!

``ファイル名#アンカー`` 形式もサポートしています。

.. code-block:: none

    A section in same file: [](#sect1)

    A section in other file: [](other.md#sect1)

    First section in other file: [](other.tglyph)

.. _footnote:

Footnote
--------

文中に\ ``{footnote}`ID```\ と記入すると Footnote となります。
別の場所で Footnote List ブロックに脚注の内容を記入します。
ID には数字も指定可能です。ただし本文中に出現した順に番号が割り振られるため数値に意味はありません。
ID は見出しレベル1以下で一意のものにする必要があります。
見出しレベル1が異なる Footnote List は参照できません。

.. code-block:: none

    The important text. {footnote}`1` And the important text too. {footnote}`2`

    ```{footnote}
    :1: This is footnote.
    :2: This is footnote too.
    ```

.. _reference:

Refenrence
----------

文中に\ ``{cite}`ID```\ と記入すると Reference となります。
別の場所で Reference List ブロックに参考文献の内容を記入します。
Reference List のリストには本文中で引用されていないものも含められます。
ID には数字も指定可能です。ただし Reference List のリスト順に番号が割り振られるため数値に意味はありません。

.. code-block:: none

    The important text. {cite}`1` And the important text too. {cite}`2`

    ```{reference}
    :1: The Awesome Document, 1990, Anonymous.
    :2: The theory of theory, 2000-01-01, Anonymous.
    :3: Unreferenced bibliograpy I, 2XXX-XX-XX, Anonymous.
    :4: Unreferenced bibliograpy II, 2XXX-XX-XX, Anonymous.
    ```

Replace
-------

``{{%``\ と\ ``%}}``\ で囲まれた文字列は Config で attrs として定義した辞書をもとに置換できます。

.. code-block:: none

    Hello, I am {{%author%}}.
