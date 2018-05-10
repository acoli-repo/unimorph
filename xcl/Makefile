########################################################################################################
# build xcl Unimorph file from the Armenian data provided at https://github.com/proiel/proiel-treebank #
########################################################################################################

# workflow
# (0) pre-requisites
# (1) download corpus (skipped)
# (2) extract relevant source files (skipped)
# (3) convert to RDF (skipped)
# (4) extract tsv file with original LEMMA WORD POS FEATS (skipped)
# (5) link to unimorph

all: xcl

################################
# DEFINE YOUR ANNOTATIONS HERE # (i.e., replace anno/ddd.owl or anno/ddd-um.ttl)
################################

# (5) link to unimorph (continue after errors)
xcl: ../tools/conll-rdf/run.sh
	-sort -u src/armenian_dict.csv | \
	sed -e s/'$$'/'\n\n'/g -e s/';'/'\t'/g | \
	../tools/conll-rdf/run.sh CoNLLStreamExtractor http://ignore.me/ IGN IGN LEMMA WORD POS POS_COARSE FEATS | \
	../tools/conll-rdf/run.sh CoNLLRDFUpdater \
		-custom -model http://purl.org/olia/owl/experimental/unimorph/unimorph.owl http://purl.org/olia/unimorph.owl \
		-custom -model http://purl.org/olia/owl/experimental/iswoc.owl http://purl.org/olia/unimorph.owl \
		-custom -model anno/iswoc-um.ttl http://purl.org/olia/unimorph.owl \
		-updates sparql/link-iswoc.sparql |\
	../tools/conll-rdf/run.sh CoNLLRDFFormatter -conll LEMMA WORD UM | \
	egrep '^[^#]' | grep -v '_$$' | tee xcl

# same as xcl, but no file created
debug: ../tools/conll-rdf/run.sh
	-sort -u src/armenian_dict.csv | \
	sed -e s/'$$'/'\n\n'/g -e s/';'/'\t'/g | \
	../tools/conll-rdf/run.sh CoNLLStreamExtractor http://ignore.me/ IGN IGN LEMMA WORD POS POS_COARSE FEATS | \
	../tools/conll-rdf/run.sh CoNLLRDFUpdater \
		-custom -model http://purl.org/olia/owl/experimental/unimorph/unimorph.owl http://purl.org/olia/unimorph.owl \
		-custom -model http://purl.org/olia/owl/experimental/iswoc.owl http://purl.org/olia/unimorph.owl \
		-custom -model anno/iswoc-um.ttl http://purl.org/olia/unimorph.owl \
		-updates sparql/link-iswoc.sparql |\
	../tools/conll-rdf/run.sh CoNLLRDFFormatter -conll LEMMA WORD POS POS_COARSE UM FEATS | \
	egrep '^[^#]' | grep -v '_$$'

################
# DO NOT TOUCH #
################

# (0) reset to rebuild
clean:
	rm -rf xcl;

# (0) pre-requisites
../tools:
	mkdir ../tools;

../tools/conll-rdf/run.sh: | ../tools
	mkdir ../tools/conll-rdf
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
