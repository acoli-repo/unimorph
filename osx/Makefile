###############################################
# build goh Unimorph file from the DDD corpus #
###############################################
# note that this is a generic pipeline for ELAN corpora, but as ELAN is an extensible format, it requires corpus adaptations
# for adapting to another ELAN corpus, you will need 
# - to modify steps (1) and (2) (download and extract corpus),
# - to modify sparql/ddd2tsv.sparql in (4) (extract annotations: ELAN tiers are not standardized, but user-defined), and
# - to modify the Unimorph linking (not in the code nor in SPARQL, but in anno/ddd.owl, anno/ddd-um.ttl)

# workflow
# (0) pre-requisites
# (1) download corpus
# (2) extract relevant source files
# (3) convert to RDF
# (4) extract tsv file with original LEMMA WORD POS FEATS
# (5) link to unimorph

all: osx

################################
# DEFINE YOUR ANNOTATIONS HERE # (i.e., replace anno/ddd.owl or anno/ddd-um.ttl)
################################

# (5) link to unimorph (continue after errors)
osx: src/osx.tsv | ../tools/conll-rdf/run.sh
	-sort -u src/osx.tsv | \
	sed s/'$$'/'\n\n'/g | \
	../tools/conll-rdf/run.sh CoNLLStreamExtractor http://ignore.me/ LEMMA WORD POS FEATS | \
	../tools/conll-rdf/run.sh CoNLLRDFUpdater \
		-custom -model http://purl.org/olia/owl/experimental/unimorph/unimorph.owl http://purl.org/olia/unimorph.owl \
		-custom -model anno/ddd.owl http://purl.org/olia/unimorph.owl \
		-custom -model anno/ddd-um.ttl http://purl.org/olia/unimorph.owl \
		-updates sparql/link-ddd.sparql | \
	../tools/conll-rdf/run.sh CoNLLRDFFormatter -conll LEMMA WORD UM | egrep '^[^#]' | \
	sed -e s/'[()\/|\-]'//g -e s/'\.\t'/'\t'/g | \
	grep -v '_' | sort -u | tee osx

#######################################
# DEFINE YOUR EXTRACTION PATTERN HERE # (i.e., replace sparql/ddd2tsv.sparql)
#######################################
	
# (4) extract tsv file with original LEMMA WORD POS FEATS (continue after errors)
src/osx.tsv: rdf | ../tools/apache-jena-3.6.0
	-for file in rdf/*.ttl; do \
		echo $$file 1>&2; \
		java -cp '../tools/apache-jena-3.6.0/lib/*' arq.sparql --data=$$file --query=sparql/ddd2tsv.sparql --results=TSV | sed s/'"'//g | egrep -v '^\?'; \
	done > src/osx.tsv;

################################
# DEFINE YOUR SOURCE DATA HERE # (i.e., where to find it and what to extract)
################################
	
# (2) extract relevant source files
src/ELAN: ../goh/src/ELAN.zip
	-mkdir src src/ELAN;
	# exclude Latin (group 1), Old High German (group 2), and metadata (group 3), further Old High German (goh) is filtered out later
	unzip -j ../goh/src/ELAN.zip -d src/ELAN -x \
		"*Latein*" \
		"*Isidor*" "*Althochdeutsche*" "*Otfrid*" "*Tatian*" "*Notker*" "*Physiologus*" "*Murbacher*" "*Monsee*" "*Benediktiner*" \
		"*meta*"

# (1) download corpus
../goh/src/ELAN.zip:
	-mkdir ../goh ../goh/src;
	wget -nc http://www.laudatio-repository.org/repository/downloadDatastream/ddd%3Addd-ad/ELAN -O ../goh/src/ELAN.zip

################
# DO NOT TOUCH #
################

# (3) convert to RDF
rdf: src/ELAN | ../tools/elan2llod.xsl
	mkdir rdf;
	for file in src/ELAN/*eaf; do \
		tgt=rdf/`basename $$file | sed s/'[^a-zA-Z0-9][^a-zA-Z0-9]*'/'_'/g`.ttl; \
		if [ !  -e $$file.ttl  ]; then \
			echo $$file '>' $$tgt 1>&2; \
			xsltproc --stringparam baseURI file:$$file ../tools/elan2llod.xsl $$file | \
			perl -pe 'while(m/file:[a-zA-Z0-9\._\\\/\#]*[^a-zA-Z0-9\._\\\/\#\%>]/) { s/(file:[a-zA-Z0-9\._\\\/\#\%]*)[^a-zA-Z0-9\._\\\/\#\%>]+/$$1_/g; }' \
			> $$tgt; \
		fi;  \
 	done;
	# note that we normalize non-ascii URIs
	
# (0) reset to rebuild
clean:
	rm -rf src rdf osx;

# (0) pre-requisites
../tools:
	mkdir ../tools;

../tools/elan2llod.xsl: | ../tools
	wget -c https://raw.githubusercontent.com/acoli-repo/LLODifier/master/elan/elan2llod.xsl -O ../tools/elan2llod.xsl
	
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