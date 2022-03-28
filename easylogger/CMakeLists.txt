cmake_minimum_required(VERSION 3.17)

PROJECT(easylogger)

set(CMAKE_CXX_STANDARD 17)

#https://stackoverflow.com/questions/36451368/get-rid-of-cmake-warning-manually-specified-variables-were-not-used-by-the-proj
set(ignoreMe "${TARGET_OS_VIRTUAL}") #set(ignoreMe "${FOO}${BAZ}${BAR}")



FILE(GLOB_RECURSE MY_HEADERS "./inc/*.h")
source_group("Headers" FILES ${MY_HEADERS})

include_directories(inc plugins/file)

AUX_SOURCE_DIRECTORY(./src A)

#AUX_SOURCE_DIRECTORY(./plugins/file C)
AUX_SOURCE_DIRECTORY(./plugins/flash D)

IF(CMAKE_SYSTEM_NAME MATCHES "Windows")
    AUX_SOURCE_DIRECTORY(../demo/os/windows/easylogger/port B)
    set_property(GLOBAL PROPERTY VcpkgEnabled true)
    add_compile_options(/bigobj)
    link_libraries(Ws2_32.lib)
    add_definitions(-D NN_HAVE_WINDOWS)
    add_definitions(-D _CRT_SECURE_NO_WARNINGS)
    ADD_LIBRARY(easylogger  ${A} ${B}   ${MY_HEADERS})
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Linux")
    AUX_SOURCE_DIRECTORY(../demo/os/windows/easylogger/port B)
    link_directories(/usr/lib /usr/lib64 /usr/local/lib /usr/local/lib64 /usr/local/include)
    link_libraries(anl rt dl pthread)
    ADD_LIBRARY(easylogger ${A} ${B}   ${MY_HEADERS})
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Darwin")
    AUX_SOURCE_DIRECTORY(../demo/os/linux/easylogger/port B)
    link_directories(/usr/lib /usr/lib64 /usr/local/lib /usr/local/lib64 /usr/local/include)
    link_libraries(anl rt dl pthread)
    ADD_LIBRARY(easylogger ${A} ${B}   ${MY_HEADERS})
ENDIF(CMAKE_SYSTEM_NAME MATCHES "Windows")

IF(CMAKE_SYSTEM_NAME MATCHES "Windows")
    set_target_properties(easylogger PROPERTIES VS_GLOBAL_VcpkgEnabled true)
    #target_link_libraries(easylogger  PUBLIC sg3_utils PRIVATE spdlog::spdlog spdlog::spdlog_header_only)
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Linux")
    #target_link_libraries(easylogger  PUBLIC sg3_utils  PRIVATE fmt::fmt)
ELSEIF(CMAKE_SYSTEM_NAME MATCHES "Darwin")
    #target_link_libraries(easylogger  PUBLIC sg3_utils  PRIVATE fmt::fmt)
ENDIF(CMAKE_SYSTEM_NAME MATCHES "Windows")


