all:	eus

# apply modifications as documented in src/Readme.md
# note that we do not correct the undocumented tag "HYP"
eus:	src/eus
	cat src/eus | \
	perl -pe \
	   'if(m/ARGER/) {				# if transitive \
			s/ARGABS([^;\t\n]+)/$$1-1/g;	# ABS => OBJ (index 1) \
			s/ARGERG([^;\t\n]+)/$$1/g;		# ERG => SBJ (no index) \
		}; \
		s/ARGABS([^;\t\n]+)/$$1/g;			# intransitive subject (no index) \
		s/ARGIO([^;\n\t]+)/$$1-2/g;			# DAT => OTH \
		s/PAST/PST/g; \
		s/PRES/PRS/g; ' > eus