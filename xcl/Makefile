################################################################
# build xcl Unimorph file from the armenian data provided at https://github.com/proiel/proiel-treebank #
################################################################

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
	-sort -u armenian_dict.csv | \
	sed s/'$$'/'\n\n'/g | \
	../tools/conll-rdf/run.sh CoNLLStreamExtractor http://ignore.me/ LEMMA WORD POS POS_COARSE FEATS | \
	../tools/conll-rdf/run.sh CoNLLRDFUpdater \
		-custom -model http://purl.org/olia/owl/experimental/unimorph/unimorph.owl http://purl.org/olia/unimorph.owl \
		-custom -model http://svn.code.sf.net/p/olia/code/trunk/owl/experimental/iswoc.owl http://purl.org/olia/unimorph.owl \
		-custom -model anno/iswoc-um.ttl http://purl.org/olia/unimorph.owl \
		-updates sparql/link-iswoc.sparql |\
	../tools/conll-rdf/run.sh CoNLLRDFFormatter -conll LEMMA WORD POS POS_COARSE UM FEATS | egrep '^[^#]' | tee xcl


#######################################
# DEFINE YOUR EXTRACTION PATTERN HERE # (i.e., replace sparql/ddd2tsv.sparql)
#######################################
	
# (4) extract tsv file with original LEMMA WORD POS FEATS (continue after errors)
src/goh.tsv: rdf | ../tools/apache-jena-3.6.0
	-for file in rdf/*.ttl; do \
		echo $$file 1>&2; \
		java -cp '../tools/apache-jena-3.6.0/lib/*' arq.sparql --data=$$file --query=sparql/ddd2tsv.sparql --results=TSV | sed s/'"'//g | egrep -v '^\?'; \
	done > src/goh.tsv;

################################
# DEFINE YOUR SOURCE DATA HERE # (i.e., where to find it and what to extract)
################################
	
# (2) extract relevant source files
src/ELAN: src/ELAN.zip
	# exclude Latin (group 1), Old Saxon (group 2) or heavily Latin-OHG mixed content (group 3) and metadata (group 4)
	unzip -u src/ELAN.zip -d src -x \
		"*Latein*" \
		"*Genesis*" "*Heliand*" "*Kleinere_Alts*" \
		"*DDD-AD-Otfrid_1.0/O_Otfr.Liutb.eaf" "*DDD-AD-Z-Notker-Martianus_Capella*" "*DDD-AD-Z-Notker-Psalmen*" "*DDD-AD-Z-Notker_Boethius-Categoriae*" "*DDD-AD-Z-Notker_Boethius-De_Consolatione*" "*DDD-AD-Z-Notker_Boethius-De_Interpretatione*" "*DDD-AD-Z-Notker_Kleinere-Ars_Rhetorica*" "*DDD-AD-Z-Notker_Kleinere-De_Partibus_logice*" "*DDD-AD-Z-Notker_Kleinere-Syllogismus*" \
		"*meta*"

# (1) download corpus
src/ELAN.zip:
	mkdir src;
	wget -c http://www.laudatio-repository.org/repository/downloadDatastream/ddd%3Addd-ad/ELAN -O src/ELAN.zip

################
# DO NOT TOUCH #
################

# (3) convert to RDF
rdf: src/ELAN | ../tools/elan2llod.xsl
	mkdir rdf;
	for file in src/ELAN/*/*eaf; do \
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
	rm -rf src rdf xcl;

# (0) pre-requisites
../tools:
	mkdir ../tools;

../tools/elan2llod.xsl: | ../tools
	wget -c https://raw.githubusercontent.com/acoli-repo/LLODifier/master/elan/elan2llod.xsl -O ../tools/elan2llod.xsl
	
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
