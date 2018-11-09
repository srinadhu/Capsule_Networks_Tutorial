
---
title:  Markdown Package User Manual
author: Vít Novotný
---


<link href="https://afeld.github.io/emoji-css/emoji.css" rel="stylesheet" />

Introduction
============
The [Markdown package][pkg] converts [markdown][] markup to TeX commands. The
functionality is provided both as a Lua module, and as plain TeX, LaTeX, and
ConTeXt macro packages that can be used to directly typeset TeX documents
containing markdown markup. Unlike other convertors, the Markdown package
makes it easy to redefine how each and every markdown element is rendered.
Creative abuse of the markdown syntax is encouraged. <i class="em em-wink"></i>

 [markdown]: https://daringfireball.net/projects/markdown/basics/
             (Daring Fireball: Markdown Basics)
 [pkg]:      https://ctan.org/pkg/markdown
             (CTAN: Package markdown)


This document is a user manual for the [Markdown package][pkg]. It provides
beginner tutorials and code examples. For an in-depth description of the
package requirements, interfaces, and implementation, please refer to the
[technical documentation][techdoc].

 [techdoc]: http://mirrors.ctan.org/macros/generic/markdown/markdown.pdf
            (A Markdown Interpreter for TeX)


Requirements
------------

The package requires a working TeX distribution. [TeX Live][] ≥ 2013 is known to
work and so are recent installation of [MikTeX][]. If you are using a minimal
installation of a TeX distribution, please consult the
[technical documentation][techdoc] for a detailed list of required packages.

 [TeX Live]: https://www.tug.org/texlive/ (TeX Live - TeX Users Group)
 [MikTeX]: https://miktex.org/ (Home - MiKTeXorg)

Installation
------------

The package comes pre-installed with [TeX Live][] ≥ 2016 and with recent
installations of [MikTeX][]. Unless you explicitly wish to use the latest
version of the package, you are encouraged to skip this step.

To install the package, first download the package from the repository
using Git:
``` sh
git clone https://github.com/witiko/markdown
``````
Next, enter the directory named `markdown` and interpret the file named
`markdown.ins` file using a Unicode-aware TeX engine, such as XeTeX or
LuaTeX:
``` sh
cd markdown
luatex markdown.ins
``````
This should produce the following files:

 * `markdown.lua` – the Lua module,
 * `markdown-cli.lua` – the Lua command-line interface,
 * `markdown.tex` – the plain TeX macro package,
 * `markdown.sty` – the LaTeX package, and
 * `t-markdown.tex` – the ConTeXt module.

### Local installation

To perform a local installation, place the above files into your TeX directory
structure. This is generally where the individual files should be placed:

 * `<TEXMF>/tex/luatex/markdown/markdown.lua`
 * `<TEXMF>/scripts/markdown/markdown-cli.lua`
 * `<TEXMF>/tex/generic/markdown/markdown.tex`
 * `<TEXMF>/tex/latex/markdown/markdown.sty`
 * `<TEXMF>/tex/context/third/markdown/t-markdown.tex`

where `<TEXMF>` corresponds to a root of your TeX distribution, such as
`/usr/share/texmf` and `~/texmf` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf` on Windows systems. When in doubt,
consult the manual of your TeX distribution.

### Portable installation ###

Alternatively, you can also store the above files in the same folder as your
TeX document and distribute them together. This way your document can be
portably typeset on legacy TeX distributions.


First document
--------------

In this section, we will take the necessary steps to typeset our first markdown
document in TeX. This will serve as our first hands-on experience with the
package and also as a reassurance that the package has been correctly installed.

### Using Lua

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input hello
\bye
```````

#### Using the Lua module

Using a text editor, create a text document named `hello.lua` with the
following content:
``` lua
#!/usr/bin/env texlua
local kpse = require("kpse")
kpse.set_program_name("luatex")
local markdown = require("markdown")
local convert = markdown.new()
print(convert("Hello *world*!"))
```````
Next, invoke LuaTeX from the terminal:
``` sh
texlua hello.lua > hello.tex
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the text
“Hello *world*!” Invoking pdfTeX should have the same effect:
``` sh
texlua hello.lua > hello.tex
pdftex document.tex
``````

#### Using the Lua CLI

Using a text editor, create a text document named `hello.md` with the
following content:
``` md
Hello *world*!
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> -- hello.md hello.tex
luatex document.tex
``````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

 [Kpathsea]: https://tug.org/kpathsea/ (Kpathsea - TeX Users Group)

A PDF document named `document.pdf` should be produced and contain the text “Hello
*world*!” Invoking pdfTeX should have the same effect:
``` sh
texlua <CLI␣PATHNAME> -- hello.md hello.tex
pdftex document.tex
``````

### Using plain TeX

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\markdownBegin
Hello *world*!
\markdownEnd
\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the text
“Hello *world*!” Invoking pdfTeX should have the same effect:
``` sh
pdftex --shell-escape document.tex
```````

### Using LaTeX

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}
\begin{markdown}
Hello *world*!
\end{markdown}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the text “Hello
*world*!” Invoking pdfTeX should have the same effect:
``` sh
pdflatex --shell-escape document.tex
``````

***

As the next step, try typesetting the example documents distributed along with
the Markdown package:
``` sh
git clone https://github.com/witiko/markdown
cd markdown/examples
lualatex latex.tex
``````
A PDF document named `latex.pdf` should be produced. Open the text documents
`latex.tex` and `example.md` in a text editor to see how the example documents
are structured. Try changing the documents and typesetting them as follows:
``` sh
lualatex latex.tex
``````
to see the effect of your changes.

### Using ConTeXt

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext
\startmarkdown
Hello *world*!
\stopmarkdown
\stoptext
```````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
``````
A PDF document named `document.pdf` should be produced and contain the text “Hello
*world*!” Invoking pdfTeX should have the same effect:
``` sh
texexec --passon=--shell-escape document.tex
``````

***

As the next step, try typesetting the example documents distributed along with
the Markdown package:
``` sh
git clone https://github.com/witiko/markdown
cd markdown/examples
context context.tex
``````
A PDF document named `context.pdf` should be produced. Open the text documents
`context.tex` and `example.md` in a text editor to see how the example documents
are structured. Try changing the documents and typesetting them as follows:
``` sh
context context.tex
``````
to see the effect of your changes.

Examples
========

This section will show how to use the package by example.


Lua
---

The Lua part of the package makes it possible to convert a markdown document
into TeX commands and typeset it later when convenient. Although the typical
user will not find this terribly useful and will instead use the plain TeX,
LaTeX, and ConTeXt macro packages to convert and typeset the markdown documents
in a single step, they will still benefit from learning the options that
control the behavior of the Lua parser.


### Interfaces

The Lua part of the package exposes two interfaces – the `markdown` Lua module,
and the Lua command-line interface (CLI).

#### The Lua module

The `markdown` Lua module exposes the `new(options)` method, which creates a
converter function from markdown to TeX. The properties of the converter
function are specified by the Lua table `options`. The parameter is optional;
when unspecified, the behaviour will be the same as if `options` were an empty
table.

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input example
\bye
```````
Using a text editor, create a text document named `example.lua` with the
following content:
``` lua
#!/usr/bin/env texlua
local kpse = require("kpse")
kpse.set_program_name("luatex")
local markdown = require("markdown")
local convert_safe = markdown.new()
local convert_unsafe = markdown.new({hybrid = true})
local input = [[$\sqrt{-1}$ *equals* $i$.]]
print(convert_safe(input) .. " " .. convert_unsafe(input))
```````
Next, invoke LuaTeX from the terminal:
``` sh
texlua example.lua > example.tex
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the text
“\$\\sqrt{-1}\$ *equals* \$i\$. √-̅1̅ *equals* $i$.” Invoking pdfTeX should have
the same effect:
``` sh
texlua example.lua > example.tex
pdftex document.tex
``````

***

Rather than use the `texlua` interpreter, we can also access the `markdown` Lua
module directly from our document. Using a text editor, create a text document
named `document.tex` with the following content:
``` tex
\input markdown
\input lmfonts
\directlua{
  local markdown = require("markdown")
  local convert_safe = markdown.new()
  local convert_unsafe = markdown.new({hybrid = true})
  local input = [[$\noexpand\sqrt{-1}$ *equals* $i$.]]
  tex.sprint(convert_safe(input) .. " " .. convert_unsafe(input)) }
\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the text
“\$\\sqrt {-1}\$ *equals* \$i\$. √-̅1̅ *equals* $i$.” In this case, we cannot
use pdfTeX, because pdfTeX does not define the `\directlua` TeX command.

#### The Lua CLI

The Lua command-line interface (CLI) accepts the same options as the `markdown`
Lua module, but now the options are specified as command-line parameters.

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input safe\ \input unsafe
\bye
```````
Using a text editor, create a text document named `example.md` with the
following content:
``` md
$\sqrt{-1}$ *equals* $i$.
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> -- example.md safe.tex
texlua <CLI␣PATHNAME> hybrid=true -- example.md unsafe.tex
luatex document.tex
``````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the text
“\$\\sqrt{-1}\$ *equals* \$i\$. √-̅1̅ *equals* $i$.” Invoking pdfTeX should have
the same effect:
``` sh
texlua <CLI␣PATHNAME> -- example.md safe.tex
texlua <CLI␣PATHNAME> hybrid=true -- example.md unsafe.tex
pdftex document.tex
``````


### Options

This section will cover the options recognized by the Lua interface. The
interfaces of the plain TeX, LaTeX, and ConTeXt macro packages recognize these
options as well, in addition to their own options.


#### File and Directory Names


##### Option `cacheDir`

`cacheDir` (default value: `"."`)

:    A path to the directory containing auxiliary cache files. If the last
     segment of the path does not exist, it will be created by the Lua
     command-line and plain TeX implementations. The Lua implementation expects
     that the entire path already exists.

     When iteratively writing and typesetting a markdown document, the cache
     files are going to accumulate over time. You are advised to clean the
     cache directory every now and then, or to set it to a temporary filesystem
     (such as `/tmp` on UN*X systems), which gets periodically emptied.


###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\directlua{
  local markdown = require("markdown")
  local convert = markdown.new({cacheDir = "cache"})
  local input = "Hello *world*!"
  tex.sprint(convert(input)) }
\bye
```````
Create an empty directory named `cache` next to our text document. Then, invoke
LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the text
“Hello *world*!” Several cache files of the Markdown package will also be
produced in the `cache` directory as we requested using the `cacheDir` option.

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input hello
\bye
```````
Using a text editor, create a text document named `hello.md` with the
following content:
``` md
Hello *world*!
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> cacheDir=cache -- hello.md hello.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the text
“Hello *world*!” A directory named `cache` containing several cache files of
the Markdown package will also be produced as we requested using the `cacheDir`
option.

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\def\markdownOptionCacheDir{cache}
\markdownBegin
Hello *world*!
\markdownEnd
\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the text “Hello
*world*!” A directory named `cache` containing several cache files of the
Markdown package will also be produced as we requested using the `cacheDir`
option.

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage[cacheDir=cache]{markdown}
\begin{document}
\begin{markdown}
Hello *world*!
\end{markdown}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the text “Hello
*world*!” A directory named `cache` containing several cache files of the
Markdown package will also be produced as we requested using the `cacheDir`
option.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\def\markdownOptionCacheDir{cache}
\starttext
\startmarkdown
Hello *world*!
\stopmarkdown
\stoptext
```````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
``````
A PDF document named `document.pdf` should be produced and contain the text “Hello
*world*!” A directory named `cache` containing several cache files of the
Markdown package will also be produced as we requested using the `cacheDir`
option.


#### Parser Options


##### Option `blankBeforeBlockquote`

`blankBeforeBlockquote` (default value: `false`)

:    true

     :  Require a blank line between a paragraph and the following blockquote.

     false

     :  Do not require a blank line between a paragraph and the following
        blockquote.


###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\directlua{
  local markdown = require("markdown")
  local newline = [[^^J^^J]]
  local convert, input

  convert = markdown.new()
  input = "A paragraph." .. newline ..
          "> A quote."   .. newline
  tex.sprint(convert(input))

  convert = markdown.new({blankBeforeBlockquote = true})
  input = "A paragraph."   .. newline ..
          "> Not a quote." .. newline
  tex.sprint(convert(input)) }
\bye
```````
Then, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> > A quote.
>
> A paragraph > Not a quote.

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input optionfalse
\input optiontrue
\bye
```````
Using a text editor, create a text document named `content.md` with the
following content:
``` md
A paragraph.
> A quote?
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> -- content.md optionfalse.tex
texlua <CLI␣PATHNAME> blankBeforeBlockquote=true -- content.md optiontrue.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> > A quote?
>
> A paragraph. > A quote?

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown

\markdownBegin
A paragraph.
> A quote.
\markdownEnd

\def\markdownOptionBlankBeforeBlockquote{true}
\markdownBegin
A paragraph.
> Not a quote.
\markdownEnd

\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> > A quote.
>
> A paragraph > Not a quote.

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}

\begin{markdown}
A paragraph.
> A quote.
\end{markdown}

\begin{markdown*}{blankBeforeBlockquote}
A paragraph.
> Not a quote.
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> > A quote.
>
> A paragraph > Not a quote.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext

\startmarkdown
A paragraph.
> A quote.
\stopmarkdown

\def\markdownOptionBlankBeforeBlockquote{true}
\startmarkdown
A paragraph.
> Not a quote.
\stopmarkdown

\stoptext
```````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> > A quote.
>
> A paragraph > Not a quote.


##### Option `blankBeforeCodeFence`

`blankBeforeCodeFence` (default value: `false`)

:    true

     :  Require a blank line between a paragraph and the following fenced code
        block.

     false

     :  Do not require a blank line between a paragraph and the following
        fenced code block.


###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\directlua{
  local markdown = require("markdown")
  local newline = [[^^J^^J]]
  local convert, input

  convert = markdown.new({fencedCode = true})
  input = "A paragraph."   .. newline ..
          "```"            .. newline ..
          "A code fence."  .. newline ..
          "```"            .. newline
  tex.sprint(convert(input))

  convert = markdown.new({
    fencedCode = true, blankBeforeCodeFence = true})
  input = "A paragraph."       .. newline ..
          "```"                .. newline ..
          "Not a code fence."  .. newline ..
          "```"                .. newline
  tex.sprint(convert(input)) }
\bye
```````
Then, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> ```
> A code fence.
> ```
>
> A paragraph. ``` Not a code fence. ```

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input optionfalse
\input optiontrue
\bye
```````
Using a text editor, create a text document named `content.md` with the
following content:
```` md
A paragraph.
```
A code fence?
```
```````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> fencedCode=true -- content.md optionfalse.tex
texlua <CLI␣PATHNAME> fencedCode=true blankBeforeCodeFence=true  -- content.md optiontrue.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> ```
> A code fence?
> ```
>
> A paragraph. ``` A code fence? ```

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
```` tex
\input markdown
\def\markdownOptionFencedCode{true}

\markdownBegin
A paragraph.
```
A code fence.
```
\markdownEnd

\def\markdownOptionBlankBeforeCodeFence{true}
\markdownBegin
A paragraph.
```
Not a code fence.
```
\markdownEnd

\bye
````````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> ```
> A code fence.
> ```
>
> A paragraph. ``` Not a code fence. ```

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
```` tex
\documentclass{article}
\usepackage[fencedCode]{markdown}
\begin{document}

\begin{markdown}
A paragraph.
```
A code fence.
```
\end{markdown}

\begin{markdown*}{blankBeforeCodeFence}
A paragraph.
```
Not a code fence.
```
\end{markdown*}

\end{document}
````````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> ```
> A code fence.
> ```
>
> A paragraph. ``` Not a code fence. ```

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
```` tex
\usemodule[t][markdown]
\def\markdownOptionFencedCode{true}
\starttext

\startmarkdown
A paragraph.
```
A code fence.
```
\stopmarkdown

\def\markdownOptionBlankBeforeCodeFence{true}
\startmarkdown
A paragraph.
```
Not a code fence.
```
\stopmarkdown

\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> ```
> A code fence.
> ```
>
> A paragraph. ``` Not a code fence. ```


##### Option `blankBeforeHeading`

`blankBeforeHeading` (default value: `false`)

:    true

     :  Require a blank line between a paragraph and the following header.

     false

     :  Do not require a blank line between a paragraph and the following
        header.


###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\def\markdownRendererHeadingOne#1{{\bf #1}\par}
\directlua{
  local markdown = require("markdown")
  local newline = [[^^J^^J]]
  local convert, input

  convert = markdown.new()
  input = "A paragraph." .. newline ..
          "A heading."   .. newline ..
          "=========="   .. newline
  tex.sprint(convert(input))

  convert = markdown.new({blankBeforeHeading = true})
  input = "A paragraph."    .. newline ..
          "Not a heading."  .. newline ..
          "=============="  .. newline
  tex.sprint(convert(input)) }
\bye
```````
Then, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> A heading.
> ==========
>
> A paragraph. Not a heading. ==============

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input optionfalse
\input optiontrue
\bye
```````
Using a text editor, create a text document named `content.md` with the
following content:
``` md
A paragraph.
A heading?
==========
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> -- content.md optionfalse.tex
texlua <CLI␣PATHNAME> blankBeforeHeading=true  -- content.md optiontrue.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> A heading?
> ==========
>
> A paragraph. A heading? ==========

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown

\markdownBegin
A paragraph.
A heading.
==========
\markdownEnd

\def\markdownOptionBlankBeforeHeading{true}
\markdownBegin
A paragraph.
Not a heading.
==============
\markdownEnd

\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> A heading.
> ==========
>
> A paragraph. Not a heading. ==============

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}

\begin{markdown}
A paragraph.
A heading.
==========
\end{markdown}

\begin{markdown*}{blankBeforeHeading}
A paragraph.
Not a heading.
==============
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> A heading.
> ==========
>
> A paragraph. Not a heading. ==============

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext

\startmarkdown
A paragraph.
A heading.
==========
\stopmarkdown

\def\markdownOptionBlankBeforeHeading{true}
\startmarkdown
A paragraph.
Not a heading.
==============
\stopmarkdown

\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> A paragraph.
>
> A heading.
> ==========
>
> A paragraph. Not a heading. ==============


##### Option `breakableBlockquotes`

`breakableBlockquotes` (default value: `false`)

:    true

     :  A blank line separates block quotes.

     false

     :  Blank lines in the middle of a block quote are ignored.


###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\def\markdownRendererHeadingOne#1{{\bf #1}\par}
\directlua{
  local markdown = require("markdown")
  local newline = [[^^J^^J]]
  local convert, input

  convert = markdown.new()
  input = "> A single"     .. newline .. newline ..
          "> block quote." .. newline
  tex.sprint(convert(input))

  convert = markdown.new({breakableBlockquotes = true})
  input = "> A block quote."       .. newline .. newline ..
          "> Another block quote." .. newline
  tex.sprint(convert(input)) }
\bye
```````
Then, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the
following text:

> > A single block quote.
>
> > A block quote.
>
> > Another block quote.

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input optionfalse
\input optiontrue
\bye
```````
Using a text editor, create a text document named `content.md` with the
following content:
``` md
> A single block quote

> or two block quotes?
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> -- content.md optionfalse.tex
texlua <CLI␣PATHNAME> breakableBlockquotes=true  -- content.md optiontrue.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the
following text:

> > A single block quote or two block quotes?
>
> > A single block quote
>
> > or two block quotes?

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown

\markdownBegin
> A single

> block quote.
\markdownEnd

\def\markdownOptionBreakableBlockquotes{true}
\markdownBegin
> A block quote.

> Another block quote.
\markdownEnd

\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> > A single block quote.
>
> > A block quote.
>
> > Another block quote.

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}

\begin{markdown}
> A single

> block quote.
\end{markdown}

\begin{markdown*}{breakableBlockquotes}
> A block quote.

> Another block quote.
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> > A single block quote.
>
> > A block quote.
>
> > Another block quote.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext

\startmarkdown
> A single

> block quote.
\stopmarkdown

\def\markdownOptionBreakableBlockquotes{true}
\startmarkdown
> A block quote.

> Another block quote.
\stopmarkdown

\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> > A single block quote.
>
> > A block quote.
>
> > Another block quote.


##### Option `citationNbsps`

`citationNbsps` (default value: `true`)

:    true

     :  Replace regular spaces with non-breakable spaces inside the prenotes
        and postnotes of citations produced via the pandoc citation syntax
        extension.

     false

     :  Do not replace regular spaces with non-breakable spaces inside the
        prenotes and postnotes of citations produced via the pandoc citation
        syntax extension.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.bib` with the
following content:
``` bib
@book{knuth:tex,
  author    = "Knuth, Donald Ervin",
  title     = "The \TeX book, volume A of Computers and typesetting",
  publisher = "Addison-Wesley",
  year      = "1984" }
```````
Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage[citations]{markdown}
\begin{document}

\begin{markdown}
The TeXbook [@knuth:tex, p. 123 and 130] is good.
\end{markdown}

\begin{markdown*}{citationNbsps = false}
The TeXbook [@knuth:tex, p. 123 and 130] is good.
\end{markdown*}

\bibliographystyle{plain}
\bibliography{document.bib}
\end{document}
```````
Next, invoke LuaTeX and BibTeX from the terminal:
``` sh
lualatex document.tex
bibtex document.aux
lualatex document.tex
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text, where the middot (`·`) denotes a non-breakable space:

> The TeXbook [1, p.·123·and·130] is good.
>
> The TeXbook [1, p. 123 and 130] is good.
>
> ### References
> [1] Donald·Ervin Knuth. _The TeXbook, volume A of Computers and typesetting._
>     Addison-Wesley, 1984.


##### Option `citations`

`citations` (default value: `false`)

:    true

     :  Enable the pandoc citation syntax extension:

        ``` md
        Here is a simple parenthetical citation [@doe99] and here
        is a string of several [see @doe99, pp. 33-35; also
        @smith04, chap. 1].

        A parenthetical citation can have a [prenote @doe99] and
        a [@smith04 postnote]. The name of the author can be
        suppressed by inserting a dash before the name of an
        author as follows [-@smith04].

        Here is a simple text citation @doe99 and here is
        a string of several @doe99 [pp. 33-35; also @smith04,
        chap. 1]. Here is one with the name of the author
        suppressed -@doe99.
        ``````

:    false

     :  Disable the pandoc citation syntax extension.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.bib` with the
following content:
``` bib
@book{knuth:tex,
  author    = "Knuth, Donald Ervin",
  title     = "The \TeX book, volume A of Computers and typesetting",
  publisher = "Addison-Wesley",
  year      = "1984" }
```````
Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage[backend=biber]{biblatex}
\addbibresource{document.bib}
\usepackage[citations]{markdown}
\begin{document}

\begin{markdown}
The TeXbook [@knuth:tex, p. 123 and 130] was written by @knuth:tex.
\end{markdown}

\printbibliography
\end{document}
```````
Next, invoke LuaTeX and Biber from the terminal:
``` sh
lualatex document.tex
biber document.bcf
lualatex document.tex
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> The TeXbook [1, p.·123 and 130] was written by Knuth [1].
>
> ### References
> [1] Donald Ervin Knuth. _The TeXbook, volume A of Computers and typesetting._
>     Addison-Wesley, 1984.


##### Option `codeSpans`

`codeSpans` (default value: `true`)

:    true

     :  Enable the code span syntax:

        ~~~ md
        Use the `printf()` function.
        ``There is a literal backtick (`) here.``
        ~~~

:    false

     :  Disable the code span syntax. This allows you to easily
        use the quotation mark ligatures in texts that do not contain code
        spans:

        ~~~
        ``This is a quote.''
        ~~~~~~


###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\directlua{
  local markdown = require("markdown")
  local convert = markdown.new()
  local input =
    "``This is a code span.'' " ..
    "``This is no longer a code span.''"
  tex.sprint(convert(input)) }
\par
\directlua{
  local markdown = require("markdown")
  local convert = markdown.new({codeSpans = false})
  local input =
    "``This is a quote.'' " ..
    "``This is another quote.''"
  tex.sprint(convert(input)) }
\bye
```````
Then, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the
following text:

> ``This is a code span.'' ``This is no longer a code span.''
>
> “This is a quote.” “This is another quote.”

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input optionfalse
\par
\input optiontrue
\bye
```````
Using a text editor, create a text document named `content.md` with the
following content:
``` md
``Is this a code span?'' ``Or a quote?''
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> codeSpans=false -- content.md optionfalse.tex
texlua <CLI␣PATHNAME> -- content.md optiontrue.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the
following text:

> “Is this a code span?” “Or a quote?”
>
> ``Is this a code span?'' ``Or a quote?''

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown

\markdownBegin
``This is a code span.''
``This is no longer a code span.''
\markdownEnd

\def\markdownOptionCodeSpans{false}
\markdownBegin
``This is a quote.''
``This is another quote.''
\markdownEnd

\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> ``This is a code span.'' ``This is no longer a code span.''
>
> “This is a quote.” “This is another quote.”

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}

\begin{markdown}
``This is a code span.''
``This is no longer a code span.''
\end{markdown}

\begin{markdown*}{codeSpans=false}
``This is a quote.''
``This is another quote.''
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> ``This is a code span.'' ``This is no longer a code span.''
>
> “This is a quote.” “This is another quote.”

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext

\startmarkdown
``This is a code span.''
``This is no longer a code span.''
\stopmarkdown

\def\markdownOptionCodeSpans{false}
\startmarkdown
``This is a quote.''
``This is another quote.''
\stopmarkdown

\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> ``This is a code span.'' ``This is no longer a code span.''
>
> “This is a quote.” “This is another quote.”


##### Option `contentBlocks`

`contentBlocks` (default value: `false`)

:    true

     :   Enable the
         iA Writer content blocks syntax extension:

        ``` md
        http://example.com/minard.jpg (Napoleon's
          disastrous Russian campaign of 1812)
        /Flowchart.png "Engineering Flowchart"
        /Savings Account.csv 'Recent Transactions'
        /Example.swift
        /Lorem Ipsum.txt
        ``````

:    false

     :   Disable the
         iA Writer content blocks syntax extension.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `table.csv` with the
following content:
``` csv
Name,Surname,Born
Albert,Einstein,1879
Marie,Curie,1867
Thomas,Edison,1847
```````
Create also a text document named `markdown-languages.json` with the following
content:
``` js
{
  "tex": "LaTeX"
}
``````
Create also a text document named `code.tex` with the following content:
``` tex
This is an example code listing in \LaTeX.
```````
Create also a text document named `part.md` with the following content:
``` md
This is a *transcluded markdown document*.
``````
Create also a text document named `document.tex` with the following content:
``` tex
\documentclass{article}
\usepackage{minted}
\usepackage[contentBlocks]{markdown}
\begin{document}
\begin{markdown}
/table.csv  (An example table)
/code.tex   (An example code listing)
/part.md    (A file transclusion example)
\end{markdown}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex --shell-escape document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> | Name   | Surname  | Born |
> | ------ | ---------| ---- |
> | Albert | Einstein | 1879 |
> | Marie  | Curie    | 1867 |
> | Thomas | Edison   | 1847 |
>
> Table 1: An example table
>
> ``` tex
> This is an example code listing in \LaTeX.
> ```````
>
> This is a *transcluded markdown document*.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `table.csv` with the
following content:
``` csv
Name,Surname,Born
Albert,Einstein,1879
Marie,Curie,1867
Thomas,Edison,1847
```````
Create also a text document named `markdown-languages.json` with the following
content:
``` js
{
  "tex": "ConTeXt"
}
``````
Create also a text document named `code.tex` with the following content:
``` tex
This is an example code listing in \ConTeXt.
```````
Create also a text document named `part.md` with the following content:
``` md
This is a *transcluded markdown document*.
``````
Create also a text document named `document.tex` with the following content:
``` tex
\usemodule[t][markdown]
\def\markdownOptionContentBlocks{true}
\definetyping [ConTeXt]
\setuptyping  [ConTeXt] [option=TEX]
\starttext
\startmarkdown
/table.csv  (An example table)
/code.tex   (An example code listing)
/part.md    (A file transclusion example)
\stopmarkdown
\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> | Name   | Surname  | Born |
> | ------ | ---------| ---- |
> | Albert | Einstein | 1879 |
> | Marie  | Curie    | 1867 |
> | Thomas | Edison   | 1847 |
>
> Table 1: An example table
>
> ``` tex
> This is an example code listing in \ConTeXt.
> ```````
>
> This is a *transcluded markdown document*.


##### Option `contentBlocksLanguageMap`

`contentBlocksLanguageMap` (default value: `"markdown-languages.json"`)

:    The filename of the JSON file that maps filename extensions to
     programming language names in the iA Writer content blocks.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `table.csv` with the
following content:
``` csv
Name,Surname,Born
Albert,Einstein,1879
Marie,Curie,1867
Thomas,Edison,1847
```````
Create also a text document named `language-map.json` with the following
content:
``` js
{
  "tex": "LaTeX"
}
``````
Create also a text document named `code.tex` with the following content:
``` tex
This is an example code listing in \LaTeX.
```````
Create also a text document named `part.md` with the following content:
``` md
This is a *transcluded markdown document*.
``````
Create also a text document named `document.tex` with the following content:
``` tex
\documentclass{article}
\usepackage{minted}
\usepackage[contentBlocks]{markdown}
\markdownSetup{
  contentBlocksLanguageMap = {language-map.json},
}
\begin{document}
\begin{markdown}
/table.csv  (An example table)
/code.tex   (An example code listing)
/part.md    (A file transclusion example)
\end{markdown}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex --shell-escape document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> | Name   | Surname  | Born |
> | ------ | ---------| ---- |
> | Albert | Einstein | 1879 |
> | Marie  | Curie    | 1867 |
> | Thomas | Edison   | 1847 |
>
> Table 1: An example table
>
> ``` tex
> This is an example code listing in \LaTeX.
> ```````
>
> This is a *transcluded markdown document*.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `table.csv` with the
following content:
``` csv
Name,Surname,Born
Albert,Einstein,1879
Marie,Curie,1867
Thomas,Edison,1847
```````
Create also a text document named `language-map.json` with the following
content:
``` js
{
  "tex": "ConTeXt"
}
``````
Create also a text document named `code.tex` with the following content:
``` tex
This is an example code listing in \ConTeXt.
```````
Create also a text document named `part.md` with the following content:
``` md
This is a *transcluded markdown document*.
``````
Create also a text document named `document.tex` with the following content:
``` tex
\usemodule[t][markdown]
\def\markdownOptionContentBlocks{true}
\def\markdownOptionContentBlocksLanguageMap{language-map.json}
\definetyping [ConTeXt]
\setuptyping  [ConTeXt] [option=TEX]
\starttext
\startmarkdown
/table.csv  (An example table)
/code.tex   (An example code listing)
/part.md    (A file transclusion example)
\stopmarkdown
\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> | Name   | Surname  | Born |
> | ------ | ---------| ---- |
> | Albert | Einstein | 1879 |
> | Marie  | Curie    | 1867 |
> | Thomas | Edison   | 1847 |
>
> Table 1: An example table
>
> ``` tex
> This is an example code listing in \ConTeXt.
> ```````
>
> This is a *transcluded markdown document*.


##### Option `definitionLists`

`definitionLists` (default value: `false`)

:    true

     :  Enable the pandoc definition list syntax extension:

        ``` md
        Term 1

        :   Definition 1

        Term 2 with *inline markup*

        :   Definition 2

                { some code, part of Definition 2 }

            Third paragraph of definition 2.
        `````

:    false

     :  Disable the pandoc definition list syntax extension.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage[definitionLists]{markdown}
\begin{document}
\begin{markdown}
Term 1

:   Definition 1

Term 2 with *inline markup*

:   Definition 2

        { some code, part of Definition 2 }

    Third paragraph of definition 2.
\end{markdown}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> Term 1
>
> :   Definition 1
>
> Term 2 with *inline markup*
>
> :   Definition 2
>
>         { some code, part of Definition 2 }
>
>     Third paragraph of definition 2.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\def\markdownOptionDefinitionLists{true}
\starttext
\startmarkdown
Term 1

:   Definition 1

Term 2 with *inline markup*

:   Definition 2

        { some code, part of Definition 2 }

    Third paragraph of definition 2.
\stopmarkdown
\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> Term 1
>
> :   Definition 1
>
> Term 2 with *inline markup*
>
> :   Definition 2
>
>         { some code, part of Definition 2 }
>
>     Third paragraph of definition 2.


##### Option `fencedCode`

`fencedCode` (default value: `false`)

:    true

     :  Enable the commonmark fenced code block extension:

        ~~~~~~~~ md
        ~~~ js
        if (a > 3) {
            moveShip(5 * gravity, DOWN);
        }
        ~~~~~~

          ``` html
          <pre>
            <code>
              // Some comments
              line 1 of code
              line 2 of code
              line 3 of code
            </code>
          </pre>
          ```
        ~~~~~~~~~~~

:    false

     :  Disable the commonmark fenced code block extension.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{minted}
\usepackage[fencedCode]{markdown}
\begin{document}
\begin{markdown}
~~~ js
if (a > 3) {
    moveShip(5 * gravity, DOWN);
}
~~~~~~

  ``` html
  <pre>
    <code>
      // Some comments
      line 1 of code
      line 2 of code
      line 3 of code
    </code>
  </pre>
  ```
\end{markdown}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex --shell-escape document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> ~~~ js
> if (a > 3) {
>     moveShip(5 * gravity, DOWN);
> }
> ~~~~~~
>
> ``` html
> <pre>
>   <code>
>     // Some comments
>     line 1 of code
>     line 2 of code
>     line 3 of code
>   </code>
> </pre>
> ```

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\def\markdownOptionFencedCode{true}
\definetyping [js]
\definetyping [html]
\setuptyping  [html] [option=XML]
\starttext
\startmarkdown
~~~ js
if (a > 3) {
    moveShip(5 * gravity, DOWN);
}
~~~~~~

  ``` html
  <pre>
    <code>
      // Some comments
      line 1 of code
      line 2 of code
      line 3 of code
    </code>
  </pre>
  ```
\stopmarkdown
\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> ~~~ js
> if (a > 3) {
>     moveShip(5 * gravity, DOWN);
> }
> ~~~~~~
>
> ``` html
> <pre>
>   <code>
>     // Some comments
>     line 1 of code
>     line 2 of code
>     line 3 of code
>   </code>
> </pre>
> ```


##### Option `footnotes`

`footnotes` (default value: `false`)

:    true

     :  Enable the pandoc footnote syntax extension:

        ``` md
        Here is a footnote reference,[^1] and another.[^longnote]

        [^1]: Here is the footnote.

        [^longnote]: Here's one with multiple blocks.

            Subsequent paragraphs are indented to show that they
        belong to the previous footnote.

                { some.code }

            The whole paragraph can be indented, or just the
            first line.  In this way, multi-paragraph footnotes
            work like multi-paragraph list items.

        This paragraph won't be part of the note, because it
        isn't indented.
        ``````

:    false

     :    Disable the pandoc footnote syntax extension.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage[footnotes]{markdown}
\begin{document}
\begin{markdown}
Here is a footnote reference,[^1] and another.[^longnote]

[^1]: Here is the footnote.

[^longnote]: Here's one with multiple blocks.

    Subsequent paragraphs are indented to show that they
belong to the previous footnote.

        { some.code }

    The whole paragraph can be indented, or just the
    first line.  In this way, multi-paragraph footnotes
    work like multi-paragraph list items.

This paragraph won't be part of the note, because it
isn't indented.
\end{markdown}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> Here is a footnote reference,[^1] and another.[^longnote]
>
> [^1]: Here is the footnote.
>
> [^longnote]: Here's one with multiple blocks.
>
>     Subsequent paragraphs are indented to show that they
> belong to the previous footnote.
>
>         { some.code }
>
>     The whole paragraph can be indented, or just the
>     first line.  In this way, multi-paragraph footnotes
>     work like multi-paragraph list items.
>
> This paragraph won't be part of the note, because it
> isn't indented.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\def\markdownOptionFootnotes{true}
\starttext
\startmarkdown
Here is a footnote reference,[^1] and another.[^longnote]

[^1]: Here is the footnote.

[^longnote]: Here's one with multiple blocks.

    Subsequent paragraphs are indented to show that they
belong to the previous footnote.

        { some.code }

    The whole paragraph can be indented, or just the
    first line.  In this way, multi-paragraph footnotes
    work like multi-paragraph list items.

This paragraph won't be part of the note, because it
isn't indented.
\stopmarkdown
\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> Here is a footnote reference,[^1] and another.[^longnote]
>
> [^1]: Here is the footnote.
>
> [^longnote]: Here's one with multiple blocks.
>
>     Subsequent paragraphs are indented to show that they
> belong to the previous footnote.
>
>         { some.code }
>
>     The whole paragraph can be indented, or just the
>     first line.  In this way, multi-paragraph footnotes
>     work like multi-paragraph list items.
>
> This paragraph won't be part of the note, because it
> isn't indented.


##### Option `hashEnumerators`

`hashEnumerators` (default value: `false`)

:    true

     :  Enable the use of hash symbols (`#`) as ordered item list
        markers:

        ``` md
        #. Bird
        #. McHale
        #. Parish
        ``````

:    false

     :  Disable the use of hash symbols (`#`) as ordered item list
        markers.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}

\begin{markdown}
#. Bird
#. McHale
#. Parish
\end{markdown}

\begin{markdown*}{hashEnumerators}
#. Bird
#. McHale
#. Parish
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> . Bird
> ========
> . McHale
> ========
> . Parish
> ========
>
> #. Bird
> #. McHale
> #. Parish

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext

\startmarkdown
#. Bird
#. McHale
#. Parish
\stopmarkdown

\def\markdownOptionHashEnumerators{true}
\startmarkdown
#. Bird
#. McHale
#. Parish
\stopmarkdown

\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> . Bird
> ========
> . McHale
> ========
> . Parish
> ========
>
> #. Bird
> #. McHale
> #. Parish


##### Option `html`

`html` (default value: `false`)

:    true

     :  Enable the recognition of HTML tags, block elements, comments, HTML
        instructions, and entities in the input.  Tags, block elements (along
        with contents), HTML instructions, and comments will be ignored and
        HTML entities will be replaced with the corresponding Unicode
        codepoints.

:    false

     :  Disable the recognition of HTML markup. Any HTML markup in the input
        will be rendered as plain text.


When the option is enabled, HTML entities are currently incorrectly parsed.
See [the corresponding issue][issue-38] in the package repository.

 [issue-36]: https://github.com/Witiko/markdown/issues/36
             (HTML entities with the `html` Lua option enabled produce a Lua
              parser error)

###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\directlua{
  local markdown = require("markdown")
  local convert = markdown.new()
  local newline = [[^^J^^J]]
  local input =
    "<div>*There is no block tag support.*</div>"        .. newline ..
    "*There is no <inline tag="tag"></inline> support.*" .. newline ..
    "_There is no <!-- comment --> support._"            .. newline ..
    "_There is no <? HTML instruction ?> support._"
  tex.sprint(convert(input)) }
\par
\directlua{
  local markdown = require("markdown")
  local convert = markdown.new({html = true})
  local input =
    "<div>*There is block tag support.*</div>"        .. newline ..
    "*There is <inline tag="tag"></inline> support.*" .. newline ..
    "_There is <!-- comment --> support._"            .. newline ..
    "_There is <? HTML instruction ?> support._"
  tex.sprint(convert(input)) }
\bye
```````
Then, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the
following text:

> \<div>There is no block tag support.\</div>
> There is no \<inline tag=”tag”>\</inline> support.
> There is no \<!-- comment --> support.
> There is no <? HTML instruction ?> support.
>
> There is support. There is support. There is support.

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input optionfalse
\par
\input optiontrue
\bye
```````
Using a text editor, create a text document named `content.md` with the
following content:
``` html
<div>
*Is there block tag support?*
</div>
*Is there <inline tag="tag"></inline> support?*
_Is there <!-- comment --> support?_
_Is there <? HTML instruction ?> support?_
````````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> -- content.md optionfalse.tex
texlua <CLI␣PATHNAME> html=true -- content.md optiontrue.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the
following text:

> \<div>Is there block tag support?\</div>
> Is there \<inline tag=”tag”>\</inline> support?
> Is there \<!-- comment --> support?
> Is there <? HTML instruction ?> support?
>
> Is there support? Is there support? Is there support?

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts

\markdownBegin
<div>
*There is no block tag support.*
</div>
*There is no <inline tag="tag"></inline> support.*
_There is no <!-- comment --> support._
_There is no <? HTML instruction ?> support._
\markdownEnd

\def\markdownOptionHtml{true}
\markdownBegin
<div>
*There is block tag support.*
</div>
*There is <inline tag="tag"></inline> support.*
_There is <!-- comment --> support._
_There is <? HTML instruction ?> support._
\markdownEnd

\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> \<div>There is no block tag support.\</div>
> There is no \<inline tag=”tag”>\</inline> support.
> There is no \<!-- comment --> support.
> There is no <? HTML instruction ?> support.
>
> There is support. There is support. There is support.

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}

\begin{markdown}
<div>
*There is no block tag support.*
</div>
*There is no <inline tag="tag"></inline> support.*
_There is no <!-- comment --> support._
_There is no <? HTML instruction ?> support._
\end{markdown}

\begin{markdown*}{html}
<div>
*There is block tag support.*
</div>
*There is <inline tag="tag"></inline> support.*
_There is <!-- comment --> support._
_There is <? HTML instruction ?> support._
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> \<div>There is no block tag support.\</div>
> There is no \<inline tag=”tag”>\</inline> support.
> There is no \<!-- comment --> support.
> There is no <? HTML instruction ?> support.
>
> There is support. There is support. There is support.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext

\startmarkdown
<div>
*There is no block tag support.*
</div>
*There is no <inline tag="tag"></inline> support.*
_There is no <!-- comment --> support._
_There is no <? HTML instruction ?> support._
\stopmarkdown

\def\markdownOptionHtml{true}
\startmarkdown
<div>
*There is block tag support.*
</div>
*There is <inline tag="tag"></inline> support.*
_There is <!-- comment --> support._
_There is <? HTML instruction ?> support._
\stopmarkdown

\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> \<div>There is no block tag support.\</div>
> There is no \<inline tag=”tag”>\</inline> support.
> There is no \<!-- comment --> support.
> There is no <? HTML instruction ?> support.
>
> There is support. There is support. There is support.


##### Option `hybrid`

`hybrid` (default value: `true`)

:    true

     :  Disable the escaping of special plain TeX characters, which makes it
        possible to intersperse your markdown markup with TeX code.  The
        intended usage is in documents prepared manually by a human author.
        In such documents, it can often be desirable to mix TeX and markdown
        markup freely.

:    false

     :  Enable the escaping of special plain TeX characters outside verbatim
        environments, so that they are not interpretted by TeX.  This is
        encouraged when typesetting automatically generated content or
        markdown documents that were not prepared with this package in mind.


###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\directlua{
  local markdown = require("markdown")
  local convert_safe = markdown.new()
  local convert_unsafe = markdown.new({hybrid = true})
  local input = [[$\noexpand\sqrt{-1}$ *equals* $i$.]]
  tex.sprint(convert_safe(input) .. " " .. convert_unsafe(input)) }
\bye
```````
Then, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the
following text:

> \$\\sqrt {-1}\$ *equals* \$i\$.
> √-̅1̅ *equals* $i$.

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\input optionfalse
\input optiontrue
\bye
```````
Using a text editor, create a text document named `content.md` with the
following content:
``` md
$\sqrt{-1}$ *equals* $i$.
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> -- content.md optionfalse.tex
texlua <CLI␣PATHNAME> hybrid=true -- content.md optiontrue.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the
following text:

> \$\\sqrt {-1}\$ *equals* \$i\$.
> √-̅1̅ *equals* $i$.

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\input lmfonts
\markdownBegin
$\sqrt{-1}$ *equals* $i$.
\markdownEnd
\def\markdownOptionHybrid{true}
\markdownBegin
$\sqrt{-1}$ *equals* $i$.
\markdownEnd
\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> \$\\sqrt {-1}\$ *equals* \$i\$.
> √-̅1̅ *equals* $i$.

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}
\begin{markdown}
$\sqrt{-1}$ *equals* $i$.
\end{markdown}
\begin{markdown*}{hybrid}
$\sqrt{-1}$ *equals* $i$.
\end{markdown*}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> \$\\sqrt {-1}\$ *equals* \$i\$.
> √-̅1̅ *equals* $i$.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext
\startmarkdown
$\sqrt{-1}$ *equals* $i$.
\stopmarkdown
\def\markdownOptionHybrid{true}
\startmarkdown
$\sqrt{-1}$ *equals* $i$.
\stopmarkdown
\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> \$\\sqrt {-1}\$ *equals* \$i\$.
> √-̅1̅ *equals* $i$.


##### Option `inlineFootnotes`

`inlineFootnotes` (default value: `false`)

:    true

     :  Enable the pandoc inline footnote syntax extension:

        ``` md
        Here is an inline note.^[Inlines notes are easier to
        write, since you don't have to pick an identifier and
        move down to type the note.]
        ``````

:    false

     :  Disable the pandoc inline footnote syntax extension.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage[footnotes, inlineFootnotes]{markdown}
\begin{document}
\begin{markdown}
Here is an inline note.^[Inlines notes are easier to
write, since you don't have to pick an identifier and
move down to type the note.]
\end{markdown}
\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> Here is an inline note.^[Inlines notes are easier to
> write, since you don't have to pick an identifier and
> move down to type the note.]

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\def\markdownOptionFootnotes{true}
\def\markdownOptionInlineFootnotes{true}
\starttext
\startmarkdown
Here is an inline note.^[Inlines notes are easier to
write, since you don't have to pick an identifier and
move down to type the note.]
\stopmarkdown
\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> Here is an inline note.^[Inlines notes are easier to
> write, since you don't have to pick an identifier and
> move down to type the note.]


##### Option `preserveTabs`

`preserveTabs` (default value: `false`)

:    true

     :  Preserve all tabs in the input.

:    false

     :  Convert any tabs in the input to spaces.


This option is currently non-functional. See [the corresponding
issue][issue-38] in the package repository.

 [issue-38]: https://github.com/Witiko/markdown/issues/38
             (Tabs are stripped even with the `preserveTabs=true`
              Lua option enabled)


##### Option `smartEllipses`

`smartEllipses` (default value: `false`)

:    true

     :   Convert any ellipses in the input to the
         `\markdownRendererEllipsis` TeX macro.

:    false

     :  Preserve all ellipses in the input.


###### Lua module example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\def\markdownRendererEllipsis{. . .}
\input lmfonts
\directlua{
  local markdown = require("markdown")
  local convert = markdown.new()
  input = "These are just three regular dots ..."
  tex.sprint(convert(input)) }
\par
\directlua{
  local markdown = require("markdown")
  local convert = markdown.new({smartEllipses = true})
  input = "... and this is a victorian ellipsis."
  tex.sprint(convert(input)) }
\bye
```````
Then, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
```````
A PDF document named `document.pdf` should be produced and contain the
following text:

> These are just three regular dots ...
>
> . . . and this is a victorian ellipsis.

###### Lua CLI example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\def\markdownRendererEllipsis{. . .}
\input lmfonts
\input optionfalse
\par
\input optiontrue
\bye
```````
Using a text editor, create a text document named `content.md` with the
following content:
``` md
Are these just three regular dots, a victorian ellipsis, or ... ?
``````
Next, invoke LuaTeX from the terminal:
``` sh
texlua <CLI␣PATHNAME> -- content.md optionfalse.tex
texlua <CLI␣PATHNAME> smartEllipses=true -- content.md optiontrue.tex
luatex document.tex
```````
where `<CLI␣PATHNAME>` corresponds to the location of the Lua CLI script file,
such as `~/texmf/scripts/markdown/markdown-cli.lua` on UN\*X systems or
`C:\Users\<YOUR␣USERNAME>\texmf\scripts\markdown\markdown-cli.lua` on Windows
systems. Use the command `kpsewhich markdown-cli.lua` to locate the Lua CLI
script file using [Kpathsea][].

A PDF document named `document.pdf` should be produced and contain the
following text:

> Are these just three regular dots, a victorian ellipsis, or ... ?
>
> Are these just three regular dots, a victorian ellipsis, or . . . ?

###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\def\markdownRendererEllipsis{. . .}

\markdownBegin
These are just three regular dots ...
\markdownEnd

\def\markdownOptionSmartEllipses{true}
\markdownBegin
... and this is a victorian ellipsis.
\markdownEnd

\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> These are just three regular dots ...
>
> . . . and this is a victorian ellipsis.

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\markdownSetup{
  renderers = {
    ellipsis = {. . .} }}
\begin{document}

\begin{markdown}
These are just three regular dots ...
\end{markdown}

\begin{markdown*}{smartEllipses}
... and this is a victorian ellipsis.
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> These are just three regular dots ...
>
> . . . and this is a victorian ellipsis.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\def\markdownRendererEllipsis{. . .}
\starttext

\startmarkdown
These are just three regular dots ...
\stopmarkdown

\def\markdownOptionSmartEllipses{true}
\startmarkdown
... and this is a victorian ellipsis.
\stopmarkdown

\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> These are just three regular dots ...
>
> . . . and this is a victorian ellipsis.


##### Option `startNumber`

`startNumber` (default value: `true`)

:    true

     :   Make the number in the first item of an ordered lists significant. The
         item numbers will be passed to the
         `\markdownRendererOlItemWithNumber` TeX macro.

:    false

     :   Ignore the numbers in the ordered list items. Each item will only
         produce a
         `\markdownRendererOlItem` TeX macro.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}

\begin{markdown}
The following list respects the numbers specified in the markup:

3. third item
4. fourth item
5. fifth item
\end{markdown}

\begin{markdown*}{startNumber=false}
The following list does not respect the numbers specified in the
markup:

3. third item
4. fourth item
5. fifth item
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> The following list respects the numbers specified in the markup:
>
> 3. third item
> 4. fourth item
> 5. fifth item
>
> The following list does not respect the numbers specified in the markup:
>
> 1. third item
> 2. fourth item
> 3. fifth item

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\starttext

\startmarkdown
The following list respects the numbers specified in the markup:

3. third item
4. fourth item
5. fifth item
\stopmarkdown

\def\markdownOptionStartNumber{false}
\startmarkdown
The following list respects the numbers specified in the markup:

3. third item
4. fourth item
5. fifth item
\stopmarkdown
\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> The following list respects the numbers specified in the markup:
>
> 3. third item
> 4. fourth item
> 5. fifth item
>
> The following list does not respect the numbers specified in the markup:
>
> 1. third item
> 2. fourth item
> 3. fifth item


##### Option `tightLists`

`tightLists` (default value: `true`)

:    true

     :   Lists whose bullets do not consist of multiple paragraphs will be
         passed to the
         `\markdownRendererOlBeginTight`, `\markdownRendererOlEndTight`,
         `\markdownRendererUlBeginTight`, `\markdownRendererUlEndTight`,
         `\markdownRendererDlBeginTight`, and
         `\markdownRendererDlEndTight` TeX macros.

:    false

     :   Lists whose bullets do not consist of multiple paragraphs will be
         treated the same way as lists that do consist of multiple paragraphs.


###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage{markdown}
\begin{document}

\begin{markdown}
The following list is tight:

  - first item
  - second item
  - third item

The following list is loose:

  - first item
  - second item that spans

    multiple paragraphs
  - third item
\end{markdown}

\begin{markdown*}{tightLists=false}
The following list is now also loose:

  - first item
  - second item
  - third item
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> The following list is tight:
>
>   - first item
>   - second item
>   - third item
>
> The following list is loose:
>
>   - first item
>   - second item that spans
>
>     multiple paragraphs
>   - third item
>
> The following list is now also loose:
>
>   - first item
>
>   - second item
>
>   - third item


##### Option `underscores`

`underscores` (default value: `true`)

:    true

     :  Both underscores and asterisks can be used to denote emphasis and
        strong emphasis:

        ``` md
        *single asterisks*
        _single underscores_
        **double asterisks**
        __double underscores__
        ``````

:    false

     :  Only asterisks can be used to denote emphasis and strong emphasis.
        This makes it easy to write math with the \Opt{hybrid} option
        without the need to constantly escape subscripts.


###### Plain TeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\input markdown
\def\markdownOptionHybrid{true}

\markdownBegin
This is _emphasized text_ and this is a math subscript: $m\_n$.
\markdownEnd

\def\markdownOptionUnderscores{false}
\markdownBegin
This is *emphasized text* and this is a math subscript: $m_n$.
\markdownEnd

\bye
```````
Next, invoke LuaTeX from the terminal:
``` sh
luatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> This is _emphasized text_ and this is a math subscript: *mₙ*.
>
> This is _emphasized text_ and this is a math subscript: *mₙ*.

###### LaTeX example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\documentclass{article}
\usepackage[hybrid]{markdown}
\begin{document}

\begin{markdown}
This is _emphasized text_ and this is a math subscript: $m\_n$.
\end{markdown}

\begin{markdown*}{underscores=false}
This is *emphasized text* and this is a math subscript: $m_n$.
\end{markdown*}

\end{document}
```````
Next, invoke LuaTeX from the terminal:
``` sh
lualatex document.tex
``````
A PDF document named `document.pdf` should be produced and contain the
following text:

> This is _emphasized text_ and this is a math subscript: *mₙ*.
>
> This is _emphasized text_ and this is a math subscript: *mₙ*.

###### ConTeXt example {.unnumbered}

Using a text editor, create a text document named `document.tex` with the
following content:
``` tex
\usemodule[t][markdown]
\def\markdownOptionHybrid{true}
\starttext

\startmarkdown
This is _emphasized text_ and this is a math subscript: $m\_n$.
\stopmarkdown

\def\markdownOptionUnderscores{false}
\startmarkdown
This is *emphasized text* and this is a math subscript: $m_n$.
\stopmarkdown

\stoptext
````````
Next, invoke LuaTeX from the terminal:
``` sh
context document.tex
`````
A PDF document named `document.pdf` should be produced and contain the
following text:

> This is _emphasized text_ and this is a math subscript: *mₙ*.
>
> This is _emphasized text_ and this is a math subscript: *mₙ*.


Plain TeX
---------

The plain TeX macro package provides TeX commands for typesetting markdown
documents that invoke the Lua parser in the background. Beside TeX commands
that correspond to the Lua options, the macro package also provides commands
corresponding to additional plain TeX-specific options, and so-called *token
renderer* commands that define how the individual markdown elements will be
typeset.

### Interfaces

<!-- TODO -->


### Options

<!-- TODO -->


### Token renderers

<!-- TODO -->


LaTeX
-----

The LaTeX macro package provides additional syntactic sugar on top of the plain
TeX macro package and provides sane default definitions of the token renderers.

<!-- TODO -->

### Interfaces

<!-- TODO -->


### Options

<!-- TODO -->


### Token renderers

<!-- TODO -->


ConTeXt
-------

The ConTeXt macro package provides additional syntactic sugar on top of the
plain TeX macro package and provides sane default definitions of the token
renderers.

### Interfaces

<!-- TODO -->

### Options

<!-- TODO -->

### Token renderers

<!-- TODO -->

