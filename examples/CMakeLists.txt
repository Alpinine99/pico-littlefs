cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

include($ENV{PICO_SDK_PATH}/external/pico_sdk_import.cmake)

project(example C CXX ASM)

pico_sdk_init()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DPICO_USE_MALLOC_MUTEX=1")

include(../littlefs-lib/CMakeLists.txt)
include(../stdinit-lib/CMakeLists.txt)

foreach(name IN ITEMS example0 example1 example2)
    add_executable(${name} 
      ${name}.c
    )
    
    target_link_libraries(${name} 
      pico_stdlib 
      littlefs-lib 
      stdinit-lib
    )
    
    pico_enable_stdio_uart(${name} 0)
    pico_enable_stdio_usb(${name} 1)
    
    pico_add_extra_outputs(${name})
endforeach()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g")

