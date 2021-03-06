# Generated automatically from Makefile.in by configure.
# makefile for the CGNS Library
#
# executing as:
#    make
# will build the CGNS library for the system
# defined in make.system
#
# executing as:
#     make SYSTEM=system_name
# will build for system "system_name"

include make.system
include make.$(SYSTEM)

CGNSVER = 2.52

.SUFFIXES :
.SUFFIXES : .c .$(O) $(EXE)

OBJDIR  = $(SYSTEM)
COPTS	= $(CFLAGS) $(CFGFLAGS) -I.
CGNSLIB = $(LIBCGNS)
INSTLIB = libcgns.$(A)

#----------

CGNSOBJS=\
	$(OBJDIR)/adf_cond.$(O) \
	$(OBJDIR)/cgns_error.$(O) \
	$(OBJDIR)/cgns_internals.$(O) \
	$(OBJDIR)/cgnslib.$(O)

# ADF/ADFH routines

ADFOBJS=$(OBJDIR)/ADF_interface.$(O) $(OBJDIR)/ADF_internals.$(O)

# Fortran interface routines
# comment this out if you don't want/need Fortran

F2COBJS = $(OBJDIR)/cg_ftoc.$(O) $(OBJDIR)/adf_ftoc.$(O) $(OBJDIR)/ADF_fortran_2_c.$(O)

#----------

all     : lib
lib     : $(CGNSLIB)

current : update all

$(CGNSLIB) : $(OBJDIR) $(CGNSOBJS) $(ADFOBJS) $(F2COBJS)
	-@$(RM) $@
	$(AR) $(AROUT)$@ $(CGNSOBJS) $(ADFOBJS) $(F2COBJS)
	$(RANLIB) $@

$(OBJDIR) :
	-$(MKDIR) $(OBJDIR)

update :
	cvs update

#----------

clean :
	-$(RM) $(OBJDIR)/*.$(O)

allclean : distclean

distclean : clean
	-cd tests && $(MAKE) allclean
	-$(RM) tests/Makefile
	-$(RM) $(CGNSLIB)
	-$(RMDIR) $(OBJDIR)
	-$(RM) make.system make.defs make.$(SYSTEM) make.$(SYSTEM).orig
	-$(RM) config.log config.status Makefile

install : $(CGNSLIB)
	$(INSTALL_DATA) cgnslib.h $(INCLUDEDIR)/cgnslib.h
	$(INSTALL_DATA) cgnslib_f.h $(INCLUDEDIR)/cgnslib_f.h
	$(INSTALL_DATA) cgnswin_f.h $(INCLUDEDIR)/cgnswin_f.h
	$(INSTALL_DATA) $(CGNSLIB) $(LIBDIR)/$(INSTLIB)

uninstall :
	-$(RM) $(INCLUDEDIR)/cgnslib.h
	-$(RM) $(INCLUDEDIR)/cgnslib_f.h
	-$(RM) $(INCLUDEDIR)/cgnswin_f.h
	-$(RM) $(LIBDIR)/$(INSTLIB)

#---------- mid-level library

$(OBJDIR)/adf_cond.$(O) : adf_cond.c $(ADFDIR)/ADF.h
	$(CC) $(COPTS) -I$(ADFDIR) $(COOUT)$@ -c adf_cond.c

$(OBJDIR)/cgns_error.$(O) : cgns_error.c cgnslib.h cgns_header.h \
	$(ADFDIR)/ADF.h
	$(CC) $(COPTS) -I$(ADFDIR) $(COOUT)$@ -c cgns_error.c

$(OBJDIR)/cgns_internals.$(O) : cgns_internals.c cgnslib.h cgns_header.h \
	$(ADFDIR)/ADF.h
	$(CC) $(COPTS) -I$(ADFDIR) $(COOUT)$@ -c cgns_internals.c

$(OBJDIR)/cgnslib.$(O) : cgnslib.c cgnslib.h cgns_header.h \
	$(ADFDIR)/ADF.h
	$(CC) $(COPTS) -I$(ADFDIR) $(COOUT)$@ -c cgnslib.c

$(OBJDIR)/cg_ftoc.$(O) : cg_ftoc.c fortran_macros.h cgnslib.h cgns_header.h \
	$(ADFDIR)/ADF.h
	$(CC) $(COPTS) -I$(ADFDIR) $(F2CFLAGS) $(COOUT)$@ -c cg_ftoc.c

#---------- ADF

$(OBJDIR)/adf_ftoc.$(O) : adf_ftoc.c fortran_macros.h \
	adf/ADF.h adf/ADF_internals.h
	$(CC) $(COPTS) -Iadf $(F2CFLAGS) $(COOUT)$@ -c adf_ftoc.c

$(OBJDIR)/ADF_interface.$(O) : adf/ADF_interface.c \
	adf/ADF.h adf/ADF_internals.h
	$(CC) $(COPTS) -Iadf $(COOUT)$@ -c adf/ADF_interface.c

$(OBJDIR)/ADF_internals.$(O) : adf/ADF_internals.c \
	adf/ADF.h adf/ADF_internals.h
	$(CC) $(COPTS) -Iadf $(COOUT)$@ -c adf/ADF_internals.c

$(OBJDIR)/ADF_fortran_2_c.$(O) : adf/ADF_fortran_2_c.c \
	adf/ADF.h adf/ADF_internals.h
	$(CC) $(COPTS) -Iadf $(F2CFLAGS) $(COOUT)$@ -c adf/ADF_fortran_2_c.c

#---------- HDF5

$(OBJDIR)/ADFH.$(O) : adfh/ADFH.c adfh/ADFH.h
	$(CC) $(COPTS) -Iadfh $(HDF5INC) $(COOUT)$@ -c adfh/ADFH.c

$(OBJDIR)/ADFH_ftoc.$(O) : adfh/ADFH_ftoc.c adfh/ADFH.h
	$(CC) $(COPTS) -Iadfh $(HDF5INC) $(F2CFLAGS) $(COOUT)$@ -c adfh/ADFH_ftoc.c

#---------- ADF <-> HDF5 convertors

CNVOBJS = $(OBJDIR)/ADF_interface.$(O) \
	  $(OBJDIR)/ADF_internals.$(O) \
	  $(OBJDIR)/ADFH.$(O)

$(OBJDIR)/adf2hdf$(EXE) : $(OBJDIR) $(OBJDIR)/adf2hdf.$(O) $(CNVOBJS)
	$(CC) $(CFLAGS) $(CEOUT)$@ $(OBJDIR)/adf2hdf.$(O) $(CNVOBJS) \
	$(HDF5LIB) $(SZIPLIB) $(ZLIBLIB) $(CLIBS)
	$(STRIP) $@

$(OBJDIR)/adf2hdf.$(O) : adfh/adf2hdf.c adfh/ADFH.h adf/ADF.h
	$(CC) $(COPTS) -Iadfh $(HDF5INC) $(COOUT)$@ -c adfh/adf2hdf.c

$(OBJDIR)/hdf2adf$(EXE) : $(OBJDIR) $(OBJDIR)/hdf2adf.$(O) $(CNVOBJS)
	$(CC) $(CFLAGS) $(CEOUT)$@ $(OBJDIR)/hdf2adf.$(O) $(CNVOBJS) \
	$(HDF5LIB) $(SZIPLIB) $(ZLIBLIB) $(CLIBS)
	$(STRIP) $@

$(OBJDIR)/hdf2adf.$(O) : adfh/hdf2adf.c adfh/ADFH.h adf/ADF.h
	$(CC) $(COPTS) -Iadfh $(HDF5INC) $(COOUT)$@ -c adfh/hdf2adf.c

