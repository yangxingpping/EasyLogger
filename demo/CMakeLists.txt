cmake_minimum_required(VERSION 3.17)

PROJECT(clogdemo)

set(CMAKE_CXX_STANDARD 17)

include_directories(../easylogger/inc)

#https://stackoverflow.com/questions/36451368/get-rid-of-cmake-warning-manually-specified-variables-were-not-used-by-the-proj
set(ignoreMe "${TARGET_OS_VIRTUAL}") #set(ignoreMe "${FOO}${BAZ}${BAR}")

FILE(GLOB_RECURSE MY_HEADERS "../easylogger/inc/*.h")
source_group("Headers" FILES ${MY_HEADERS})

IF(CMAKE_SYSTEM_NAME MATCHES "Windows")
    add_compile_options(/bigobj)
    link_libraries(Ws2_32.lib)
    AUX_SOURCE_DIRECTORY(./os/windows A)
    add_definitions(-D NN_HAVE_WINDOWS)
    add_definitions(-D _CRT_SECURE_NO_WARNINGS)
    add_executable(clogdemo  ${A}   ${MY_HEADERS})
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Linux")
    link_directories(/usr/lib /usr/lib64 /usr/local/lib /usr/local/lib64 /usr/local/include)
    link_libraries(anl rt dl pthread)
    AUX_SOURCE_DIRECTORY(./os/linux A)
    add_executable(clogdemo ${A}   ${MY_HEADERS})
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Darwin")
    link_directories(/usr/lib /usr/lib64 /usr/local/lib /usr/local/lib64 /usr/local/include)
    link_libraries(anl rt dl pthread)
    AUX_SOURCE_DIRECTORY(./os/linux A)
    add_executable(clogdemo ${A}   ${MY_HEADERS})
ENDIF(CMAKE_SYSTEM_NAME MATCHES "Windows")

IF(CMAKE_SYSTEM_NAME MATCHES "Windows")
    target_link_libraries(clogdemo  PUBLIC easylogger)
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Linux")
    target_link_libraries(clogdemo  PUBLIC easylogger)
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Darwin")
    target_link_libraries(clogdemo  PUBLIC easylogger)
ENDIF(CMAKE_SYSTEM_NAME MATCHES "Windows")


