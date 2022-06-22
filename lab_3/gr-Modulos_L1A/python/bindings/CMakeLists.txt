# Copyright 2020 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# SPDX-License-Identifier: GPL-3.0-or-later
#

########################################################################
# Check if there is C++ code at all
########################################################################
if(NOT Modulos_L1A_sources)
    MESSAGE(STATUS "No C++ sources... skipping python bindings")
    return()
endif(NOT Modulos_L1A_sources)

########################################################################
# Check for pygccxml
########################################################################
GR_PYTHON_CHECK_MODULE_RAW(
    "pygccxml"
    "import pygccxml"
    PYGCCXML_FOUND
    )

include(GrPybind)

########################################################################
# Python Bindings
########################################################################

list(APPEND Modulos_L1A_python_files
    python_bindings.cc)

GR_PYBIND_MAKE_OOT(Modulos_L1A
   ../..
   gr::Modulos_L1A
   "${Modulos_L1A_python_files}")

# copy in bindings .so file for use in QA test module
add_custom_target(
  copy_bindings_for_tests ALL
  COMMAND
    ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/*.so"
    ${CMAKE_BINARY_DIR}/test_modules/Modulos_L1A/
  DEPENDS Modulos_L1A_python)

install(TARGETS Modulos_L1A_python DESTINATION ${GR_PYTHON_DIR}/Modulos_L1A COMPONENT pythonapi)
