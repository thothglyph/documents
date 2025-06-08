===========
Get started
===========

Installation
============

最小構成のインストール

.. code-block:: bash

    pip install thothglyph-doc

出力形式を増やしたい場合

.. code-block:: bash

    # pdf
    sudo apt install texlive-luatex texlive-fonts-recommended texlive-fonts-extra texlive-lang-cjk

    # docx
    pip install python-docx

PDF 出力は Texlive 2021 以上の環境が必要です。
OS によってはパッケージマネージャで入手できる Texlive のバージョンが古く、正常に出力できない場合があります。
その場合、 :doc:`Texlive のインストール方法 <./install-texlive>` を参照して最新の Texlive を導入してください。

数式や図表などを表示したい場合

.. code-block:: bash

    # graphviz
    sudo apt install graphviz
    pip install graphviz

    # blockdiag
    pip install blockdiag actdiag seqdiag nwdiag

    # wavedrom
    pip install wavedrom

Convert documents
=================

``thothglyph`` コマンドを用いてドキュメントを HTML や PDF に変換します。


.. code-block:: bash

    thothglyph [-h] [--version] [--to TYPE] [--output FILE] [--from TYPE] input

使用例：

.. code-block:: bash

    thothglyph -t html document.tglyph

input
    入力ファイル名を指定します。

--to TYPE, -t TYPE
    入力ファイル形式を指定します。
    tglyph, md から選択できます。
    指定がない場合、入力ファイル名の拡張子から判断します。

--output FILE, -o FILE
    出力ファイル名を指定します。
    指定がない場合、入力ファイル名と出力ファイル形式から決定します。

--from TYPE, -f TYPE
    出力ファイル形式を指定します。
    html, latex, pdf, docx から選択できます。
    指定がない場合、出力ファイル名の拡張子から判断します。
    デフォルトは html です。