main.pdf: pre.tex main.tex
	xelatex main.tex

pre.tex: pre.md
	pandoc -t beamer pre.md --slide-level 3 -o pre.tex