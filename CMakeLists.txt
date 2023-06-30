#THIS FILE IS AUTO GENERATED FROM THE TEMPLATE! DO NOT CHANGE!
#THIS FILE IS AUTO GENERATED FROM THE TEMPLATE! DO NOT CHANGE!
set(CMAKE_SYSTEM_NAME Generic)
set(CMAKE_SYSTEM_VERSION 1)
cmake_minimum_required(VERSION 3.10)

# specify cross compilers and tools
set(CMAKE_C_COMPILER arm-none-eabi-gcc)
set(CMAKE_CXX_COMPILER arm-none-eabi-g++)
set(CMAKE_ASM_COMPILER  arm-none-eabi-gcc)
set(CMAKE_AR arm-none-eabi-ar)
set(CMAKE_OBJCOPY arm-none-eabi-objcopy)
set(CMAKE_OBJDUMP arm-none-eabi-objdump)
set(SIZE arm-none-eabi-size)
set(CMAKE_TRY_COMPILE_TARGET_TYPE STATIC_LIBRARY)

# project settings
project(OKMX8MM_Template C CXX ASM)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_C_STANDARD 11)

#Uncomment for hardware floating point
add_compile_definitions(ARM_MATH_CM4;ARM_MATH_MATRIX_CHECK;ARM_MATH_ROUNDING)
add_compile_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)
add_link_options(-mfloat-abi=hard -mfpu=fpv4-sp-d16)

add_compile_options(
        -DCPU_MIMX8MM6DVTLZ
        -DFSL_RTOS_FREE_RTOS
        -Os
        -Wall
        -fno-common
        -ffunction-sections
        -fdata-sections
        -ffreestanding
        -fno-builtin
        -mthumb
        -mapcs
        -mcpu=cortex-m4
        -mfloat-abi=hard
        -mfpu=fpv4-sp-d16
        -MMD
        -MP
)

if ("${CMAKE_BUILD_TYPE}" STREQUAL "Release")
    message(STATUS "Maximum optimization for speed")
    add_compile_options(-Ofast)
elseif ("${CMAKE_BUILD_TYPE}" STREQUAL "RelWithDebInfo")
    message(STATUS "Maximum optimization for speed, debug info included")
    add_compile_options(-Ofast -g)
elseif ("${CMAKE_BUILD_TYPE}" STREQUAL "MinSizeRel")
    message(STATUS "Maximum optimization for size")
    add_compile_options(-Os)
else ()
    message(STATUS "Minimal optimization, debug info included")
    add_compile_options(-Og -g)
endif ()

include_directories(Core/Inc amazon-freertos/lib/FreeRTOS/portable/GCC/ARM_CM4F CMSIS/Include CMSIS/Driver/Include devices amazon-freertos/lib/include amazon-freertos/lib/include/private devices/MIMX8MM6/drivers devices/MIMX8MM6 devices/MIMX8MM6/utilities/str devices/MIMX8MM6/utilities devices/MIMX8MM6/cmsis_drivers devices/MIMX8MM6/template)

file(GLOB_RECURSE SOURCES "Core/*.*" "devices/*.*" "amazon-freertos/*.*" "CMSIS/*.*")

set(LINKER_SCRIPT ${CMAKE_SOURCE_DIR}/MIMX8MM6xxxxx_cm4_ram.ld)

add_link_options(-Wl,-gc-sections,--print-memory-usage,-Map=${PROJECT_BINARY_DIR}/${PROJECT_NAME}.map)
add_link_options(-mcpu=cortex-m4 -mthumb -mthumb-interwork)
add_link_options(-T ${LINKER_SCRIPT})

add_link_options(
        --specs=nano.specs
        --specs=nosys.specs
        -Wall
        -fno-common
        -ffunction-sections
        -fdata-sections
        -ffreestanding
        -fno-builtin
        -mthumb
        -mapcs
        --data-sections
        -Xlinker
        -static
        -z
        muldefs
)

add_executable(${PROJECT_NAME}.elf ${SOURCES} ${LINKER_SCRIPT})

set(HEX_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.hex)
set(BIN_FILE ${PROJECT_BINARY_DIR}/${PROJECT_NAME}.bin)

add_custom_command(TARGET ${PROJECT_NAME}.elf POST_BUILD
        COMMAND ${CMAKE_OBJCOPY} -Oihex $<TARGET_FILE:${PROJECT_NAME}.elf> ${HEX_FILE}
        COMMAND ${CMAKE_OBJCOPY} -Obinary $<TARGET_FILE:${PROJECT_NAME}.elf> ${BIN_FILE}
        COMMENT "Building ${HEX_FILE}
Building ${BIN_FILE}")