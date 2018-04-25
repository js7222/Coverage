cmake_minimum_required(VERSION 3.2)

enable_testing()

project(dev)

option(ENABLE_COVERAGE "ENABLE_COVERAGE" ON)

if (ENABLE_COVERAGE MATCHES ON)
  if (NOT CMAKE_COMPILER_IS_GNUCXX)
    MESSAGE(FATAL_ERROR "COVERAGE ONLY ENABLED FOR GCC")
  endif()
  if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE Debug CACHE STRING "Choose the type of build." FORCE)
  endif()
  if (NOT CMAKE_BUILD_TYPE MATCHES Debug)
    MESSAGE(FATAL_ERROR "COVERAGE ENABLED FOR NON DEBUG")
  endif()

endif()

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14")
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)
find_package(Boost REQUIRED COMPONENTS system chrono)
find_package(Threads REQUIRED)
if(ENABLE_COVERAGE_MATCHES ON)
  include(CodeCoverage)
  APPEND_COVERAGE_COMPILER_FLAGS()
endif()

add_subdirectory(src)
