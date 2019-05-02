all : fm-seminar-talk.pdf

fm-seminar-talk.pdf : fm-seminar-talk.tex train-ha.tex
	pdflatex fm-seminar-talk.tex
#	bibtex fm-seminar-talk.tex
#	pdflatex fm-seminar-talk.tex
#	pdflatex fm-seminar-talk.tex

.PHONY : clean

clean :
	rm -f *.aux *.blg *.out *.bbl *.log *.pdf *.gz *.fdb_latexmk

