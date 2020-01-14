cmake_minimum_required(VERSION 3.0)
project(myserver)

set(open62541_DIR "${CMAKE_SOURCE_DIR}/../open62541")
set(UA_NAMESPACE_ZERO FULL CACHE STRING "" FORCE)
set(CMAKE_BUILD_TYPE MinSizeRel)
set(open62541_TOOLS_DIR "${CMAKE_SOURCE_DIR}/../open62541/tools")
set(open62541_NODESET_DIR "${CMAKE_SOURCE_DIR}/../open62541/deps/ua-nodeset")
add_subdirectory(${open62541_DIR} open62541)

add_executable(myserver myserver.c)
target_include_directories(myserver PRIVATE "${open62541_DIR}/include"})
target_link_libraries(myserver open62541::open62541)
