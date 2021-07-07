cmake_minimum_required(VERSION 3.10)
project(tm_classification)

set(CMAKE_CXX_STANDARD 11)

include_directories(tengine/include/ src/common/)

add_library(tengine SHARED IMPORTED)
set_target_properties(tengine PROPERTIES IMPORTED_LOCATION ${CMAKE_SOURCE_DIR}/tengine/lib/libtengine-lite.so)

add_executable(tm_classification src/tm_classification.c  "src/common/tengine_operations.c" )

target_link_libraries(tm_classification -lm tengine)