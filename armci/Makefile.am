##############################################################################
# Makefile.am for ARMCI.
#
# Rationale:
# This Makefile.am follows many of the suggestions outlined in the paper
# "Recursive Make Considered Harmful".  We do not use Automake's
# 'include' feature (instead preferring a single, large Makefile.am).
#
# Additional targets:
# Besides the traditional make targets supplied by Automake, we have added the
# "examples" and "checkprogs" targets to build example programs and test
# programs, respectively.
#
# Notes:
# In general, each subdirectory has a corresponding section of this
# Makefile.am with the notable exception being the many examples getting
# rolled up into the examples section.
#

# The usual aclocal nonsense to get include paths right.
ACLOCAL_AMFLAGS = -I m4 -I ../m4

# All public headers, installed programs, test programs, and example programs
# are listed in these variables.  Appended to throughout.  These are the
# automake variables used.
include_HEADERS =
bin_PROGRAMS =
check_PROGRAMS =
check_LTLIBRARIES =
lib_LTLIBRARIES = libarmci.la
noinst_LTLIBRARIES = libarmcii.la
libarmci_la_SOURCES =
nodist_libarmci_la_SOURCES =
libarmci_la_LIBADD =
libarmcii_la_LIBADD =
libarmcii_la_SOURCES = $(libarmci_la_SOURCES)
nodist_libarmcii_la_SOURCES = $(nodist_libarmci_la_SOURCES)
EXTRA_DIST = README
BUILT_SOURCES =
MOSTLYCLEANFILES =
CLEANFILES =
DISTCLEANFILES =
MAINTAINERCLEANFILES =
AM_FFLAGS =
AM_CFLAGS =
AM_CXXFLAGS =
AM_CPPFLAGS =
AM_LDFLAGS =
LDADD =

AM_FFLAGS += $(ARMCI_FOPT)
AM_FFLAGS += $(GA_F_WARN)
AM_FFLAGS += $(FFLAG_INT)
AM_CFLAGS += $(ARMCI_COPT)
AM_CFLAGS += $(GA_C_WARN)
AM_CXXFLAGS += $(ARMCI_CXXOPT)
AM_CXXFLAGS += $(GA_CXX_WARN)
AM_CPPFLAGS += $(GA_MP_CPPFLAGS)
AM_CPPFLAGS += $(ARMCI_NETWORK_CPPFLAGS)
if MSG_COMMS_TCGMSG
AM_CPPFLAGS += -I$(top_srcdir)/tcgmsg
endif
LDFLAGS_BASE =
LDFLAGS_BASE += $(GA_MP_LDFLAGS)
LDFLAGS_BASE += $(ARMCI_NETWORK_LDFLAGS)
AM_LDFLAGS += $(LDFLAGS_BASE)
if ENABLE_F77
AM_LDFLAGS += $(FLD_NOMAIN)
endif
LDADD += libarmci.la
LDADD += $(LTLIBOBJS)
LDADD += $(GA_MP_LIBS)
LDADD += $(ARMCI_NETWORK_LIBS)

libarmci_la_LIBADD += $(GA_MP_LIBS)
libarmci_la_LIBADD += $(ARMCI_NETWORK_LIBS)

# Certain trickery when turning Fortran support on or off.
if ENABLE_F77
MAYBE_DUMMY_F_LINK = dummy.f
MAYBE_FLIBS = $(FLIBS)
endif

##############################################################################
# compiler and linker flags
#
# Important for external tools wanting to know how to link to ARMCI.
SED_NORMALIZE_WHITESPACE = $(SED) 's/ [ ]*/ /g;s/" /"/g;s/ "/"/g'
.PHONY: flags
flags:
	@echo ' =========================================================================== '
	@echo ' Suggested compiler/linker options are as follows.'
	@echo ' ARMCI libraries are installed in $(libdir)' 
	@echo ' ARMCI headers are installed in $(includedir)' 
	@echo ' '
	@echo ' CPPFLAGS="$(GA_MP_CPPFLAGS) $(ARMCI_NETWORK_CPPFLAGS) -I$(includedir)"' | $(SED_NORMALIZE_WHITESPACE)
	@echo ' '
	@echo ' LDFLAGS="$(GA_MP_LDFLAGS) $(ARMCI_NETWORK_LDFLAGS) -L$(libdir)"' | $(SED_NORMALIZE_WHITESPACE)
	@echo ' '
	@echo ' For C/C++ Programs: '
	@echo ' LIBS="-larmci $(GA_MP_LIBS) $(ARMCI_NETWORK_LIBS) $(MAYBE_FLIBS)"' | $(SED_NORMALIZE_WHITESPACE)
	@echo ' =========================================================================== '

##############################################################################
# config
#
# The config.h file is automatically generated from config.h.in at configure
# time.  However, it contains symbols which certain Fortran compilers do not
# understand.  Therefore, we create a config.fh file which is Fortran-specific
# and works with all Fortran compilers.
#
BUILT_SOURCES += config.fh
CLEANFILES += config.fh
nodist_libarmci_la_SOURCES += config.fh

config.fh: config.h
	@-rm -f config.fh
	$(SED_V) \
	$(SED) "/^#/!d" config.h > config.fh

##############################################################################
# gaf2c
#
libarmci_la_SOURCES += gaf2c/gaf2c.c
libarmci_la_SOURCES += gaf2c/drand.c
if ENABLE_F77
libarmci_la_SOURCES += gaf2c/farg.F
endif

include_HEADERS += gaf2c/typesf2c.h
include_HEADERS += gaf2c/farg.h

AM_CPPFLAGS += -I$(top_build_prefix)gaf2c
AM_CPPFLAGS += -I$(top_srcdir)/gaf2c

if ENABLE_F77
check_PROGRAMS += gaf2c/testarg
endif

GAF2C_SERIAL_TESTS =
GAF2C_SERIAL_TESTS_XFAIL =
GAF2C_PARALLEL_TESTS =
GAF2C_PARALLEL_TESTS_XFAIL =
GAF2C_TESTS = $(GAF2C_SERIAL_TESTS) $(GAF2C_PARALLEL_TESTS)
GAF2C_TESTS_XFAIL = $(GAF2C_SERIAL_TESTS_XFAIL) $(GAF2C_PARALLEL_TESTS_XFAIL)

if ENABLE_F77
GAF2C_SERIAL_TESTS += gaf2c/testarg$(EXEEXT)
endif

gaf2c_testarg_SOURCES = gaf2c/testarg.c gaf2c/testargf.f
gaf2c_testarg_LDFLAGS = $(FLDFLAGS)

##############################################################################
# tcgmsg
#
include $(top_srcdir)/tcgmsg/Makefile.inc

##############################################################################
# src-portals
#
if ARMCI_SRC_DIR_PORTALS
include $(top_srcdir)/src-portals/Makefile.inc
endif

##############################################################################
# src-dmapp
#
if ARMCI_SRC_DIR_DMAPP
include $(top_srcdir)/src-dmapp/Makefile.inc
endif

##############################################################################
# src-openib
#
if ARMCI_SRC_DIR_OPENIB
include $(top_srcdir)/src-openib/Makefile.inc
endif

##############################################################################
# src-mpi
#
if ARMCI_SRC_DIR_MPI
include $(top_srcdir)/src-mpi/Makefile.inc
endif

##############################################################################
# src
#
if ARMCI_SRC_DIR_SRC
include $(top_srcdir)/src/Makefile.inc
endif

##############################################################################
# profiling
#
if ENABLE_PROFILING
if HAVE_SYS_WEAK_ALIAS_PRAGMA
lib_LTLIBRARIES += libarmci_prof.la
libarmci_prof_la_SOURCES = 
libarmci_prof_la_SOURCES += tools/armci_prof.c
libarmci_la_SOURCES += tools/capi.c
else # HAVE_SYS_WEAK_ALIAS_PRAGMA
libarmci_la_SOURCES += tools/armci_prof.c
endif # HAVE_SYS_WEAK_ALIAS_PRAGMA
else # ENABLE_PROFILING
libarmci_la_SOURCES += tools/capi.c
endif

include_HEADERS += tools/parmci.h
AM_CPPFLAGS += -I$(top_srcdir)/tools

##############################################################################
# testing
#
check_PROGRAMS += testing/msgcheck
check_PROGRAMS += testing/perf
check_PROGRAMS += testing/perf_aggr
check_PROGRAMS += testing/perf_amo
check_PROGRAMS += testing/perf_contig
check_PROGRAMS += testing/perf_nb
check_PROGRAMS += testing/perf_strided
check_PROGRAMS += testing/shift
check_PROGRAMS += testing/shmclean
check_PROGRAMS += testing/shmtest
check_PROGRAMS += testing/simplelock
check_PROGRAMS += testing/test
check_PROGRAMS += testing/test2
check_PROGRAMS += testing/testnotify
if ENABLE_CHECKPOINT
check_PROGRAMS += testing/fttest
endif
if ARMCI_ENABLE_GPC_CALLS
check_PROGRAMS += testing/gpctest
endif
if MSG_COMMS_MPI
check_PROGRAMS += testing/simple
check_PROGRAMS += testing/test_groups
if SYSV
#check_PROGRAMS += testing/ipctest
endif
endif
if THREAD_SAFE
check_PROGRAMS += testing/test_mt
endif
if ENABLE_F77
#check_PROGRAMS += testing/acc_test
#check_PROGRAMS += testing/copy_test
endif
if ARMCI_NETWORK_PORTALS
check_PROGRAMS += testing/clone
check_PROGRAMS += testing/fork
check_PROGRAMS += testing/origptl
check_PROGRAMS += testing/ptltest
check_PROGRAMS += testing/te
endif
if ARMCI_SRC_DIR_SRC
check_PROGRAMS += testing/testitr
endif

ARMCI_SERIAL_TESTS =
ARMCI_SERIAL_TESTS_XFAIL =
ARMCI_PARALLEL_TESTS =
ARMCI_PARALLEL_TESTS_XFAIL =
ARMCI_TESTS = $(ARMCI_SERIAL_TESTS) $(ARMCI_PARALLEL_TESTS)
ARMCI_TESTS_XFAIL = $(ARMCI_SERIAL_TESTS_XFAIL) $(ARMCI_PARALLEL_TESTS_XFAIL)

ARMCI_PARALLEL_TESTS += testing/msgcheck$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/perf$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/perf_aggr$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/perf_amo$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/perf_contig$(EXEEXT)
#ARMCI_PARALLEL_TESTS += testing/perf_nb$(EXEEXT) # needs bug fixes
ARMCI_PARALLEL_TESTS += testing/perf_strided$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/shift$(EXEEXT)
#ARMCI_SERIAL_TESTS += testing/shmclean$(EXEEXT) # not sure we need this
#ARMCI_SERIAL_TESTS += testing/shmtest$(EXEEXT) # not sure we need this
ARMCI_PARALLEL_TESTS += testing/simplelock$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/test$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/test2$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/testnotify$(EXEEXT)
if ENABLE_CHECKPOINT
ARMCI_PARALLEL_TESTS += testing/fttest$(EXEEXT)
endif
if ARMCI_ENABLE_GPC_CALLS
ARMCI_PARALLEL_TESTS += testing/gpctest$(EXEEXT)
endif
if MSG_COMMS_MPI
ARMCI_PARALLEL_TESTS += testing/simple$(EXEEXT)
ARMCI_PARALLEL_TESTS += testing/test_groups$(EXEEXT)
if SYSV
#ARMCI_PARALLEL_TESTS += testing/ipctest$(EXEEXT) # needs bug fixes
endif
endif
if THREAD_SAFE
ARMCI_PARALLEL_TESTS += testing/test_mt$(EXEEXT)
endif
if ENABLE_F77
#ARMCI_SERIAL_TESTS += testing/acc_test$(EXEEXT)
#ARMCI_SERIAL_TESTS += testing/copy_test$(EXEEXT)
endif
if ARMCI_NETWORK_PORTALS
#ARMCI_PARALLEL_TESTS += testing/clone$(EXEEXT)
#ARMCI_PARALLEL_TESTS += testing/fork$(EXEEXT)
#ARMCI_PARALLEL_TESTS += testing/origptl$(EXEEXT)
#ARMCI_PARALLEL_TESTS += testing/ptltest$(EXEEXT)
#ARMCI_PARALLEL_TESTS += testing/te$(EXEEXT)
endif
if ARMCI_SRC_DIR_SRC
ARMCI_SERIAL_TESTS += testing/testitr$(EXEEXT)
endif

#testing_acc_test_SOURCES     = testing/acc_test.c
testing_clone_SOURCES        = testing/clone.c
#testing_copy_test_SOURCES    = testing/copy_test.c
testing_fork_SOURCES         = testing/fork.c
testing_fttest_SOURCES       = testing/fttest.c
testing_gpctest_SOURCES      = testing/gpctest.c
#testing_ipctest_SOURCES      = testing/ipctest.c
testing_msgcheck_SOURCES     = testing/msgcheck.c
testing_origptl_SOURCES      = testing/origptl.c
testing_perf_aggr_SOURCES    = testing/perf_aggr.c
testing_perf_amo_SOURCES     = testing/perf_amo.c
testing_perf_contig_SOURCES  = testing/perf_contig.c
testing_perf_nb_SOURCES      = testing/perf_nb.c
testing_perf_strided_SOURCES = testing/perf_strided.c
testing_perf_SOURCES         = testing/perf.c
testing_ptltest_SOURCES      = testing/ptltest.c
testing_shift_SOURCES        = testing/shift.c
testing_shmclean_SOURCES     = testing/shmclean.c
testing_shmtest_SOURCES      = testing/shmtest.c
testing_simple_SOURCES       = testing/simple.c
testing_simplelock_SOURCES   = testing/simplelock.c
testing_te_SOURCES           = testing/te.c
testing_test2_SOURCES        = testing/test2.c
testing_test_groups_SOURCES  = testing/test_groups.c
testing_testitr_SOURCES      = testing/testitr.c
testing_test_mt_SOURCES      = testing/test_mt.c
testing_testnotify_SOURCES   = testing/testnotify.c
testing_test_SOURCES         = testing/test.c

testing_testitr_CPPFLAGS    = $(AM_CPPFLAGS) -I$(top_srcdir)/src/include

#nodist_EXTRA_testing_acc_test_SOURCES     = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_clone_SOURCES        = $(MAYBE_DUMMY_F_LINK)
#nodist_EXTRA_testing_copy_test_SOURCES    = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_fork_SOURCES         = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_fttest_SOURCES       = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_gpctest_SOURCES      = $(MAYBE_DUMMY_F_LINK)
#nodist_EXTRA_testing_ipctest_SOURCES      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_msgcheck_SOURCES     = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_origptl_SOURCES      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_perf_aggr_SOURCES    = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_perf_amo_SOURCES     = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_perf_contig_SOURCES  = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_perf_nb_SOURCES      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_perf_strided_SOURCES = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_perf_SOURCES         = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_ptltest_SOURCES      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_shift_SOURCES        = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_shmclean_SOURCES     = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_shmtest_SOURCES      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_simple_SOURCES       = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_simplelock_SOURCES   = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_te_SOURCES           = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_test2_SOURCES        = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_test_groups_SOURCES  = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_testitr_SOURCES      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_test_mt_SOURCES      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_testnotify_SOURCES   = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_testing_test_SOURCES         = $(MAYBE_DUMMY_F_LINK)

##############################################################################
# examples
#
check_PROGRAMS += examples/benchmarks/cg/armci_sharedmemory/cg
check_PROGRAMS += examples/benchmarks/lu/armci_blocking/lu-b-bc
check_PROGRAMS += examples/benchmarks/lu/armci_blocking/lu-block
check_PROGRAMS += examples/benchmarks/lu/armci_blocking/lu
check_PROGRAMS += examples/benchmarks/lu/armci_nonblocking/lu_nb_get
check_PROGRAMS += examples/benchmarks/lu/armci_nonblocking/lu_nb_put
check_PROGRAMS += examples/benchmarks/lu/pthreads/lu-thread
check_PROGRAMS += examples/features/aggregation/simple/simple
check_PROGRAMS += examples/features/aggregation/sparse_matvecmul/sparse_matvecmul
check_PROGRAMS += examples/features/concurrency/simple/comdegree
check_PROGRAMS += examples/features/non-blocking/simple/simple
check_PROGRAMS += examples/features/notification/simple/testnotify
check_PROGRAMS += examples/features/symmetric_memory/simple/simple
check_PROGRAMS += examples/simpleApps/transp1D-c
if ARMCI_ENABLE_GPC_CALLS
check_PROGRAMS += examples/features/gpc/hashtable/test_hashtable
if HAVE_BLAS
if HAVE_LAPACK
check_PROGRAMS += examples/features/proc_affinity/computation_impact/computation_impact
endif # HAVE_LAPACK
endif # HAVE_BLAS
check_PROGRAMS += examples/features/proc_affinity/simple/simple
endif # ARMCI_ENABLE_GPC_CALLS
if MSG_COMMS_MPI
check_PROGRAMS += examples/features/concurrency/multidma/multidma
check_PROGRAMS += examples/features/non-blocking/overlap/overlap
endif
if RANDOM_ACCESS
check_PROGRAMS += examples/benchmarks/RandomAccess/simple/simple
check_PROGRAMS += examples/benchmarks/RandomAccess/smp_bucket/smp_bucket
endif
if THREAD_SAFE
check_PROGRAMS += examples/benchmarks/lu/armci_multithreaded/lu-block-th
endif

#ARMCI_PARALLEL_TESTS += examples/benchmarks/cg/armci_sharedmemory/cg$(EXEEXT) # needs input file
ARMCI_PARALLEL_TESTS += examples/benchmarks/lu/armci_blocking/lu-b-bc$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/benchmarks/lu/armci_blocking/lu-block$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/benchmarks/lu/armci_blocking/lu$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/benchmarks/lu/armci_nonblocking/lu_nb_get$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/benchmarks/lu/armci_nonblocking/lu_nb_put$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/benchmarks/lu/pthreads/lu-thread$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/features/aggregation/simple/simple$(EXEEXT)
#ARMCI_PARALLEL_TESTS += examples/features/aggregation/sparse_matvecmul/sparse_matvecmul$(EXEEXT) # needs input file
ARMCI_PARALLEL_TESTS += examples/features/concurrency/simple/comdegree$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/features/non-blocking/simple/simple$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/features/notification/simple/testnotify$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/features/symmetric_memory/simple/simple$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/simpleApps/transp1D-c$(EXEEXT)
if ARMCI_ENABLE_GPC_CALLS
ARMCI_PARALLEL_TESTS += examples/features/gpc/hashtable/test_hashtable$(EXEEXT)
if HAVE_BLAS
if HAVE_LAPACK
ARMCI_PARALLEL_TESTS += examples/features/proc_affinity/computation_impact/computation_impact$(EXEEXT)
endif # HAVE_LAPACK
endif # HAVE_BLAS
ARMCI_PARALLEL_TESTS += examples/features/proc_affinity/simple/simple$(EXEEXT)
endif # ARMCI_ENABLE_GPC_CALLS
if MSG_COMMS_MPI
#ARMCI_PARALLEL_TESTS += examples/features/concurrency/multidma/multidma$(EXEEXT) # needs bug fixes
#ARMCI_PARALLEL_TESTS += examples/features/non-blocking/overlap/overlap$(EXEEXT) # needs bug fixes
endif
if RANDOM_ACCESS
ARMCI_PARALLEL_TESTS += examples/benchmarks/RandomAccess/simple/simple$(EXEEXT)
ARMCI_PARALLEL_TESTS += examples/benchmarks/RandomAccess/smp_bucket/smp_bucket$(EXEEXT)
endif
if THREAD_SAFE
ARMCI_PARALLEL_TESTS += examples/benchmarks/lu/armci_multithreaded/lu-block-th$(EXEEXT)
endif

examples_benchmarks_cg_armci_sharedmemory_cg_SOURCES = \
examples/benchmarks/cg/armci_sharedmemory/cg.c         \
examples/benchmarks/cg/armci_sharedmemory/compute.c    \
examples/benchmarks/cg/armci_sharedmemory/read_input.c \
examples/benchmarks/cg/armci_sharedmemory/timing.c

examples_benchmarks_lu_armci_blocking_lu_SOURCES = \
examples/benchmarks/lu/armci_blocking/lu.c         \
examples/benchmarks/lu/armci_blocking/timing.c

examples_benchmarks_lu_armci_blocking_lu_block_SOURCES = \
examples/benchmarks/lu/armci_blocking/lu-block.c         \
examples/benchmarks/lu/armci_blocking/timing.c

examples_benchmarks_lu_armci_blocking_lu_b_bc_SOURCES =  \
examples/benchmarks/lu/armci_blocking/lu-b-bc.c          \
examples/benchmarks/lu/armci_blocking/timing.c

examples_benchmarks_lu_armci_multithreaded_lu_block_th_SOURCES = \
examples/benchmarks/lu/armci_multithreaded/lu-block-th.c         \
examples/benchmarks/lu/armci_multithreaded/timing.c

examples_benchmarks_lu_armci_nonblocking_lu_nb_put_SOURCES = \
examples/benchmarks/lu/armci_nonblocking/lu_nb_put.c         \
examples/benchmarks/lu/armci_nonblocking/timing.c

examples_benchmarks_lu_armci_nonblocking_lu_nb_get_SOURCES = \
examples/benchmarks/lu/armci_nonblocking/lu_nb_get.c         \
examples/benchmarks/lu/armci_nonblocking/timing.c

examples_benchmarks_lu_pthreads_lu_thread_SOURCES = \
examples/benchmarks/lu/pthreads/barrier.c           \
examples/benchmarks/lu/pthreads/barrier.h           \
examples/benchmarks/lu/pthreads/errors.h            \
examples/benchmarks/lu/pthreads/lu-thread.c         \
examples/benchmarks/lu/pthreads/timing.c
examples_benchmarks_lu_pthreads_lu_thread_LDADD = -lpthread

examples_benchmarks_RandomAccess_simple_simple_SOURCES = \
examples/benchmarks/RandomAccess/simple/simple.c         \
examples/benchmarks/RandomAccess/timing.c

examples_benchmarks_RandomAccess_smp_bucket_smp_bucket_SOURCES = \
examples/benchmarks/RandomAccess/smp_bucket/smp_bucket.c         \
examples/benchmarks/RandomAccess/timing.c

examples_features_aggregation_simple_simple_SOURCES = \
examples/features/aggregation/simple/simple.c

examples_features_aggregation_sparse_matvecmul_sparse_matvecmul_SOURCES = \
examples/features/aggregation/sparse_matvecmul/sparse_matvecmul.c

examples_features_concurrency_multidma_multidma_SOURCES = \
examples/features/concurrency/multidma/multidma.c

examples_features_concurrency_simple_comdegree_SOURCES = \
examples/features/concurrency/simple/comdegree.c

examples_features_gpc_hashtable_test_hashtable_SOURCES = \
examples/features/gpc/hashtable/DistHashmap.cc           \
examples/features/gpc/hashtable/DistHashmap.h            \
examples/features/gpc/hashtable/GPCHashmap.cc            \
examples/features/gpc/hashtable/GPCHashmap.h             \
examples/features/gpc/hashtable/GPCHashmapHandler.cc     \
examples/features/gpc/hashtable/Hash_common.h            \
examples/features/gpc/hashtable/HashFunctions.cc         \
examples/features/gpc/hashtable/hash_map.h               \
examples/features/gpc/hashtable/HashUtil.cc              \
examples/features/gpc/hashtable/test_hashtable.cc        \
examples/features/gpc/hashtable/Util.h
examples_features_gpc_hashtable_test_hashtable_LDFLAGS = $(LDFLAGS_BASE)
examples_features_gpc_hashtable_test_hashtable_LDADD = $(LDADD) $(MAYBE_FLIBS)

examples_features_non_blocking_overlap_overlap_SOURCES = \
examples/features/non-blocking/overlap/overlap.c

examples_features_non_blocking_simple_simple_SOURCES = \
examples/features/non-blocking/simple/simple.c

examples_features_notification_simple_testnotify_SOURCES = \
examples/features/notification/simple/testnotify.c

examples_features_proc_affinity_computation_impact_computation_impact_SOURCES = \
examples/features/proc_affinity/computation_impact/computation_impact.c
examples_features_proc_affinity_computation_impact_computation_impact_LDFLAGS = \
$(AM_LDFLAGS) \
$(LAPACK_LDFLAGS) \
$(BLAS_LDFLAGS)
examples_features_proc_affinity_computation_impact_computation_impact_LDADD = \
$(LDADD) \
$(LAPACK_LIBS) \
$(BLAS_LIBS)

examples_features_proc_affinity_simple_simple_SOURCES = \
examples/features/proc_affinity/simple/simple.c

examples_features_symmetric_memory_simple_simple_SOURCES = \
examples/features/symmetric_memory/simple/simple.c

examples_simpleApps_transp1D_c_SOURCES = \
examples/simpleApps/transp1D-c.c

nodist_EXTRA_examples_benchmarks_cg_armci_sharedmemory_cg_SOURCES                          = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_lu_armci_blocking_lu_SOURCES                              = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_lu_armci_blocking_lu_block_SOURCES                        = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_lu_armci_blocking_lu_b_bc_SOURCES                         = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_lu_armci_multithreaded_lu_block_th_SOURCES                = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_lu_armci_nonblocking_lu_nb_put_SOURCES                    = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_lu_armci_nonblocking_lu_nb_get_SOURCES                    = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_lu_pthreads_lu_thread_SOURCES                             = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_RandomAccess_simple_simple_SOURCES                        = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_benchmarks_RandomAccess_smp_bucket_smp_bucket_SOURCES                = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_aggregation_simple_simple_SOURCES                           = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_aggregation_sparse_matvecmul_sparse_matvecmul_SOURCES       = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_concurrency_multidma_multidma_SOURCES                       = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_concurrency_simple_comdegree_SOURCES                        = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_gpc_hashtable_test_hashtable_SOURCES                        = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_non_blocking_overlap_overlap_SOURCES                        = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_non_blocking_simple_simple_SOURCES                          = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_notification_simple_testnotify_SOURCES                      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_proc_affinity_computation_impact_computation_impact_SOURCES = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_proc_affinity_simple_simple_SOURCES                         = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_features_symmetric_memory_simple_simple_SOURCES                      = $(MAYBE_DUMMY_F_LINK)
nodist_EXTRA_examples_simpleApps_transp1D_c_SOURCES                                        = $(MAYBE_DUMMY_F_LINK)

EXTRA_DIST += examples/benchmarks/lu/README
EXTRA_DIST += examples/features/aggregation/README
EXTRA_DIST += examples/features/concurrency/README
EXTRA_DIST += examples/features/gpc/hashtable/README
EXTRA_DIST += examples/features/non-blocking/README
EXTRA_DIST += examples/features/notification/README
EXTRA_DIST += examples/features/proc_affinity/README
EXTRA_DIST += examples/README

##############################################################################
# the end
#
.PHONY: checkprogs
checkprogs: $(check_PROGRAMS)

# support verbose/silent make rules for additional programs
# sed
SED_V = $(SED__v_$(V))
SED__v_ = $(SED__v_$(AM_DEFAULT_VERBOSITY))
SED__v_0 = @echo "  SED   " $@;
# ARMCI_TAS_AS
ARMCI_TAS_AS_V = $(ARMCI_TAS_AS__v_$(V))
ARMCI_TAS_AS__v_ = $(ARMCI_TAS_AS__v_$(AM_DEFAULT_VERBOSITY))
ARMCI_TAS_AS__v_0 = @echo "  GCC   " $@;
# ARMCI_X86COPY_AS
ARMCI_X86COPY_AS_V = $(ARMCI_X86COPY_AS__v_$(V))
ARMCI_X86COPY_AS__v_ = $(ARMCI_X86COPY_AS__v_$(AM_DEFAULT_VERBOSITY))
ARMCI_X86COPY_AS__v_0 = @echo "  GCC   " $@;

.PHONY: pfiles clean-pfiles
if MSG_COMMS_TCGMSG4
pfiles: $(check_PROGRAMS)
	for p in $(check_PROGRAMS); do \
	    echo "`whoami` `hostname` $(NPROCS) `pwd`/$$p /tmp" > $$p.p; \
	done
check: pfiles
clean-pfiles:
	find . -name '*.p' -exec rm {} \; ;
clean-local: clean-pfiles
endif # MSG_COMMS_TCGMSG4

##############################################################################
# test suite
#
# Some tests were commented out either because they required an input file or
# they were failing for unknown reasons and we didn't want to further debug.
#
SERIAL_TESTS =
SERIAL_TESTS += $(GAF2C_SERIAL_TESTS)
SERIAL_TESTS += $(TCGMSG_SERIAL_TESTS)
SERIAL_TESTS += $(ARMCI_SERIAL_TESTS)

SERIAL_TESTS_XFAIL =
SERIAL_TESTS_XFAIL += $(GAF2C_SERIAL_TESTS_XFAIL)
SERIAL_TESTS_XFAIL += $(TCGMSG_SERIAL_TESTS_XFAIL)
SERIAL_TESTS_XFAIL += $(ARMCI_SERIAL_TESTS_XFAIL)

PARALLEL_TESTS =
PARALLEL_TESTS += $(GAF2C_PARALLEL_TESTS)
PARALLEL_TESTS += $(TCGMSG_PARALLEL_TESTS)
PARALLEL_TESTS += $(ARMCI_PARALLEL_TESTS)

PARALLEL_TESTS_XFAIL =
PARALLEL_TESTS_XFAIL += $(GAF2C_PARALLEL_TESTS_XFAIL)
PARALLEL_TESTS_XFAIL += $(TCGMSG_PARALLEL_TESTS_XFAIL)
PARALLEL_TESTS_XFAIL += $(ARMCI_PARALLEL_TESTS_XFAIL)

TESTS =
TESTS += $(SERIAL_TESTS)
TESTS += $(PARALLEL_TESTS)

XFAIL_TESTS =
XFAIL_TESTS += $(SERIAL_TESTS_XFAIL)
XFAIL_TESTS += $(PARALLEL_TESTS_XFAIL)

if MSG_COMMS_TCGMSG4
LOG_COMPILER = \
maybe_parexec=`if echo "$(SERIAL_TESTS)" | $(GREP) "$$p" > /dev/null; then echo ""; else echo "$(PAREXEC)"; fi`; eval $$maybe_parexec
else
if MSG_COMMS_TCGMSG5
LOG_COMPILER = tst="$$dir$$f -np $(NPROCS)"; eval
else
if CROSS_COMPILING
maybe_mpiexec=`if echo "$(SERIAL_TESTS)" | $(GREP) "$$p" > /dev/null; then echo "$(MPIEXEC)" | $(SED) 's/%NP%/1/'; else echo "$(MPIEXEC)" | $(SED) 's/%NP%/$(NPROCS)/'; fi`; eval $$maybe_mpiexec
else
LOG_COMPILER = \
maybe_mpiexec=`if echo "$(SERIAL_TESTS)" | $(GREP) "$$p" > /dev/null; then echo ""; else if echo "$(MPIEXEC)" | $(GREP) "%NP%" > /dev/null; then echo "$(MPIEXEC)" | $(SED) 's/%NP%/$(NPROCS)/'; else echo "$(MPIEXEC)"; fi; fi`; eval $$maybe_mpiexec
endif # CROSS_COMPILING
endif # MSG_COMMS_TCGMSG5
endif # MSG_COMMS_TCGMSG4

.PHONY: check-gaf2c
check-gaf2c: $(all-am) $(check_LTLIBRARIES) $(GAF2C_TESTS) $(MAYBE_PFILES)
	$(MAKE) $(AM_MAKEFLAGS) TESTS="$(GAF2C_TESTS)" XFAIL_TESTS="$(GAF2C_TESTS_XFAIL)" check-TESTS

.PHONY: check-tcgmsg
check-tcgmsg: $(all-am) $(check_LTLIBRARIES) $(TCGMSG_TESTS) $(MAYBE_PFILES)
	$(MAKE) $(AM_MAKEFLAGS) TESTS="$(TCGMSG_TESTS)" XFAIL_TESTS="$(TCGMSG_TESTS_XFAIL)" check-TESTS

.PHONY: check-armci
check-armci: $(all-am) $(check_LTLIBRARIES) $(ARMCI_TESTS) $(MAYBE_PFILES)
	$(MAKE) $(AM_MAKEFLAGS) TESTS="$(ARMCI_TESTS)" XFAIL_TESTS="$(ARMCI_TESTS_XFAIL)" check-TESTS
