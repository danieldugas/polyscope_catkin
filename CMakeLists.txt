cmake_minimum_required(VERSION 2.8.3)
project(polyscope_catkin)
find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)
file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)
ExternalProject_Add(polyscope_src
  GIT_REPOSITORY https://github.com/danieldugas/polyscope.git
  GIT_TAG 92aa91e198d1f886c6b3caf74a1cf9b93cc6a972
  UPDATE_COMMAND ""
  PATCH_COMMAND ""
  CONFIGURE_COMMAND cmake -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX}
  -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=${CMAKE_C_COMPILER} -DCMAKE_CXX_COMPILER=${CMAKE_CXX_COMPILER} ../polyscope_src
  BUILD_COMMAND make -j2
  INSTALL_COMMAND make install
)

# add_subdirectory(polyscope)
# cs_install()
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include/polyscope
        DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
        FILES_MATCHING PATTERN "*.h")
cs_export(INCLUDE_DIRS  ${CATKIN_DEVEL_PREFIX}/include
            LIBRARIES     polyscope)