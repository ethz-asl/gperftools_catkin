cmake_minimum_required(VERSION 2.8.3)
project(gperftools_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(gperftools_src
  URL https://googledrive.com/host/0B6NtGsLhIcf7MWxMMF9JdTN3UVk/gperftools-2.2.1.tar.gz
  UPDATE_COMMAND ""
  CONFIGURE_COMMAND cd ../gperftools_src && ./configure --with-pic 
    --prefix=${CATKIN_DEVEL_PREFIX}
  BUILD_COMMAND cd ../gperftools_src && make -j 8
  INSTALL_COMMAND cd ../gperftools_src && make install -j 8
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} gperftools_src)
target_link_libraries(${PROJECT_NAME} ${CATKIN_DEVEL_PREFIX}/lib/libtcmalloc${CMAKE_SHARED_LIBRARY_SUFFIX})

cs_install()

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include
  CFG_EXTRAS gperftools-extras.cmake)
