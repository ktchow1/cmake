cmake_minimum_required(VERSION 3.18.1)
project(alglib0_project)

message(">> [alglib0_project]----------------------------------------------------")
message(">> MY_LIST = ${MY_LIST} [see variables in cmake are global]")


####################
### Set filepath ###
####################
# CMake stores filepaths as full-path.
# Do not hardcode full-path, we generate full-path with CMAKE_CURRENT_SOURCE_DIR.

set(INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include")
set(SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")

include_directories(${INCLUDE_DIR})
file(GLOB SOURCES
  "${SOURCE_DIR}/*.cpp"
  "${SOURCE_DIR}/folder0/*.cpp"
  "${SOURCE_DIR}/folder1/*.cpp"
)

message(">>>> INCLUDE_DIR = ${INCLUDE_DIR}")
message(">>>> SOURCE_DIR = ${SOURCE_DIR}")
message(">>>> SOURCE = ${SOURCES}")
list(APPEND SOURCES "aaa")
list(APPEND SOURCES "bbb")
list(APPEND SOURCES "ccc")
list(APPEND SOURCES "xyz")
message(">>>> SOURCES = ${SOURCES}")
list(REMOVE_ITEM SOURCES "aaa")
list(REMOVE_ITEM SOURCES "bbb")
list(REMOVE_ITEM SOURCES "ccc")
list(REMOVE_ITEM SOURCES "xyz")
message(">>>> SOURCES = ${SOURCES}")
message("")


###########################################
# 1. Add executable / library
# 2. Add dependency (like MSVS setting)
# 3. target include
# 4. target link
# 5. target property
###########################################

add_library(alglib0 STATIC ${SOURCES})
# add_dependencies(alglib0 yaml-cpp reckless)
target_include_directories(alglib0 PUBLIC ${INCLUDE_DIR})
target_link_libraries(alglib0 PUBLIC pthread)
set_target_properties(alglib0 PROPERTIES
    C_STANDARD 11
    C_STANDARD_REQUIRED TRUE
    C_EXTENSIONS FALSE
    CXX_STANDARD 20
    CXX_STANDARD_REQUIRED TRUE
    CXX_EXTENSIONS FALSE
)

