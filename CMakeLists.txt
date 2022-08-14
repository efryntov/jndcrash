cmake_minimum_required(VERSION 3.4.1)
project(jndcrash)

# Setting default configuration parameters values. Everything is ON.
set(ENABLE_INPROCESS OFF)
set(ENABLE_OUTOFPROCESS ON)
set(ENABLE_OUTOFPROCESS_ALL_THREADS OFF)
set(ENABLE_LIBCORKSCREW OFF)
set(ENABLE_LIBUNWIND ON)
set(ENABLE_LIBUNWINDSTACK OFF)
set(ENABLE_CXXABI OFF)
set(ENABLE_STACKSCAN OFF)

# Optional JNDCrash build config.
include(${CMAKE_SOURCE_DIR}/../jndcrash.cmake OPTIONAL)

# Adding NDCrash submodule subdirectory
add_subdirectory(libndcrash)

# Setting up definitions. The same as used for NDCrash.
if (${ENABLE_INPROCESS})
    add_definitions(-DENABLE_INPROCESS)
endif()
if (${ENABLE_OUTOFPROCESS})
    add_definitions(-DENABLE_OUTOFPROCESS)
endif()

# NDCrash submodule include path.
include_directories(${CMAKE_SOURCE_DIR}/libndcrash/include)

# JNDCrash source code.
file(GLOB JNDCRASH_SOURCES ${CMAKE_SOURCE_DIR}/src/main/jni/*.c)

# Adding dynamic library and linking it with log and NDCrash.
add_library(jndcrash SHARED ${JNDCRASH_SOURCES})
find_library(LOG_LIB log)
target_link_libraries(jndcrash ndcrash ${LOG_LIB})
