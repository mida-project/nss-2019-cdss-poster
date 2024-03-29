#### makefile, 2018/10/07
#### Copyright (C) 2018 Luiz E. M. Lima (luizeduardomlima@gmail.com)
##
## This makefile provides commands to make compiling LaTeX files.

## Source and base name
src :=	utfprpgtex-poster.tex
ifeq ($(strip $(src)),)
  src :=	$(shell read -p "Enter source filename with 3-digits extension: " src; echo $$src)
endif
base_name =	$(basename $(src))

## Command lines: latex, pdflatex, dvips, and ps2pdf
cmd_latex    =	latex -interaction=errorstopmode ${base_name}.tex
cmd_pdflatex =	pdflatex -interaction=errorstopmode ${base_name}.tex
cmd_dvips    =	dvips -P pdf ${base_name}.dvi -o ${base_name}.ps
cmd_ps2pdf   =	ps2pdf ${base_name}.ps ${base_name}.pdf

## bibtex or biber command line
prg_bib :=	biber
ifeq ($(strip $(prg_bib)),)
  cmd_bib =	@echo "Warning: no biber or bibtex execution."
  ifneq "$(findstring biber, $(MAKECMDGOALS))" ""
    prg_bib =	biber
    cmd_bib =	${prg_bib} ${base_name}
    biber:
			@echo "Info: pdf done using biber."
  endif
  ifneq "$(findstring bibtex, $(MAKECMDGOALS))" ""
    prg_bib =	bibtex
    cmd_bib =	${prg_bib} ${base_name}
    bibtex:
			@echo "Info: pdf done using bibtex."
  endif
else
  cmd_bib =	${prg_bib} ${base_name}
endif

## makeindex command line
prg_mkidx :=
ifeq ($(strip $(prg_mkidx)),)
  cmd_mkidx =	@echo "Warning: no makeindex execution."
  ifneq "$(findstring mkidx, $(MAKECMDGOALS))" ""
    prg_mkidx =	makeindex
    cmd_mkidx =	${prg_mkidx} ${base_name}.idx
    mkidx:
			@echo "Info: pdf done using makeindex."
  endif
else
  cmd_mkidx =	${prg_mkidx} ${base_name}.idx
endif

## Creates dvi file
dvi:
	${cmd_latex}
	${cmd_bib}
	${cmd_mkidx}
	${cmd_latex}
	${cmd_latex}

## Creates ps file
ps:	dvi
	${cmd_dvips}

## Creates pdf file using latex or pdflatex
pdf1:	ps
	${cmd_ps2pdf}
pdf2:
	${cmd_pdflatex}
	${cmd_bib}
	${cmd_mkidx}
	${cmd_pdflatex}
	${cmd_pdflatex}

## Removes intermediate files
inter_files=	*.aux *.log *.bbl *.bcf *.blg *.brf *.mw *.out *.run.xml \
	*.acn* *.acr* *.alg* *.glg *.glo *.gls *.idx *.ilg *.ind *.ist *.nlo *.nls \
	*.loa *.loc *.lod *.lof *.loh *.lop *.lot *.tdo *.toc \
	*.nav *.snm
clean:
	rm -f ${inter_files}
	rm -f ${base_name}.dvi
	rm -f ${base_name}*.ps
	rm -f `find . -name "*.aux"`
	rm -f `find . -name "*.log"`

## Creates pdf file using latex or pdflatex and removes intermediate files
all1:	pdf1 clean
all2:	pdf2 clean

## Shows help
help:
	@echo "##############################################################################"
	@echo "# Commands to make compiling LaTeX source files"
	@echo "##############################################################################"
	@echo "# make dvi:       creates dvi, converts tex-dvi."
	@echo "# make ps:        creates ps, converts tex-dvi-ps."
	@echo "# make pdf1:      creates pdf using latex, converts tex-dvi-ps-pdf."
	@echo "# make pdf2:      creates pdf using pdflatex, converts tex-pdf."
	@echo "# make clean:     removes intermediate files."
	@echo "# make all1:      creates pdf using latex and removes intermediate files."
	@echo "# make all2:      creates pdf using pdflatex and removes intermediate files."
	@echo "# make 1 biber:   uses biber to generate references with option 1."
	@echo "# make 1 bibtex:  uses bibtex to generate references with option 1."
	@echo "# make 1 2 mkidx: uses makeindex to generate index with options 1 and 2."
	@echo "# make help:      shows help."
	@echo "# option 1:       dvi, ps, pdf1, pdf2, all1, or all2."
	@echo "# option 2:       biber or bibtex."
	@echo "##############################################################################"
