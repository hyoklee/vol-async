USE_MPI = FALSE
USE_MPI = TRUE
#HDF5_DIR  = ../../../build_parallel_debug/hdf5
#ABT_DIR = /Users/koziol/HDF5/github/hdfgroup/async/argobots/build_argobots
HDF5_DIR  = /gpfs/alpine/csc300/world-shared/gnu_build/hpc-io/hdf5/async_optional_build/hdf5
ABT_DIR = /gpfs/alpine/csc300/world-shared/gnu_build/hpc-io/vol-async/argobots/build

ifeq ($(USE_MPI), FALSE)
HDF5_DIR  = /gpfs/alpine/csc300/world-shared/gnu_build/hpc-io/hdf5/async_optional_nompi_build/hdf5
endif

#CC = cc
#CC = gcc-9
CC = mpicc

ifeq ($(USE_MPI), FALSE)
CC = gcc
endif

# DEBUG = -g -O0 -DENABLE_ASYNC_LOGGING
# DEBUG = -g -O0 
DEBUG = -g -O0 -DENABLE_DSET_MEMCPY
ifeq ($(USE_MPI), FALSE)
DEBUG = -g -O0 -DASYNC_VOL_NO_MPI 
endif

CFLAGS = $(DEBUG) -fPIC -I$(ABT_DIR)/include -I$(HDF5_DIR)/include -Wall
LIBS = -L$(HDF5_DIR)/lib -L$(ABT_DIR)/lib -labt -lhdf5 -lz
DYNLDFLAGS = $(DEBUG) -shared -fPIC $(LIBS) -Wl,-rpath=$(HDF5_DIR)/lib -Wl,-rpath=$(ABT_DIR)/lib
# DYNLDFLAGS = $(DEBUG) -dynamiclib -current_version 1.0  -fPIC $(LIBS)
ARFLAGS = rs

DYNSRC = h5_vol_external_async_native.c
DYNHDR = h5_vol_external_async_native.h
DYNOBJ = $(DYNSRC:.c=.o)
DYNPCH = $(DYNHDR:.h=.h.gch)
DYNLIB = libh5async.so
# DYNLIB = libh5async.dylib

STATSRC = h5_async_lib.c
STATHDR = h5_async_lib.h
STATOBJ = $(STATSRC:.c=.o)
STATPCH = $(STATHDR:.h=.h.gch)
STATLIB = libasynchdf5.a

all: $(DYNLIB) $(STATLIB)

$(DYNOBJ): $(DYNSRC) $(DYNHDR)

$(DYNLIB): $(DYNOBJ)
	$(CC) $(CFLAGS) $(DYNLDFLAGS) $^ -o $@

$(STATOBJ): $(STATSRC) $(STATHDR)
	$(CC) -c $(CFLAGS) $^

$(STATLIB): $(STATOBJ)
	$(AR) $(ARFLAGS) $@ $^

.PHONY: clean
clean:
	rm -rf $(DYNLIB) $(DYNOBJ) $(DYNPCH) $(STATLIB) $(STATOBJ) $(STATPCH)
