# Adapted from https://github.com/opencv/opencv/blob/master/3rdparty/ippicv/CMakeLists.txt
cmake_minimum_required(VERSION 3.0)
project(ippiw)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)
add_compile_definitions(IW_BUILD ICV_BASE)
include_directories(icv/include iw/include)
file(GLOB lib_srcs iw/src/*.c)
file(GLOB lib_hdrs iw/include/*.h iw/include/iw/*.h iw/include/iw++/*.hpp)
file(GLOB IPPICV_LIBRARY
    icv/lib/*/${CMAKE_STATIC_LIBRARY_PREFIX}*${CMAKE_STATIC_LIBRARY_SUFFIX})

add_library(ippiw STATIC ${lib_srcs} ${lib_hdrs})

if(UNIX)
    if(${CMAKE_C_COMPILER_ID} MATCHES GNU OR
            ${CMAKE_C_COMPILER_ID} MATCHES Clang OR
            ${CMAKE_C_COMPILER_ID} MATCHES Intel)
        add_compile_options(-Wno-unused-function -Wno-missing-braces -Wno-missing-field-initializers)
  endif()
  if(${CMAKE_C_COMPILER_ID} MATCHES Clang)
      add_compile_options(-Wno-self-assign)
  endif()
endif()

install(TARGETS ippiw
    ARCHIVE DESTINATION lib)
install(FILES ${IPPICV_LIBRARY}
    DESTINATION lib)
install(DIRECTORY iw/include/ DESTINATION include
    FILES_MATCHING PATTERN "*.h*")
install(DIRECTORY icv/include/ DESTINATION include/icv
    FILES_MATCHING PATTERN "*.h*")
