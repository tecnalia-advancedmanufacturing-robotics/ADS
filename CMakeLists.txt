cmake_minimum_required(VERSION 3.10.2)

project(ads)

find_package(Threads)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  set(COMPILE_OPTIONS
    -pedantic
    -Wall
    -Wextra
  )
  add_definitions(-D_GNU_SOURCE)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  # Compiler flags and definitions for Visual Studio come here
endif()

option(BUILD_SHARED_LIBS "Build using shared libraries" ON)

add_subdirectory(AdsLib)
# add_subdirectory(AdsLibTest)
# add_subdirectory(example)

# Install the export (generates .cmake files)
install(EXPORT ${PROJECT_NAME}
  DESTINATION ${CMAKE_INSTALL_PREFIX}/share/${PROJECT_NAME}/cmake
  NAMESPACE ${PROJECT_NAME}::
  FILE ${PROJECT_NAME}-targets.cmake
)
install(FILES cmake/ads-config.cmake
  DESTINATION ${CMAKE_INSTALL_PREFIX}/share/${PROJECT_NAME}/cmake
)

# Install package.xml file for catkin worksapce compatibility
install(FILES package.xml
  DESTINATION share/${PROJECT_NAME}
)
