# VSCodeとの連携
## 設定
VSCodeの設定ファイルに以下を追加
- settings.json
    ````
    {
    // ---------- Language ----------

    "[tex]": {
        // スニペット補完中にも補完を使えるようにする
        "editor.suggest.snippetsPreventQuickSuggestions": false,
        // インデント幅を2にする
        "editor.tabSize": 2
    },

    "[latex]": {
        // スニペット補完中にも補完を使えるようにする
        "editor.suggest.snippetsPreventQuickSuggestions": false,
        // インデント幅を2にする
        "editor.tabSize": 2
    },

    "[bibtex]": {
        // インデント幅を2にする
        "editor.tabSize": 2
    },

    // ---------- LaTeX Workshop ----------

    // 使用パッケージのコマンドや環境の補完を有効にする
    "latex-workshop.intellisense.package.enabled": true,

    // 生成ファイルを削除するときに対象とするファイル
    // デフォルト値に "*.synctex.gz" を追加
    "latex-workshop.latex.clean.fileTypes": [
        "*.aux",
        "*.bbl",
        "*.blg",
        "*.idx",
        "*.ind",
        "*.lof",
        "*.lot",
        "*.out",
        "*.toc",
        "*.acn",
        "*.acr",
        "*.alg",
        "*.glg",
        "*.glo",
        "*.gls",
        "*.ist",
        "*.fls",
        "*.log",
        "*.fdb_latexmk",
        "*.snm",
        "*.nav",
        "*.dvi",
        "*.synctex.gz"
    ],

    // ビルドのレシピ
    "latex-workshop.latex.recipes": [
        {
            "name": "latexmk",
            "tools": [
                "latexmk"
            ]
        },
    ],

    // ビルドのレシピに使われるパーツ
    "latex-workshop.latex.tools": [
        {
            "name": "latexmk",
            "command": "latexmk",
            "args": [
                "-silent",
                "-outdir=%OUTDIR%",
                "%DOC%"
            ],
        },
    ],
    "latex-workshop.latex.autoClean.run": "onFailed",
    "latex-workshop.view.pdf.viewer": "external",
    "latex-workshop.view.pdf.zoom": "page-width",
    "python.dataScience.alwaysTrustNotebooks": true,
    "latex-workshop.message.badbox.show": false,
    "latex-workshop.view.pdf.external.viewer.command": "/Applications/Skim.app/Contents/SharedSupport/displayline",
    "latex-workshop.view.pdf.external.viewer.args": [
        "0",
        "%PDF%"
    ],
    "latex-workshop.view.pdf.external.synctex.command": "/Applications/Skim.app/Contents/SharedSupport/displayline",
    "latex-workshop.view.pdf.external.synctex.args": [
        "-r",
        "%LINE%",
        "%PDF%",
        "%TEX%"
    ],
    }
    ````
## キーバインドの設定
Cmd+tでコンパイルできる。
以降は、ファイルを保存するとコンパイルが自動的に行われる。
- keybindings.json
    ````
    [
    {
        "key": "cmd+t",
        "command": "latex-workshop.build",
        "when": "resourceLangId == latex"
    }
    ]
    ````
## latexmkの設定
ホームディレクトリに以下のファイルをおく
- ~/.latexmkrc
    ````
    #!/usr/bin/env perl
    # LaTeX
    $latex = 'platex -synctex=1 -halt-on-error -file-line-error %O %S';
    $max_repeat = 5;
    # BibTeX
    $bibtex = 'pbibtex %O %S';
    $biber = 'biber --bblencoding=utf8 -u -U --output_safechars %O %S';
    # index
    $makeindex = 'mendex %O -o %D %S';
    # DVI / PDF
    $dvipdf = 'dvipdfmx %O -o %D %S';
    $pdf_mode = 3;
    # preview
    $pvc_view_file_via_temporary = 0;
    #$dvi_previewer = "open %S";
    #$pdf_previewer = "open %S";
    $pdf_previewer    = "open -ga /Applications/Skim.app %S";
    # clean up
    $clean_full_ext = "%R.synctex.gz"
    ````
## PDF Viewer (Skim)をインストール
Previewはソースコードとのsync機能が使えないため。
[Skim PDF Viewer](http://itea40.jp/application/mac-productivity/skim/)
- 環境設定/同期: すべてチェックマーク, 初期値:Visual Studio Code
- PDF: 自動的にリサイズ, 現在の表示設定をデフォルトにする
- ソースコードの該当行
PDF上でCmd+Shift+Clickすると、その内容の箇所のソースコードへとぶ。

## 自動整形
ソースコードで右クリックし、ドキュメントのフォーマットを選ぶとソースコードが整形される。
Macではエラーがでることがある。その場合は、以下の処置を行う。
  ````
  $ brew install perl
  $ brew install cpanm
  $ cpanm Log::Log4perl Log::Dispatch::File YAML::Tiny File::HomeDir Unicode::GCString
  ````
[Format のエラー回避](https://qiita.com/khys/items/332c3a3f34a82acf7a7a)

## 不要なファイルの削除
コンパイル時に生成されるファイルはTeXメニューのBuild LaTeX project/Clean up auxiliary filesで行う。
