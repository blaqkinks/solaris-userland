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
# Copyright (c) 2011, 2017, Oracle and/or its affiliates. All rights reserved.
#
BUILD_BITS= 64_and_32
include ../../make-rules/shared-macros.mk

COMPONENT_NAME=		libexpat
COMPONENT_VERSION=	2.2.1
COMPONENT_PROJECT_URL=	http://expat.sourceforge.net/
COMPONENT_SRC_NAME=	expat
COMPONENT_ARCHIVE=	$(COMPONENT_SRC).tar.bz2
COMPONENT_ARCHIVE_HASH=	\
    sha256:1868cadae4c82a018e361e2b2091de103cd820aaacb0d6cfa49bd2cd83978885
COMPONENT_ARCHIVE_URL= \
    http://downloads.sourceforge.net/project/expat/expat/$(COMPONENT_VERSION)/$(COMPONENT_ARCHIVE)
COMPONENT_BUGDB=	library/libexpat
COMPONENT_ANITYA_ID=	770

TPNO=			36098

include $(WS_MAKE_RULES)/common.mk

CFLAGS += $(CPP_LARGEFILES)

PATCH_LEVEL = 2

COMPONENT_TEST_MASTER = $(COMPONENT_TEST_RESULTS_DIR)/results-all.master

COMPONENT_TEST_TRANSFORMS = \
	'-e "/^Expat version: /d" ' \
	'-e "s/^make\[.*$$/[EnD]/" ' \
	'-e "/^\.\/run\.sh tests\/runtests/,/^\[EnD\]/p" ' \
	'-e "/.*/d" '
