###############################################
# build gmh Unimorph file from the ReM corpus #
###############################################

# note that ReM uses an ad hoc XML format (CorA-XML), it builds on generic XML processing components

# workflow
# (0) pre-requisites
# (1) download corpus
# (2) extract relevant source files
# (3) convert to RDF
# (4) extract tsv file with original LEMMA WORD POS FEATS
# (5) link to unimorph

all: gmh

################################
# DEFINE YOUR ANNOTATIONS HERE # (i.e., replace anno/ddd.owl or anno/ddd-um.ttl)
################################

# (3) link to unimorph (continue after errors; note that we label *both* INFL and INFLCLASS as FEATS, so that inherent case of nouns is treated like INFL annotation )
gmh: src/gmh.tsv | ../tools/conll-rdf/run.sh
	sed -e s/'$$'/'\n\n'/g src/gmh.tsv | # one word per "sentence" \
	../tools/conll-rdf/run.sh CoNLLStreamExtractor http://ignore.me/ LEMMA WORD POS FEATS FEATS | \
	../tools/conll-rdf/run.sh CoNLLRDFUpdater \
		-custom -model http://purl.org/olia/owl/experimental/unimorph/unimorph.owl http://purl.org/olia/unimorph.owl \
		-custom -model anno/rem.owl http://purl.org/olia/unimorph.owl \
		-custom -model anno/rem-um.ttl http://purl.org/olia/unimorph.owl \
		-updates sparql/link-rem.sparql | \
	../tools/conll-rdf/run.sh CoNLLRDFFormatter -conll LEMMA WORD UM | \
	egrep '^[^#]' | \
	sed -e s/'\&amp;'/'\&'/g -e s/'\&[lg]t;'//g \
		-e s/'\\[^\t]*'//g -e s/'\+[^\t]*'//g -e s/'\.\t'/'\t'/g -e s/'[?!0-9]'//g -e s/'\.\t'/'\t'/g -e s/'\['//g -e s/'\]'//g \
		-e s/'\t\t'/'\t_\t'/g | \
	grep -v '_' | \
	sed -e s/'\t;*'/'\t'/g -e s/';*$$'//g -e s/';;*'/';'/g | \
	uniq | tee gmh
	
# (2) extract tsv file with original LEMMA* WORD POS INFL INFLCLASS
# 		* LEMMA is simplified to the longest possible version
src/gmh.tsv: src/REM.zip
	unzip -c src/*.zip | \
	perl -pe "\
		s/[ \t\n\r]+/ /gs; \
		s/</\n</g; \
		s/>/>\n/g; \
	" | \
	egrep '<token |<norm |<lemma |<pos |<infl |<inflClass |<lemma_idmwb ' | \
	perl -pe "\
		s/[ \n]+/ /gs;\
		s/<token[^>]*>/\n/g;\
	" | \
	egrep '<lemma_idmwb tag="[^-]' | 		# => ibmwb, i.e., no Latin, no punctuation \
	perl -pe "\
		while(m/<lemma tag=\"[^\"]*[\(\)\-]/) { \
			s/(<lemma tag=\"[^\"]*)[\(\)\-]/\1/g; \
		}; \
	" | \
	perl -pe "\
		s/.*<norm tag=\"([^\"]*)\".*<lemma tag=\"([^\"]*)\".*<pos tag=\"([^\"]*)\".*<infl tag=\"([^\"]*)\".*<inflClass tag=\"([^\"]*)\".*$$/\2\t\1\t\3\t\4\t\5/g; \
	" | \
	grep -v "/" | 																		# no particle verbs \
	egrep '^[aAàâÂæbBcCdDeEéÉèÈêëfFgGhHiIîjJkKlLmMnNoOôÔöpPqrRsSſtTuUûüvVwWxyzZ]' | 	# valid lemmas only \
	sort -u > src/gmh.tsv;

# (1) download corpus
src/REM.zip:
	$(error Please download the current version of the ReM corpus from https://www.linguistics.ruhr-uni-bochum.de/rem/, move it to src/rem.zip)
	# no automated download supported (but file included here)

# (0) reset to rebuild
clean:
	rm -rf gmh src/gmh.tsv;

# (0) pre-requisites
../tools:
	mkdir ../tools;

../tools/conll-rdf/run.sh: | ../tools
	mkdir ../tools/conll-rdf >&/dev/null;
	wget -c https://github.com/acoli-repo/conll-rdf/archive/master.zip -O ../tools/conll-rdf/master.zip
	unzip ../tools/conll-rdf/master.zip -d ../tools/conll-rdf/
	mv ../tools/conll-rdf/conll-rdf-master/* ../tools/conll-rdf/
	rm -rf ../tools/conll-rdf/conll-rdf-master
	rm ../tools/conll-rdf/master.zip
	chmod u+x ../tools/conll-rdf/*.sh
	../tools/conll-rdf/compile.sh
	
../tools/apache-jena-3.6.0: | ../tools
	wget -c http://ftp.fau.de/apache/jena/binaries/apache-jena-3.6.0.zip -O ../tools/apache-jena.zip;
	unzip ../tools/apache-jena.zip -d ../tools/