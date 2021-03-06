#
#  Makefile --
#
#     Makefile for building file dialog plug-in for Mac.
#
#  Copyright (c) 2002-2013 Bjorn Gustavsson
#
#  See the file "license.terms" for information on usage and redistribution
#  of this file, and for a DISCLAIMER OF ALL WARRANTIES.
#
#     $Id$
#
LIBDIR = ../../plugins/mac_file
WINGS_EBIN = ../../ebin
WINGS_INTL=../../intl_tools
CFLAGS = $(UNIVERSAL_FLAGS)

ERL_DIR := $(shell echo 'io:format("~s~n",[code:root_dir()]),halt().' | erl | sed 's,^[0-9]*> *,,g' | tail +2)

BEAM = $(LIBDIR)/wp8_mac_file.beam \
       $(LIBDIR)/wp8_mac_image.beam \
       $(LIBDIR)/wpc_mac_misc.beam

ERLC = erlc
ERLC_FLAGS = -Werror +debug_info -o $(LIBDIR) -I ../../e3d -I ../../src -pa $(WINGS_EBIN) -pa $(WINGS_INTL) -I $(WINGS_INTL)

DRV = $(LIBDIR)/mac_wings_image_drv.so

opt: $(DRV) $(BEAM)

template: opt
	erl -pa $(WINGS_INTL) -noinput -run tools generate_template $(LIBDIR) -run erlang halt

lang: template
	cp *.lang $(LIBDIR)
	erl -pa $(WINGS_INTL) -noinput -run tools diff_lang_files $(LIBDIR)

$(LIBDIR)/%.beam: %.erl
	install -d $(LIBDIR)
	$(ERLC) $(ERLC_FLAGS) $<

$(LIBDIR)/mac_wings_image_drv.so: mac_wings_image_drv.m
	install -d $(LIBDIR)
	$(CC) -Werror -m32 -I $(ERL_DIR)/usr/include \
	  -bundle -flat_namespace -undefined suppress \
          -framework Cocoa -o $(LIBDIR)/mac_wings_image_drv.so $(UNIVERSAL_FLAGS) \
	  mac_wings_image_drv.m
