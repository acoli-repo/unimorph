all:	nav

# apply modifications as documented in src/Readme.md
nav:	src/nav
	cat src/nav | \
	perl -pe " \
		s/(PSS4)(;.*)?;ARG(AC)3(S)/\1\4\3\2/g; \
	" > nav
