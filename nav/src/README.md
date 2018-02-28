# nav: Navajo

Original data from http://unimorph.github.io/, annotated according to Sylak-Glassman (2016), https://unimorph.github.io/doc/unimorph-schema.pdf.

## tags

| original tag	| frequency	| comment* |
|---------------|:---------:|:---------|
| 3161	 		| 1	 		| 	 |
| 3165	 		| 2	 		| 	 |
| 2111	 		| 3	 		| 	 |
| 2107	 		| 4	 		| 	 |
| 181	 		| ARGAC3S	| applies to nouns only	 |
| 4239	 		| DU	 	| 	 |
| 3602	 		| IPFV	 	| 	 |
| 100	 		| ITER	 	| 	 |
| 1448	 		| N	 		| 	 |
| 100	 		| OPT	 	| 	 |
| 4854	 		| PFV	 	| 	 |
| 8430	 		| PL	 	| 	 |
| 4854	 		| PRF	 	| 	 |
| 3602	 		| PROG	 	| 	 |
| 1888	 		| PROSP	 	| 	 |
| 181	 		| PSS0	 	| 	 |
| 181	 		| PSS1D	 	| 	 |
| 181	 		| PSS1P	 	| 	 |
| 181	 		| PSS1S	 	| 	 |
| 181	 		| PSS2D	 	| 	 |
| 181	 		| PSS2P	 	| 	 |
| 181	 		| PSS2S	 	| 	 |
| 181	 		| PSS3	 	| 	 |
| 362	 		| PSS4	 	| 	 |
| 10444	 		| REAL	 	| 	 |
| 4235	 		| SG	 	| 	 |
| 10544	 		| V	 		| 	 |

	* cf. Sylak-Glassman (2016)

In general, the encoding seems to follow Sylak-Glassman (2016), but the ARG feature is applied only to nouns. This requires an explanation. 

## Analyzing a sample

According to Sylak-Glassman (2016), ARGAC3S corresponds to a third person singular accusative argument in verbal inflection, not clear how this relates to nominal inflection.

	awooʼ   hawooʼ  N;PSS4
	awooʼ   yiwooʼ  N;PSS4;ARGAC3S
	awos    hawos   N;PSS4
	awos    yiwos   N;PSS4;ARGAC3S

According to http://www.lapahie.com/Dine_Bizaad.cfm, these may be *’awoo’* (tooth) and *’awos* (shoulder). According to https://en.wikipedia.org/wiki/Navajo_grammar, PSS4 is expressed by the *yi-* prefix for 3o (ARGAC3s) and by *ha-* for 3s (unmarked). Obviously, ARGAC3SG provides additional information about the argument *of the possessor*. Note that this is a violation of Sylak-Glassman (2016, p.47) who, instead, suggests to expand the PSS template. A more UniMorph-compliant representation would thus be

	awooʼ   hawooʼ  N;PSS4(NO)
	awooʼ   yiwooʼ  N;PSS4SAC
	awos    hawos   N;PSS4(NO)
	awos    yiwos   N;PSS4SAC

However, neither the fourth person nor the case extension are currently foreseen in the Unimorph schema.

Transformation to this alternative encoding is implemented in ../Makefile