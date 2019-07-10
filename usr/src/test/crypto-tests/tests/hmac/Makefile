#
# This file and its contents are supplied under the terms of the
# Common Development and Distribution License ("CDDL"), version 1.0.
# You may only use this file in accordance with the terms of version
# 1.0 of the CDDL.
#
# A full copy of the text of the CDDL should have accompanied this
# source.  A copy of the CDDL is also available via the Internet at
# http://www.illumos.org/license/CDDL.
#

#
# Copyright 2019 Joyent, Inc.
#

include $(SRC)/cmd/Makefile.cmd
include $(SRC)/test/Makefile.com
include $(SRC)/cmd/Makefile.ctf

ALGS = hmac_sha1
CRYPTO = pkcs kcf

PROGS_pkcs_32 = $(ALGS:%=%_32_pkcs)
PROGS_pkcs_64 = $(ALGS:%=%_64_pkcs)
PROGS_kcf_32 = $(ALGS:%=%_32_kcf)
PROGS_kcf_64 = $(ALGS:%=%_64_kcf)
PROGS_pkcs = $(PROGS_pkcs_32) $(PROGS_pkcs_64)
PROGS_kcf = $(PROGS_kcf_32) $(PROGS_kcf_64)
PROGS_32 = $(PROGS_pkcs_32) $(PROGS_kcf_32)
PROGS_64 = $(PROGS_pkcs_64) $(PROGS_kcf_64)
PROGS = $(PROGS_32) $(PROGS_64)

ROOTOPTPKG = $(ROOT)/opt/crypto-tests
TESTROOT = $(ROOTOPTPKG)/tests/hmac
TESTDIR_pkcs = $(TESTROOT)/pkcs
TESTDIR_kcf = $(TESTROOT)/kcf

COMMONDIR = ../common

$(CRYPTO:%=hmac_sha1_32_%)	:= OBJS = hmac_sha1.32.o
$(CRYPTO:%=hmac_sha1_64_%)	:= OBJS = hmac_sha1.64.o

$(PROGS_32)		:= OBJS_COMMON = main.32.o $(COMMONDIR)/testfuncs.32.o
$(PROGS_64)		:= OBJS_COMMON = main.64.o $(COMMONDIR)/testfuncs.64.o
$(PROGS_pkcs_32)	:= OBJS_COMMON += $(COMMONDIR)/cryptotest_pkcs.32.o
$(PROGS_pkcs_64)	:= OBJS_COMMON += $(COMMONDIR)/cryptotest_pkcs.64.o
$(PROGS_kcf_32)		:= OBJS_COMMON += $(COMMONDIR)/cryptotest_kcf.32.o
$(PROGS_kcf_64)		:= OBJS_COMMON += $(COMMONDIR)/cryptotest_kcf.64.o

CSTD = $(CSTD_GNU99)

CPPFLAGS += -D_LARGEFILE_SOURCE -D_FILE_OFFSET_BITS=64
CPPFLAGS += -I$(COMMONDIR) -I$(SRC)/common/crypto

$(PROGS_pkcs) := LDLIBS += -lpkcs11 -lcryptoutil

CMDS_pkcs = $(PROGS_pkcs:%_pkcs=$(TESTDIR_pkcs)/%)
CMDS_kcf = $(PROGS_kcf:%_kcf=$(TESTDIR_kcf)/%)
CMDS = $(CMDS_pkcs) $(CMDS_kcf)

$(CMDS) := FILEMODE = 0555

all: $(PROGS)

$(PROGS): main.32.o main.64.o

%_32_pkcs: %.32.o
	$(LINK.c) -o $@ $(OBJS) $(OBJS_COMMON) $(LDLIBS)
	$(POST_PROCESS)

%_64_pkcs: %.64.o
	$(LINK64.c) -o $@ $(OBJS) $(OBJS_COMMON) $(LDLIBS)
	$(POST_PROCESS)

%_32_kcf: %.32.o
	$(LINK.c) -o $@ $(OBJS) $(OBJS_COMMON) $(LDLIBS)
	$(POST_PROCESS)

%_64_kcf: %.64.o
	$(LINK64.c) -o $@ $(OBJS) $(OBJS_COMMON) $(LDLIBS)
	$(POST_PROCESS)

%.32.o: %.c
	$(COMPILE.c) -o $@ $<
	$(POST_PROCESS_O)

%.64.o: %.c
	$(COMPILE64.c) -o $@ $<
	$(POST_PROCESS_O)


install: all $(CMDS)

clobber: clean
	-$(RM) $(PROGS)

clean:
	-$(RM) $(ALGS:%=%.32.o) $(ALGS:%=%.64.o) main.32.o main.64.o

$(CMDS): $(TESTDIR_pkcs) $(TESTDIR_kcf) $(PROGS)

$(TESTDIR_pkcs) $(TESTDIR_kcf):
	$(INS.dir)

$(TESTDIR_pkcs)/%_32: %_32_pkcs
	$(INS.rename)

$(TESTDIR_pkcs)/%_64: %_64_pkcs
	$(INS.rename)

$(TESTDIR_kcf)/%_32: %_32_kcf
	$(INS.rename)

$(TESTDIR_kcf)/%_64: %_64_kcf
	$(INS.rename)
