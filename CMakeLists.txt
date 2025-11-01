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

# Collect CMSIS DSP sources using glob (博客方法)
file(GLOB_RECURSE CMSIS_DSP_SOURCES "embedded_framework/code/algorithms/cmsis_dsp/Source/*.*")
list(FILTER CMSIS_DSP_SOURCES INCLUDE REGEX ".*\\.c$")  # 只包含.c文件
# 排除NEON相关文件（STM32H7不支持NEON）
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*neon.*")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*_neon\\.c$")
# 排除排序相关文件（缺少头文件）
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*sort.*")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*bitonic.*")
# 排除包含排序文件的SupportFunctions.c
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/SupportFunctions\\.c$")
# 排除聚合文件（避免重复定义）- 这些文件包含其他.c文件
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/BasicMathFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/BayesFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/CommonTables\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/ComplexMathFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/ControllerFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/DistanceFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/FastMathFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/FilteringFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/InterpolationFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/MatrixFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/QuaternionMathFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/StatisticsFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/SVMFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/TransformFunctions\\.c$")
list(FILTER CMSIS_DSP_SOURCES EXCLUDE REGEX ".*/WindowFunctions\\.c$")

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
        embedded_framework/code/algorithms/mahony/MahonyAHRS.c

        # CMSIS DSP Library (使用通配符包含所有源文件)
        ${CMSIS_DSP_SOURCES}

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
        embedded_framework/code/algorithms/mahony
        embedded_framework/code/algorithms/cmsis_dsp/Include

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
    ARM_MATH_CM7
    ARM_MATH_MATRIX_CHECK
    ARM_MATH_ROUNDING
    # __FPU_PRESENT=1  # 已由STM32头文件定义，避免重复定义警告
)

# Remove wrong libob.a library dependency when using cpp files
list(REMOVE_ITEM CMAKE_C_IMPLICIT_LINK_LIBRARIES ob)

# Add linked libraries
target_link_libraries(${CMAKE_PROJECT_NAME}
    stm32cubemx
    m  # 数学库，CMSIS DSP需要

    # Add user defined libraries
)
