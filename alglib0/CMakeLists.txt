cmake_minimum_required(VERSION 3.18.1)
project(alglib0_project)


message(">> MY_LIST = ${MY_LIST} [see variables in cmake are global]")

####################
### Set variable ###
####################
# CMake stores filepath as full-path.
# Instead of hardcoded full-path, we generate full-path with CMAKE_CURRENT_SOURCE_DIR.

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
list(APPEND SOURCES "aaaaa")
list(APPEND SOURCES "bbb")
list(APPEND SOURCES "ccc")
list(APPEND SOURCES "xyz")
message(">>>> SOURCES = ${SOURCES}")
list(REMOVE_ITEM SOURCES "xyz")
message(">>>> SOURCEs = ${SOURCES}")

#   add_library(alglib0 STATIC ${SOURCES} ${SQLITE3})
#   add_dependencies(alglib0 yaml-cpp reckless) # specify the build-order (like MSVS)
#   target_include_directories(alglib0 PUBLIC ${INCLUDE_DIR})
#   target_link_libraries(alglib0 PUBLIC pthread dl ${YAML_LIB} ${RECKLESS_LIB})
