###########################################################################
#
# Copyright 2016 Samsung Electronics All Rights Reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
# http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing,
# software distributed under the License is distributed on an
# "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND,
# either express or implied. See the License for the specific
# language governing permissions and limitations under the License.
#
###########################################################################

############################################################################
# apps/examples/testcase/Makefile
#
#   Copyright (C) 2011-2014 Gregory Nutt. All rights reserved.
#   Author: Gregory Nutt <gnutt@nuttx.org>
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions
# are met:
#
# 1. Redistributions of source code must retain the above copyright
#    notice, this list of conditions and the following disclaimer.
# 2. Redistributions in binary form must reproduce the above copyright
#    notice, this list of conditions and the following disclaimer in
#    the documentation and/or other materials provided with the
#    distribution.
# 3. Neither the name NuttX nor the names of its contributors may be
#    used to endorse or promote products derived from this software
#    without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS
# "AS IS" AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT
# LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS
# FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE
# COPYRIGHT OWNER OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT,
# INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING,
# BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS
# OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED
# AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
# LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN
# ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.
#
############################################################################

-include $(TOPDIR)/.config
-include $(TOPDIR)/Make.defs
include $(APPDIR)/Make.defs

# built-in application info

APPNAME = tc
FUNCNAME = tc_main
THREADEXEC = TASH_EXECMD_ASYNC

# testcase example

ASRCS =
CSRCS =

ifeq ($(CONFIG_EXAMPLES_TESTCASE),y)

CSRCS += tc_main.c
DEPPATH = --dep-path .
VPATH = .

ifeq ($(WINTOOL),y)
INCDIROPT = -w
endif

include le_tc/filesystem/Make.defs
include le_tc/kernel/Make.defs
include le_tc/network/Make.defs
include le_tc/ttrace/Make.defs
include ta_tc/systemio/utc/Make.defs
include ta_tc/systemio/itc/Make.defs
include ta_tc/arastorage/utc/Make.defs
include ta_tc/arastorage/itc/Make.defs
include ta_tc/device_management/utc/Make.defs
include ta_tc/device_management/itc/Make.defs

endif

AOBJS		= $(ASRCS:.S=$(OBJEXT))
COBJS		= $(CSRCS:.c=$(OBJEXT))

SRCS		= $(ASRCS) $(CSRCS)
OBJS		= $(AOBJS) $(COBJS)

ifeq ($(CONFIG_WINDOWS_NATIVE),y)
  BIN		= ..\..\libapps$(LIBEXT)
else
ifeq ($(WINTOOL),y)
  BIN		= ..\\..\\libapps$(LIBEXT)
else
  BIN		= ../../libapps$(LIBEXT)
endif
endif

# Build targets

all:	.built
.PHONY: context .depend depend clean distclean

ifeq ($(CONFIG_EXAMPLES_TESTCASE),y)

$(AOBJS): %$(OBJEXT): %.S
	$(call ASSEMBLE, $<, $@)

$(COBJS): %$(OBJEXT): %.c
	$(call COMPILE, $<, $@)
endif

.built: $(OBJS)
ifeq ($(CONFIG_EXAMPLES_TESTCASE),y)
	$(call ARCHIVE, $(BIN), $(OBJS))
	$(Q) touch .built
endif

install:

ifeq ($(CONFIG_BUILTIN_APPS)$(CONFIG_EXAMPLES_TESTCASE),yy)
$(BUILTIN_REGISTRY)$(DELIM)$(APPNAME)_main.bdat: $(DEPCONFIG) Makefile
	$(Q) $(call REGISTER,$(APPNAME),$(APPNAME)_main,$(THREADEXEC),$(PRIORITY),$(STACKSIZE))

context: $(BUILTIN_REGISTRY)$(DELIM)$(APPNAME)_main.bdat

else
context:

endif

.depend: Makefile $(SRCS)
ifeq ($(CONFIG_EXAMPLES_TESTCASE),y)
	$(Q) $(MKDEP) $(DEPPATH) "$(CC)" -- $(CFLAGS) -- $(SRCS) >Make.dep
	$(Q) touch $@
endif

depend: .depend

clean:
	$(call DELFILE, .built)
	$(call CLEAN)

distclean: clean
	$(call DELFILE, Make.dep)
	$(call DELFILE, .depend)


-include Make.dep
.PHONY: preconfig
preconfig:
