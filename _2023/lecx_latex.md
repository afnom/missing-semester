---
layout: lecture
title: "LaTeX"
date: 2023-01-01
ready: false
---

# A quick introduction to LaTeX

## What is LaTeX?

LaTeX is a *document preparation system* which is able to produce high-quality output for various typesetting goals, and is often used in technical setting because of its extensibility and handling of typesetting of math formulae.

It is build on a typesetting system called *TeX* designed by Donald Knuth to typeset 'The Art of Computer Programming' first released in 1978. TeX is a token based programming language, and is the engine behind the extensibility of LaTeX.

LaTeX and its many packages are an extension to TeX. The goal of LaTeX is to simplify preparation of documents, and split the document structure and content from the design.


## When to and not to use LaTeX



# LaTeX with Overleaf

Overleaf is one of many LaTeX editors available to use. To quote their website, Overleaf is "The easy to use, online, collaborative LaTeX editor". It is a freely available browser application, and it has a paid Premium version which is free to access for all University of Birmingham students and staff.
To get started, head to [https://www.overleaf.com/login](https://www.overleaf.com/login) and log in through your institution. We will discuss FOSS alternatives to Overleaf at the end of the lesson.

![overleaf_login](/2023/files/latex/overleaf_login.png "Login page of overleaf.com.")

Let's create a new blank and explore what Overleaf has to offer.

![overleaf_editor](/2023/files/latex/overleaf_editor.png "Overleaf's editor view")

# The basics

## Document structure and a simple example

```latex
% TOPMATTER AND METADATA
\documentclass{article}
\usepackage{amssymb}

\author{A.M. Turing}
\date{28 May, 1936}
\title{On Computable Numbers, with an Application to the Entscheidungsproblem}

% CONTENT
\begin{document}

\maketitle

...

\section{Computing machines}

We have said that the computable numbers are those whose decimals are calculable by finite means. This requires rather more explicit definition. No real attempt will be made to justify the definitions given until we reach Section~\ref{section:extent}. For the present I shall only say that the justification lies in the fact that the human memory is necessarily limited.

We may compare a man in the process of computing a real number to a machine which is only capable of a finite number of conditions $q_1, q_2, \dots, q_R$ which will be called ``$m$-configurations''. The machine is supplied with a ``tape'', (the analogue of paper) running through it, and divided into sections (called ``squares'') each capable of bearing a ``symbol''. At any moment there is just one square, say the $s$-th, bearing the symbol $\mathfrak{S}(r)$ which is ``in the machine''. We may call this square the ``scanned square''. [...]

[...]

\end{document}
```

## Syntax

(La)TeX has several special symbols that are used to invoke commands (e.g., `\maketitle`), environments (e.g., `\begin{document} ... \end{document}`), etc.

*Commands* are entered as backslash followed by a command name which is composed of ASCII letters, e.g., `\command`. Often commands have parameters, and optional parameters which are (usually) entered in curly, and square braces, e.g., `\usepackage[margin=1in]{geometry}`. Square braces denote optional parameters.

*Environments* start with the command `\begin{name}` and end with a matching `\end{name}`. They are used for elements with substantial content (e.g., tables, lists, abstract, figures, etc.). They may also have required and optional parameters which follows the `\begin{name}` command.

*Comments* start with `%` and end with an end of line. Multiline comments must include `%` on the start of each line.

There are several special symbols, e.g., `%`, `#`, `$`, `&`, `\`, `{`, `}`, that have a reserved meaning in the language, and must be replaced by commands in text, e.g., the symbols `$`, `%`, and `&` are typeset by escaping them with a backslash (`\$`, `\#`, and `\&`), `\` is typeset by `\backslash`, `{` and `}` are typeset as `\{` and `\}`.


# Document classes and packages

Every document has a *class* which is selected by the command `\documentclass{...}'. In the above example, the class is a LaTeX standard class `article'.

*Packages* are loaded using the command `\usepackage{...}`. There are uncountably many packages.
Several packages can be loaded at one, e.g., `\usepackage{amsmath,amssymb,amsthm}` loads the packages `amsmath`, `amssymb`, and `amsthm`. Some packages allow parameters which are entered in square brackets, e.g., `\usepackage[margin=2cm]{geometry}` (in this case, only one package can be loaded with a single invocation of `\usepackage`).

# Typesetting more exciting stuff

## Mathematical expressions

LaTeX really starts to shine when used to typeset complex content that cannot be easily formatted in a WYSIWYG editor. A good example of this is mathematical expressions and equations. While this is not supported by LaTeX entirely out-of-the-box, mathematical statements can be typeset using the `amsmath` package with symbols from the `amssymb` package.

Below is a simple equation typeset inline with other text. Observe how the expression is enclosed in `$` signs.

```latex
This is an equation of the form $x + y = z$
```

![output_amsmath_inline](/2023/files/latex/output_amsmath_inline.png "A simple equation inline with other text.")

We can also typeset special characters using specific commands. For example,

```latex
$\alpha \rightarrow \sin(\Delta x)$
```

yields
![output_amsmath_special](/2023/files/latex/output_amsmath_special.png "An equation with special characters.")

Below are a few commonly used maths symbols:

| Category             | Symbols   | Commands                                         |
|----------------------|-----------|--------------------------------------------------|
| Relational operators | ≤ ≥ ≠ ⊂   |`\leq`, `\geq`, `\neq`, `\subset`                 |
| Vector operators     | ⋅ × ∇      | `\cdot`, `\times`, `\nabla`                      |
| Greek letters        | α β δ Δ    | `\alpha`, `\beta`, `\delta`, `\Delta`            |
| Miscellaneous        | ∫ ∑ ∴ ∵ ∎ | `\int`, `\sum`, `\therefore`, `\because`, `\QED` |

A comprehensive list of symbols can be found [here](https://tug.ctan.org/info/symbols/comprehensive/symbols-a4.pdf).

Fractions can be typeset using the `\frac` command which takes two arguments -- the numerator and denominator expression.

```latex
$\frac{5x}{10} = \frac{x}{2}$
```

![output_amsmath_frac](/2023/files/latex/output_amsmath_frac.png "An equation with fractions.")

### Equations and displayed math

So far, we have only learned how to inline our maths expressions. Some use cases may require a full-width expression, say a theorem we might wish to introduce in our document. In plain LaTeX, this is done by enclosing the mathematical expression in `\[` and `\]`, or double dollars (which is discouraged).

```latex
\[
    c^2 = b^2 + a^2
\]
```

The package `amsmath` provides an `equation` environment that numbers such equations throughout the whole document.

```latex
\begin{equation}
    c^2 = b^2 + a^2
\end{equation}
```

![output_amsmath_equation](/2023/files/latex/output_amsmath_equation.png "A full-width equation.")

Observe how LaTeX automatically numbers and centre-aligns the equation.
To typeset and align multiline equations, we use the `align` environment:

```latex
\begin{align}
    x^2 &= 49\\
    \therefore x &= \pm 7
\end{align}
```

![output_amsmath_align](/2023/files/latex/output_amsmath_align.png "A multi-line equation.")

The output can be thought of as a table where columns are demarcated by `&` and rows by `\\`. In other words, `&` denotes the end of a column and `\\` denotes the end of a line. Using this information, `amsmath` typesets the equation such that all `&` are aligned one below the other. Observe how the two lines of differing length are aligned to the `=` symbol.

A handy tip to note is that automatic line numbering can be switched off by suffixing `*` to `amsmath` environments, e.g. `\begin{align*}` and `\end{align*}`. Alternatively, numbering can be turned off for specific lines by starting them with `\nonumber`. For example,

```latex
\begin{align}
    \nonumber x^2 &= 49\\
    \therefore x &= \pm 7
\end{align}
```

![output_amsmath_nonumber](/2023/files/latex/output_amsmath_nonumber.png "A multi-line equation where some lines are unnumbered.")

## Code

As programmers, we often need to embed code snippets or entire programs in documents. There are LaTeX packages that, along with formatting code nicely, also provide syntax highlighting for a variety of languages. We will briefly cover two such packages: `minted` and `listings`.

### Using `minted`

Code is embed inside a `minted` environment that allows for several customisations. For example, the following is some Java code that has been typeset with syntax highlighting and line numbers:

```latex
\begin{minted}[linenos]{java}
class Latex {
    public static final String message = "Typesetting code with LaTeX!";
}
\end{minted}
```

![output_minted](/2023/files/latex/output_minted.png "Some Java code typeset using minted.")

Similar to `linenos`, we can pass other options to the `minted` environment. Below is some python code highlighted using vim's colour scheme and with a black background.

```latex
\begin{minted}[style=vim,bgcolor=black]{python}
def add(x, y):
    return x + y
\end{minted}
```

![output_minted_vim_scheme](/2023/files/latex/output_minted_vim_scheme.png "Some python code typeset using minted.")

`minted` also allows for for embedding a single line of code:

```latex
\mint{js}|let someVariable = 5;|
```

![output_mint](/2023/files/latex/output_mint.png "A single line of JavaScript typeset using \mint.")

A comprehensive list of what `minted` is capable of can be found [here](https://tug.ctan.org/macros/latex/contrib/minted/minted.pdf).

<!--
### Using `listings`
-->

## Images

## Tables and lists

## Graphs

# Integrating your work with Git

# Citing and referencing content

# Using LaTeX offline

# Additional reading
