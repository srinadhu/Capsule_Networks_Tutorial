Markdown
========
[![release](https://img.shields.io/github/release/witiko/markdown.svg)][release]
[![CircleCI](https://circleci.com/gh/Witiko/markdown/tree/master.svg?style=shield)][CircleCI]

 [CircleCI]: https://circleci.com/gh/Witiko/markdown/tree/master (CircleCI)
 [release]:  https://github.com/Witiko/markdown/releases/latest  (Releases · Witiko/markdown)

The Markdown package converts [markdown][] markup to TeX commands. The
functionality is provided both as a Lua module, and as plain TeX, LaTeX, and
ConTeXt macro packages that can be used to directly typeset TeX documents
containing markdown markup. Unlike other convertors, the Markdown package
makes it easy to redefine how each and every markdown element is rendered.
Creative abuse of the markdown syntax is encouraged. 😉

 [markdown]: https://daringfireball.net/projects/markdown/basics/ (Daring Fireball: Markdown Basics)

Contributing
------------
Apart from the example markdown documents and tests, which are placed in the
`examples/` and `tests/` directories, the complete source code and
documentation of the package are placed in the `markdown.dtx` document
following the literate programming paradigm.

When the file `markdown.ins` is interpreted using a Unicode-aware TeX engine,
such as XeTeX (`xetex markdown.ins`) or LuaTeX (`luatex markdown.ins`), several
files are produced from the `markdown.dtx` document. In `markdown.dtx`, the
boundaries between the produced files are marked up using a XML-like syntax
provided by the [DocStrip][] plain TeX package.

Running the [LaTeXMK][LaTeXMK] tool on the `markdown.dtx` file
(`latexmk markdown.dtx`) after the Markdown package has been installed typesets
the documentation. In `markdown.dtx`, the documentation is placed inside TeX
comments and marked up using the [ltxdockit][] LaTeX document class. Support
for typesetting the documentation is provided by the [doc][] LaTeX package.

 [DocStrip]:  https://ctan.org/pkg/docstrip   (docstrip – Remove comments from file)
 [LaTeXMK]:   https://ctan.org/pkg/latexmk    (latexmk – Fully automated LaTeX document generation)
 [ltxdockit]: https://ctan.org/pkg/ltxdockit  (ltxdockit – Documentation support)
 [doc]:       https://ctan.org/pkg/doc        (doc – Format LaTeX documentation)

Further information
-------------------
For further information, consult:

1. the [user manual][manual], which can be produced by interpreting the
   `markdown.ins` file using a Unicode-aware TeX engine, such as XeTeX
   (`xetex markdown.ins`) or LuaTeX (`luatex markdown.ins`). The manual will
   reside in the file `markdown.md` and the CSS stylesheet `markdown.css`, and
2. the [technical documentation][techdoc], which can be typeset by running the
   [LaTeXMK][LaTeXMK] tool on the `markdown.dtx` file
   (`latexmk markdown.dtx`) after installing the Markdown package.
   [LaTeXMK][LaTeXMK] should be included in your TeX distribution. The typeset
   documentation will reside in the file `markdown.pdf`.

 [techdoc]: http://mirrors.ctan.org/macros/generic/markdown/markdown.pdf  (A Markdown Interpreter for TeX)
 [manual]:  http://mirrors.ctan.org/macros/generic/markdown/markdown.html (Markdown Package User Manual)
