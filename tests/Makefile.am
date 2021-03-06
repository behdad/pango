## Process this file with automake to create Makefile.in.

EXTRA_DIST = 			\
	boundaries.utf8		\
	GraphemeBreakTest.txt

CLEANFILES = pangorc
DISTCLEANFILES = all-unicode.txt

AM_CPPFLAGS =					\
	-DSRCDIR=\"$(srcdir)\"			\
	-I$(top_srcdir)				\
	$(PANGO_DEBUG_FLAGS)			\
	$(GLIB_CFLAGS)				\
	$(XFT_CFLAGS)				\
	$(CAIRO_CFLAGS)				\
	$(FREETYPE_CFLAGS)			\
	$(X_CFLAGS)

if HAVE_XFT
AM_CPPFLAGS += -DHAVE_X
endif
if HAVE_XFT
AM_CPPFLAGS += -DHAVE_XFT
endif
if HAVE_FREETYPE
AM_CPPFLAGS += -DHAVE_FREETYPE
endif
if HAVE_WIN32
AM_CPPFLAGS += -DHAVE_WIN32
endif


if CROSS_COMPILING
TESTS =
else
TESTS = $(check_PROGRAMS)
endif

TESTS_ENVIRONMENT =                    \
   srcdir=$(srcdir)                    \
   G_TEST_SRCDIR=$(abs_srcdir)         \
   G_TEST_BUILDDIR=$(abs_builddir)     \
   PANGO_RC_FILE=./pangorc

check_PROGRAMS = testboundaries testboundaries_ucd testcolor testscript

if HAVE_CAIRO
check_PROGRAMS += testiter test-pangocairo-threads
endif
if HAVE_FREETYPE
check_PROGRAMS += test-ot-tags
endif

TEST_PANGO_LIBS = ../pango/libpango-$(PANGO_API_VERSION).la
TEST_PANGOFT2_LIBS = $(TEST_PANGO_LIBS) ../pango/libpangoft2-$(PANGO_API_VERSION).la
TEST_PANGOCAIRO_LIBS = $(TEST_PANGO_LIBS) ../pango/libpangocairo-$(PANGO_API_VERSION).la

gen_all_unicode_LDADD = $(GLIB_LIBS)
testboundaries_LDADD = $(TEST_PANGO_LIBS) $(GLIB_LIBS)
testboundaries_ucd_LDADD = $(TEST_PANGO_LIBS) $(GLIB_LIBS)
testcolor_LDADD = $(TEST_PANGO_LIBS) $(GLIB_LIBS)
testiter_LDADD = $(TEST_PANGOCAIRO_LIBS) $(GLIB_LIBS)
testscript_LDADD = $(TEST_PANGO_LIBS) $(GLIB_LIBS)
test_ot_tags_LDADD = $(TEST_PANGOFT2_LIBS) $(GLIB_LIBS)
test_pangocairo_threads_LDADD = $(TEST_PANGOCAIRO_LIBS) $(CAIRO_LIBS) $(GLIB_LIBS)
dump_boundaries_LDADD = $(TEST_PANGO_LIBS) $(GLIB_LIBS)

if HAVE_CXX
check_PROGRAMS += cxx-test
endif
cxx_test_SOURCES = cxx-test.C

all-unicode.txt: gen-all-unicode$(EXEEXT)
	./gen-all-unicode > all-unicode.txt

pangorc: $(srcdir)/../modules/pangorc
	cp $(srcdir)/../modules/pangorc $@

$(noinst_PROGRAMS) $(check_PROGRAMS): pangorc

noinst_PROGRAMS = $(check_PROGRAMS) gen-all-unicode dump-boundaries

if BUILDOPT_INSTALL_TESTS
insttestdir=$(libexecdir)/installed-tests/$(PACKAGE)
insttest_PROGRAMS = $(check_PROGRAMS)

testdatadir = $(insttestdir)
testdata_DATA = 		\
	all-unicode.txt 	\
	boundaries.utf8 	\
	GraphemeBreakTest.txt

testmetadir = $(datadir)/installed-tests/$(PACKAGE)
testmeta_DATA = $(check_PROGRAMS:=.test)

%.test: %$(EXEEXT) Makefile
	$(AM_V_GEN) (echo '[Test]' > $@.tmp; \
	 echo 'Type=session' >> $@.tmp; \
	 echo 'Exec=$(libexecdir)/installed-tests/$(PACKAGE)/$<' >> $@.tmp; \
	 mv $@.tmp $@)
endif

-include $(top_srcdir)/git.mk
