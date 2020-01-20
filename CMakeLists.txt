cmake_minimum_required(VERSION 3.0)
project(myserver)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Debug)
  # set(CMAKE_BUILD_TYPE MinSizeRel)
endif()

set(open62541_DIR ${PROJECT_SOURCE_DIR}/../open62541)
set(open62541_BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR}/../open62541/)
set(open62541_TOOLS_DIR "${PROJECT_SOURCE_DIR}/../open62541/tools")
set(open62541_NODESET_DIR "${PROJECT_SOURCE_DIR}/../open62541/deps/ua-nodeset")
add_subdirectory(${open62541_DIR} open62541 EXCLUDE_FROM_ALL)


# Generate types and namespace for DI
ua_generate_nodeset_and_datatypes(
    NAME "di"
#    FILE_CSV "${COMPANION_NODESET_DIR}/DI/OpcUaDiModel.csv"
    FILE_CSV "${open62541_NODESET_DIR}/DI/OpcUaDiModel.csv"
#    FILE_BSD "${COMPANION_NODESET_DIR}/DI/Opc.Ua.Di.Types.bsd"
    FILE_BSD "${open62541_NODESET_DIR}/DI/Opc.Ua.Di.Types.bsd"
    NAMESPACE_IDX 2
#    FILE_NS "${COMPANION_NODESET_DIR}/DI/Opc.Ua.Di.NodeSet2.xml"
    FILE_NS "${open62541_NODESET_DIR}/DI/Opc.Ua.Di.NodeSet2.xml"
    INTERNAL
)

# generate Robotics namespace which is using DI
ua_generate_nodeset_and_datatypes(
    NAME "plc"
#    FILE_NS "${COMPANION_NODESET_DIR}/Robotics/Opc.Ua.Robotics.NodeSet2.xml"
    FILE_NS "${open62541_NODESET_DIR}/PLCopen/Opc.Ua.Plc.NodeSet2.xml"
    # PLCopen depends on the di nodeset, which must be generated before
    DEPENDS "di"
    INTERNAL
)


add_executable(myserver myserver.c ${UA_NODESET_DI_SOURCES} ${UA_NODESET_PLC_SOURCES})
add_dependencies(myserver open62541-generator-ns-plc)
target_include_directories(myserver PRIVATE
  ${open62541_DIR}/include
  ${open62541_DIR}/plugins/include
  ${open62541_DIR}/arch
  ${open62541_DIR}/deps
  ${open62541_BINARY_DIR}/src_generated
  ${CMAKE_CURRENT_BINARY_DIR}/src_generated)
target_link_libraries(myserver PRIVATE ${open62541_BINARY_DIR}/bin/libopen62541.a)
