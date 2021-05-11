cmake_minimum_required(VERSION 3.18.1)
project(my_cmake_project)

####################################################################
# 1. CMake is a group of functions manipulating string variables.
#    CMake is a hierarchy of CMakeLists.txt files.
#    CMake is invoked by    >> cmake -DCMAKE_BUILD_TYPE=Debug 
#                                    -DMY_VARIABLE=value 
#                                    -DMY_OPTION=ON 
#                                    ../.. <--- location of CMakeLists.txt
#
# 2. CMake option is bool (0/off/oFF/faLSE are false, otherwise it is true, i.e. false_xx is true).
#    CMake variable is string / list<string>.
#    CMake variable and option are global ???
#
#    input command argument >> cmake -DMY_VARIABLE=value 
#    define local option    >> option(MY_OPTION "description" default_value)  
#    defind local variable  >> set(MY_VARIABLE "value")
#    update local variable  >> set(MY_VARIABLE "${MY_VARIABLE} append_value")
#
#    Print them by message function :
#    message("print my variable = ${MY_VARIABLE} ")
#
# 3. Some pre-defined variables for CMake : 
#    CMAKE_CURRENT_SOURCE_DIR = path of current CMakeLists.txt
#    CMAKE_CURRENT_BINARY_DIR = path of current directory running >> cmake ../..
#    CMAKE_BUILD_TYPE 
#    CMAKE_C_FLAGS_DEBUG      = suggested to initialize it with custom-defined FLAGS_DEBUG
#    CMAKE_C_FLAGS_RELEASE    = suggested to initialize it with custom-defined FLAGS_RELEASE
#    CMAKE_CXX_FLAGS_DEBUG    = suggested to initialize it with custom-defined FLAGS_DEBUG
#    CMAKE_CXX_FLAGS_RELEASE  = suggested to initialize it with custom-defined FLAGS_RELEASE
#    CMAKE_C_FLAGS            = select between CMAKE_C_FLAGS_DEBUG   and CMAKE_C_FLAGS_RELEASE
#    CMAKE_CXX_FLAGS          = select between CMAKE_CXX_FLAGS_DEBUG and CMAKE_CXX_FLAGS_RELEASE
#
# 4. MY_VARIABLE is usually list<string> storing the options for gcc/g++.
#    MY_VARIABLE is usually list<string> determined by the CMake options.
#    Effectively, we pass the CMake options to gcc options.
####################################################################
message("Type cmake ../.. -DMY_OPTION0=on -DMY_OPTION1=off")


##################
### Set option ###
##################
option(MY_OPTION0 "This is my_option0" OFF)
option(MY_OPTION1 "This is my_option1" OFF)
option(MY_OPTION2 "This is my_option2" OFF)
option(MY_OPTION3 "This is my_option3" OFF)
message(">> MY_OPTION0 = ${MY_OPTION0}")
message(">> MY_OPTION1 = ${MY_OPTION1}")
message(">> MY_OPTION2 = ${MY_OPTION2}")
message(">> MY_OPTION3 = ${MY_OPTION3}")


####################
### Set variable ###
####################
list(APPEND MY_LIST "value0_${MY_OPTION0}")
list(APPEND MY_LIST "value1_${MY_OPTION1}")
list(APPEND MY_LIST "value2_${MY_OPTION2}")
list(APPEND MY_LIST "value3_${MY_OPTION3}")
message(">> MY_LIST = ${MY_LIST}")
list(REMOVE_ITEM MY_LIST "value3_${MY_OPTION3}") # remove item by exact match
message(">> MY_LIST = ${MY_LIST}")

set(MY_STR "my_str=123")
list(REMOVE_ITEM MY_STR "123")
message(">> MY_STR = ${MY_STR}")
list(REMOVE_ITEM MY_STR "my_str=123")
message(">> MY_STR = ${MY_STR}")


################
### Set flag ###
################
add_compile_options(-MMD -MD 
                    -Wall -Wextra -Wpedantic -Werror 
                    -fno-builtin-memcmp 
                    -pthread $<$<COMPILE_LANGUAGE:CXX>:-fconcepts-diagnostics-depth=2>)

set(FLAGS_DEBUG "-flag0 -flag1 -flag2")
set(FLAGS_RELEASE "-flag2 -flag3 -xxx")

set(CMAKE_C_FLAGS_DEBUG ${FLAGS_DEBUG})
set(CMAKE_C_FLAGS_RELEASE ${FLAGS_RELEASE})
if("${CMAKE_BUILD_TYPE}" STREQUAL "Debug")
    set(CMAKE_C_FLAGS ${FLAGS_DEBUG})
elseif("${CMAKE_BUILD_TYPE}" STREQUAL "Release")
    set(CMAKE_C_FLAGS ${FLAGS_RELEASE})
endif()

set(CMAKE_CXX_FLAGS_DEBUG ${FLAGS_DEBUG})
set(CMAKE_CXX_FLAGS_RELEASE ${FLAGS_RELEASE})
if("${CMAKE_BUILD_TYPE}" STREQUAL "Debug")
    set(CMAKE_CXX_FLAGS ${FLAGS_DEBUG})
elseif("${CMAKE_BUILD_TYPE}" STREQUAL "Release")
    set(CMAKE_CXX_FLAGS ${FLAGS_RELEASE})
endif()

message(">> CMAKE_C_FLAGS = ${CMAKE_C_FLAGS}")
message(">> CMAKE_CXX_FLAGS = ${CMAKE_CXX_FLAGS}")


################
### Set path ###
################
message(">> CMAKE_CURRENT_SOURCE_DIR = ${CMAKE_CURRENT_SOURCE_DIR}")
message(">> CMAKE_CURRENT_BINARY_DIR = ${CMAKE_CURRENT_BINARY_DIR}")
get_filename_component(OUTPUT0 ${CMAKE_CURRENT_SOURCE_DIR} NAME) # i.e. get last part of the path
get_filename_component(OUTPUT1 ${CMAKE_CURRENT_BINARY_DIR} NAME) # i.e. get last part of the path
message(">> CMAKE_CURRENT_SOURCE_DIR NAME = ${OUTPUT0}")
message(">> CMAKE_CURRENT_BINARY_DIR NAME = ${OUTPUT1}")


########################
### Set subdirectory ###
########################
# Input the path to subdirectory's CMakeLists.txt here.

if(MY_OPTION0)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D_ALGLIB0")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -D_ALGLIB0")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -D_ALGLIB0")
    add_subdirectory(alglib0) 
endif()

if(MY_OPTION1)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -D_ALGLIB1")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -D_ALGLIB1")
    set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -D_ALGLIB1")
    add_subdirectory(alglib1)
endif()

if(MY_OPTION2)
    add_subdirectory(app)
endif()

if(MY_OPTION3)
    add_subdirectory(test)
    enable_testing() # auto-invoke executable registered by function : add_test
endif()

