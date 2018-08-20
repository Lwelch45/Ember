cmake_minimum_required(VERSION 3.3 FATAL_ERROR)
list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/CMake/")

set(PACKAGE_NAME      "ember")
set(PACKAGE_VERSION   "0.0.1-dev")
set(PACKAGE_STRING    "${PACKAGE_NAME} ${PACKAGE_VERSION}")
set(PACKAGE_TARNAME   "${PACKAGE_NAME}-${PACKAGE_VERSION}")
set(PACKAGE_BUGREPORT "https://github.com/Lwelch45/Ember/issues")
project(${PACKAGE_NAME} CXX C)

set(CMAKE_CXX_COMPILER /usr/bin/g++-7 CACHE PATH "" FORCE)
# 150+ tests in the root folder anyone? No? I didn't think so.
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

set(CMAKE_EXPORT_COMPILE_COMMANDS 1)
set(CMAKE_EXE_LINKER_FLAGS  "${CMAKE_EXE_LINKER_FLAGS} -v")
set(CMAKE_CXX_FLAGS_DEBUG "-O0 -g")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -DNDEBUG")
set(CMAKE_CXX_FLAGS_RELWITHDEBINFO "-O3 -g -DNDEBUG")
set(CMAKE_CXX_FLAGS_MINSIZEREL "-Os")
if(NOT CMAKE_BUILD_TYPE)
   set(CMAKE_BUILD_TYPE Release)
endif()

set(INCLUDE_INSTALL_DIR include CACHE STRING
    "The subdirectory where header files should be installed")
set(LIB_INSTALL_DIR lib CACHE STRING
    "The subdirectory where libraries should be installed")
set(BIN_INSTALL_DIR bin CACHE STRING
    "The subdirectory where binaries should be installed")
set(CMAKE_INSTALL_DIR lib/cmake/ember CACHE STRING
    "The subdirectory where CMake package config files should be installed")

option(EMBER_ENABLE_TESTS "Useful for disabling all tests" ON)
option(EMBER_ENABLE_INTEGRATION_TESTS "control if integrations are bulit and ran" ON)
option(EMBER_ENABLE_UNIT_TESTS "control if unit tests are bulit and ran" ON)
option(EMBER_ENABLE_BENCHMARK_TESTS "control if benchmarks are bulit and ran" OFF)
option(EMBER_ENABLE_CMAKE_PROJECT_FLAGS "control cmake_cxx_flags_*" OFF)
option(EMBER_BUILD_PROGRAMS "Build EMBER programs and demos" ON)

if(NOT EMBER_ENABLE_TESTS)
  set(EMBER_ENABLE_INTEGRATION_TESTS  OFF)
  set(EMBER_ENABLE_UNIT_TESTS  OFF)
  set(EMBER_ENABLE_BENCHMARK_TESTS  OFF)
endif()

function(auto_sources RETURN_VALUE PATTERN SOURCE_SUBDIRS)
  if ("${SOURCE_SUBDIRS}" STREQUAL "RECURSE")
    SET(PATH ".")
    if (${ARGC} EQUAL 4)
      list(GET ARGV 3 PATH)
    endif ()
  endif()

  if ("${SOURCE_SUBDIRS}" STREQUAL "RECURSE")
    unset(${RETURN_VALUE})
    file(GLOB SUBDIR_FILES "${PATH}/${PATTERN}")
    list(APPEND ${RETURN_VALUE} ${SUBDIR_FILES})

    file(GLOB subdirs RELATIVE ${PATH} ${PATH}/*)

    foreach(DIR ${subdirs})
      if (IS_DIRECTORY ${PATH}/${DIR})
        if (NOT "${DIR}" STREQUAL "CMakeFiles")
          file(GLOB_RECURSE SUBDIR_FILES "${PATH}/${DIR}/${PATTERN}")
          list(APPEND ${RETURN_VALUE} ${SUBDIR_FILES})
        endif()
      endif()
    endforeach()
  else()
    file(GLOB ${RETURN_VALUE} "${PATTERN}")

    foreach (PATH ${SOURCE_SUBDIRS})
      file(GLOB SUBDIR_FILES "${PATH}/${PATTERN}")
      list(APPEND ${RETURN_VALUE} ${SUBDIR_FILES})
    endforeach()
  endif ()

  set(${RETURN_VALUE} ${${RETURN_VALUE}} PARENT_SCOPE)
endfunction(auto_sources)

set(FOLLY_DIR "${CMAKE_CURRENT_SOURCE_DIR}/ember")

add_subdirectory(ember)
