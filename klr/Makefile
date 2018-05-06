all:	klr

# apply modifications as documented in src/Readme.md
# note that we do not correct the undocumented tag "HYP"
klr:	src/klr
	sed s/'ARG\([^;]*\)'/'\1-1'/g src/klr > klr