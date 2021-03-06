# Copyright (C) 2006 International Business Machines and others.
# All Rights Reserved.
# This file is distributed under the Common Public License.

## $Id$

# Author:  Andreas Waechter           IBM    2006-04-13

AUTOMAKE_OPTIONS = foreign

########################################################################
#                          Subdirectories                              #
########################################################################

SUBDIRS = src

# We don't want to compile the tests subdirectory, unless the test target is
# specified.  But we need to list it as subdirectory to make sure that it is
# included in the tarball

if ALWAYS_FALSE
  SUBDIRS += tests
endif

########################################################################
#             Additional files to be included in tarball               #
########################################################################

# Here we need include all files that are not mentioned in other Makefiles

EXTRA_DIST =

########################################################################
#                           Extra Targets                              #
########################################################################

test: all
	cd tests; make test

unitTest: test

# Doxygen documentation

doxydoc:
	doxygen doxydoc/doxygen.conf

clean-doxydoc:
	( cd doxydoc ; rm -rf html *.log *.tag )

clean-local: clean-doxydoc
	if test -r tests/Makefile; then cd tests; make clean; fi

distclean-local:
	if test -r tests/Makefile; then cd tests; make distclean; fi

install-exec-local: install-doc

uninstall-local: uninstall-doc

.PHONY: tests unitTest doxydoc

########################################################################
#                  Installation of the addlibs file                    #
########################################################################

pkgconfiglibdir = $(libdir)/pkgconfig
pkgconfiglib_DATA = rehearse.pc

addlibsdir = $(DESTDIR)$(datadir)/coin/doc/Rehearse

install-data-hook:
	@$(mkdir_p) "$(addlibsdir)"
if COIN_HAS_PKGCONFIG
	PKG_CONFIG_PATH=@COIN_PKG_CONFIG_PATH@:$(DESTDIR)$(pkgconfiglibdir) \
	$(PKG_CONFIG) --libs rehearse > $(addlibsdir)/rehearse_addlibs.txt
else
if COIN_CXX_IS_CL
	echo "-libpath:`$(CYGPATH_W) @abs_lib_dir@` librehearse.lib @REHEARSE_LIBS_INSTALLED@" > $(addlibsdir)/rehearse_addlibs.txt
else
	echo -L@abs_lib_dir@ -lrehearse @REHEARSE_LIBS_INSTALLED@ > $(addlibsdir)/rehearse_addlibs.txt
endif
endif

uninstall-hook:
	rm -f $(addlibsdir)/rehearse_addlibs.txt

########################################################################
#                         Maintainer Stuff                             #
########################################################################

# Files that are generated and should be cleaned with make distclean
DISTCLEANFILES =

include BuildTools/Makemain.inc
