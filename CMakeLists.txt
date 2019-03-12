cmake_minimum_required (VERSION 2.8.8)
project (arduino_mock)

find_package(Threads REQUIRED)
find_package(gtest REQUIRED)
#add_subdirectory(lib/gtest)
#add_subdirectory(lib/gmock)

cmake_policy(SET CMP0048 NEW)
#project(gtest VERSION ${GOOGLETEST_VERSION} LANGUAGES CXX C)
if (POLICY CMP0063) # Visibility
  cmake_policy(SET CMP0063 NEW)
endif (POLICY CMP0063)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)


message ("building arduino_mock")
message("Gtest include: ${GTEST_INCLUDE_DIRS}")

include_directories(${GTEST_INCLUDE_DIRS}  include)
include_directories(SYSTEM)
LINK_DIRECTORIES(/usr/local/lib)

file(GLOB SRC
    "src/ArduinoMockAll.cc"
)

add_library(arduino_mock STATIC ${SRC})

target_include_directories(arduino_mock
    PUBLIC "include"
)

target_link_libraries(arduino_mock
    ${GTEST_BOTH_LIBRARIES}
    ${CMAKE_THREAD_LIBS_INIT}
)

set_target_properties( arduino_mock
  PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dist/lib"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dist/lib"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/dist/bin"
)

add_dependencies(arduino_mock gtest gmock)

option(test "Build all tests." OFF)

if (test)
  enable_testing()
  add_subdirectory(test)
endif()
