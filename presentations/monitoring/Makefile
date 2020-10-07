TARGET=event-derivatives

LATEX=pdflatex
BIBTEX=bibtex
TEXFILES = ${wildcard *.tex} # ${wildcard tables/*.tex} ${wildcard */*.tex}
BIBFILES = ${wildcard *.bib} # ${wildcard bibtex/*.bib}
# FIGFILES = $(wildcard figs/*.pdf)
# DOTFILES = ${wildcard figs/*.dot}
# FIGFILES +=${DOTFILES:%.dot=%.ps}
# FIGFILES +=${wildcard figs/*.tex} ${wildcard figs/*.ps} ${wildcard figs/*.eps}
# CONFFILES += ${wildcard *.sty} ${wildcard *.cls}
# MDFILES  = ${wildcard *.md}
# TEXFILES +=${MDFILES:%.md=%.tex}

.PRECIOUS: %.ps %.pdf

.PHONY: $(TARGET) $(TARGET).pdf clean clean-full

$(TARGET): $(TARGET).pdf

# %.tex: %.md
# 	pandoc --wrap=preserve -f markdown -t latex $< > $@

$(TARGET).pdf: $(TEXFILES) $(FIGFILES) $(BIBFILES) $(CONFILES)
#	-rm -f *.aux
#	for i in $(TEXFILES); do sed -i.bak 's/`\([^` ]\+\)`/\\t{\1}/g' $$i; done
#	for i in $(TEXFILES); do sed -i.bak 's/^```.\+$$/\\begin{verbatim}/g' $$i; done
#	for i in $(TEXFILES); do sed -i.bak 's/^```$$/\\end{verbatim}/g' $$i; done
	$(LATEX) $(TARGET).tex
ifneq ($(shell grep "cite{.*}" $(TEXFILES)),)
	$(BIBTEX) $(TARGET)
	$(LATEX) $(TARGET).tex
endif
	$(LATEX) $(TARGET).tex
	@/bin/echo ""
	@/bin/echo "++++++++++++++++++++++++++++++++++++++++++++++++++++++++++"
	@/bin/echo "               ++++ ANY UNDEFINED REFERENCES ++++"
	-@grep -i undef $(TARGET).log || echo "No undefined references."
	@/bin/echo "                 ++++ ANY EMPTY REFERENCES ++++"
	-@egrep -i -n -e 'cite{ *}' -e 'ref{ *}' $(TEXFILES) $(FIGFILES) || echo "No empty references."
	@/bin/echo "++++++++++++++++++++++++++++++++++++++++++++++++++++++++++"
	@/bin/echo ""

clean:
	rm -f *.aux *.bbl *.dvi *.lof *.log *.toc *.lot *.blg *.out *.cut *.fdb_latexmk *.fls *.synctex.gz

clean-full: clean
	rm -f $(TARGET).pdf
open: $(TARGET).pdf
	open $(TARGET).pdf

# %.pdf: %.dvi
# 	dvipdfmx -o $@ $<
