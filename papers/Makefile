PDFs := $(patsubst %.md,%.pdf,$(wildcard *.md))

all : $(PDFs)

%.pdf : %.md
	pandoc -o $@ $<

.PHONY: clean
clean :
	rm -f *.aux *.blg *.out *.bbl *.log *.pdf *.gz *.fdb_latexmk



