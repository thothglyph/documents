=======================
Texlive のインストール方法
=======================

最小構成インストール
=================

`Texlive 公式 <https://www.tug.org/texlive/acquire-netinstall.html>`_ からネットワークインストーラをダウンロードします。

* install-tl-unx.tar.gz

展開してインストーラを起動します。

.. code-block:: bash

   tar zxf install-tl-unx.tar.gz
   cd install-tl-2025mmdd
   ./install-tl

CUI の対話形式でインストールする画面が開きます。
次に示すような設定でインストールします。

.. code-block:: bash

   > S enter
   > e enter 上下キーで "minimal scheme (plain only)" を選択
   > R enter で戻る

   > D enter
   > 1 enter で TEXDIR を編集
   > "New value for TEXDIR" に 任意のディレクトリを指定し enter
     例: /home/myuser/local/textlive/2025  (※ $HOME や $PWD などの環境変数は使えません。)
   > R enter で戻る

   > I enter でインストール開始

インストール後のメッセージに従い .bashrc に PATH を追加します。

.. code-block:: bash

   export PATH=/home/myuser/local/textlive/2025/bin/x86_64-linux:$PATH

追加のパッケージインストール
=======================

Thothglyph ツールの利用に必要なパッケージを ``tlmgr`` コマンドでインストールします。

.. code-block:: bash

   tlmgr install \
    latex-bin \
    luatexja

   tlmgr install \
    fancyhdr \
    hyperref \
    listings \
    ulem \
    fancybox \
    enumitem \
    float \
    caption \
    capt-of \
    tabularray \
    multirow \
    varwidth \
    cellspace \
    colortbl \
    xcolor \
    soul \
    pgf \
    infwarerr \
    ltxcmds \
    pdftexcmds \
    xkeyval \
    etoolbox \
    l3packages \
    fontspec \
    geometry \
    tools \
    epstopdf-pkg \
    ninecolors \
    tex-gyre
   
   tlmgr install \
    haranoaji \
    haranoaji-extra

以上で、Thothglyph の PDF 出力に必要な Texlive のインストールが完了です。