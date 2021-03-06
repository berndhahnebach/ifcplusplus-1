## Process this file with automake to generate Makefile.in.
## 19990904 mortene.

RegularSources = \
	error.cpp \
	debugerror.cpp \
	SoDebugError.cpp \
	SoError.cpp \
	SoMemoryError.cpp \
	SoReadError.cpp
LinkHackSources = \
	all-errors-cpp.cpp
PublicHeaders =
PrivateHeaders =
ObsoleteHeaders =

##$ BEGIN TEMPLATE Make-Common(errors, errors)
# **************************************************************************
# This template depends on the following variables having been set up before
# its invokation.
#
# PublicHeaders    - header files that should be installed
# PrivateHeaders   - source files that should be distributed, but not
#                    installed - including source-included source files.
# RegularSources   - source files compiled in this directory
# LinkHackSources  - alternative source files used on -enable-compact compilation
# ObsoletedHeaders - header files that should be removed upon installation

# Everything below is autogenerated from a macro, so don't modify
# within the Makefile.am itself. 19991201 mortene.

DISTCLEANFILES = \
	ii_files/*.ii

if HACKING_COMPACT_BUILD
BuildSources = $(LinkHackSources)
ExtraSources = $(RegularSources)
else
BuildSources = $(RegularSources)
ExtraSources = $(LinkHackSources)
endif

if BUILD_WITH_MSVC
noinst_LIBRARIES = errors.lst
else
if HACKING_DYNAMIC_MODULES
lib_LTLIBRARIES = liberrors@SUFFIX@LINKHACK.la
else
noinst_LTLIBRARIES = liberrors.la
endif
endif

# Remove any linklibraries detected by configure, as they are not
# needed in the submodules -- only in the toplevel libCoin build.
LIBS = 

DEFAULT_INCLUDES = -I$(top_builddir)/include -I$(top_srcdir)/include $(ExtraIncludeDirs) -I$(top_builddir)/src -I$(top_srcdir)/src

# header installation
if MAC_FRAMEWORK
liberrorsincdir = $(MAC_FRAMEWORK_PREFIX)/$(MAC_FRAMEWORK_NAME).framework/Versions/$(MAC_FRAMEWORK_VERSION)/Headers/errors
# hack to change libdir value when installing Mac OS X compiler framework
lib@nop@dir = $(MAC_FRAMEWORK_PREFIX)/$(MAC_FRAMEWORK_NAME).framework/Versions/$(MAC_FRAMEWORK_VERSION)/Libraries
else
liberrorsincdir = $(includedir)/Inventor/errors
endif
liberrorsinc_HEADERS = $(PublicHeaders)

liberrors_la_SOURCES = $(BuildSources)
EXTRA_liberrors_la_SOURCES = \
	$(PublicHeaders) $(PrivateHeaders) $(ExtraSources)

liberrors@SUFFIX@LINKHACK_la_SOURCES = $(BuildSources)
EXTRA_liberrors@SUFFIX@LINKHACK_la_SOURCES = \
	$(PublicHeaders) $(PrivateHeaders) $(ExtraSources)

errors_lst_SOURCES = $(BuildSources)
EXTRA_errors_lst_SOURCES = \
	$(PublicHeaders) $(PrivateHeaders) $(ExtraSources)

# this rule causes a make warning, so we remove it when it is not in use
errors.lst: Makefile $(errors_lst_OBJECTS)
	@echo "Linking errors.lst..."; \
	rm -f $@; touch $@; \
	for i in $(errors_lst_OBJECTS); do echo $$i >>$@; done

install-liberrorsincHEADERS: $(liberrorsinc_HEADERS)
	@$(NORMAL_INSTALL)
	@if test x"$(liberrorsinc_HEADERS)" = x""; then :; else \
	  echo " $(mkinstalldirs) $(DESTDIR)$(liberrorsincdir)"; \
	  $(mkinstalldirs) $(DESTDIR)$(liberrorsincdir); \
	fi
	@list='$(liberrorsinc_HEADERS)'; for p in $$list; do \
	  if test -f "$$p"; then d=; else d="$(srcdir)/"; fi; \
	  f="`echo $$p | sed -e 's|^.*/||'`"; \
	  src="$$d$$p"; dst="$(DESTDIR)$(liberrorsincdir)/$$f"; \
	  if cmp -s "$$src" "$$dst"; then :; else \
	    echo " $(INSTALL_HEADER) $$src $$dst"; \
	    $(INSTALL_HEADER) "$$src" "$$dst" || exit 1; \
	  fi \
	done
	@list='$(ObsoletedHeaders)'; for f in $$list; do \
	  file="$(DESTDIR)$(liberrorsincdir)/$$f"; \
	  if test -x "$$file"; then \
	    echo " rm -f $$file"; \
	    rm -f "$$file"; \
	  fi \
	done; \
	:

if MACOSX
# When using --enable-hacking, the user tends to manually replace some 
# of the installed libraries by symlinks back to the build directory. 
# But Mac OS X's install program refuses to overwrite symlinks => we 
# have to explicitly remove existing symlinks before installation.
install-exec-local:
	@for file in $(DESTDIR)$(lib@nop@dir)/liberrors@SUFFIX@LINKHACK*.dylib; do \
	  if test -L "$$file"; then \
	    rm -f "$$file"; \
	  fi \
	done; 
else 
install-exec-local:
endif

install-symlinks:
	@echo "Creating symlink to liberrors@SUFFIX@LINKHACK*@LIBEXT@*"; \
        for dir in $(SUBDIRS) ; do \
          (cd $$dir; $(MAKE) install-symlinks;) \
        done ; \
        here=`pwd`; \
	(cd $(DESTDIR)$(lib@nop@dir); \
	for file in liberrors@SUFFIX@LINKHACK*@LIBEXT@*; do \
	  ln -sf "$$here/.libs/$$file" "$$file"; \
	done; \
        cd -; )
##$ END TEMPLATE Make-Common
