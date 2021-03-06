
#  Copyright (C) 2007  Stanislav Sinyagin
#
#  This program is free software; you can redistribute it and/or modify
#  it under the terms of the GNU General Public License as published by
#  the Free Software Foundation; either version 2 of the License, or
#  (at your option) any later version.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#  GNU General Public License for more details.
#
#  You should have received a copy of the GNU General Public License
#  along with this program; if not, write to the Free Software
#  Foundation, Inc., 675 Mass Ave, Cambridge, MA 02139, USA.

# Stanislav Sinyagin <ssinyagin@yahoo.com>
#

# Torrus plugin Automake script

SUBST = @abs_top_builddir@/substvars.sh

noinst_SCRIPTS = substvars.sh
EXTRA_DIST = siam-config.pl siam-siteconfig.pl


#####   Perl modules  #####
myperlmoddir = $(perllibdir)/Torrus
dist_myperlmod_DATA = SIAM.pm SIAMLogger.pm

myperlmoddiscdir = $(perllibdir)/Torrus/DevDiscover
dist_myperlmoddisc_DATA = SIAMDD.pm

myperlmodsiamdddir = $(perllibdir)/Torrus/DevDiscover/SIAMDD
dist_myperlmodsiamdd_DATA = \
	SIAMDD/IFMIB.pm \
	SIAMDD/APC_PowerNet.pm


#####   CLI wrapper  #####
wrapperdir = $(plugwrapperdir)
dist_wrapper_DATA = siam-wrapper.sh


#####   Executable scripts  #####
pkgbindir = @pkgbindir@
pkgbin_SCRIPTS = siam_rrdfetch siam_genddx siam_genxml

siam_rrdfetch: siam_rrdfetch.in
	$(SUBST) siam_rrdfetch.in > siam_rrdfetch

siam_genddx: siam_genddx.in
	$(SUBST) siam_genddx.in > siam_genddx

siam_genxml: siam_genxml.in
	$(SUBST) siam_genxml.in > siam_genxml

#####   Documentation files   #####
pkgdocdir  = @pkgdocdir@
dist_pkgdoc_DATA = tp-siam.txt tp-siam.pod.in 
SUFFIXES = .pod.in .pod .txt
.pod.in.pod:
	$(SUBST) $<  > $@
.pod.txt:
	pod2text $< $@ 


install-data-local:
	for f in siam-config.pl; do \
	  $(SUBST) $$f > \
		$(DESTDIR)$(cfgdefdir)/$$f; \
	done
	for f in siam-ddconfig.pl; do \
	  $(SUBST) $$f > \
		$(DESTDIR)$(plugdevdisccfgdir)/$$f; \
	done
	for f in siam-siteconfig.pl; do \
	  if test ! -r $(DESTDIR)$(siteconfdir)/$$f; then \
	    $(INSTALL_DATA) $$f $(DESTDIR)$(siteconfdir)/$$f; \
	  fi \
	done



#####   What you do not need after "make clean"  #####
CLEANFILES = $(pkgbin_SCRIPTS)

htdocs: tp-siam.pod
	for f in *.pod; do \
	  pod2html -css=../torrusdoc.css $$f > ../../htdocs/plugins/$$f.html; \
	done

UPLOADPATH = \
   ssinyagin,torrus@web.sourceforge.net:/home/groups/t/to/torrus/htdocs/devel

upload: dist
	scp $(distdir).tar.gz $(UPLOADPATH)
