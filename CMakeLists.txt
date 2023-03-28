cmake_minimum_required (VERSION 3.0)

project (sixaxispairing C)

set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules")
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin)
set (CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib)
set (PACKAGE_DIRECTORY ${PROJECT_BINARY_DIR}/package)

# find_package (HIDAPI REQUIRED)
# set(HIDAPI_WITH_LIBUSB FALSE) # surely will be used only on Linux
# set(BUILD_SHARED_LIBS FALSE) # HIDAPI as static library on all platforms
add_subdirectory(hidapi)

set (SOURCES
    main.c
)

# Mac USB linking.
if (${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    find_library(IOKIT NAMES IOKit)
    set (PLATFORM_LIBS
        ${IOKIT}
    )
endif ()

add_executable (${PROJECT_NAME} ${SOURCES})
target_link_libraries (${PROJECT_NAME} PRIVATE hidapi::hidapi)
