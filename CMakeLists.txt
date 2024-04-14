# Generated Cmake Pico project file

cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)
set(PICO_SDK_PATH "/data/tom/git/pico-sdk")

set(PICO_BOARD pico CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

if (PICO_SDK_VERSION_STRING VERSION_LESS "1.4.0")
  message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.4.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

project(1d_pong C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1
add_executable(1d_pong 1d_pong.c ws2812/animations.c)

pico_set_program_name(1d_pong "1d_pong")
pico_set_program_version(1d_pong "0.1")

pico_enable_stdio_uart(1d_pong 1)
pico_enable_stdio_usb(1d_pong 0)


file(MAKE_DIRECTORY ${CMAKE_CURRENT_LIST_DIR}/ws2812/generated)

# generate the header file into the source tree as it is included in the RP2040 datasheet
pico_generate_pio_header(1d_pong ${CMAKE_CURRENT_LIST_DIR}/ws2812/ws2812.pio OUTPUT_DIR ${CMAKE_CURRENT_LIST_DIR}/ws2812/generated)


# Add the standard library to the build
target_link_libraries(1d_pong
        pico_stdlib)

# Add the standard include files to the build
target_include_directories(1d_pong PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
  ${CMAKE_CURRENT_LIST_DIR}/.. # for our common lwipopts or any other standard includes, if required
)

# Add any user requested libraries
target_link_libraries(1d_pong pico_stdlib hardware_pio)

pico_add_extra_outputs(1d_pong)

