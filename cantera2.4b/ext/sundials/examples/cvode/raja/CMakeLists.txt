# ---------------------------------------------------------------
# Programmer:  Slaven Peles @ LLNL
# ---------------------------------------------------------------
# Based on CMakeLists.txt in cvode/serial by Radu Serban @ LLNL
# ---------------------------------------------------------------
# LLNS Copyright Start
# Copyright (c) 2014, Lawrence Livermore National Security
# This work was performed under the auspices of the U.S. Department
# of Energy by Lawrence Livermore National Laboratory in part under
# Contract W-7405-Eng-48 and in part under Contract DE-AC52-07NA27344.
# Produced at the Lawrence Livermore National Laboratory.
# All rights reserved.
# For details, see the LICENSE file.
# LLNS Copyright End
# ---------------------------------------------------------------
# CMakeLists.txt file for CVODE raja examples
# ---------------------------------------------------------------

# Example lists are tuples "name\;type" where the type is
# 'develop' for examples excluded from 'make test' in releases

# Examples using SUNDIALS linear solvers
SET(CVODE_examples
  "cvAdvDiff_kry_raja\;develop"
  )

# Add source directory to include directories
INCLUDE_DIRECTORIES(. ${CUDA_TOOLKIT_ROOT_DIR}/samples/common/inc ${RAJA_INCLUDE_PATH})

# Specify libraries to link against (through the target that was used to
# generate them) based on the value of the variable LINK_LIBRARY_TYPE
IF(LINK_LIBRARY_TYPE MATCHES "static")
  SET(CVODE_LIB sundials_cvode_static)
  SET(NVECS_LIB sundials_nvecraja_static)
ELSE()
  SET(CVODE_LIB sundials_cvode_shared)
  SET(NVECS_LIB sundials_nvecraja_shared)
ENDIF()

# Set-up linker flags and link libraries
SET(SUNDIALS_LIBS ${CVODE_LIB} ${NVECS_LIB} ${EXTRA_LINK_LIBS} -L${RAJA_LIB_PATH} -lRAJA)


# Add the build and install targets for each CVODE example
FOREACH(example_tuple ${CVODE_examples})

  # parse the example tuple
  LIST(GET example_tuple 0 example)
  LIST(GET example_tuple 1 example_type)

  set_source_files_properties(${example}.cu PROPERTIES CUDA_SOURCE_PROPERTY_FORMAT OBJ)

  # add example source files
  CUDA_ADD_EXECUTABLE(${example} ${example}.cu)

  SET_TARGET_PROPERTIES(${example} PROPERTIES FOLDER "Examples")

  # add example to regression tests
  SUNDIALS_ADD_TEST(${example} ${example}
    ANSWER_DIR ${CMAKE_CURRENT_SOURCE_DIR}
    ANSWER_FILE ${example}.out
    EXAMPLE_TYPE ${example_type})

  # libraries to link against
  TARGET_LINK_LIBRARIES(${example} ${SUNDIALS_LIBS})

  # install example source and out files
  IF(EXAMPLES_INSTALL)
    INSTALL(FILES ${example}.cu ${example}.out
      DESTINATION ${EXAMPLES_INSTALL_PATH}/cvode/raja)
  ENDIF()

ENDFOREACH(example_tuple ${CVODE_examples})


# create Makfile and CMakeLists.txt for examples
IF(EXAMPLES_INSTALL)

  # Install the README file
  INSTALL(FILES README DESTINATION ${EXAMPLES_INSTALL_PATH}/cvode/raja)

  # Prepare substitution variables for Makefile and/or CMakeLists templates
  SET(SOLVER "CVODE")
  SET(SOLVER_LIB "sundials_cvode")

  EXAMPLES2STRING(CVODE_examples EXAMPLES)

  # Regardless of the platform we're on, we will generate and install
  # CMakeLists.txt file for building the examples. This file  can then
  # be used as a template for the user's own programs.

  # generate CMakelists.txt in the binary directory
  CONFIGURE_FILE(
    ${PROJECT_SOURCE_DIR}/examples/templates/cmakelists_raja_CUDA_ex.in
    ${PROJECT_BINARY_DIR}/examples/cvode/raja/CMakeLists.txt
    @ONLY
    )

  # install CMakelists.txt
  INSTALL(
    FILES ${PROJECT_BINARY_DIR}/examples/cvode/raja/CMakeLists.txt
    DESTINATION ${EXAMPLES_INSTALL_PATH}/cvode/raja
    )

  # On UNIX-type platforms, we also  generate and install a makefile for
  # building the examples. This makefile can then be used as a template
  # for the user's own programs.

  IF(UNIX)
    # generate Makefile and place it in the binary dir
    CONFIGURE_FILE(
      ${PROJECT_SOURCE_DIR}/examples/templates/makefile_raja_CUDA_ex.in
      ${PROJECT_BINARY_DIR}/examples/cvode/raja/Makefile_ex
      @ONLY
      )
    # install the configured Makefile_ex as Makefile
    INSTALL(
      FILES ${PROJECT_BINARY_DIR}/examples/cvode/raja/Makefile_ex
      DESTINATION ${EXAMPLES_INSTALL_PATH}/cvode/raja
      RENAME Makefile
      )
  ENDIF(UNIX)

ENDIF(EXAMPLES_INSTALL)
