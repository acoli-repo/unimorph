
studying the structure of DDD ELAN RDF files.

    PREFIX elan: <http://www.mpi.nl/tools/elan/EAFv2.7.xsd#>
    PREFIX ag: <http://www.ldc.upenn.edu/atlas/ag/>
    
    SELECT ?l ?annos
    WHERE {
    	?lemma a elan:lemma; elan:ANNOTATION_VALUE ?l.
    	{ SELECT ?lemma (GROUP_CONCAT(DISTINCT ?anno; separator=" ") AS ?annos)
    	  WHERE {
    		?lemma a elan:lemma.
    		?lemma ag:start/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:start ?x.
            ?lemma ag:end/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:end ?x.
    		?x ag:type ?att.
    		?x elan:ANNOTATION_VALUE ?val.
    		BIND(concat(str(?att),'=',?val) AS ?anno)
    	} GROUP BY ?lemma ORDER BY ?lemma ?l ?att ?val
    	}
    } 	  LIMIT 5

applied to `MF_1_M.I.eaf.ttl` yields

    sagen	text=sagetun edition=sagetun lemma=sagen lang=goh translation=sagen, sprechen comp=sagētun posLemma=VV pos=VVFIN inflectionClassLemma=WK3 inflectionClass=WK3 inflection=IND_PAST_PL_3
    al	text=al edition=al lemma=al lang=goh translation=all comp=al posLemma=DI pos=DIS inflectionClassLemma=A,O inflectionClass=A,O inflection=MASC_PL_NOM_0
    joh	text=ioh edition=ioh lemma=joh lang=goh translation=und comp=joh posLemma=KO pos=KON
    fona	text=fona edition=fona lemma=fona lang=goh translation=von comp=fona posLemma=AP pos=APPR
    der	text=dhem edition=dhem lemma=der lang=goh translation=der, die, das comp=dëmu posLemma=DD pos=DDA inflection=MASC_SG_DAT

so, we extract Old High German (`lang=goh`) words (`edition`) with their `lemma`, `pos` and `inflection`:

    PREFIX elan: <http://www.mpi.nl/tools/elan/EAFv2.7.xsd#>
    PREFIX ag: <http://www.ldc.upenn.edu/atlas/ag/>
    
    SELECT DISTINCT ?l ?w ?p ?i
    WHERE {
    	?lemma a elan:lemma; elan:ANNOTATION_VALUE ?l.
		
		?lemma ag:start/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:start ?lang.
		?lang ag:type "lang"; elan:ANNOTATION_VALUE "goh".
		
		?lemma ag:start/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:start ?word.
        ?lemma ag:end/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:end ?word.
		?word ag:type "edition"; elan:ANNOTATION_VALUE ?wordForm.
        BIND(lcase(?wordForm) AS ?w)
		
		?lemma ag:start/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:start ?pos.
        ?lemma ag:end/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:end ?pos.
		?pos ag:type "pos"; elan:ANNOTATION_VALUE ?p.

		OPTIONAL {
			?lemma ag:start/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:start ?inflection.
			?lemma ag:end/elan:TIME_VALUE/^elan:TIME_VALUE/^ag:end ?inflection.
			?inflection ag:type "inflection"; elan:ANNOTATION_VALUE ?i.
		}
	} ORDER BY ?l ?p ?w ?i
