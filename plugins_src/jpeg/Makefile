#
#  Makefile --
#
#     Makefile for building file JPEG image plug-in.
#
#  Copyright (c) 2004 Bjorn Gustavsson
#
#  See the file "license.terms" for information on usage and redistribution
#  of this file, and for a DISCLAIMER OF ALL WARRANTIES.
#
#     $Id: Makefile,v 1.3 2004/01/18 18:49:40 bjorng Exp $
#

ifeq ($(findstring CYGWIN,$(shell uname -s)),CYGWIN)
  CYGWIN := true
endif

LIBDIR = ../../plugins/jpeg

ERL_DIR := $(shell echo 'io:format("~s~n",[code:root_dir()]),init:stop().' | erl | sed -n '/^1>/s/^1> //p')
ERL_INC = $(ERL_DIR)/usr/include
BEAM = $(LIBDIR)/wp8_jpeg_image.beam
LIBS = -shared -ljpeg

ERLC = erlc
ERLC_FLAGS = -o $(LIBDIR) -I ../../e3d -I ../../src -I $(ESDL_PATH)/include -pa $(ESDL_PATH)

ifdef CYGWIN
SO_EXT = dll
else
SO_EXT = so
endif

DRV = $(LIBDIR)/wings_jpeg_image_drv.$(SO_EXT)

all: $(BEAM) $(DRV)

$(LIBDIR)/%.beam: %.erl
	install -d $(LIBDIR)
	$(ERLC) $(ERLC_FLAGS) $<

ifdef CYGWIN
$(LIBDIR)/wings_jpeg_image_drv.dll: wings_jpeg_image_drv.c \
  erl_driver.h erl_win_dyn_driver.h
	install -d $(LIBDIR)
	mingw32-gcc -o $(LIBDIR)/wings_jpeg_image_drv.dll \
  wings_jpeg_image_drv.c $(LIBS)

erl_driver.h: $(ERL_INC)/erl_driver.h
	cp $(ERL_INC)/erl_driver.h .

erl_win_dyn_driver.h: $(ERL_INC)/erl_win_dyn_driver.h
	sed 's,##Params, Params,g' "$(ERL_INC)/erl_win_dyn_driver.h" > erl_win_dyn_driver.h
else

$(LIBDIR)/wings_jpeg_image_drv.so: wings_jpeg_image_drv.c
	install -d $(LIBDIR)
	gcc -o $(LIBDIR)/wings_jpeg_image_drv.so -I$(ERL_INC) \
  wings_jpeg_image_drv.c $(LIBS)

endif
