cmake_minimum_required(VERSION 3.10)
project(foils_hid VERSION 0.3 DESCRIPTION "HID device UDP client library" LANGUAGES C)

# Set global compiler warnings
add_compile_options(-Wstrict-prototypes -Wmissing-prototypes -Wno-unused-parameter)

# Add subdirectories for the libraries
add_subdirectory(subprojects/libela)
add_subdirectory(subprojects/librudp)

# Include directories for the main application
include_directories(include)

# Source files for the main application
set(MAIN_SOURCES
    src/foils_hid.c
    src/rudp_hid_client.c)

# Add the main application library
add_library(foils_hid ${MAIN_SOURCES})
target_link_libraries(foils_hid PRIVATE ela rudp)
target_include_directories(foils_hid PUBLIC include)

# Installation rules for the main library
install(TARGETS foils_hid
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    INCLUDES DESTINATION include)

# Option for building tests (default OFF)
option(BUILD_TESTS "Build test applications" OFF)
if(BUILD_TESTS)
    enable_testing()
    add_subdirectory(test)
endif()

# If needed, add pkg-config support
find_package(PkgConfig)
if(PKG_CONFIG_FOUND)
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/foils_hid.pc.in ${CMAKE_CURRENT_BINARY_DIR}/foils_hid.pc @ONLY)
    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/foils_hid.pc DESTINATION lib/pkgconfig)
endif()
