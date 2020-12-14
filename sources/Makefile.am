RAW_API = $(ASSEMBLY_NAME)-api.raw
API = $(ASSEMBLY_NAME)-api.xml
METADATA = $(ASSEMBLY_NAME).metadata
DLL = $(top_srcdir)/out/$(ASSEMBLY_NAME).dll
DLLMAP = $(top_srcdir)/out/$(ASSEMBLY_NAME).dll.config

pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = $(PCFILE)

sources =

build_sources = AssemblyInfo.cs $(sources)

CLEANFILES = $(DLL) generated-stamp generated/*/*.cs $(API)

DISTCLEANFILES = AssemblyInfo.cs $(DLLMAP)

noinst_DATA = $(DLL)

EXTRA_DIST = \
	$(RAW_API) \
	$(sources) \
	$(METADATA) \
	AssemblyInfo.cs.in \
	$(ASSEMBLY_NAME).snk

all: generated-stamp $(ASSEMBLY)


GTK_DOWNLOADS = \
	http://ftp.gnome.org/pub/GNOME/sources/gdl/3.34/gdl-3.34.0.tar.xz

get-source-code:
	for i in $(GTK_DOWNLOADS); do                  \
		wget $$i --output-document=- | tar -xJ ;   \
	done;

api:
	gapi3-parser sources.xml

$(API): $(srcdir)/$(RAW_API) $(srcdir)/$(METADATA)
	cp $(srcdir)/$(RAW_API) $(API)
	chmod u+w $(API)
	$(GAPI_FIXUP) --api=$(API) --metadata=$(srcdir)/$(METADATA)

generated-stamp: $(API)
	 $(GAPI_CODEGEN) --generate $(API) $(GTK_SHARP_CFLAGS) \
		--outdir=generated \
	 	--assembly-name=$(ASSEMBLY_NAME) && touch generated-stamp

$(DLL): $(build_sources) generated-stamp
	$(CSC) -unsafe -target:library $(GTK_SHARP_LIBS) $(build_sources) generated/*/*.cs -out:$(DLL)

install-data-local:
	install -d -m 755 $(DESTDIR)$(libdir)
	install -m 644 $(ASSEMBLY) $(DESTDIR)$(libdir)/

uninstall-local:
	-rm $(DESTDIR)$(libdir)/$(ASSEMBLY)
