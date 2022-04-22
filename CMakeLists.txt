cmake_minimum_required(VERSION 3.13)

project(MVNtoSteamVR VERSION 0.0.1)

# Use C14
set(CMAKE_CXX_STANDARD 17)
set(CXX_STANDARD_REQUIRED)

# Setup Cmake paths
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake;${CMAKE_MODULE_PATH}")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin/${CMAKE_SYSTEM_NAME})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${CMAKE_BINARY_DIR}/bin/${CMAKE_SYSTEM_NAME})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${CMAKE_BINARY_DIR}/bin/${CMAKE_SYSTEM_NAME})

# Detect 64bit
if(${CMAKE_SIZEOF_VOID_P} STREQUAL "8")
  set(64bit TRUE)
elseif(${CMAKE_SIZEOF_VOID_P} STREQUAL "8")
  set(64bit FALSE)
endif()

# Xsens types library
find_package(xstypes REQUIRED)

include(FetchContent)
FetchContent_Declare(
  SimpleOpenVRBridge
  GIT_REPOSITORY https://github.com/Mystfit/Simple-OpenVR-Bridge-Driver.git
)
FetchContent_MakeAvailable(SimpleOpenVRBridge)

set(SOURCE_DIR "${CMAKE_CURRENT_LIST_DIR}/src")
set(BRIDGE_HEADERS 
    "${SOURCE_DIR}/angularsegmentkinematicsdatagram.h"
    "${SOURCE_DIR}/centerofmassdatagram.h"
    "${SOURCE_DIR}/datagram.h"
    "${SOURCE_DIR}/eulerdatagram.h"
    "${SOURCE_DIR}/jointanglesdatagram.h"
    "${SOURCE_DIR}/linearsegmentkinematicsdatagram.h"
    "${SOURCE_DIR}/metadatagram.h"
    "${SOURCE_DIR}/parsermanager.h"
    "${SOURCE_DIR}/positiondatagram.h"
    "${SOURCE_DIR}/quaterniondatagram.h"
    "${SOURCE_DIR}/scaledatagram.h"
    "${SOURCE_DIR}/streamer.h"
    "${SOURCE_DIR}/timecodedatagram.h"
    "${SOURCE_DIR}/trackerkinematicsdatagram.h"
    "${SOURCE_DIR}/udpserver.h"
)

set(BRIDGE_SRC
    "${SOURCE_DIR}/angularsegmentkinematicsdatagram.cpp"
    "${SOURCE_DIR}/centerofmassdatagram.cpp"
    "${SOURCE_DIR}/datagram.cpp"
    "${SOURCE_DIR}/eulerdatagram.cpp"
    "${SOURCE_DIR}/jointanglesdatagram.cpp"
    "${SOURCE_DIR}/linearsegmentkinematicsdatagram.cpp"
    "${SOURCE_DIR}/metadatagram.cpp"
    "${SOURCE_DIR}/parsermanager.cpp"
    "${SOURCE_DIR}/positiondatagram.cpp"
    "${SOURCE_DIR}/quaterniondatagram.cpp"
    "${SOURCE_DIR}/scaledatagram.cpp"
    "${SOURCE_DIR}/streamer.cpp"
    "${SOURCE_DIR}/timecodedatagram.cpp"
    "${SOURCE_DIR}/trackerkinematicsdatagram.cpp"
    "${SOURCE_DIR}/udpserver.cpp"
)

set(BRIDGE_APP MVNtoSteamVRBridge)

add_executable(${BRIDGE_APP} "${SOURCE_DIR}/main.cpp")

# Include files in target
target_sources(${BRIDGE_APP} PRIVATE 
    ${BRIDGE_HEADERS}
    ${BRIDGE_SRC}
)

get_target_property(SimpleOpenVRBridge_INCLUDES driver_apriltagtrackers INCLUDE_DIRECTORIES)
target_include_directories(${BRIDGE_APP} PRIVATE ${SimpleOpenVRBridge_INCLUDES})

# Link libraries
target_link_libraries(${BRIDGE_APP} PRIVATE xstypes)
# set_target_properties(${BRIDGE_APP} PROPERTIES LINK_FLAGS "/SUBSYSTEM:WINDOWS")
