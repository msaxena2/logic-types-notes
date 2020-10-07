all : open

%.pdf : %.tex
	pdflatex $<

open : *.pdf
	open $<

.PHONY : open clean

clean :
	rm -f *.aux *.blg *.out *.bbl *.log *.pdf *.gz *.fdb_latexmk

