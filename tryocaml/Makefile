# remove this variable to use the default ocaml toplevel
INCLUDE_TOPLEVELLIB= -I toplevellib
TOPLEVELLIB=toplevellib/

COMPILER=js_of_ocaml
LIBNAME=js_of_ocaml


TOPLEVEL_CMIS=toplevel.cmi
TOPLEVEL_CMOS= toplevel.cmo topmain.cmo
TOPLEVEL_CMAS= $(JS)/lib/$(LIBNAME).cma $(JS)/compiler/compiler.cma  $(TOPLEVELLIB)toplevellib.cma tutorial/tutorial.cma

JS=js_of_ocaml-patched
INCLUDES= -I $(JS)/lib -I $(JS)/compiler -I tutorial $(INCLUDE_TOPLEVELLIB) -I .

# To add a new toplevel, add a new name below, add it to TOPLEVELS, copy
# the lines defining TRYOCAML_WITH_JS* at the end, and replace with
# your own settings

TRYOCAML_NAME=toplevel
TRYOCAML_WITH_JS_NAME=toplevel-with-js
TOPLEVELS= $(TRYOCAML_NAME).js $(TRYOCAML_WITH_JS_NAME).js

all: compiler tuto $(TOPLEVELS)
tryocaml: toplevel.js

compiler:
	$(MAKE) -C $(JS) all

#include $(JS)/Makefile.conf

COMP=$(JS)/compiler/$(COMPILER)
#COMP=$(LIBRARY)
JSFILES=$(JS)/runtime/runtime.js $(JS)/runtime/weak.js toplevel_runtime.js
OCAMLC=ocamlfind ocamlc -annot -package lwt,str -pp "camlp4o $(JS)/lib/syntax/pa_js.cmo"
EXPUNGE=$(shell ocamlc -where)/expunge
# Removed gc and sys
STDLIB_MODULES=\
  arg \
  array \
  arrayLabels \
  buffer \
  callback \
  camlinternalLazy \
  camlinternalMod \
  camlinternalOO \
  char \
  complex \
  digest \
  filename \
  format \
  genlex \
  hashtbl \
  int32 \
  int64 \
  lazy \
  lexing \
  list \
  listLabels \
  map \
  marshal \
  moreLabels \
  nativeint \
  obj \
  oo \
  parsing \
  pervasives \
  printexc \
  printf \
  queue \
  random \
  scanf \
  set \
  sort \
  stack \
  stdLabels \
  stream \
  string \
  stringLabels \
  weak

TOPLEVEL_MODULES=$(STDLIB_MODULES) n regexp tutorial outcometree topdirs toploop toplevel

%.cmo: %.ml
	$(OCAMLC) $(INCLUDES) -c $<
%.cmi: %.mli
	$(OCAMLC) $(INCLUDES) -c $<

%.cmi:$(JS)/compiler/compiler.cma

$(JS)/compiler/compiler.cma:
	$(MAKE) -C $(JS)/compiler compiler.cma

tuto:
	$(MAKE) -C tutorial all

clean::
	rm -f *.cm[io] $(TOPLEVELS:.js=.byte) *.annot *~
	$(MAKE) -C tutorial clean
	$(MAKE) -C $(JS) clean

depend:
	ocamldep -pp "camlp4o $(JS)/lib/syntax/pa_js.cmo" $(INCLUDES) *.ml *.mli > .depend

toplevel.cmo: toplevellib/*.cmi

include .depend


############### TRYOCAML_NAME=toplevel

TRYOCAML_EXTRA_CMIS=
TRYOCAML_EXTRA_CMAS=
TRYOCAML_EXTRA_MODULES=

$(TRYOCAML_NAME).js: $(TRYOCAML_NAME).byte $(JSFILES)
	$(COMP) $(INCLUDES) -toplevel -linkall -noinline -noruntime $(JSFILES) $(TRYOCAML_NAME).byte $(OPTIONS)

$(TRYOCAML_NAME).byte: $(TOPLEVEL_CMIS) $(TOPLEVEL_CMOS) $(TOPLEVEL_CMAS) $(TRYOCAML_EXTRA_CMAS) $(TRYOCAML_EXTRA_CMIS)
	$(OCAMLC) $(INCLUDES) -linkall -package str -linkpkg -o $@.tmp $(TOPLEVEL_CMAS) $(TRYOCAML_EXTRA_CMAS) $(TOPLEVEL_CMOS)
	$(EXPUNGE) $@.tmp $@ $(TOPLEVEL_MODULES) $(TRYOCAML_EXTRA_MODULES)
	rm -f $@.tmp


############### TRYOCAML_WITH_JS_NAME=toplevel-with-js

TRYOCAML_WITH_JS_EXTRA_CMIS=
TRYOCAML_WITH_JS_EXTRA_CMAS=
TRYOCAML_WITH_JS_EXTRA_MODULES= \
  css \
  dom \
  dom_events \
  dom_html \
  event_arrows \
  file \
  firebug \
  form \
  js \
  json \
  deriving_Json \
  lwt_js \
  regexp \
  url \
  xmlHttpRequest \


$(TRYOCAML_WITH_JS_NAME).js: $(TRYOCAML_WITH_JS_NAME).byte $(JSFILES)
	$(COMP) $(INCLUDES) -toplevel -linkall -noinline -noruntime $(JSFILES) $(TRYOCAML_WITH_JS_NAME).byte $(OPTIONS)

$(TRYOCAML_WITH_JS_NAME).byte: $(TOPLEVEL_CMIS) $(TOPLEVEL_CMOS) $(TOPLEVEL_CMAS) $(TRYOCAML_WITH_JS_EXTRA_CMAS) $(TRYOCAML_WITH_JS_EXTRA_CMIS)
	$(OCAMLC) $(INCLUDES) -linkall -package str -linkpkg -o $@.tmp $(TOPLEVEL_CMAS) $(TRYOCAML_WITH_JS_EXTRA_CMAS) $(TOPLEVEL_CMOS)
	$(EXPUNGE) $@.tmp $@ $(TOPLEVEL_MODULES) $(TRYOCAML_WITH_JS_EXTRA_MODULES)
	rm -f $@.tmp

