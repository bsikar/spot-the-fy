cmake_minimum_required(VERSION 3.12)
project(ws CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(Drogon CONFIG REQUIRED)

# Locate JsonCpp headers and library
find_path(JSONCPP_INCLUDE_DIR json/json.h)
find_library(JSONCPP_LIBRARY jsoncpp)
if(NOT JSONCPP_INCLUDE_DIR OR NOT JSONCPP_LIBRARY)
    message(FATAL_ERROR "JsonCpp not found")
endif()
message(STATUS "Found JsonCpp: ${JSONCPP_LIBRARY}")

# Locate hiredis headers and library
find_path(HIREDIS_INCLUDE_DIR hiredis/hiredis.h)
find_library(HIREDIS_LIBRARY hiredis)
if(NOT HIREDIS_INCLUDE_DIR OR NOT HIREDIS_LIBRARY)
    message(FATAL_ERROR "hiredis not found")
endif()
message(STATUS "Found hiredis: ${HIREDIS_LIBRARY}")

file(GLOB_RECURSE SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/v*/*.cpp")
add_executable(${PROJECT_NAME} main.cpp ${SRC_FILES})

target_link_libraries(${PROJECT_NAME} PRIVATE Drogon::Drogon ${JSONCPP_LIBRARY} ${HIREDIS_LIBRARY})
target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR} ${JSONCPP_INCLUDE_DIR} ${HIREDIS_INCLUDE_DIR})

message(STATUS "Using C++${CMAKE_CXX_STANDARD}")

