BUNGLE 		= ../node_modules/.bin/bungle
JSHINT 		= ../node_modules/.bin/jshint
PATTERNS 	= $(shell cd ../src; find . -name "*.js")
TESTS 		= $(wildcard ../tests/specs/*.js) $(wildcard ../tests/specs/**/*.js)
TARGETS 	= index.html \
		  TestRunner-bundle.html \
		  TestRunner-bundle-min.html \
		  TestRunner-bundle.js \
		  TestRunner-modules.html \
		  TestRunner-modules.js


all:: $(TARGETS)

clean:
	rm -f $(TARGETS)

index.html: TestRunner-modules.html
	ln -sf $< $@

TestRunner-bundle.html: TestRunner.html.in
	sed -e 's#@PATTERNS@#src="../bundles/patterns.js"#' \
	     -e 's#@TESTRUNNER@#src="TestRunner-bundle.js"#' $< > $@

TestRunner-bundle-min.html: TestRunner.html.in
	sed -e 's#@PATTERNS@#src="../bundles/patterns.min.js"#' \
	     -e 's#@TESTRUNNER@#src="TestRunner-bundle.js"#' $< > $@

TestRunner-modules.html: TestRunner.html.in
	sed -e 's#@PATTERNS@#src="../bungledeps/require.js" data-main="../src/main"#' \
	     -e 's#@TESTRUNNER@#src="TestRunner-modules.js"#' $< > $@

empty  		:=
space 		:= $(empty) $(empty)
comma 		:= ,
TESTLIST 	= $(subst $(space),$(comma),$(patsubst %,\"%\",$(TESTS)))

jshint: $(TESTS)
	$(JSHINT) $(TESTS)

TestRunner-modules.js: TestRunner.js.in $(TESTS) jshint
	sed -e "s#@TESTS@#$(TESTLIST)#" $< > $@

TestRunner-bundle.js: TestRunner-modules.js
	cd ..; \
	tests/$(BUNGLE) build -i ../tests/$<,$(subst $(space),$(comma),$(TESTS)) \
	    -E $(subst $(space),$(comma),$(PATTERNS)) -o tests/$@ 

.PHONY: all clean
