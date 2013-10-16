## Process this file with automake to create Makefile.in

## Makefile for Automake.

# Copyright (C) 1995-2013 Free Software Foundation, Inc.

# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2, or (at your option)
# any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

## Might be updated later.
CLEANFILES =
DISTCLEANFILES =
EXTRA_DIST =
TAGS_FILES =
dist_noinst_DATA =
nodist_noinst_DATA =
dist_noinst_SCRIPTS =
nodist_noinst_SCRIPTS =

## ------------ ##
##  Top level.  ##
## ------------ ##

EXTRA_DIST += \
  bootstrap.sh \
  GNUmakefile \
  HACKING \
  PLANS

# We want a handful of substitutions to be fully-expanded by make;
# then use config.status to substitute the remainder where a single
# expansion is sufficient.  We use a funny notation here to avoid
# configure substitutions in our text.
do_subst = ( sed \
  -e "s,[@]configure_input[@],Generated from $$in; do not edit by hand.,g" \
  -e 's,[@]datadir[@],$(datadir),g' \
  -e 's,[@]amdir[@],$(amdir),g' \
  -e 's,[@]bindir[@],$(bindir),g' \
  -e 's,[@]docdir[@],$(docdir),g' \
  -e 's,[@]pkgvdatadir[@],$(pkgvdatadir),g' \
  -e 's,[@]scriptdir[@],$(scriptdir),g' \
  -e 's,[@]automake_acdir[@],$(automake_acdir),g' \
  -e 's,[@]system_acdir[@],$(system_acdir),g' \
## Hack to avoid a spurious substitution in the Automake script (part 1).
  -e 's,[@]am__isrc[@],!!@!!am__isrc!!@!!,g' \
  | $(SHELL) ./config.status --file=- \
## Hack to avoid a spurious substitution in the Automake script (part 2).
  | sed -e 's,!!@!!am__isrc!!@!!,@''am__isrc@,g' \
  )

# Generated  files shouldn't contain unexpanded '@substitutions@', and
# should be made read-only, to prevent them from being edited by mistake
# instead of the file the are generated from.
generated_file_finalize = $(AM_V_at) \
  if LC_ALL=C grep '@[a-zA-Z0-9_][a-zA-Z0-9_]*@' $@-t; then \
    echo "$@ contains unexpanded substitution (see lines above)"; \
    exit 1; \
  fi; \
  chmod a-w $@-t && mv -f $@-t $@

# For some tests or targets, we need to have the just-build automake and
# aclocal scripts avaiable on PATH.
extend_PATH = \
  { PATH='$(abs_builddir)/t/wrap'$(PATH_SEPARATOR)$$PATH && export PATH; }

# The master location for INSTALL is lib/INSTALL.
# This is where "make fetch" will install new versions.
# Make sure we also update this copy.
INSTALL: lib/INSTALL
	$(AM_V_GEN)cp $(srcdir)/lib/INSTALL $@

# We don't use the default name for the autom4te cache directory,
# so we need this.
maintainer-clean-local:
	rm -rf .autom4te.cache

# So that automake won't complain about the missing ChangeLog.
# The real rule for ChangeLog generation is now in maintainer/maint.mk
# (as it is maintainer-specific).
ChangeLog:

# Third-party, obsolescent or experimental stuff.
EXTRA_DIST += \
  contrib/check-html.am \
  contrib/multilib/README \
  contrib/multilib/config-ml.in \
  contrib/multilib/symlink-tree \
  contrib/multilib/multilib.am \
  contrib/multilib/multi.m4 \
  contrib/README

# Older files, kept mostly for historical interest.
EXTRA_DIST += \
  old/ChangeLog-tests \
  old/ChangeLog.96 \
  old/ChangeLog.98 \
  old/ChangeLog.00 \
  old/ChangeLog.01 \
  old/ChangeLog.02 \
  old/ChangeLog.03 \
  old/ChangeLog.04 \
  old/ChangeLog.09 \
  old/ChangeLog.11 \
  old/TODO

# Maintainer-specific files and scripts.
EXTRA_DIST += \
  maintainer/am-ft \
  maintainer/am-xft \
  maintainer/rename-tests \
  maintainer/maint.mk \
  maintainer/syntax-checks.mk

# Most work delegated to sub-dir makefile fragments.
include $(srcdir)/bin/Makefile.inc
include $(srcdir)/doc/Makefile.inc
include $(srcdir)/lib/Makefile.inc
include $(srcdir)/lib/Automake/Makefile.inc
include $(srcdir)/lib/am/Makefile.inc
include $(srcdir)/m4/Makefile.inc
include $(srcdir)/t/Makefile.inc

## Starlink: We keep a few generated files in the repository, because
## this component is built at a time when the autotools aren't
## available (obviously).  Unfortunately, this leaves us prey to
## timestamp problems: checked out files are given a timestamp of
## their checkin time, and this can lead to problems where things are
## spuriously rebuilt.  So make the checkout-fixup target early on in
## the build process, to force these timestamps into the correct order
## in those few cases where it's necessary.  If the relevant source
## files are genuinely subsequently modified, then a make before
## commit will ensure they have the correct contents.
BUILT_SOURCES = checkout-fixup

checkout-fixup:
	@echo "Fixing up post-checkout timestamps"
## Doing all of these is slight overkill, since the README in the
## parent directory advises that these generated files should be
## committed separately from the sources, but it at least does no
## harm, and means we don't lose if someone forgets to do that.
	for f in \
## Generated by aclocal
	  aclocal.m4 \
## Generated by autoconf
	  configure \
## Generated by automake
	  Makefile.in m4/Makefile.in doc/Makefile.in \
	  lib/am/Makefile.in lib/Makefile.in lib/Automake/tests/Makefile.in \
	  lib/Automake/Makefile.in tests/Makefile.in \
## doc/automake.info is the important one, but we also have to touch
## doc/version.texi and doc/stamp-vti, to make sure they're newer than
## ./configure, touched above.
	  doc/stamp-vti doc/version.texi doc/automake.info \
	; do \
	    touch $$f; \
	  done
	date >checkout-fixup

MAINTAINERCLEANFILES = checkout-fixup

# vim: ft=automake noet
