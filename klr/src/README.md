# klr: Khaling

Original data from http://unimorph.github.io/, annotated according to Sylak-Glassman (2016), https://unimorph.github.io/doc/unimorph-schema.pdf.

## tags

| original tag	| frequency	| comment* |
|---------------|:---------:|:---------|
| 46055	 		| 1			| 	 |
| 48456	 		| 2	 		| 	 |
| 60953	 		| 3	 		| 	 |
| 47254	 		| ARG1	 	| violates Sylak-Glassman (2016)	 |
| 31284	 		| ARG2	 	| violates Sylak-Glassman (2016)	 |
| 64764	 		| ARG3	 	| violates Sylak-Glassman (2016)	 |
| 50260	 		| ARGDU	 	| violates Sylak-Glassman (2016)	 |
| 26016	 		| ARGEXCL	| violates Sylak-Glassman (2016)	 |
| 10536	 		| ARGINCL	| violates Sylak-Glassman (2016)	 |
| 50260	 		| ARGPL	 	| violates Sylak-Glassman (2016)	 |
| 42782	 		| ARGSG	 	| violates Sylak-Glassman (2016)	 |
| 53148	 		| DU	 	| 	 |
| 22418	 		| EXCL	 	| 	 |
| 16926	 		| IMP	 	| 	 |
| 12098	 		| INCL	 	| 	 |
| 9088	 		| INTR	 	| 	 |
| 77732	 		| NEG	 	| 	 |
| 633	 		| NFIN	 	| 	 |
| 53448	 		| PL	 	| 	 |
| 78365	 		| POS	 	| 	 |
| 71116	 		| PRS	 	|	 |
| 67422	 		| PST	 	| 	 |
| 3250	 		| REFL	 	| 	 |
| 48868	 		| SG	 	| 	 |
| 143759		| TR	 	| 	 |
| 156097	 	| V	 		| 	 |

	* cf. Sylak-Glassman (2016)

Note that the encoding of multiple arguments violates Sylak-Glassman (2016), but *seems* to be based on the following strategy:
 * primary argument (subject?) without ARG prefix
 * secondary argument (object?) with ARG prefix
 * every agreement feature of the secondary argument is encoded in isolation

The current encoding seems to be very much in line with the suggestion of Chiarcos et al. (accepted@LREC-2018) for a more compact and more scalable alternative to the ARG encoding on grounds of a (language-specific) ranking-based of arguments.
Assuming that the presence of the ARG prefix indicates a ranking between two arguments, we can define a language-specific two-level hierarchy

	unmarked > ARG-marked

We presume that this corresponds to a functional mapping 

	SBJ > OBJ
	
However, this needs to be confirmed by a language specialist. 

Transformation to numerical encoding is trivial and implemented in ../Makefile