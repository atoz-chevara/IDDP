#
# Makefile for DDP documentation generated with xmlto etc
#
# (Repository HOWTO, How software producers can distribute software
# in .deb format, etc)

manual := $(notdir $(CURDIR)).en \
	  $(notdir $(CURDIR)).id

manualdir := $(notdir $(CURDIR))
# this can and will be overriden by a higher level makefile
PUBLISHDIR := /org/www.debian.org/www/doc/manuals

sources := $(foreach manual, $(manual), $(wildcard $(manual).xml))

all: html # pdf dvi ps # god damn dvi pdf and ps don't work

$(notdir $(CURDIR)).en.xml: $(notdir $(CURDIR)).xml
	ln -sf $< $@

publish: all
	test -d $(PUBLISHDIR)/$(manualdir) || install -d -m 755 $(PUBLISHDIR)/$(manualdir)
	dir="$(PUBLISHDIR)/$(manualdir)" && \
	rm -f "$$dir"/*.html "$$dir"/*.pdf "$$dir"/*.ps "$$dir"/*.dvi
	for built in $(manual); do \
	install -p -m 644 $$built.html $(PUBLISHDIR)/$(manualdir) ; \
	done
	# Since dvi and ps do not work remove the following rules
	#install -p -m 644 $$built.pdf $(PUBLISHDIR)/$(manualdir) ; \
	#install -p -m 644 $$built.dvi $(PUBLISHDIR)/$(manualdir) ; 
	#install -p -m 644 $$built.ps $(PUBLISHDIR)/$(manualdir) ; 

validate: $(sources)
	for checking in $(sources); do \
	xmllint --noout --loaddtd --catalogs $$checking ;\
	done

html fo dvi ps pdf: %: $(addsuffix .%,$(manual))


%.html: %.xml
	xsltproc --catalogs -o $@ \
	/usr/share/sgml/docbook/stylesheet/xsl/nwalsh/html/docbook.xsl \
	$<

%.fo:  %.xml
	xsltproc --catalogs -o $@ \
	/usr/share/sgml/docbook/stylesheet/xsl/nwalsh/fo/docbook.xsl \
	$<
	# workaround to avoid -4pc -4pc on top of each page which is currently
	# only partially catched by
	# /usr/share/sgml/docbook/stylesheet/xsl/nwalsh/fo/pagesetup.xsl
	sed 's/margin-left="1in - -4pc"/margin-left="1in"/g' $@ > $@.tmp && mv $@.tmp $@
	rm -f *.fmt

%.dvi:  %.fo
	# needs being run twice to generate toc
	xmltex --interaction=batchmode $<
	xmltex --interaction=batchmode $<

%.ps:  %.dvi
	dvips $<

%.pdf: %.fo
	# needs being run twice to generate toc
	# yes, this is suboptimal but it's the easiest way
	pdfxmltex --interaction=batchmode $<
	pdfxmltex --interaction=batchmode $<

clean distclean:
	for removal in $(manual); do \
	  for suf in aux html dvi fo log pdf ps out toc; do \
	    rm -f $$removal.$$suf; \
	  done \
	done
	rm -f *~ .*~ core tsa* ;
	

.PHONY: all publish clean distclean dvi fo html pdf ps validate
