cmake_minimum_required(VERSION 3.12)

include(pico_sdk_import.cmake)

set(FREERTOS_KERNEL_PATH ${CMAKE_CURRENT_LIST_DIR}/freertos)
include(FreeRTOS_Kernel_import.cmake)

project(base-freertos)

pico_sdk_init()

add_executable(base-freertos
        src/freertos_hello_world.c
)

target_include_directories(base-freertos PRIVATE
        include/
        )

target_compile_options(base-freertos PRIVATE -Wall)

if (DEFINED ENV{BASE_FREERTOS_LED})
        message("base-freertos_LED is defined as " $ENV{BASE_FREERTOS_LED})
        target_compile_definitions(base-freertos PRIVATE BASE_FREERTOS_LED=$ENV{BASE_FREERTOS_LED})
endif()

target_include_directories(base-freertos PRIVATE src)

#target_compile_definitions (base-freertos PRIVATE
#	EXAMPLE_DEFINITION=1
#)

target_link_libraries(base-freertos PRIVATE
        pico_multicore
        pico_stdlib
        pico_unique_id
        FreeRTOS-Kernel
        FreeRTOS-Kernel-Heap1
)

pico_set_binary_type(base-freertos copy_to_ram)

pico_add_extra_outputs(base-freertos)
