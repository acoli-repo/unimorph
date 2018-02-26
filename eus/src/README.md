# eus

Original data from http://unimorph.github.io/, annotated according to Sylak-Glassman (2016), https://unimorph.github.io/doc/unimorph-schema.pdf. Verbs, only

## tags

| original tag	| frequency	| correct*	| comment |
|---------------|:---------:|:---------:|:--------|
| ARGABS1		| 1758		| ARGAB1?	| number missing, must be after person |
| ARGABS2		| 1822		| ARGAB2?	| number missing, must be after person |
| ARGABS3		| 8764		| ARGAB3?	| number missing, must be after person |
| ARGABSINFM	| 456		| ARGAB2S	| INFM tag not standardized |
| ARGABSPL		| 5488		| ARGAB?P	| person missing, must be before number |
| ARGABSSG		| 6856		| ARGAB?S	| person missing, must be before number |
| ARGERG1		| 2367		| ARGER1?	| person missing, must be before number |
| ARGERG2		| 2458		| ARGER2?	| person missing, must be before number |
| ARGERG3		| 3817		| ARGER3?	| person missing, must be before number |
| ARGERGFEM		| 319		| ARGER??	| person, number missing; gender not standardized |
| ARGERGINFM	| 1038		| ARGER2S	| INFM tag not standardized |
| ARGERGMASC	| 318		| ARGER??	| person, number missing;  gender not standardized |
| ARGERGPL		| 3810		| ARGER?P	| person missing |
| ARGERGSG		| 4832		| ARGER?S	| person missing |
| ARGIO1		| 2287		| ARGDA1?	| number missing |
| ARGIO2		| 2434		| ARGDA2?	| number missing |
| ARGIO3		| 3410		| ARGDA3?	| number missing |
| ARGIOFEM		| 605		| ARGDA??	| person, number missing;  gender not standardized |
| ARGIOINFM		| 1211		| ARGDA2S	| INFM tag not standardized |
| ARGIOMASC		| 606		| ARGDA??	| person, number missing; gender not standardized |
| ARGIOPL		| 3451		| ARGDA?P	| person missing |
| ARGIOSG		| 4680		| ARGDA?S	| number missing |
| FEM			| 1342		| FEM		|  |
| HYP			| 3341		| ?			| not found |
| IMP			| 830		| IMP		|  |
| IND			| 6239		| IND		|  |
| INFM			| 2694		| INFM		|  |
| MASC			| 1352		| MASC		|  |
| PAST			| 4213		| PST		|  |
| POT			| 4820		| POT		|  |
| PRES			| 3505		| PRS		|  |
| V				| 11889		| V			|  |
	* "correct", according to Sylak-Glassman (2016)

Note that these tags are partially ill-formed, and that *all* ARG tags violate the specifications by Sylak-Glassman (2016, p.13). In particular, the original dataset uses a different mechanism to express agreement information than recommended, i.e.,

	ARGABS1;ARGABSPL			

instead of 			

	ARGAB1P			

## tag conversion

As suggested by Chiarcos et al. (accepted@LREC-2018), a more compact and more scalable alternative to the ARG encoding is a ranking-based encoding.
For Basque, a ranking 

	ABS > DAT > ERG

could be motivated on grounds of the typical order of morphemes (cf. https://www.ehu.eus/documents/2430735/0/A-brief-grammar-of-euskara.pdf), 
but more transparent would be a ranking on grounds of grammatical roles, i.e.,

	SBJ > OBJ > OTH

Here, we assume the latter, with the following mapping

	ERG -> SBJ
	ABS -> SBJ (if no ERG argument found)
	    -> OBJ (if ERG argument found)
	DAT -> OTH
	
Accordingly, the SBJ argument goes unmarked, the OBJ argument carries a -1 suffix, and the DAT argument carries a -2 suffix.

This is implemented in ../Makefile