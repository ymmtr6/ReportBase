# ReportBase

レポートをmdファイルとして扱うための
VSCodeとして利用することを前提に，taskの設定を行なっている．

### 環境

* Macbook Air(13-inch) 2015
* OS X
* LaTeX (pLaTeX)
* pandoc

### MarkDown->LaTeX (pandoc)

```
$ pandoc -f markdown \
-f markdown-auto_identifiers \
-t latex report.md -o report.tex
```

このコマンドによって，mdファイルからtexファイルの本文に相当する部分が生成できる．
ただし，pandoc version1.4からは仕様に変更があり，\tightlistというコマンドが一部で生成されるため，platexではコンパイルすることが出来ない．その問題を解決するため，何もしない関数を定義する．

```
% tightlist回避
\def\tightlist{\itemsep1pt\parskip0pt\parsep0pt}

```

また，本文のみがmdより生成される対処のため，プリアンブルに相当する部分をあらかじめ作っておく．

```
\documentclass[11pt, a4paper]{jsarticle}

\usepackage{amsmath,amssymb}
\usepackage{bm}
\usepackage{booktabs}
\usepackage[dvipdfmx]{graphicx}
\usepackage{comment}
\usepackage{longtable}

% tightlist回避
\def\tightlist{\itemsep1pt\parskip0pt\parsep0pt}

\title{This is Title Zone}
\author{NAME}
\date{\today}

\begin{document}
\maketitle
\input{report}
\end{document}
```

その関係で，レポートを生成するときは，

```
$ platex report.tex
$ platex report.tex
$ dvipdfmx report.dvi
```
となる．