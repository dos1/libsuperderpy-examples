cmake_minimum_required(VERSION 2.8)

project(libsuperderpy-examples C)

SET(LIBSUPERDERPY_GAMENAME "superexamples")
SET(LIBSUPERDERPY_GAMENAME_PRETTY "Super Examples")
SET(LIBSUPERDERPY_INITIAL_GAMESTATE "dosowisko")

include("libsuperderpy/cmake/include.cmake")
include("libsuperderpy/cmake/SetPaths.cmake")

add_subdirectory(libsuperderpy)
add_subdirectory(src)
add_subdirectory(data)

# uninstall target
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/libsuperderpy/cmake/cmake_uninstall.cmake.in" "${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake" IMMEDIATE @ONLY)
add_custom_target(uninstall COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/cmake_uninstall.cmake)
