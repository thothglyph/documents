=========================================
vscode-thothglyph
=========================================
Thothglyph support for Visual Studio Code

Installation
============

VSCode の Marketplace からインストールできます。

インストールが完了すると、ユーザ・ストレージ領域に自動で Python 仮想環境を作成し、thothglyph がインストールされます。

.tglyph ファイルの編集
======================

拡張子 .tglyph のファイルを開くと、専用のエディタ機能が有効になります。

ASCII 文字の組み合わせで、マークアップシンボルを入力できます。

.. list-table:: ASCII 文字とマークアップシンボル対応表
   :widths: 15 10 30
   :header-rows: 1

   * - 入力文字
     - マークアップシンボル
     - 説明
   * - %%
     - ⑇
     - Config ブロックの開始と終了
   * - ##
     - ▮
     - セクションの開始
   * - @@
     - ¤
     - Custom ブロックの開始と終了、Role の開始
   * - \`\`
     - ⸌
     - Code ブロックの開始と終了
   * - \*\*
     - •
     - Bullet List (箇条書きリスト)
   * - ++
     - ꓾
     - Ordered List (番号付きリスト)
   * - \:\:
     - ᛝ
     - Description List (説明リスト) の用語部分
   * - --
     - ◃
     - リストの終了
   * - [[
     - ⟦
     - リンクのテキスト、Role のオプション
   * - ]]
     - ⟧
     - 〃
   * - ((
     - ⸨
     - リンクのURL、Role の値
   * - ))
     - ⸩
     - 〃
   * - {{
     - ⁅
     - Replace
   * - }}
     - ⁆
     - 〃
   * - <<
     - ⏴
     - テーブルの水平結合
   * - >>
     - ⏵
     - 未使用
   * - ^^
     - ⏶
     - テーブルの垂直結合
   * - ~~
     - ⏷
     - 未使用
   * - %<space>
     - ⑇
     - コメント、制御フロー(入力文字 %% と同一文字)
   * - /<space>
     - ⁒
     - インライン Emphasis (強調)
   * - \*<space>
     - ⋄
     - インライン Strong (重要)
   * - _<space>
     - ‗
     - インライン Marked (マーカー)
   * - -<space>
     - ¬
     - インライン Strike (打ち消し)
   * - ^<space>
     - ⌃
     - インライン Sup (上付き文字)
   * - ~<space>
     - ⌄
     - インライン Sub (下付き文字)
   * - \`<space>
     - ⸌
     - インライン Code (コード、入力文字 \`\` と同一文字)
   * - :<space>
     - ⫶
     - インライン Var (変数)

ドキュメントの変換
==================

.tlyph ファイルの編集中に VSCode 上でドキュメントの変換ができます。

"Command Palette" > "Thothglyph: Export File As ..." と選択します。
html, pdf, docx から選択できます (現在 html のみ動作)。

一度 Export すると Ctrl+E で上書きできるようになります。