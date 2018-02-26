# kat: Georgian

Original data from http://unimorph.github.io/, annotated according to Sylak-Glassman (2016), https://unimorph.github.io/doc/unimorph-schema.pdf.

## tags

| original tag	| frequency	| comment* |
|---------------|:---------:|:---------|
| 1	 			| 1092 		| should be ARG feature |
| 2	 			| 1140 		| should be ARG feature |
| 3	 			| 1043 		| should be ARG feature |
| ADJ			| 1805 		| |
| COND		 	| 288 		| |
| DAT	 		| 11217 	| |
| ERG	 		| 11217 	| |
| FUT	 		| 576 		| |
| GEN	 		| 11217 	| |
| IMP	 		| 144 		| |
| IND	 		| 1703 		| |
| INST	 		| 7478 		| |
| IPFV	 		| 324 		| |
| LGSPEC1	 	| 7478 		| undocumented |
| LGSPEC2	 	| 18695 	| undocumented |
| N	 			| 69236 	| |
| NOM	 		| 11217 	| |
| OPT	 		| 288 		| |
| PFV	 		| 288 		| |
| PL	 		| 46530 	| |
| PRF	 		| 912 		| |
| PRS	 		| 551 		| |
| PST	 		| 576 		| |
| SBJV	 		| 852 		| |
| SG	 		| 27786 	| |
| V	 			| 3371 		| |
| V.MSDR	 	| 96 		| occurs (incorrectly) as V;V.MSDR |
| VOC	 		| 11217 	| |

	* cf. Sylak-Glassman (2016)
	
Note that these tags *appear* to be UniMorph compliant, but *they are not*, because Georgian verbs express multiple agreement, and this is not reflected here.
The current encoding may be in line with the suggestion of Chiarcos et al. (accepted@LREC-2018) for a more compact and more scalable alternative to the ARG encoding on grounds of a (language-specific) ranking-based of arguments.
Assuming that the (undefined) argument is an absolutive argument of an intransitive verb, this is probably the subject, and with a ranking of grammatical roles such as

	SBJ > OBJ > OTH

and with the following mapping

	ERG -> SBJ
	ABS -> SBJ (if no ERG argument found)
	    -> OBJ (if ERG argument found)
	DAT -> OTH
	
the SBJ argument goes unmarked, the OBJ argument carries a -1 suffix, and the DAT argument carries a -2 suffix.

However, this needs to be double-checked by a language expert.