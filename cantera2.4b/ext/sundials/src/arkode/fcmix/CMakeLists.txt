# ---------------------------------------------------------------
# Programmer:  Daniel R. Reynolds @ SMU
# ---------------------------------------------------------------
# LLNS/SMU Copyright Start
# Copyright (c) 2017, Southern Methodist University and 
# Lawrence Livermore National Security
#
# This work was performed under the auspices of the U.S. Department 
# of Energy by Southern Methodist University and Lawrence Livermore 
# National Laboratory under Contract DE-AC52-07NA27344.
# Produced at Southern Methodist University and the Lawrence 
# Livermore National Laboratory.
#
# All rights reserved.
# For details, see the LICENSE file.
# LLNS/SMU Copyright End
# ---------------------------------------------------------------
# CMakeLists.txt file for the FARKODE library

# Add variable farkode_SOURCES with the sources for the FARKODE library
SET(farkode_SOURCES
  farkadapt.c
  farkband.c
  farkbandmass.c
  farkbbd.c
  farkbp.c
  farkdense.c
  farkdensemass.c
  farkewt.c
  farkexpstab.c
  farkjtimes.c
  farkmtimes.c
  farkode.c
  farkpreco.c
  farkmasspreco.c
  farkroot.c
  farksparse.c 
  farksparsemass.c
  )

# Add variable shared_SOURCES with the common SUNDIALS sources which will
# also be included in the ARKODE library
SET(shared_SOURCES
  ${sundials_SOURCE_DIR}/src/nvec_ser/fnvector_serial.c
  )

# Add variable sunmatrix_SOURCES with the common SUNMatrix sources which will
# also be included in the ARKODE library
SET(sunmatrix_SOURCES
  ${sundials_SOURCE_DIR}/src/sunmat_band/fsunmatrix_band.c
  ${sundials_SOURCE_DIR}/src/sunmat_dense/fsunmatrix_dense.c
  ${sundials_SOURCE_DIR}/src/sunmat_sparse/fsunmatrix_sparse.c
  )

# Add variable sunlinsol_SOURCES with the common SUNLinearSolver sources which will
# also be included in the ARKODE library
SET(sunlinsol_SOURCES
  ${sundials_SOURCE_DIR}/src/sunlinsol_band/fsunlinsol_band.c
  ${sundials_SOURCE_DIR}/src/sunlinsol_dense/fsunlinsol_dense.c
  ${sundials_SOURCE_DIR}/src/sunlinsol_spbcgs/fsunlinsol_spbcgs.c
  ${sundials_SOURCE_DIR}/src/sunlinsol_spfgmr/fsunlinsol_spfgmr.c
  ${sundials_SOURCE_DIR}/src/sunlinsol_spgmr/fsunlinsol_spgmr.c
  ${sundials_SOURCE_DIR}/src/sunlinsol_sptfqmr/fsunlinsol_sptfqmr.c
  ${sundials_SOURCE_DIR}/src/sunlinsol_pcg/fsunlinsol_pcg.c
  )

IF(KLU_FOUND)
  LIST(APPEND sunlinsol_SOURCES
    ${sundials_SOURCE_DIR}/src/sunlinsol_klu/fsunlinsol_klu.c
    )
ENDIF()

IF(SUPERLUMT_FOUND)
  LIST(APPEND sunlinsol_SOURCES
    ${sundials_SOURCE_DIR}/src/sunlinsol_superlumt/fsunlinsol_superlumt.c
    )
ENDIF()

IF(LAPACK_FOUND)
  LIST(APPEND sunlinsol_SOURCES
    ${sundials_SOURCE_DIR}/src/sunlinsol_lapackband/fsunlinsol_lapackband.c
    ${sundials_SOURCE_DIR}/src/sunlinsol_lapackdense/fsunlinsol_lapackdense.c
    )
ENDIF()


# Add source directories to include directories for access to
# implementation only header files (both for farkode and arkode)
INCLUDE_DIRECTORIES(.)
INCLUDE_DIRECTORIES(..)

# Define C preprocessor flag -DBUILD_SUNDIALS_LIBRARY 
ADD_DEFINITIONS(-DBUILD_SUNDIALS_LIBRARY)

# Only build STATIC libraries (we cannot build shared libraries 
# for the FCMIX interfaces due to unresolved symbol errors 
# coming from inexistent user-provided functions)

# Add the build target for the FARKODE library
ADD_LIBRARY(sundials_farkode_static STATIC
  ${farkode_SOURCES} ${shared_SOURCES} ${sunmatrix_SOURCES} ${sunlinsol_SOURCES})

# Set the library name and make sure it is not deleted
SET_TARGET_PROPERTIES(sundials_farkode_static
  PROPERTIES OUTPUT_NAME sundials_farkode CLEAN_DIRECT_OUTPUT 1)

# Install the FARKODE library
INSTALL(TARGETS sundials_farkode_static DESTINATION lib)

#
MESSAGE(STATUS "Added ARKODE FCMIX module")
