cmake_minimum_required(VERSION 3.22)

#
# This file is generated only once,
# and is not re-generated if converter is called multiple times.
#
# User is free to modify the file as much as necessary
#

# Setup compiler settings
set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)


# Define the build type
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

# Set the project name
set(CMAKE_PROJECT_NAME Four-link)

# Enable compile command to ease indexing with e.g. clangd
set(CMAKE_EXPORT_COMPILE_COMMANDS TRUE)

# Core project settings
project(${CMAKE_PROJECT_NAME})
message("Build type: " ${CMAKE_BUILD_TYPE})

# Enable CMake support for ASM and C languages
enable_language(C ASM)

# Create an executable object type
add_executable(${CMAKE_PROJECT_NAME})

# Add STM32CubeMX generated sources
add_subdirectory(cmake/stm32cubemx)

# Link directories setup
target_link_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined library search paths
)

# Add sources to executable
target_sources(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user sources here
        embedded_framework/code/platform/log/SEGGER_RTT.c
        embedded_framework/code/platform/log/SEGGER_RTT_printf.c
        embedded_framework/code/platform/log/plf_log.c

#        embedded_framework/code/Bsp/can/bsp_fdcan.c
#        embedded_framework/code/bsp/can/bsp_can.c
        embedded_framework/code/bsp/usart/bsp_usart.c
        embedded_framework/code/bsp/dwt/bsp_dwt.c
        embedded_framework/code/bsp/gpio/bsp_gpio.c
        embedded_framework/code/bsp/spi/bsp_spi.c
        embedded_framework/code/bsp/bmi088/bmi088.c

        embedded_framework/code/algorithms/data_structure/circular_queue.c
        embedded_framework/code/algorithms/calculate/basic_math.c

        embedded_framework/code/modules/serial_protocols/sbus/sbus.c
        embedded_framework/code/modules/remote_control/fs_i6x/fs_i6x.c
        embedded_framework/code/modules/watchdog/watch_dog.c
        embedded_framework/code/modules/imu/attitude_estimation.c

        embedded_framework/code/app/cmd/cmd_fun.c
        embedded_framework/code/app/init/app_init.c
        embedded_framework/code/app/monitor/monitor_fun.c
)

# Add include paths
target_include_directories(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined include paths

        embedded_framework/code/platform/log

        embedded_framework/code/bsp/usart
        embedded_framework/code/bsp/typedef
#        embedded_framework/code/bsp/can
        embedded_framework/code/bsp/dwt
        embedded_framework/code/bsp/gpio
        embedded_framework/code/bsp/spi
        embedded_framework/code/bsp/bmi088

        embedded_framework/code/algorithms/memory
        embedded_framework/code/algorithms/data_structure
        embedded_framework/code/algorithms/calculate

        embedded_framework/code/modules/serial_protocols/sbus
        embedded_framework/code/modules/remote_control/fs_i6x
        embedded_framework/code/modules/typedef
        embedded_framework/code/modules/watchdog
        embedded_framework/code/modules/imu


        embedded_framework/code/app/cmd
        embedded_framework/code/app/init
        embedded_framework/code/app/monitor

        embedded_framework/code/config
)

# Add project symbols (macros)
target_compile_definitions(${CMAKE_PROJECT_NAME} PRIVATE
    # Add user defined symbols
)

# Remove wrong libob.a library dependency when using cpp files
list(REMOVE_ITEM CMAKE_C_IMPLICIT_LINK_LIBRARIES ob)

# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME}
    stm32cubemx

    # Add user defined libraries
)
