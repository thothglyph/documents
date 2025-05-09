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

カーソルの左に特定の1〜3文字がある状態で **Shift+Space** キーを入力することで、その文字を変換してマークアップシンボルを入力できます。

.. list-table:: カーソル文字とマークアップシンボル対応表
   :widths: 15 10 30
   :header-rows: 1

   * - カーソル文字
     - マークアップシンボル
     - 用途
   * - %%%
     - ⑇⑇⑇
     - Config ブロックの開始と終了
   * - %%
     - ⑇⑇
     - コメント
   * - %
     - ⑇
     - 制御フロー
   * - #
     - ▮
     - セクションの開始
   * - @@@
     - ¤¤¤
     - Custom ブロックの開始と終了、Role の開始
   * - \`\`\`
     - ⸌⸌⸌
     - Code ブロックの開始と終了
   * - \*
     - •
     - Bullet List (箇条書きリスト)
   * - \+
     - ꓾
     - Ordered List (番号付きリスト)
   * - \:
     - ᛝ
     - Description List (説明リスト) の用語部分
   * - \-
     - ◃
     - リストの終了
   * - \\
     - ⊹
     - 段落の終了
   * - [
     - ⟦
     - リンクのテキスト、Role のオプション
   * - ]
     - ⟧
     - 〃
   * - (
     - ⸨
     - リンクのURL、Role の値
   * - )
     - ⸩
     - 〃
   * - {
     - ⦃
     - Scoped Blocks のスコープ
   * - }
     - ⦄
     - 〃
   * - ⟦ （'['の変換後）
     - ⁅
     - Replace
   * - ⟧ （']'の変換後）
     - ⁆
     - 〃
   * - <
     - ⏴
     - テーブルの水平結合
   * - >
     - ⏵
     - 未使用
   * - ^
     - ⏶
     - テーブルの垂直結合
   * - ~
     - ⏷
     - 未使用
   * - ⊹（'\\'の変換後）
     - ↲
     - 改行
   * - /
     - ⁒
     - インライン Emphasis (強調)
   * - • （'*'の変換後）
     - ⋄
     - インライン Strong (重要)
   * - _
     - ‗
     - インライン Marked (マーカー)
   * - ◃ （'-'の変換後）
     - ¬
     - インライン Strike (打ち消し)
   * - ⏶ （'^'の変換後）
     - ⌃
     - インライン Sup (上付き文字)
   * - ⏷ （'~'の変換後）
     - ⌄
     - インライン Sub (下付き文字)
   * - \`
     - ⸌
     - インライン Code (コード)
   * - ᛝ （':'の変換後）
     - ⫶
     - インライン Var (変数)
   * - ;
     - ⟠
     - インラインの終了
   * - @1
     - 🔴
     - インライン カラー1
   * - @2
     - 🟡
     - インライン カラー2
   * - @3
     - 🟢
     - インライン カラー3
   * - @4
     - 🔵
     - インライン カラー4
   * - @5
     - 🟣
     - インライン カラー5

ドキュメントの変換
==================

.tglyph ファイルの編集中に VSCode 上でドキュメントの変換ができます。

"Command Palette" > "Thothglyph: Export File As ..." と選択します。
html, pdf, docx から選択できます (現在 html のみ動作)。

一度 Export すると Ctrl+E で上書きできるようになります。
