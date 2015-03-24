# tex2jem

## What this is about

This is an hacky extension of Jemdoc+Mathjax.
* **Jemdoc**, written by Jacob Mattingley ([link](http://jemdoc.jaboc.net)), a light markup format inspired from AsciiDoc,
* **Jemdoc+Mathjax**, extension written by Wonseok Shin ([link](http://web.stanford.edu/~wsshin/jemdoc+mathjax.html)), an extension which among other things uses MathJax for a much improved math rendering.

The aim is to go from a reasonably simple `.tex` file to a corresponding `.html` with as little added work from the user as possible (hopefully *none*). I use this quite a lot to write notes on ([my website](http://www.stats.ox.ac.uk/~lienart)).

## Quick setup

After downloading the repo, run the following commands:

```
./compile -stex example
```

This will look for the file `example.tex` in the folder `_source/`, will translate it to an equivalent `example.jemdoc` file. That file will then be compiled with **Jemdoc** (+**Mathjax**), the resulting `example.html` file (and images that are in the `_source/_figs` folder) are then copied to `_html/`.

The file `example.tex` aims at demonstrating the kind of acceptable LaTeX document that can be inputted (see further for more details). Modify it and try it out. The overall compilation process should take less than a second for any  standard `.tex` of reasonable size.

In general, you might want to:

1. compile a single `.tex` file (see above)
2. compile all `.tex` files in `_source/` for which you'd do:
```
./compile -tex
```
3. just recompile the `.html` files (eg. when you just want to update menus etc.)
```
./compile
```

You might also like to do it all manually or see the process. The example case could be written:
```
./t2j _source/example.tex
./jemdoc -c jemdoc.conf _source/example.jemdoc
cp -r _source/_figs _html/
mv _source/example.html _html/
open _html/example.html
```
