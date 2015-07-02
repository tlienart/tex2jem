# tex2jem

## Warning

This project is still under infrequent development but can be considered reasonably stable.

## What this is about

This is an hacky extension of Jemdoc+Mathjax.
* **Jemdoc**, written by Jacob Mattingley ([link](http://jemdoc.jaboc.net)), a light markup format inspired from AsciiDoc,
* **Jemdoc+Mathjax**, extension written by Wonseok Shin ([link](http://web.stanford.edu/~wsshin/jemdoc+mathjax.html)), an extension which among other things uses MathJax for a much improved math rendering.

The aim is to go from a reasonably simple `.tex` file to a corresponding `.html` with as little added work from the user as possible (hopefully *none*). I use this quite a lot to write notes on ([my website](http://www.stats.ox.ac.uk/~lienart)).

## What you need

- A working version of `Python 2.x` (Tested on `2.7.6`)

## Setup
### Quick example

After downloading the repo, run the following commands in the folder:

```
./compile -stex example
```

This will look for the file `example.tex` in the folder `_source/`, will translate it to an equivalent `example.jemdoc` file. That file will then be compiled with **Jemdoc** (+**Mathjax**), the resulting `example.html` file (and images that are in the `_source/_figs` folder) are then copied to `_html/`.

The file `example.tex` aims at demonstrating the kind of acceptable LaTeX document that can be inputted (see further for more details). Modify it and try it out. The overall compilation process should take less than a second for any  standard `.tex` of reasonable size.

### General workflow

Just put your `.tex` files in the `_source/` folder with corresponding images in `_source/_figs` (making sure you're calling the figures via `\includegraphics{_figs/myfig.png}`). Then, you might want to:

* compile a single `.tex` file (whence the flag `-stex`) file and see how it looks like for which, as above you'd do:
```
./compile -stex filename
```
* compile all `.tex` files in `_source/` for which you'd do:
```
./compile -tex
```
* just recompile the `.html` files (eg. when you just want to update menus, figures, ...)
```
./compile
```
The script `compile` is pretty basic. The single file example amounts to:
```
./t2j _source/example.tex
./jemdoc -c jemdoc.conf _source/example.jemdoc
cp -r _source/_figs _html/
mv _source/example.html _html/
open _html/example.html
```

## What works and what doesn't
### Assumptions
First, I'm assuming your code compiles in LaTeX and I'm assuming you don't rely on packages too much apart from the way your file looks (have a look at `t2jPreamble.tex` which loads a number of things to make the `.pdf` file and the `.html` file look similar).

Then there are a few things that have been voluntarily ignored like:
* any crazy LaTeX is likely to not be processed accurately, parsing LaTeX is not easy and this code merely parses a tiny subset of well-defined LaTeX,
* `def` is ignored, use `newcommand`,
* `newcommand` is used with all brackets ie.: `\newcommand{\R}{\mathbb R}` is valid, `\newcommand\R\mathbb R` is not,
* valid form for commands is `\command{stuff}`, `{\command stuff}` will not work,
* page specific environments like `minipage`, `flushleft`, `flushright` are not dealt with, same for modification in font or fontsize (but you can do that with raw HTML).

### Partial list of things that will not work + bypass

Will not work | Comment / way around it
------------ | -------------
`newcommand` with more than 1 arg | *might be added*
font modification | use raw HTML `{{raw html}}` (1)
nested itemize/enumerate | **will be added**
textcolor cannot be nested (2) | use raw HTML, *might be added*
hyperref cannot be nested | use raw HTML, *might be added*

(1) for raw HTML insertions, you might want to escape the line from LaTeX but still have Jemdoc looking at it, this is how to do it:
```
%jem: {{<font face="verdana", color="Red">Some text here</font>}}
and continue here
```

(2) an example of nesting is
```
\textbf{blah \emph{this is nested} blah}
```
`hyperref` and `textcolor` can't be because these commands are themselves inserted as raw HTML.

## How this code is built

If you have a look at `t2j`, you'll see some (messy) python and a lot of Regex, I have good friends from CompSci who've essentially told me that this was evil, that it was much cleaner to define a grammar etc. etc. Yeah yeah, well my aim was not to revolutionize the world, I just wanted a quick hack which I ended up using a lot and thought could be helpful to others.

If you're into nice code translators written by people who actually know what they're doing, I would recommend having a look at the very impressive **Pandoc** written by John Macfarlane ([link](http://johnmacfarlane.net/pandoc/)) in Haskell (!).
