set(EXECUTABLE ${LIBSUPERDERPY_GAMENAME})

SET(CMAKE_INSTALL_RPATH "\$ORIGIN/../lib/${LIBSUPERDERPY_GAMENAME}:\$ORIGIN/gamestates:\$ORIGIN:\$ORIGIN/../lib:\$ORIGIN/lib:\$ORIGIN/bin")

add_executable(${EXECUTABLE} WIN32 MACOSX_BUNDLE "main.c")
target_link_libraries(${EXECUTABLE} libsuperderpy)
install(TARGETS ${EXECUTABLE} DESTINATION ${BIN_INSTALL_DIR})

add_library("libsuperderpy-${LIBSUPERDERPY_GAMENAME}" SHARED "common.c")
set_target_properties("libsuperderpy-${LIBSUPERDERPY_GAMENAME}" PROPERTIES PREFIX "")
target_link_libraries("libsuperderpy-${LIBSUPERDERPY_GAMENAME}" ${ALLEGRO5_LIBRARIES} ${ALLEGRO5_FONT_LIBRARIES} ${ALLEGRO5_TTF_LIBRARIES} ${ALLEGRO5_PRIMITIVES_LIBRARIES} ${ALLEGRO5_AUDIO_LIBRARIES} ${ALLEGRO5_ACODEC_LIBRARIES} ${ALLEGRO5_IMAGE_LIBRARIES} ${ALLEGRO5_COLOR_LIBRARIES} m libsuperderpy)
install(TARGETS "libsuperderpy-${LIBSUPERDERPY_GAMENAME}" DESTINATION ${LIB_INSTALL_DIR})

add_subdirectory("gamestates")

libsuperderpy_copy(${EXECUTABLE})

if(MINGW)
    # resource compilation for MinGW
    add_custom_command( OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/icon.o COMMAND ${CMAKE_RC_COMPILER} -I${CMAKE_SOURCE_DIR} -i${CMAKE_SOURCE_DIR}/data/icons/icon.rc -o ${CMAKE_CURRENT_BINARY_DIR}/icon.o )
    set(EXECUTABLE_SRC_LIST ${EXECUTABLE_SRC_LIST} ${CMAKE_CURRENT_BINARY_DIR}/icon.o)
    set(LINK_FLAGS -Wl,-subsystem,windows)
endif(MINGW)

if(ALLEGRO5_MAIN_FOUND)
    target_link_libraries(${EXECUTABLE} ${ALLEGRO5_MAIN_LIBRARIES})
endif(ALLEGRO5_MAIN_FOUND)

if(APPLE)
    include(BundleUtilities)
    fixup_bundle(${EXECUTABLE} "" "")
endif(APPLE)
