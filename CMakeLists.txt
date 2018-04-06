# Created by Manuel Blum on 2011-05-25.
# Copyright 2013 University of Freiburg.
# Modified by Einar J.M. Baumann (2017) <einar.baumann@gmail.com>

CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT(libgp CXX C)

# if no option is given, standard is release
IF(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
   SET(CMAKE_BUILD_TYPE Release)
ENDIF(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)

set(CMAKE_CXX_FLAGS_RELEASE "-DCLSQUARE -Wall -O2 -fPIC")
set(CMAKE_CXX_FLAGS_DEBUG "-DCLSQUARE -Wall -g -fPIC")

SET(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR} ${CMAKE_MODULE_PATH})


IF (USE_LOCAL_EIGEN3)
    message(STATUS "Using local Eigen3 in RpolyPlusPlus build.")
    set(EIGEN3_INCLUDE_DIR "${EIGEN3_ROOT}")
    message(STATUS "${EIGEN3_INCLUDE_DIR}")
    INCLUDE_DIRECTORIES(${EIGEN3_INCLUDE_DIR})
ELSE()
    FIND_PACKAGE(Eigen3 ${MIN_EIGEN_VERSION} REQUIRED)
    IF (EIGEN3_FOUND)
        MESSAGE(STATUS "Found Eigen version ${EIGEN3_VERSION}: ${EIGEN3_INCLUDE_DIR}")
    ENDIF (EIGEN3_FOUND)
ENDIF(USE_LOCAL_EIGEN3)

# ----- Add include directories -----
INCLUDE_DIRECTORIES(SYSTEM ${EIGEN3_INCLUDE_DIR})
INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/include)

# ----- Add sources -----
INCLUDE("Sources.cmake")

# ----- Add library target -----
ADD_LIBRARY(gp ${LIBGP_SRC} ${LIBGP_INTERFACES})

# ----- Add install targets ----
set_target_properties(gp PROPERTIES PUBLIC_HEADER "${LIBGP_INTERFACES}")
install(TARGETS gp DESTINATION lib
        PUBLIC_HEADER DESTINATION include/gp)


