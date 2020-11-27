cmake_minimum_required(VERSION 3.10)
project(simple)

set(LIBRARY_NAME simple_lib)

set(CMAKE_CXX_STANDARD 11)

################################################################################
# Dependencies
################################################################################

find_package(OpenCV REQUIRED)

################################################################################
# Sources
################################################################################

set(root ${CMAKE_CURRENT_SOURCE_DIR})

set(HEADER_FILES
        ${root}/include/simple.h
)

set(SOURCE_FILES
        ${root}/src/simple.cpp
)

# Organize the source and header files into groups
source_group("Headers" FILES ${HEADER_FILES})
source_group("Source" FILES ${SOURCE_FILES})

if(CMAKE_VERSION VERSION_GREATER 3.8)
    source_group(TREE ${root} FILES ${HEADER_FILES} ${SOURCE_FILES})
endif()


################################################################################
# Build
################################################################################

include_directories(
        ${CMAKE_CURRENT_BINARY_DIR}
        ${CMAKE_BINARY_DIR}
        ${CMAKE_CURRENT_SOURCE_DIR}/include
        ${OpenCV_INCLUDE_DIR}
)

add_library(${LIBRARY_NAME} SHARED
        ${SOURCE_FILES}
        ${HEADER_FILES}
)

target_link_libraries(${LIBRARY_NAME}
        ${OpenCV_LIBRARIES}
)

################################################################################
# Executables
################################################################################

add_executable(simple_exe main.cpp)
target_link_libraries(simple_exe
        PUBLIC
            ${LIBRARY_NAME}
        )
