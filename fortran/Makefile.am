## This is an automake file, part of Unidata's netCDF package.
# Copyright 2005-2006, see the COPYRIGHT file for more information.

# This file controls the building of the fortran 90 API.

# Ed Hartnett, Russ Rew, Dennis Heimbigner, Ward Fisher

# Turn off parallel builds in this directory.
.NOTPARALLEL:

libnetcdff_la_LDFLAGS = -version-number 6:1:1

# Find the netcdf.inc file.
AM_CPPFLAGS = -I${top_builddir}/libsrc

# This, plus either netcdf4.f90 or netcdf.f90, is the main source.
libnetcdff_la_SOURCES = typeSizes.f90

if USE_NETCDF4
NETCDF_O = netcdf4.o
else
NETCDF_O = netcdf.o
endif

# Add dependencies for source files included in "meta" source files,
# so the meta source is rebuilt properly when an included file is
# modified.
netcdf.$(OBJEXT): netcdf_constants.f90 netcdf_externals.f90 netcdf_overloads.f90 \
        netcdf_visibility.f90 netcdf_file.f90 netcdf3_file.f90 netcdf_dims.f90 \
        netcdf_attributes.f90 netcdf_variables.f90 netcdf_text_variables.f90 \
        netcdf_expanded.f90 netcdf_eightbyte.f90

netcdf4.$(OBJEXT): netcdf_constants.f90 netcdf_externals.f90 netcdf4_externals.f90 \
        netcdf_overloads.f90 netcdf_visibility.f90 netcdf4_visibility.f90 \
        netcdf_file.f90 netcdf4_file.f90 netcdf_dims.f90 netcdf_attributes.f90 \
        netcdf4_variables.f90 netcdf_text_variables.f90 netcdf_expanded.f90 \
        netcdf4_eightbyte.f90 netcdf4_func.f90 netcdf4_overloads.f90

libnetcdff_la_SOURCES += module_netcdf_nc_data.F90			\
module_netcdf_nc_interfaces.f90 module_netcdf_nf_data.F90		\
module_netcdf_nf_interfaces.F90 module_netcdf_f03.f90 nf_attio.F90	\
nf_control.F90 nf_dim.f90 nf_misc.f90 nf_genatt.f90 nf_geninq.f90	\
nf_genvar.f90 nf_vario.F90 nf_var1io.F90 nf_varaio.F90 nf_varmio.F90	\
nf_varsio.F90

if USE_NETCDF4
# Use netcdf4.f90 to build the library.
libnetcdff_la_SOURCES += module_netcdf4_nc_interfaces.f90		\
module_netcdf4_nf_interfaces.F90 module_netcdf4_f03.f90 nf_lib.c	\
nf_nc4.f90
endif USE_NETCDF4

if USE_LOGGING
# add nf_logging to build
libnetcdff_la_SOURCES += nf_logging.F90
endif USE_LOGGING

# The file netcdf.f90 includes all of these.
libnetcdff_la_DEPENDENCIES = netcdf_attributes.f90			\
netcdf_constants.f90 netcdf_dims.f90 netcdf_expanded.f90		\
netcdf_externals.f90 netcdf_file.f90 netcdf_overloads.f90		\
netcdf_text_variables.f90 netcdf_variables.f90 netcdf_visibility.f90	\
netcdf_eightbyte.f90

if USE_NETCDF4
libnetcdff_la_SOURCES += netcdf4.f90
# These are the extra netCDF-4 F90 source files.
libnetcdff_la_DEPENDENCIES += netcdf4_func.f90 netcdf4_externals.f90	\
netcdf4_visibility.f90 netcdf4.f90 netcdf4_eightbyte.f90		\
netcdf4_variables.f90 netcdf4_file.f90 netcdf4_overloads.f90
else !USE_NETCDF4
libnetcdff_la_SOURCES += netcdf.f90 netcdf3_file.f90
endif !USE_NETCDF4

if BUILD_V2
libnetcdff_la_SOURCES += nf_v2compat.c			\
module_netcdf_fortv2_c_interfaces.f90 nf_fortv2.f90
endif

# Tell make to rebuld fortran library if netcdf.inc has changed
libnetcdff_la_DEPENDENCIES += netcdf.inc

# Some fortran compilers change the file names of .mod files to all
# uppercase. Just to be special.

LCHEADERS = netcdf_nc_data.mod netcdf_nc_interfaces.mod	\
netcdf_nf_data.mod netcdf_nf_interfaces.mod netcdf_f03.mod

if BUILD_V2
LCHEADERS += netcdf_fortv2_c_interfaces.mod
endif BUILD_V2

if USE_NETCDF4
LCHEADERS += netcdf4_nc_interfaces.mod netcdf4_nf_interfaces.mod	\
netcdf4_f03.mod
endif USE_NETCDF4

netcdf_nc_data.mod: module_netcdf_nc_data.o
netcdf_nc_interfaces.mod: module_netcdf_nc_interfaces.o
netcdf_nf_data.mod: module_netcdf_nf_data.o
netcdf_nf_interfaces.mod: module_netcdf_nf_interfaces.o
netcdf_f03.mod: module_netcdf_f03.o
if BUILD_V2
netcdf_fortv2_c_interfaces.mod: module_netcdf_fortv2_c_interfaces.o
endif BUILD_V2

if USE_NETCDF4
netcdf4_nc_interfaces.mod: module_netcdf4_nc_interfaces.o
netcdf4_nf_interfaces.mod: module_netcdf4_nf_interfaces.o
netcdf4_f03.mod: module_netcdf4_f03.o
endif USE_NETCDF4

# These are always used
LCCOMMON = typesizes.mod netcdf.mod
netcdf.mod: $(NETCDF_O)
typesizes.mod: typeSizes.o

COMMONHEADERS = $(LCHEADERS) $(LCCOMMON)
BUILT_SOURCES = $(LCHEADERS) $(LCCOMMON)

nodist_include_HEADERS = $(COMMONHEADERS)
libnetcdff_la_DEPENDENCIES += $(COMMONHEADERS)

# This is what we are building: a convenience library of Fortran 2003
# functions.
lib_LTLIBRARIES = libnetcdff.la

nodist_include_HEADERS += netcdf.inc

BUILT_SOURCES += netcdf.inc

# Build netcdf.inc file from netcdf2, netcdf3 and netcdf4 files
netcdf.inc: $(top_srcdir)/fortran/netcdf2.inc $(top_srcdir)/fortran/netcdf3.inc $(top_srcdir)/fortran/netcdf4.inc
	echo '!     NetCDF-3.' > netcdf.inc
	cat $(top_srcdir)/fortran/netcdf3.inc >> netcdf.inc
if USE_NETCDF4
	echo >> netcdf.inc
	echo '!     NetCDF-4.' >> netcdf.inc
	cat $(top_srcdir)/fortran/netcdf4.inc >> netcdf.inc
endif
if BUILD_V2
	echo >> netcdf.inc
	echo '!     NetCDF-2.' >> netcdf.inc
	cat $(top_srcdir)/fortran/netcdf2.inc >> netcdf.inc
endif
if USE_LOGGING
	echo >> netcdf.inc
	echo '!     This is to turn on netCDF internal logging.' >> netcdf.inc
	echo '      integer nf_set_log_level' >> netcdf.inc
	echo '      external nf_set_log_level' >> netcdf.inc
endif

# All of the _DEPENDENCIES must also be in EXTRA_DIST
EXTRA_DIST = netcdf_attributes.f90 netcdf_constants.f90			\
netcdf_dims.f90 netcdf_expanded.f90 netcdf_externals.f90		\
netcdf_file.f90 netcdf_overloads.f90 netcdf_text_variables.f90		\
netcdf_variables.f90 netcdf_visibility.f90 netcdf_eightbyte.f90		\
netcdf4_func.f90 netcdf4_externals.f90 netcdf4_visibility.f90		\
netcdf4.f90 netcdf4_eightbyte.f90 netcdf4_overloads.f90			\
netcdf4_variables.f90 netcdf4_file.f90 netcdf.inc CMakeLists.txt	\
netcdf2.inc netcdf3.inc netcdf4.inc

CLEANFILES = netcdf_nc_data.mod netcdf_nc_interfaces.mod		\
netcdf_nf_data.mod netcdf_nf_interfaces.mod				\
netcdf_fortv2_c_interfaces.mod netcdf_f03.mod				\
netcdf4_nc_interfaces.mod netcdf4_nf_interfaces.mod netcdf4_f03.mod	\
netcdf.inc $(LCHEADERS) $(LCCOMMON)
