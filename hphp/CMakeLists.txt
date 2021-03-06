#
#   +----------------------------------------------------------------------+
#   | HipHop for PHP                                                       |
#   +----------------------------------------------------------------------+
#   | Copyright (c) 2010 Facebook, Inc. (http://www.facebook.com)          |
#   | Copyright (c) 1997-2010 The PHP Group                                |
#   +----------------------------------------------------------------------+
#   | This source file is subject to version 3.01 of the PHP license,      |
#   | that is bundled with this package in the file LICENSE, and is        |
#   | available through the world-wide-web at the following url:           |
#   | http://www.php.net/license/3_01.txt                                  |
#   | If you did not receive a copy of the PHP license and are unable to   |
#   | obtain it through the world-wide-web, please send a note to          |
#   | license@php.net so we can mail you a copy immediately.               |
#   +----------------------------------------------------------------------+
#

if (EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/facebook")
  # I'm sorry, but facebook's internal repo has the top level dir stored inside
  # of hphp/, so we need to pull that one in first if it exists
  CMAKE_MINIMUM_REQUIRED(VERSION 2.8.7 FATAL_ERROR)
  set(HPHP_HOME ${CMAKE_CURRENT_SOURCE_DIR}/..)
  set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/facebook/CMake"
                        "${CMAKE_CURRENT_SOURCE_DIR}/public_tld/CMake"
                        ${CMAKE_MODULE_PATH})
  include(FBTLD)
endif()

include(HPHPSetup)
include(FollySetup)

add_definitions("-DHHVM")
add_definitions("-DUSE_CMAKE")

add_subdirectory(tools/bootstrap)

add_subdirectory(compiler)
add_subdirectory(hack)
add_subdirectory(hhbbc)
add_subdirectory(neo)
add_subdirectory(parser)

add_subdirectory(runtime)
add_subdirectory(runtime/ext)
add_subdirectory(runtime/ext_hhvm)
if (ENABLE_ZEND_COMPAT)
  add_subdirectory(runtime/ext_zend_compat)
endif()
add_subdirectory(system)
add_subdirectory(util)
add_subdirectory(vixl)
add_subdirectory(zend)

add_subdirectory(hhvm)

option(TEST_BIN "Create the HHVM test binary" OFF)
if (TEST_BIN)
  add_subdirectory(test)
endif ()

# Keep this last
add_subdirectory(tools/hphpize)
