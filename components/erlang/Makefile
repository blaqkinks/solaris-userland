#
# CDDL HEADER START
#
# The contents of this file are subject to the terms of the
# Common Development and Distribution License (the "License").
# You may not use this file except in compliance with the License.
#
# You can obtain a copy of the license at usr/src/OPENSOLARIS.LICENSE
# or http://www.opensolaris.org/os/licensing.
# See the License for the specific language governing permissions
# and limitations under the License.
#
# When distributing Covered Code, include this CDDL HEADER in each
# file and include the License file at usr/src/OPENSOLARIS.LICENSE.
# If applicable, add the following below this CDDL HEADER, with the
# fields enclosed by brackets "[]" replaced with your own identifying
# information: Portions Copyright [yyyy] [name of copyright owner]
#
# CDDL HEADER END
#

#
# Copyright (c) 2011, 2018, Oracle and/or its affiliates. All rights reserved.
#

# Built in SFW with gcc and builds cleanly now only with gcc
BUILD_BITS= 64
COMPILER= gcc
include ../../make-rules/shared-macros.mk

PATH=$(JAVA_HOME)/bin:$(USRBINDIR)

COMPONENT_NAME=			erlang
COMPONENT_VERSION=		19.3
COMPONENT_PROJECT_URL=	http://www.erlang.org/
COMPONENT_SRC_NAME=	otp_src
COMPONENT_SRC=		$(COMPONENT_SRC_NAME)_$(COMPONENT_VERSION)
COMPONENT_ARCHIVE_HASH=	\
    sha256:fe4a00651db39b8542b04530a48d24b2f2e7e0b77cbe93d728c9f05325bdfe83
COMPONENT_ARCHIVE_URL=	$(COMPONENT_PROJECT_URL)download/$(COMPONENT_ARCHIVE)
COMPONENT_BUGDB=	utility/erlang
COMPONENT_ANITYA_ID=	707

# manpages
COMPONENT_NAME_1=	otp_doc_man
COMPONENT_SRC_1=	$(COMPONENT_NAME_1)_$(COMPONENT_VERSION)
COMPONENT_ARCHIVE_1=	$(COMPONENT_SRC_1).tar.gz
COMPONENT_ARCHIVE_HASH_1=	\
    sha256:f8192ffdd7367083c055695eeddf198155da43dcc221aed1d870d1e3871dd95c
COMPONENT_ARCHIVE_URL_1=	$(COMPONENT_PROJECT_URL)download/$(COMPONENT_ARCHIVE_1)
UNPACK_ARGS_1=	-r $(COMPONENT_SRC_1)
PKG_PROTO_DIRS +=	$(COMPONENT_SRC_1)

# HTML documentation
COMPONENT_NAME_2=	otp_doc_html
COMPONENT_SRC_2=	$(COMPONENT_NAME_2)_$(COMPONENT_VERSION)
COMPONENT_ARCHIVE_2=	$(COMPONENT_SRC_2).tar.gz
COMPONENT_ARCHIVE_HASH_2=	\
    sha256:dc3e3a82d1aba7f0deac1ddb81b7d6f8dee9a75e1d42b90c677a2b645f19a00c
COMPONENT_ARCHIVE_URL_2=	$(COMPONENT_PROJECT_URL)download/$(COMPONENT_ARCHIVE_2)
UNPACK_ARGS_2=	-r $(COMPONENT_SRC_2)
PKG_PROTO_DIRS +=	$(COMPONENT_SRC_2)

TPNO=			35510

SYSTEM_TEST_TARGET= configure $(SYSTEM_TEST_64)

include $(WS_MAKE_RULES)/common.mk

# Fix 64-bit linking.
LDFLAGS += $(CC_BITS)

CXXFLAGS += -std=c++11

CONFIGURE_OPTIONS += --enable-smp-support
CONFIGURE_OPTIONS += --enable-threads
# The HiPE portion is a bit sensitive, and only seems to work 64-bit on
# amd64 (thus the variant.arch tagging in erlang.p5m). erts/config.log
# can be very interesting if issues occur.
CONFIGURE_OPTIONS += --enable-hipe
CONFIGURE_OPTIONS += --with-ssl
CONFIGURE_OPTIONS += --enable-dynamic-ssl-lib

# Erlang doesn't appear to like ASLR: it uses mmap() with MAP_FIXED, and
# makes assumptions about memory layout that aren't true on Solaris x86,
# leading to segmentation faults in erlc.
ASLR_MODE =	$(ASLR_DISABLE)

# Use cp(1) instead of cloney because erlang's 'install' target uses 'tar'
# to copy files and our tar doesn't handle long links by default.  Between
# the use of 'tar' and 'ld', a path adjustment wasn't an option due to the
# need for /usr/bin/ld and /usr/gnu/bin/tar.  This is still the case with
# erlang version 17.5.
COMPONENT_PRE_CONFIGURE_ACTION = \
	($(CP) -rp $(SOURCE_DIR)/. $(@D))
CONFIGURE_SCRIPT =		$(@D)/configure

# We need to re-run the nested configure script for the wx sub-component so
# that it will properly detect wxWidgets support by adding -std=c++11 to $(CXX)
# (workaround for the configure script ignoring CXXFLAGS) and with ERL_TOP set
# so that the wx component knows we're building inside the erlang source tree.
COMPONENT_POST_CONFIGURE_ACTION = \
	(cd $(@D)/lib/wx; $(ENV) $(CONFIGURE_ENV) \
		CXX="$(CXX) -std=c++11" \
		ERL_TOP="$(@D)" \
		$(CONFIG_SHELL) \
		./configure $(CONFIGURE_OPTIONS))

# Always show the build and link lines for easier debugging.
COMPONENT_BUILD_ARGS +=	V=1

COMPONENT_INSTALL_ARGS += INSTALL_PREFIX=$(PROTO_DIR)

COMPONENT_TEST_ENV +=		ERL_TOP=$(BUILD_DIR_64)
COMPONENT_TEST_TARGETS =	tests

COMPONENT_SYSTEM_TEST_ENV +=	ERL_TOP=$(BUILD_DIR_64)
COMPONENT_SYSTEM_TEST_TARGETS =	tests

COMPONENT_TEST_TRANSFORMS += \
	'-e "/^gmake.*release_tests_spec/d" '

# remove INSTALL_PREFIX from path
COMPONENT_POST_INSTALL_ACTION += \
	$(PERL) -pi -e "s|$(PROTO_DIR)||g" \
		$(PROTOUSRLIBDIR64)/erlang/bin/erl \
		$(PROTOUSRLIBDIR64)/erlang/releases/RELEASES

REQUIRED_PACKAGES += library/graphics/wxwidgets
REQUIRED_PACKAGES += library/ncurses
REQUIRED_PACKAGES += library/security/openssl
REQUIRED_PACKAGES += library/security/openssl/openssl-fips-140
REQUIRED_PACKAGES += library/zlib
REQUIRED_PACKAGES += shell/ksh93
REQUIRED_PACKAGES += system/core-os
REQUIRED_PACKAGES += system/library/gcc/gcc-c++-runtime
REQUIRED_PACKAGES += system/library/gcc/gcc-c-runtime
REQUIRED_PACKAGES += system/library/math
REQUIRED_PACKAGES += x11/library/mesa
