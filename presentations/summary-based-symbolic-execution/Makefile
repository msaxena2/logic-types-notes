all : open

solar.pdf : solar.tex blockchain.tex contract.tex solidity_support.tex
	pdflatex $<

open : *.pdf
	open $<

.PHONY : open clean

clean :
	rm -f *.aux *.blg *.out *.bbl *.log *.pdf *.gz *.fdb_latexmk

