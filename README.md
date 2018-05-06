UniMorph Data
=============

This repository contains UniMorph data for several languages which are investigated by the Applied Computational Linguistics (ACoLi) lab at Goethe University Frankfurt,  the associated project "Linked Open Dictionaries" (LiODi), or provided by the Unimorph community (http://unimorph.github.io/).

Note that this is an exploratory fork and supposed to feed back into a future version of Unimorph.

## Novel data sets (ACoLi/LiODi)
 
 - gmh/ (Middle High German@ACoLi)
 - gml/ (Middle Low German@ACoLi)
 - goh/ (Old High German@ACoLi)
 - kjj/ (Khinalug@LiODi)
 - osx/ (Old Saxon@ACoLi)
 - xcj/ (Classical Armenian@LiODi; upcoming)
 - xmf/ (Megrelian@LiODi; upcoming)

The LiODi data sets are original resources (kjj, xmf), resp., converted from existing corpora (xcj).
The ACoLi data sets use conventional UniMorph annotations, but in opposition to conventional UniMorph data, we include source data and build scripts (Makefile). A technological innovation is that gmh, goh and osx use ontologies and SPARQL update for transforming the original annotations to UniMorph annotations. 

## Revised UniMorph annotations for argument marking (LiODi)

A critical assessment of the use of UniMorph ARG features, originally intended to express arguments of polyvalent verbs in head-marking languages.

 - eus/ (Basque@UniMorph, revised by LiODi)
 - kat/ (Georgian@UniMorph, commented by LiODi)
 - klr/ (Khilang@UniMorph, revised by LiODi)
 - nav/ (Navajo@UniMorph, revised by LiODi)

In fact, we found that *not a single* UniMorph data set uses the ARG annotations as devised by Sylak-Glassman (2016). We consider this a strong justification to the alternative encoding of multiple arguments as suggested by Chiarcos et al. (submitted@LREC 2018). This transformation has been applied, the original UniMorph data sets are preserved as source (src/) files.