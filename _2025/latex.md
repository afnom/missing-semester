---
layout: lecture
title: "#7: Introduction to LaTeX"
date: 2025-11-24
ready: true
hide: false
---
# Introduction to LaTeX

# What is LaTeX?
LaTeX is a document preparation system for high-quality typesetting, most often used for scientific documents and publishing.

LaTeX encourages authors to focus on the content of what they're writing rather than how it should appear in the document or how the document should be designed^[1].

In many ways, LaTeX is similar to HTML where the author specifies the document structure and content, and the styling is handled by CSS.

Like some programming languages, we write LaTeX code which exists in a `.tex` file which we then compile using a LaTeX engine to produce a final output (e.g. a PDF).

# When to use LaTeX
LaTeX is a useful tool when you're:
- Writing a research paper, dissertation, presentation or letter.
- Typesetting content with mathematical equations, tables or graphs.
- Writing large documents (thesis or book) which has chapters, citations, cross-references etc.
    
It's **amazing** for maths!

But, it's not great at giving fine control over layouts (unless you *really* want to dive into the TeX code).

# How can I get it?
There are lots of LaTeX compilers and editors available:

- [TeX Live](https://www.tug.org/texlive/)
- [MiKTeX](https://miktex.org/)
- [TeX Maker](https://www.xm1math.net/texmaker/)
- [MacTeX](https://www.tug.org/mactex/)

We'll be focusing on [Overleaf](https://www.overleaf.com) which we have (premium!) access to through the University (log in through your institution's SSO).

# Let's make a document!
## The basics
---
Let's start with a really simple document:
```latex
\documentclass{article}

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

Hello, World!

\end{document}
```
The first part (between `\documentclass{}` and `\begin{document}`) is the preamble which allows us to define the document configuration (and like many programming languages) include any external packages we use in the document.

Everything between `\begin{document}` and `\end{document}` is the content which will be rendered. 

Like in other languages, we can add comments to our code by using the `%` symbol - anything after this symbol on a given line will not be rendered.

## Document classes
---
In the previous code block, we have the `\documentclass` set as `article`, which is the basic class for just getting words on a page. Many document classes have multiple options for the layout as well, e.g. we can make an article in a landscape orientation with the option `landscape`, which can be invoked with `\documentclass[landscape]{article}`. 

Lots of document classes and options are available to suit whatever you want to do with LaTeX (e.g. `beamer` for presentations, `letter` for letters). An exhaustive list can be found on the [Comprehensive TeX Archive Network (CTAN)](https://ctan.org/topic/class).

## Packages
---
The base LaTeX package has a lot of useful functionality, but we can always use more! For example, if we want to add images to our document or extend our access to math symbols, we need to go beyond what the basic LaTeX configuration allows us to do. Enter packages!

In the preamble we can include packages. For example, if we want to include graphics, we can change the document code from before:
```latex
\documentclass{article}

\usepackage{graphicx} % Allows us to include images!

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

Hello, World!

\end{document}
```

Many modern TeX distributions come with a good number of pre-installed packages. If a package isn't installed, you'll need to download it and include it in the building of the LaTeX document. A comprehensive list of packages can be found on the [CTAN package list](https://ctan.org/pkg?lang=en).

## Basic Typesetting
---
We have a very basic document with a title and some "Hello, World!" text, but this isn't too interesting, so let's add some extra content.

```latex
\documentclass{article}

\usepackage{graphicx} % Allows us to include images!

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Introduction}
Welcome to \LaTeX! This is a \textbf{pretty cool} way to \textit{write things}! How is \texttt{programming} \underline{so far}?

\end{document}
```

We basically write the content, and the document class handles the typeface, positioning and font size according to how we define the structure. This automated process is incredibly useful for large documents because we don't need to think about formatting at all!

You may also have noticed some text formatting such as `textbf{}`, `\textit{}`, `\texttt{}` and `\underline{}`, which affect the rendering of the text with bold, italicised, typewriter (monospaced) and underlined text, respectively, with all text within the curly braces being included in the formatting.

## Typesetting Maths
---
LaTeX is AMAZING for typesetting mathematical equations. Nothing else comes close! 
We'll start with an inline equation, which we can express by placing the formula between two dollar signs (`$ ... $`):

```latex
\documentclass{article}

\usepackage{graphicx} % Allows us to include images!

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Introduction}
Welcome to \LaTeX! This is a \textbf{pretty cool} way to \textit{write things}! How is \texttt{programming} \underline{so far}?

\section{Maths}
Here's an inline equation $y = mx + c$. Here's something a bit more complex: $\hat{y} = \sigma (\mathbf{w}^{\top} \mathbf{x} + b)$.

\end{document}
```

Note that we've also started to include special symbols such as `$\sigma$` and `$\top$`. A comprehensive list of special symbols can be found [here](https://tug.ctan.org/info/symbols/comprehensive/symbols-a4.pdf), or, if you want something a little shorter, examples of many common mathematical symbols/commands can be found [here](https://en.wikibooks.org/wiki/LaTeX/Mathematics). We can also see superscripts `a^{b}` where whatever is to the right of the carrot `^` is the superscript attached to the symbol to the left of `^` (note if you just have a single letter/number/symbol, then the curly braces aren't necessary `a^{b} \equiv a^b`). Likewise, subscripts can be invoked as `a_{b}`, and we can have both sub- and super-script `a^{b}_{c}`.

So far, we've dealt with inline equations, but what if we want them to take pride of place in a document ('display mode')? We have multiple ways to achieve this:

```latex
\documentclass{article}

\usepackage{graphicx} % Allows us to include images!

\usepackage{amsmath} % These two allow us to use more advanced maths
\usepackage{amssymb}

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Introduction}
Welcome to \LaTeX! This is a \textbf{pretty cool} way to \textit{write things}! How is \texttt{programming} \underline{so far}?

\section{Maths}
Here's an inline equation $y = mx + c$. Here's something a bit more complex: $\hat{y} = \sigma (\mathbf{w}^{\top} \mathbf{x} + b)$.

This is an important equation:
$$
\sigma(x) = \frac{1}{1+\exp(-x)}
$$

This is another important equation which we may refer to later:
\begin{equation}
    \text{MSE} = \frac{1}{N} \sum_{i=1}^N (y_i - \hat{y}_i)^2 \label{eq:MSE}
\end{equation}

\end{document}
```

We can use `$$ ... $$` to quickly add functions on their own line if we don't really need to reference them in the future. This is not the recommended way to add equations, but it will be discussed later! We can also use the equation environment with `\begin{equation} ... \end{equation}`, which gives us an equation number and the ability to reference it later. Note that if you don't want the equation number, you can use `\begin{equation*} ... \end{equation*}`. To reference the equation, you need to add a label after the equation before the `end` command. The label can be anything, but it's advisable to use a scheme that makes sense to you! In this example, the label is `\label{eq:MSE}` because it's an equation for the Mean Squared Error loss function common in ML.

I won't focus too much on the maths symbols above, but an important one to point out is `\frac{}{}`, which allows you to write a fraction, with the first set of curly braces being the numerator and the second being the denominator. There are other math commands like this, such as `\underset{}{}`, which places the input in the first brace under the second. It's helpful to be aware that the math commands can vary in number of parameters depending on what you're using!

Another **really** useful environment is `align`, which allows us to typeset and align multiple equations!
```latex
Here are some aligned equations:
\begin{align}
    \hat{y} =& w_1x_1 + w_2x_2 + \dots + w_nx_n + b \\
    \hat{y} =& \mathbf{w}^\top \mathbf{x} + b \\
    w_1x_1 + w_2x_2 + \dots + w_nx_n + b =& \mathbf{w}^\top \mathbf{x} + b
\end{align}
```

Here, we have two special characters, `&` and `\\`. The equations are aligned so that the `&` symbols line up when the equation is rendered (they aren't rendered themselves), and `\\` indicates that a line break should be inserted. By default all equation lines will be numbered; however, this can be turned off by using `\begin{align*} ... \end{align*}`, or specific lines can be omitted from numbering by adding `\nonumber` to the start of the line:
```latex
\begin{align}
    \nonumber \hat{y} =& w_1x_1 + w_2x_2 + \dots + w_nx_n + b \\
    \nonumber \hat{y} =& \mathbf{w}^\top \mathbf{x} + b \\
    w_1x_1 + w_2x_2 + \dots + w_nx_n + b =& \mathbf{w}^\top \mathbf{x} + b
\end{align}
```
To reference these equations, we can add a label after each one (before the `\\`), which we can refer to later in the document.

To wrap things up, we'll look at matrices which use a combination of everything we've seen so far. We define matrices as:
```latex
Here's a matrix!

\begin{equation*}
\mathbf{I} = 
    \begin{bmatrix}
		1 & 0 & 0 \\
		0 & 1 & 0 \\
		0 & 0 & 1
	\end{bmatrix}
\end{equation*}
```

It is also worth mentioning that some modern note-taking applications, such as [Obsidian](https://obsidian.md/) and [Notion](https://www.notion.so/), support (a subset of) LaTeX math equations. These equations are usually invoked inline, as above (`$ ... $`), or centred on a new line with `$$ ... $$`. This makes math-based note-taking easy because we have the convenience of markdown with the power of LaTeX math!

## Tables
---
We often want to add data to our documents in the form of tables. Like programming languages in general, you start to pick up on patterns of how things are done, and LaTeX is no different. A lot of what we learned in the last section will be familiar here.

We'll work on this document:
```latex
\documentclass{article}

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Tables}

\end{document}
```

The skeleton of a table is in the code block below:
```latex
\begin{table}[]
    \centering
    \begin{tabular}{c|c}
         &  \\
         & 
    \end{tabular}
    \caption{Caption}
    \label{tab:my_label}
\end{table}
```
Everything here should be familiar. We have a `table` environment we'll be working in, `\centering` centres the table we draw, and then we have a `tabular` environment where the actual data is added. Part of the tabular environment is `{c|c}`, which says that we have two columns divided by a centre line, each aligned to the centre. We could also have `l` or `r` replace `c` to have a left or right alignment, respectively, or a mix between them. If we want more columns, we can add more `l`s, `r`s and `c`s. We can also give the table a caption to explain the contents and a label so we can reference it in the document.

Let's add some data:
```latex
Here's some data from Stack Overflow:
\begin{table}[htbp]
    \centering
    \begin{tabular}{r|l}
         \textbf{Technology} & \textbf{Percentage} \\
         \hline
         Docker     & 58.7 \\
         npm        & 52.3 \\
         pip        & 29.6 \\
         Homebrew   & 24.4 \\
         Kubernetes & 22.0 \\
    \end{tabular}
    \caption{Stack Overflow survey results looking at the percentage of professional developers using a certain tool.}
    \label{tab:so_survey}
\end{table}
```
In the table properties we've added `[htbp]` which are placement options that LaTeX can work with: 
- `h`- Place it here (or as close to here as possible) 
- `t`- If it can't be placed here, place it at the top of a page
- `b`- If it can't be placed there, place it at the bottom of a page
- `p`- If it can't be placed anywhere else, put it on its own page!
If `[htbp]` isn't used, LaTeX defaults to `tbp`. The `float` package also supports `H` which forces the placement to the exact location it is defined in the LaTeX document. But this can break some of the document formatting we use LaTeX for!

This is just scratching the surface on tables! For more information see [here](https://en.wikibooks.org/wiki/LaTeX/Tables).

## Code
---
Most people reading this are probably doing some programming at the moment. So, can we add code to a LaTeX document? Absolutely!

We'll work on this document:
```latex
\documentclass{article}

\usepackage{minted} % Lets us add code!

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Code}

\end{document}
```

We'll focus on `minted`, which is an easy-to-use package to embed code in our documents. 
```latex
Some Java code:
\begin{minted}[linenos]{java}
class MissingSemester {
	public static final String message = "Hello, World!";
}
\end{minted}
```
So we can add code using the `\begin{minted} ... \end{minted}` environment, specifying options such as `[linenos]` to give us line numbers and the syntax highlighting with the `{java}` option.

If you prefer dark mode, vim and python:
```latex
\begin{minted}[style=vim,bgcolor=black]{python}
def reverse_list(x):
    return x[::-1]    
\end{minted}
```

`minted` also lets us add a single line quickly which is really useful!
```latex
Here's a single-line embed
\mint{js}|let cool = 0;| 
That's quick!
```

The full capabilities of `minted` can be found [here](https://tug.ctan.org/macros/latex/contrib/minted/minted.pdf). The list of supported languages in `minted` can be found [here](https://pygments.org/docs/lexers).

`listings` is another package that is available for use for code embedding. It's more customisable than `minted`, but this leads to greater complexity! `listings` documentation is [here](https://mirror.apps.cam.ac.uk/pub/tex-archive/macros/latex/contrib/listings/listings.pdf).

## Tikz
---
`tikz` is a very popular and well-known diagram drawing package for LaTeX. We can use it by including the `tikz` package.

We'll work on the document:
```latex
\documentclass{article}

\usepackage{tikz} % Used to add diagrams

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Tikz}

\end{document}
```

Let's start small with a single node for our graph:
```latex
\begin{tikzpicture} 
    \node[{draw, circle}] (node1) {1}; 
\end{tikzpicture}
```
Once we open the `tikzpicture` environment for drawing, we can specify a node with `\node` with the stroke and shape of the node `(draw, circle)` followed by a unique identifier `(node1)` and the text to render at the node `{1}`. 

We can draw multiple nodes and specify their position by expanding a little on the code above:
```latex
\begin{tikzpicture}[basic_node/.style = {draw, circle}]
    \node[basic_node] (node1) {1}; 
    \node[basic_node] (node2) [below left of = node1] {10}; 
    \node[basic_node] (node3) [below right of = node1] {100}; 
    \node[basic_node] (node4) [below left of = node2] {1000}; 
    \node[{draw}] (node5) [below left of = node3] {10000}; 
\end{tikzpicture}
```
The locations of the nodes are specified by an extra option, which uses the unique identifiers of each node to place them relative to one another. We can also see that `basic_node` now refers to the style of `{draw, circle}`, which helps with consistency in how the nodes are drawn and reduces the amount we need to type!

Then, with another small extension to the code above, we can specify edges between the nodes:
```latex
\begin{tikzpicture}[basic_node/.style = {draw, circle}]
    \node[basic_node] (node1) {1}; 
    \node[basic_node] (node2) [below left of = node1] {10}; 
    \node[basic_node] (node3) [below right of = node1] {100}; 
    \node[basic_node] (node4) [below left of = node2] {1000}; 
    \node[{draw}] (node5) [below left of = node3] {10000};

    \draw[->] (node1) -- (node2);
    \draw[->] (node1) -- (node3);
    \draw[->] (node2) -- (node4);
    \draw[->] (node3) -- (node5);
\end{tikzpicture}
```

## Proof Trees
---
In addition to everything else, you can typeset proof trees! We use the `ebproof` package for this.

With document:
```latex
\documentclass{article}

\usepackage{ebproof} % Used to draw proof trees

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Proof Trees}

\end{document}
```

Trees are typeset in `prooftree` environments and are mainly composed of hypotheses and inferences that are stacked up against each other. An example proof tree is given below:
```latex
\begin{prooftree}
\infer0[$[Id]$]{A \vdash A}
\infer1[$[\neg L]$]{\neg A, A \vdash B}

\infer0[$[Id]$]{B, A \vdash B}

\infer2[$[\vee L]$]{\neg A \vee B, A \vdash B}
\end{prooftree}
```

The proof tree can be broken down into several other subtrees, each of which is typeset separately and then combined to form the full tree. The `\infer<N>` set of commands define inferences that depend on N other inferences/hypotheses already in the stack. For example, `\infer0` does not pop anything from the stack, but `\infer2` pops two previously defined inferences from the stack.

The documentation for the `ebproof` package can be found [here](https://mirror.apps.cam.ac.uk/pub/tex-archive/macros/latex/contrib/ebproof/ebproof.pdf).

## Images
---
All of this text, maths, tables, code, and diagrams is interesting. But sometimes, you just have to add an image to make a document complete. So, let's do just that!

We'll add the `graphicx` package and work on the following document:
```latex
\documentclass{article}

\usepackage{graphicx} % Used to add images

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Images}

\end{document}
```

Here's how we can add an image:
```latex
Here's an image of Bronco the Beagle:
\begin{figure}
    \centering
    \includegraphics[width=0.5\linewidth]{bronco.jpg}
    \caption{Bronco the Beagle}
    \label{fig:bronco}
\end{figure}
```
This should be reminiscent of everything we've seen so far. The only new part is the `\includegraphics` command. We can also pass the option of width, which allows us to control the image size and `\linewidth` which allows us to scale the image relative to the width of the line as specified by the document class. When adding an image, the last thing we need to do is specify the path to the image, which is relative to the directory of the main `.tex` file.

If Bronco (or any other images) don't appear where you would expect them to, then you can use the `[htbp]` placement options again to specify your desired order of placements.

## References and Citations
---
Throughout the session, we've used a lot of `\label` commands to give unique identifiers to document elements. Why is that important? Because we can now reference them! 

Working with the long document:
```latex
\documentclass{article}

\usepackage{graphicx} % Allows us to include images!

\usepackage{amsmath} % These two allow us to use more advanced maths
\usepackage{amssymb}

\usepackage{minted} % Allows us to add code!

\usepackage{tikz} % Used to add diagrams

\usepackage{ebproof} % Used to add proof trees

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Introduction}
Welcome to \LaTeX! This is a \textbf{pretty cool} way to \textit{write things}! How is \texttt{programming} \underline{so far}?

\section{Maths}
Here's an inline equation $y = mx + c$. Here's something a bit more complex: $\hat{y} = \sigma (\mathbf{w}^{\top} \mathbf{x} + b)$.

This is an important equation:
$$
\sigma(x) = \frac{1}{1+\exp(-x)}
$$

This is another important equation which we may refer to later:
\begin{equation}
    \text{MSE} = \frac{1}{N} \sum_{i=1}^N ( y_i - \hat{y}_i)^2 \label{eq:MSE}
\end{equation}

Here are some aligned equations:
\begin{align}
    \hat{y} =& w_1x_1 + w_2x_2 + \dots + w_nx_n + b \label{eq:expanded_nn}\\
    \hat{y} =& \mathbf{w}^\top \mathbf{x} + b \\
    w_1x_1 + w_2x_2 + \dots + w_nx_n + b =& \mathbf{w}^\top \mathbf{x} + b
\end{align}

Here's a matrix!

\begin{equation*}
\mathbf{I} = 
    \begin{bmatrix}
		1 & 0 & 0 \\
		0 & 1 & 0 \\
		0 & 0 & 1
	\end{bmatrix}
\end{equation*}

\section{Tables}
Here's some data from Stack Overflow:
\begin{table}[]
    \centering
    \begin{tabular}{r|l}
         \textbf{Technology} & \textbf{Percentage} \\
         \hline
         Docker     & 58.7 \\
         npm        & 52.3 \\
         pip        & 29.6 \\
         Homebrew   & 24.4 \\
         Kubernetes & 22.0 \\
    \end{tabular}
    \caption{Stack Overflow survey results looking at the percentage of professional developers using a certain tool.}
    \label{tab:so_survey}
\end{table}

\section{Code}
Some Java code:
\begin{minted}[linenos]{java}
class Latex{
    public static final string message = "Hello, World!";
}
\end{minted}

If you prefer dark mode, vim and python:
\begin{minted}[style=vim,bgcolor=black]{python}
def reverse_list(x):
    return x[::-1]    
\end{minted}

Minted also allows us to embed a single line quickly:
\mint{js}|let cool = 0;| 
Which is useful!

\section{Tikz}

\begin{tikzpicture}[basic_node/.style = {draw, circle}]
    \node[basic_node] (node1) {1}; 
    \node[basic_node] (node2) [below left of = node1] {10}; 
    \node[basic_node] (node3) [below right of = node1] {100}; 
    \node[basic_node] (node4) [below left of = node2] {1000}; 
    \node[{draw}] (node5) [below left of = node3] {10000};

    \draw[->] (node1) -- (node2);
    \draw[->] (node1) -- (node3);
    \draw[->] (node2) -- (node4);
    \draw[->] (node3) -- (node5);
\end{tikzpicture}

\section{Proof Trees}
\begin{prooftree}
\infer0[$[Id]$]{A \vdash A}
\infer1[$[\neg L]$]{\neg A, A \vdash B}

\infer0[$[Id]$]{B, A \vdash B}

\infer2[$[\vee L]$]{\neg A \vee B, A \vdash B}
\end{prooftree}

\section{Images}
Here's an image of Bronco the Beagle:
\begin{figure}
    \centering
    \includegraphics[width=0.5\linewidth]{bronco.jpg}
    \caption{Bronco the Beagle}
    \label{fig:bronco}
\end{figure}

\section{References}


\end{document}
```

We can begin to refer to things!
```latex
Hey, remember equation \ref{eq:MSE}? The Mean Squared Error from the start of the document. Or maybe that really interesting table (table \ref{tab:so_survey}) from Stack Overflow? Or the best boy Bronco the beagle (see figure \ref{fig:bronco})?
```

The really great thing about LaTeX is that it keeps track of our references and makes sure each reference is up to date. So you may notice that the citation of the Stack Overflow table includes the text `Table 1`. If we add a table at a later stage which appears before it, then LaTeX will update the `Table 1` text to reflect this change in order, and so will the reference. It's all handled for us! So if you add any new figures/tables/equations etc, you never need to track down that one reference you made ages ago!

LaTeX also makes it very easy to cite other people's work. To do this we add a `.bib` file to the project (at the same level as the `.tex` file) and add bibliography entries there. Most websites which serve research papers should have an option to export a bibtex reference, which should look something like this:
```biblatex
@inproceedings{geometry07shacham,
author = {Shacham, Hovav},
title = {The geometry of innocent flesh on the bone: return-into-libc without function calls (on the x86)},
year = {2007},
isbn = {9781595937032},
publisher = {Association for Computing Machinery},
address = {New York, NY, USA},
url = {https://doi.org/10.1145/1315245.1315313},
doi = {10.1145/1315245.1315313},
abstract = {We present new techniques that allow a return-into-libc attack to be mounted on x86 executables that calls no functions at all. Our attack combines a large number of short instruction sequences to build gadgets that allow arbitrary computation. We show how to discover such instruction sequences by means of static analysis. We make use, in an essential way, of the properties of the x86 instruction set.},
booktitle = {Proceedings of the 14th ACM Conference on Computer and Communications Security},
pages = {552–561},
numpages = {10},
keywords = {turing completeness, return-into-libc, instruction set},
location = {Alexandria, Virginia, USA},
series = {CCS '07}
}
```

We then include the package `biblatex` and use the command `\addbibresource{references.bib}`. Then we can cite the paper:
```latex
Here is the paper that introduced the concept of a return-into-libc attack without using any function calls \cite{geometry07shacham}. This is a fundamental paper in the field of cybersecurity!
```
The bibliography can then be rendered with `\printbibliography`. A minimal example is:
```latex
\documentclass{article}

\usepackage{biblatex}
\addbibresource{references.bib}

\title{Introduction to \LaTeX}
\author{Missing Semester}

\begin{document}
\maketitle

\section{Papers}

Here is the paper that introduced the concept of a return-into-libc attack without using any function calls \cite{geometry07shacham}. This is a fundamental paper in the field of cybersecurity!

\printbibliography

\end{document}
```

Some of this work may already be done for you depending on the document class you're using, so it's always good to check!

It is also worth noting that you can use the `style=` parameter with the `biblatex` package to change the referencing style. Further examples of these can be found in the [biblatex documentation](https://linorg.usp.br/CTAN/macros/latex/contrib/biblatex/doc/biblatex.pdf).

## Misc.
---
Here are some interesting things that didn't fit in the document.
- A lot of things can be wrapped in a figure environment, so if you want to be able to reference your proof tree or add a caption, just wrap it in `\begin{figure} ... \end{figure}`.
- While [Google Scholar](https://scholar.google.com/) is a great way to find papers, the citations can sometimes be incorrect or incomplete, so make sure to double-check your sources!
- If you have any LaTeX, like a large table, complex proof, or busy diagram, you can offload it to another `.tex` file (e.g. `extra_file.tex`) and add it to your main document using `\input{extra_file}`, which will include the LaTeX code as if it was a part of the main document at that position (a bit like C includes).
- If you have a mathematical equation with brackets that aren't large enough, use `\left( ... \right)`.
- You can add bulleted lists in LaTeX using `\begin{itemize} \item ... \item ... \end{itemize}` and numbered lists using `\begin{enumerate} \item ... \item ... \end{enumerate}`

# Beamer
If we want to create a presentation whilst still keeping the power of LaTeX's typesetting, we can use the document class `beamer`.

## The Basics
---
Here is a minimal example showing the basics of `beamer`.

```latex
\documentclass{beamer}
\title{Beamer Demo}
\author{Missing Semester}

\begin{document}

\frame{\titlepage}

\begin{frame}
\frametitle{Introduction}
Welcome to Beamer in \LaTeX! It is useful for creating great looking presentations whilst keeping the powerful typesetting features we've seen previously.
\end{frame}

\end{document}
```

As with an `article`, we start with the `\documentclass` and then the preamble, specifying the title of the presentation and the author. Any imported packages would also go here.

We then begin the document as before, and use `\frame{\titlepage}` to create the title frame of our presentation - think of this as the `beamer` equivalent of `\maketitle` in an article.

We then create a new `frame` using the `\begin{frame}...\end{frame}` environment. Within this, we define the title using `\frametitle{...}` and then enter some text. This gives us a very basic second frame to our presentation.

**Note that we can simply put the frame's title in `{...}` after the `\begin{frame}` commmand to save typing out `\frametitle` every time.**

While it may seem that a `frame` is just the `beamer` term for a slide, they are different - one `frame` can contain more than one slide, as we'll see shortly.

## Themes
---
We can change the style and look of a presentation using the `\usetheme` command in the preamble. An example of this is the Copenhagen theme:

```latex
\documentclass{beamer}
\title{Beamer Demo}
\author{Missing Semester}
\usetheme{Copenhagen}

\begin{document}

\frame{\titlepage}

\begin{frame}
\frametitle{Introduction}
Welcome to Beamer in \LaTeX! It is useful for creating great looking presentations whilst keeping the powerful typesetting features we've seen previously.
\end{frame}

\end{document}
```

For a list of other themes available, see [here](https://latex-beamer.com/tutorials/beamer-themes/). I also like [this site](https://deic.uab.cat/~iblanes/beamer_gallery/) where you can see examples in the browser before pasting any code in to your `.tex` file.

## Table of Contents
---
We can use the `\tableofcontents` command to create a table of contents near the start of the presentation, which can be useful if you have a long presentation with many sections and subsections.

```latex
\begin{frame}{Table of Contents}
\tableofcontents
\end{frame}
```

## Visual Effects
---
We can reveal parts of the frame in turn, displaying as separate slides, in a couple of ways. The simpler way is to use the `\pause` command. The following example is one frame, but displays as multiple slides.

```latex
\begin{frame}{Pausing}
In \LaTeX, we can pause. \pause

This will show different parts of the slide one after the other. \pause

As such!

\end{frame}
```

If we want more control, we can instead use the `< >` syntax:

```latex
\begin{frame}{Controlled pauses}
    Here is some filler text for the demo.

    \begin{itemize}
        \item <2-> Here is the first line.
        \item <3-> And here is the second line!
        \item <4-> Surprise, there's a third too.
    \end{itemize}
\end{frame}
```

In this example, our first slide will only contain the basic text, with subsequent slides revealing each bullet point in turn.

## Highlighting
---
We may sometimes way to highlight or otherwise draw attention to a key point on a given slide, e.g. an important remark or warning. In `beamer`, this can be done with a `block`. There are various types of blocks, a list of which can be found [here](https://latex-beamer.com/tutorials/blocks/). A short example of some of the most common blocks:

```latex
\begin{frame}{Blocks}

We may find a key remark we want to highlight in the slide.

\begin{block}{Remark}
Missing Semester rules!
\end{block}

\begin{alertblock}{Alerting you}
Become familiar with \LaTeX for your own benefit
\end{alertblock}

\begin{examples}
You might (should) use \LaTeX for:
\begin{itemize}
    \item Team project
    \item Final year project
    \item Literature reviews
    \item Any other presentations or submitted work
\end{itemize}
\end{examples}
\end{frame}
```

# Git Integration
---
Since LaTeX is just a collection of files compiled by an external program, it's very easy to back up your work by pushing the directory to git!

Overleaf also has excellent integration with Git. On the top left of an overleaf document, you have the option to sync with Git, GitHub or Dropbox. More information about Git integration can be found [here](https://www.overleaf.com/learn/how-to/Git_integration), with GitHub sync [here](https://www.overleaf.com/learn/how-to/GitHub_Synchronization) and Dropbox [here](https://www.overleaf.com/learn/how-to/Dropbox_Synchronization).

---
^[1]: The LaTeX Project. [An introduction to LaTeX](https://www.latex-project.org/about/) - Accessed: 2024-11-07
