===================
Thothglyph Language
===================

はじめに
========

この文書は Thothglyph Language (トトグリフ言語) のリファレンスマニュアルです。

言語の特徴
----------

Thothglyph Language は、マークアップシンボルの多くに非 ASCII 文字を使っています。
これらの文字を使うことで、既存のプログラミング言語やマークアップ言語の構文との衝突を避け、
様々なソースコードやドキュメントをエスケープなしで文書に含められます。
また、シンボルに使用可能な文字が増えたことで、視認のしやすさと構文の単純化を両立できます。

一方で、通常の文書にはあまり出現しない文字をシンボルに使用しているため、書きにくく感じるかもしれません。
その書きにくさを軽減するために各種エディタ向けに Thothglyph 用の入力補助やシンタックスハイライトのプラグインを用意しています。
例として :doc:`VSCode拡張 <../tool/vscode>` があります。

Preprocess
==========

Thothglyph はドキュメントを構文解析する前に簡単なプリプロセスを実行します。

Config
------

``⑇⑇⑇``\ のみの行で囲まれたテキストは Config ブロックとなります。
ドキュメントの最初のみ記述可能な構文です。
ドキュメントの設定を Python 言語で記述します。

.. code-block:: none

    ⑇⑇⑇
    title = 'Document Title'
    version = '1.2.3'
    author = cmd("git config user.name")
    attrs = {'author': 'Foo Bar'}
    templatedir = './my_template'  # copy from thothglyph/template
    theme = 'preview'  # or 'default'
    ⑇⑇⑇

後述の Include ロールで外部ファイルをインクルードできます。

.. code-block:: none

    ⑇⑇⑇
    ¤include⸨./conf.py⸩
    ⑇⑇⑇

代表的な設定には次のものがあります。

`title`
    ドキュメントのタイトル。表紙のタイトルやファイル名に反映される。

`version`
    ドキュメントのバージョン。

`author`
    ドキュメントの著者。

`date`
    ドキュメントの日付。

`attrs`
    ドキュメント内の変数。

`templatedir`
    出力ファイルのテンプレートディレクトリ。

`theme`
    テンプレートのテーマ。テンプレートは最終的に `<templatedir>/<output-format>/<theme>/` が選択される。


コマンドラインと Config
~~~~~~~~~~~~~~~~~~~~

コマンドラインでいくつかの Config 設定を指定できます。コマンドラインの設定はドキュメント内の設定より優先されます。

`--template`
    templatedir を指定します。

    相対パスで設定した場合、ドキュメント内の templatedir は最初の入力ファイルがあるディレクトリを起点にしますが、
    コマンドラインの設定はコマンド実行時のカレントディレクトリを起点にします。

`--theme`
    theme を指定します。


ControlFlow
-----------

1文字の\ ``⑇``\ の後に特定のキーワードを記入すると ControlFlow となります。
ドキュメントの一部分を条件により表示/非表示できます。
サポートしているキーワードは if, elif, end です。
条件には Python 構文が使用でき、Config ブロックで定義した attrs の値を使用できます。

.. code-block:: none

    ⑇if author == 'Smith'
    Profile about Smith.
    ⑇elif author == 'Tanaka'
    Profile about Tanaka.
    ⑇end

Comment
-------

2文字の\ ``⑇``\ に続く行末までの文字列はコメントとなります。
コメントはプリプロセスの段階で文中から削除されます。

.. code-block:: none

    This line includes comment --> ⑇⑇ This is comment.

Blocks
======

ドキュメントはブロックの木構造で構成されています。
ブロックは基本的に行単位でまとまっています。

Section
-------

1文字以上の\ ``▮``\ の後に空白とテキストを記入すると Section (見出し) となります。
ドキュメントの最も大枠となるブロックです。
``▮``\ の文字数が見出しレベルに相当します。

.. code-block:: none

    ▮ Section Lv.1 Title
    ▮▮ Section Lv.2 Title
    ▮▮▮ Section Lv.3 Title
    ▮▮▮▮ Section Lv.4 Title
    ▮▮▮▮▮ Section Lv.5 Title
    ▮▮▮▮▮▮ Section Lv.6 Title
    ▮▮▮▮▮▮▮ Section Lv.7 Title

見出しレベル1, 2のみ ATX-style の記法をサポートしています。
見出しの次の行にレベル1では\ ``=``\ 、レベル2では\ ``-``\ を4文字以上記入します。

.. code-block:: none

    Section Lv.1 ATX-style
    ======================

    Section Lv.2 ATX-style
    ----------------------

各見出しにはラベルを付けられます。ラベルは後述の Cross Reference で利用できます。

.. code-block:: none

    ▮ Section Title ⟦sect1⟧

``▮``\ の末尾に\ ``*``\ を記入すると見出しの番号付けをスキップし、目次に表示されなくなります。
\ ``+``\ を記入すると見出しの番号付をスキップしますが、目次には表示されます。

.. code-block:: none

    ▮ まえがき (1. まえがき)
    ▮* 目次 (目次)
    ▮ XXとは (2. XXとは)
    ▮ YYとは (3. YYとは)

Paragraph
---------

通常の文字から始まる行は Paragraph (段落) となります。
Paragraph は空行が出現するまで継続します。

.. code-block:: none

    これは段落1のテキストです。
    改行しても段落は継続します。

    これは段落2のテキストです。

段落終端記号\ ``⊹``\ を行末に挿入すると空行なしで次の段落に移行できます。

.. code-block:: none

    これは段落1のテキストです。⊹
    これは段落2のテキストです。

Bullet List
-----------

1文字以上の\ ``•``\ と空白から始まるブロックは Bullet List (箇条書きリスト) となります。

.. code-block:: none

    • apple
    • orange
    • grape

``•``\ の文字数がリストのレベルに相当します。

.. code-block:: none

    • List item 1
    •• List item 1-1
    ••• List item 1-1-1
    ••• List item 1-1-2
    •• List item 1-2
    ••• List item 1-2-1
    ••• List item 1-2-2
    • List item 2

リストの各アイテムの本文には複数ブロックを記入できます。
行頭のインデントを揃える必要はありません。

.. code-block:: none

    • Item 1 paragraph 1.
    new line.

    paragraph 2.

    • Item 2 paragraph 1.
    new line.

    paragraph 2.

リストを終了して別のリストや段落を開始するには、そのレベルと同数の\ ``◃``\ から成る行を記入します。

.. code-block:: none

    • My favorite food
    •• apple
    •• orange
    •• grape
    ◃◃
    •• sushi
    •• tempura
    ◃
    New Paragraph.

Ordered List
------------

1文字以上の\ ``꓾``\ と空白から始まるブロックは Ordered List (順序付きリスト) となります。
``꓾``\ の文字数がリストのレベルに相当します。

.. code-block:: none

    ꓾ List item 1
    ꓾꓾ List item 1-1
    ꓾꓾꓾ List item 1-1-1
    ꓾꓾꓾ List item 1-1-2
    ꓾꓾ List item 1-2
    ꓾꓾꓾ List item 1-2-1
    ꓾꓾꓾ List item 1-2-2
    ꓾ List item 2
    ◃
    ꓾ List item new 1

Description List
----------------

1文字以上の\ ``ᛝ``\ から始まり途中\ ``ᛝ ``\ \ (記号のあと空白が含まれる)ブロックは Description List (説明リスト) となります。
最初の\ ``ᛝ``\ の文字数がリストのレベルに相当します。
``ᛝ``\ で囲まれた文字列は用語、\ ``ᛝ``\ 以降は本文です。

.. code-block:: none

    ᛝTerm 1ᛝ List item 1
    ᛝᛝTerm 1-1ᛝ List item 1-1
    ᛝᛝᛝTerm 1-1-1ᛝ List item 1-1-1
    ᛝᛝᛝTerm 1-1-2ᛝ List item 1-1-2
    ᛝᛝTerm 1-2ᛝ List item 1-2
    ᛝᛝᛝTerm 1-2-1ᛝ List item 1-2-1
    ᛝᛝᛝTerm 1-2-2ᛝ List item 1-2-2
    ᛝTerm 2ᛝ List item 2
    ◃
    ᛝTerm 1ᛝ List item new 1

用語と本文は通常横並びで出力されます。
用語の終端に\ ``◃``\ を記入すると用語の後改行して本文を出力します。

.. code-block:: none

    ᛝTerm 1◃ᛝ List item 1
    ᛝTerm 2◃ᛝ List item 2

Check List
----------

1文字以上の\ ``•``\ と\ ``[ ]``\ と空白から始まるブロックは Check List (チェックリスト) となります。
``•``\ の文字数がリストのレベルに相当します。
チェックボックスの状態は\ ``[ ]``\ , \ ``[x]``\ , \ ``[-]``\ の3つを選択できます。

.. code-block:: none

    •[ ] List item 1
    ••[-] List item 1-1
    •••[x] List item 1-1-1
    •••[ ] List item 1-1-2
    ••[x] List item 1-2
    •••[x] List item 1-2-1
    •••[x] List item 1-2-2
    •[ ] List item 2
    ◃
    •[x] List item new 1

複合リスト
----------

これまで説明したリストは別種のリストを入れ子にできます。
ただしリストのレベルは種類に関係なく設定する必要があります。

.. code-block:: none

    • List item 1
    ꓾꓾ List item 1-1
    ᛝᛝᛝAᛝ List item 1-1-1
    ᛝᛝᛝBᛝ List item 1-1-2
    ꓾꓾ List item 1-2
    •••[x] List item 1-2-1
    •••[ ] List item 1-2-2
    • List item 2

リストの汎用的な字下げ
----------------------

行の先頭で同じシンボルを続ける代わりに、\ ``𐬹 ``\ \ (記号のあと空白が含まれる)でもリストのレベルを下げられます。

先の複合リストは次のようにも書けます。

.. code-block:: none

    • List item 1
    𐬹 ꓾ List item 1-1
    𐬹 𐬹 ᛝAᛝ List item 1-1-1
    𐬹 𐬹 ᛝBᛝ List item 1-1-2
    𐬹 ꓾ List item 1-2
    𐬹 𐬹 •[x] List item 1-2-1
    𐬹 𐬹 •[ ] List item 1-2-2
    • List item 2

Scoped Blocks
-------------

ブロックのスコープ (開始位置と終了位置) を\ ``⦃``\ と\ ``⦄``\ で指定できます。
このブロックは Scoped Blocks となります。

1行で複数段落を記述したり、リストのレベルを気にすることなく入れ子にできます。

.. code-block:: none

    ⦃Paragraph 1.⦄⦃Paragraph 2.⦄

    • ⦃List item 1
      ꓾ ⦃List item 1-1
        ᛝAᛝ List item 1-1-1
        ᛝBᛝ List item 1-1-2
        ⦄
      ꓾ ⦃List item 1-2
        •[x] List item 1-2-1
        •[ ] List item 1-2-2
        ⦄
      ⦄
    • List item 2

Footnote List
-------------

1文字だけの\ ``•``\ と\ ``[^ID]``\ と空白から始まるブロックは Footnote List (脚注リスト) となります。
リストは入れ子にできません。
文中の脚注の書き方は :ref:`footnote` 参照。

.. code-block:: none

    •[^1] This is footnote.
    •[^2] This is footnote too.

Reference List
--------------

1文字だけの\ ``•``\ と\ ``[#ID]``\ と空白から始まるブロックは Reference List (参照リスト) となります。
リストは入れ子にできません。
文中の参照の書き方は :ref:`reference` 参照。

.. code-block:: none

    •[#1] The Awesome Document, 1990, Anonymous.
    •[#2] The theory of theory, 2000-01-01, Anonymous.

Basic Table
-----------

``|``\ で囲まれた行が連続するブロックは Basic Table となります。
基本的な構文は既存の軽量マークアップ言語のものと似ています。

.. code-block:: none

    | data11 | data12 | data13 |
    | data21 | data22 | data23 |

``:-:``\ で構成された行はヘッダ部とデータ部を分割し、セル内のテキストアライメントを設定します。
ヘッダ部がない場合はテキストアライメントのみ設定します。
``+-``\ は左アライメントかつセル幅をページ幅に合うよう調節します。(latex, pdfのみ)

.. code-block:: none

    | head11 | head12 | head13 | head14 |
    | head21 | head22 | head23 | head24 |
    |:-------|:------:|-------:|+-------|
    | data11 | data12 | data13 | data14 |
    | data21 | data22 | data23 | data24 |
    | a | b | c | d |

セルの内容を\ ``⏴``\ もしくは\ ``⏶``\ で開始することで、セルを結合できます。

.. code-block:: none

    | head11 | head12 | ⏴      | ⏴      |
    |--------|--------|--------|--------|
    | data11 | data12 | data13 | data14 |
    | data21 | data22 | ⏴      | data24 |
    | data31 | ⏶      | ⏴      | ⏶      |
    | data41 | data42 | ⏴      | data44 |
    | data51 | data52 | data53 |⏴data54 |
    | data61 |⏶data62 |⏶data63 |⏴data64 |

先頭に ``⟦⟧`` で囲まれた行を挿入することでオプションを記述できます。

.. code-block:: none

    ⟦type="long" w="100%" widths="1,2,3" fontsize="small"⟧
    | data11 | data12 | data13 |
    | data21 | data22 | data23 |

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

``|¤¤¤``\ という行から始まり\ ``¤¤¤``\ という行で終わるブロックは List Table となります。

(v0.2.9 以降 ``|===``\ という行から始まり\ ``===|``\ という行で終わる書式は非推奨となりました。)

List Table 内はレベル2以上の Bullet List で構成されます。
レベル1の文は無視され、レベル2のリストアイテムが各セルの内容になります。
レベル3のリストは表内のレベル1のリストに置き換わります。

.. code-block:: none

    |¤¤¤
    • •• data11
      •• data12
         ••• item1
         ••• item2
         ••• item3
      •• data13
    • •• data21
      •• data22
      •• data23
    ¤¤¤

※見やすくするためにインデントしていますが、インデントは必須ではありません。

``◃``\ でリストを分割すると、第1リストがヘッダ、第2リストがデータになります。

.. code-block:: none

    |¤¤¤
    • •• head1
      •• head2
      •• head3
    ◃
    • •• data11
      •• data12
      •• data13
    • •• data23
      •• data22
      •• data23
    ¤¤¤

Basic Tableと 同様にセルの内容を\ ``⏴``\ もしくは\ ``⏶``\ で開始することで、セルを結合できます。

.. code-block:: none

    |¤¤¤
    • •• head1
      •• head2
      •• ⏴
    ◃
    • •• data11
      •• data12
      •• data13
    • •• data23
      •• ⏶data22
      •• data23
    ¤¤¤

開始行の\ ``|¤¤¤``\ に続き\ ``⟦⟧``\ でオプションを記述できます。

.. code-block:: none

    |¤¤¤⟦align="lcr"⟧
    • •• data11
      •• data12
      •• data13
    • •• A
      •• B
      •• C
    ¤¤¤

Figure
------

後述の Role という記法で図や表にキャプションを付けられます。
実際にキャプションが表示される位置は出力形式やテンプレートに依存します。

.. code-block:: none

    ¤figure⸨caption⸩
    ¤image⸨./tglyph_64.png⸩

.. code-block:: none

    ¤figure⸨caption⸩
    | head11 | head12 | head13 |
    | head21 | head22 | head23 |
    |--------|--------|--------|
    | data11 | data12 | data13 |
    | data21 | data22 | data23 |
    | data31 | data32 | data33 |

.. code-block:: none

    ¤figure⸨caption⸩
    [Not Image.]

Quote Block
-----------

``>``\ と空白で始まる行が連続したブロックは Quote Block (引用ブロック) となります。

.. code-block:: none

    > Quote text text text.
    > new line text.
    > > Nested quote text.
    > return first quote.

    > New quote text.

Code Block
----------

``⸌⸌⸌``\ という行で囲まれたブロックは Code Block となります。
始めの\ ``⸌⸌⸌``\ に続き言語名を記入することでシンタックスハイライトのヒントを与えます。

.. code-block:: none

    ⸌⸌⸌c
    #include <stdio.h>
    # include <stdlib.h>
    int main()
    {
    printf("Hello World!!\n");
    exit(0);
    }
    ⸌⸌⸌

後述の Include ロールで外部ファイルをインクルードできます。

.. code-block:: none

    ⸌⸌⸌c
    ¤include⸨./example.c⸩
    ⸌⸌⸌

Custom Block
------------

``¤¤¤``\ という行で囲まれたブロックは Custom Block となります。
始めの\ ``¤¤¤``\ に続き拡張名を記入することで様々な拡張機能を実行します。
拡張名には ``math``, ``graphviz`` , ``blockdiag`` , ``wavedrom`` を使用できます。

.. code-block:: none

    ¤¤¤graphviz
    digraph graph_name {
    alpha;
    beta;
    alpha -> beta;
    }
    ¤¤¤

後述の Include ロールで外部ファイルをインクルードできます。

.. code-block:: none

    ¤¤¤graphviz
    ¤include⸨./graph1.dot⸩
    ¤¤¤

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

    装飾の種類は🙼強調🙼、⧫重要⧫、‗挿入‗、¬削除¬があります。
    ⧫🙼強調かつ重要🙼⧫のように入れ子にできます。
    また⌃上付き文字⌃や⌄下付き文字⌄にもできます。
    更に⁒変数⁒や⸌コード⸌も記入できます。

共通の終了シンボルを用いることもできます。

.. code-block:: none

    🙼強調⟠、⧫重要⟠、‗挿入⟠、¬削除⟠

Color Decoration
----------------

色を表すシンボルと終了シンボルでテキストを囲むことで、テキストの色を指定できます。
指定できる色は5種類です。

.. code-block:: none

    text 🔴color1⟠ 🟡color2⟠ 🟢color3⟠ 🔵color4⟠ 🟣color5⟠ text.

ネストした場合内側の色が反映されます。

.. code-block:: none

    🔵Color 🟣Decoration⟠ can⟠ be nested.

Linebreak
---------

段落内の改行は多くの出力形式で無視されますが、\ ``↲``\ を挿入すると改行を表す特殊文字に変換されます。
例えば HTML 出力の場合 `<br \>` となります。

.. code-block:: none

    First line text.↲
    Second line text.

Role
----

``¤ロール名⟦オプション⟧⸨本文⸩``\ という構文は Role となります。
``⟦オプション⟧``\ は省略可能です。

Image Role
----------

画像を挿入します。

.. code-block:: none

    Thothglyph のアイコンはこちら: ¤image⸨./tglyph_64.png⸩

オプションで画像の幅を設定できます。縦横比は固定です。

.. code-block:: none

    ピクセル数で指定: ¤image⟦w="150px"⟧⸨./tglyph_64.png⸩

    ページ幅の割合で指定: ¤image⟦w="20%"⟧⸨./tglyph_64.png⸩

Include Role
------------

外部のtglyphファイルを解釈して挿入します。
ファイルのパスは最初の入力ファイルを基点とした相対パスで指定します。

.. code-block:: none

    ¤include⸨./sub1.tglyph⸩

Keyboard / Button / Menu Role
-----------------------------

テキストの装飾の一種です。

.. code-block:: none

    Type ¤kbd⸨Ctrl A⸩ right now.

    Click ¤btn⸨OK⸩ or ¤btn⸨Cancel⸩.

    Select ¤menu⸨File > Quit⸩ to exit application.

Hyper Link
----------

``⟦テキスト⟧⸨URL⸩``\ という構文は Hyper Link となります。
``⟦テキスト⟧``\ は省略可能です。
Role に似ていますが別の構文です。

.. code-block:: none

    Search ⸨https://www.yahoo.com/⸩ !

    For more information, check ⟦here⟧⸨https://www.google.com/⸩ !

Cross Reference
---------------

Hyper Link と同じ構文でURLの代わりに文書中のラベル名を指定すると Cross Reference となります。
テキストを指定しない場合、ラベルの参照先から取得します。

.. code-block:: none

    Cross refecence to section 1: ⸨sect1⸩!

    ⟦Here⟧⸨sect1⸩ is the same!

HTML や Markdown のような ``ファイル名#アンカー`` 形式もサポートしています。

.. code-block:: none

    A section in same file: ⸨#sect1⸩

    A section in other file: ⸨other.tglyph#sect1⸩

    First section in other file: ⸨other.tglyph⸩

.. _footnote:

Footnote
--------

文中に\ ``[^ID]``\ と記入すると Footnote となります。
別の場所で Footnote List ブロックに脚注の内容を記入します。
ID には数字も指定可能です。ただし本文中に出現した順に番号が割り振られるため数値に意味はありません。
ID は見出しレベル1以下で一意のものにする必要があります。
見出しレベル1が異なる Footnote List は参照できません。

.. code-block:: none

    The important text. [^1] And the important text too. [^2]

    •[^1] This is footnote.
    •[^2] This is footnote too.

.. _reference:

Refenrence
----------

文中に\ ``[#ID]``\ と記入すると Reference となります。
別の場所で Reference List ブロックに参考文献の内容を記入します。
Reference List のリストには本文中で引用されていないものも含められます。
ID には数字も指定可能です。ただし Reference List のリスト順に番号が割り振られるため数値に意味はありません。

.. code-block:: none

    The important text. [#1] And the important text too. [#2]

    •[#1] The Awesome Document, 1990, Anonymous.
    •[#2] The theory of theory, 2000-01-01, Anonymous.
    •[#3] Unreferenced bibliograpy I, 2XXX-XX-XX, Anonymous.
    •[#4] Unreferenced bibliograpy II, 2XXX-XX-XX, Anonymous.

Replace
-------

``⁅``\ と\ ``⁆``\ で囲まれた文字列は Config で attrs として定義した辞書をもとに置換できます。

.. code-block:: none

    Hello, I am ⁅author⁆.
