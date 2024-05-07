############################################################################
# apps/canutils/libdronecan/Makefile
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.  The
# ASF licenses this file to you under the Apache License, Version 2.0 (the
# "License"); you may not use this file except in compliance with the
# License.  You may obtain a copy of the License at
#
#   http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS, WITHOUT
# WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.  See the
# License for the specific language governing permissions and limitations
# under the License.
#
############################################################################

include $(APPDIR)/Make.defs

WGET = wget
UNPACK = unzip
PACKEXT = .zip

LIBDRONECAN_URL = $(patsubst "%",%,$(strip $(CONFIG_LIBDRONECAN_URL)))
LIBDRONECAN_VERSION = $(patsubst "%",%,$(strip $(CONFIG_LIBDRONECAN_VERSION)))
LIBDRONECAN_UNPACKNAME = libcanard-$(LIBDRONECAN_VERSION)
LIBDRONECAN_PACKNAME = $(LIBDRONECAN_UNPACKNAME)$(PACKEXT)
LIBDRONECAN_SRCDIR = $(LIBDRONECAN_UNPACKNAME)

APPS_INCDIR = $(APPDIR)$(DELIM)include$(DELIM)canutils

CFLAGS += -std=c11 -I$(APPS_INCDIR) -DCANARD_ASSERT=DEBUGASSERT

CSRCS = $(LIBDRONECAN_SRCDIR)$(DELIM)canard.c

$(LIBDRONECAN_PACKNAME):
	@echo "Downloading: $@"
	$(Q) $(WGET) -O $@ $(LIBDRONECAN_URL)$(DELIM)$(LIBDRONECAN_VERSION)$(PACKEXT)

$(LIBDRONECAN_UNPACKNAME): $(LIBDRONECAN_PACKNAME)
	@echo "Unpacking: $< -> $@"
	$(call DELDIR, $@)
	$(Q) $(UNPACK) $<
	$(Q) touch $@

$(LIBDRONECAN_SRCDIR)$(DELIM)canard.h: $(LIBDRONECAN_UNPACKNAME)

$(LIBDRONECAN_SRCDIR)$(DELIM)canard_internals.h: $(LIBDRONECAN_UNPACKNAME)

$(APPS_INCDIR)$(DELIM)canard.h: $(LIBDRONECAN_SRCDIR)$(DELIM)canard.h
	$(Q) cp $< $@

$(APPS_INCDIR)$(DELIM)canard_internals.h: $(LIBDRONECAN_SRCDIR)$(DELIM)canard_internals.h
	$(Q) cp $< $@


context:: $(APPS_INCDIR)$(DELIM)canard.h $(APPS_INCDIR)$(DELIM)canard_internals.h

clean::
	$(foreach OBJ, $(OBJS), $(call DELFILE, $(OBJ)))

distclean::
	$(call DELFILE, $(APPS_INCDIR)$(DELIM)canard.h)
	$(call DELFILE, $(APPS_INCDIR)$(DELIM)canard_internals.h)
	$(call DELDIR, $(LIBDRONECAN_UNPACKNAME))
	$(call DELFILE, $(LIBDRONECAN_PACKNAME))

include $(APPDIR)/Application.mk
