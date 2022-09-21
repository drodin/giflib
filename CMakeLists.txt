cmake_minimum_required(VERSION 3.0.0)

project(giflib VERSION 5.2.1)

set(GIFLIB_SRCS
    "lib/dgif_lib.c"
    "lib/egif_lib.c"
    "lib/gif_font.c"
    "lib/gif_hash.h"
    "lib/gifalloc.c"
    "lib/openbsd-reallocarray.c"
    "lib/gif_err.c"
    "lib/quantize.c"
    "lib/gif_hash.c"
    "lib/gif_lib.h"
    "lib/gif_lib_private.h"
)
set(GIFLIB_INCLUDES
    "lib/gif_lib.h"
)

add_library(giflib ${GIFLIB_SRCS})

if (MSVC)
  # Suppress warnings to reduce build log size.
  target_compile_options(giflib PRIVATE
    "/wd4267"
    "/wd4244"
    "/wd4800"
    "/wd4503"
    "/wd4554"
    "/wd4996"
    "/wd4348"
    "/wd4018"
    "/wd4099"
    "/wd4146"
    "/wd4267"
    "/wd4305"
    "/wd4307"
    "/wd4715"
    "/wd4722"
    "/wd4723"
    "/wd4838"
    "/wd4309"
    "/wd4334"
    "/wd4003"
    "/wd4244"
    "/wd4267"
    "/wd4503"
    "/wd4506"
    "/wd4800"
    "/wd4996"
    "/wd8029"
    )
endif()

target_include_directories(giflib
  PUBLIC
  "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/lib>"
)

if(WIN32)
  target_include_directories(giflib
    PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/win32>"
    ) 
endif()

### Install ###
set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
  "${version_config}" COMPATIBILITY SameMajorVersion
  )

# Note: use 'targets_export_name'
configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/cmake/Config.cmake.in"
  "${project_config}"
  INSTALL_DESTINATION "${config_install_dir}"
  )

install(
  TARGETS ${PROJECT_NAME}
  EXPORT "${targets_export_name}"
  LIBRARY DESTINATION "lib"
  ARCHIVE DESTINATION "lib"
  RUNTIME DESTINATION "bin"
  INCLUDES DESTINATION "${include_install_dir}"
  )

# expects: include <gif_lib.h>
install(
  FILES "lib/gif_lib.h"
  DESTINATION "${include_install_dir}"
  )

install(
  FILES "${project_config}" "${version_config}"
  DESTINATION "${config_install_dir}"
  )

install(
  EXPORT "${targets_export_name}"
  NAMESPACE "${namespace}"
  DESTINATION "${config_install_dir}"
  )
  

