set(EXE_NAME mv)

set(OPENCL_WRAPPER_INCLUDE_PATH ${SUPPORT_PATH}/${OPENCL_WRAPPER_DIR}/${OPENCL_WRAPPER_INCLUDE_DIR}/${OPENCL_WRAPPER_CODE_LIB_DIR})
set(BENCH_SUPPORT_INCLUDE_PATH ${SUPPORT_PATH}/${BENCH_SUPPORT_LIB_DIR}/${BENCH_SUPPORT_INCLUDE_DIR})
set(BUILD_DIR ${CMAKE_BINARY_DIR}/${BENCHMARKS_DIR}/${MATRIX_DIR}/${MV_DIR})

include_directories(${OPENCL_WRAPPER_INCLUDE_PATH})
include_directories(${BENCH_SUPPORT_INCLUDE_PATH})
include_directories(${BOOST_INCLUDE_PATHS})
include_directories(${BUILD_DIR})

# Full paths.
set(KERNEL_DIRECTORY "${MV_PATH}/")

# Src files.
file(GLOB SOURCE_FILES_LIST "${MV_PATH}/*.cpp")
add_executable(${EXE_NAME} ${SOURCE_FILES_LIST})

configure_file(${MV_PATH}/SystemConfiguration.h.cmake 
               ${BUILD_DIR}/SystemConfiguration.h)

#-------------------------------------------------------------------------------
# Libraries.

# OpenCL library.
target_link_libraries(${EXE_NAME} ${OPENCL_LIB_PATH})

# OpenCLWrapper Utils library.
link_directories(${CMAKE_BINARY_DIR}/${SUPPORT_DIR}/${OPENCL_WRAPPER_DIR}/${OPENCL_WRAPPER_LIB_DIR}/${OPENCL_WRAPPER_UTILS_LIB_DIR})
target_link_libraries(${EXE_NAME} ${OPENCL_WRAPPER_UTILS_LIB})
                                      
# OpenCLWrapper library.              
link_directories(${CMAKE_BINARY_DIR}/${SUPPORT_DIR}/${OPENCL_WRAPPER_DIR}/${OPENCL_WRAPPER_LIB_DIR}/${OPENCL_WRAPPER_CODE_LIB_DIR})
target_link_libraries(${EXE_NAME} ${OPENCL_WRAPPER_LIB})

# BenchSupport library.
link_directories(${CMAKE_BINARY_DIR}/${SUPPORT_DIR}/${BENCH_SUPPORT_LIB_DIR})
target_link_libraries(${EXE_NAME} ${BENCH_SUPPORT_LIB})

# Boost FileSystem library.
find_library(BOOST_FILESYSTEM_LIBRARY_PATH boost_filesystem ${BOOST_LIB_PATHS}
             DOC "The Boost FileSystem Library")
if(NOT EXISTS ${BOOST_FILESYSTEM_LIBRARY_PATH})
  message(FATAL_ERROR "Boost-FileSystem library not found")
endif(NOT EXISTS ${BOOST_FILESYSTEM_LIBRARY_PATH})

target_link_libraries(${EXE_NAME} ${BOOST_FILESYSTEM_LIBRARY_PATH})
 
# Boost System library.
find_library(BOOST_SYSTEM_LIBRARY_PATH boost_system ${BOOST_LIB_PATHS}
              DOC "The Boost-System Library")
if(NOT EXISTS ${BOOST_SYSTEM_LIBRARY_PATH})
   message(FATAL_ERROR "Boost-System library not found")
endif(NOT EXISTS ${BOOST_SYSTEM_LIBRARY_PATH})
target_link_libraries(${EXE_NAME} ${BOOST_SYSTEM_LIBRARY_PATH})

#Boost Program Options library
find_library(BOOST_PROGRAM_OPTS_LIBRARY_PATH boost_program_options ${BOOST_LIB_PATHS}
              DOC "The Boost Program Options Library")
if(NOT EXISTS ${BOOST_PROGRAM_OPTS_LIBRARY_PATH})
   message(FATAL_ERROR "Boost Program Options library not found")
endif(NOT EXISTS ${BOOST_PROGRAM_OPTS_LIBRARY_PATH})
target_link_libraries(${EXE_NAME} ${BOOST_PROGRAM_OPTS_LIBRARY_PATH})
