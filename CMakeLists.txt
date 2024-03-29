cmake_minimum_required(VERSION 3.13)

# Set the arm toolchain path 
if ( NOT ARM_TOOLCHAIN_PATH )
    set( ARM_TOOLCHAIN_PATH /opt/toolchain/gcc-arm-none-eabi-10.3-2021.10/ )
endif()
message("Using following path for arm toolchain: ${ARM_TOOLCHAIN_PATH}")

# Set toolchain file from the platform (Needs to be set before the project name)
set(CMAKE_TOOLCHAIN_FILE ${CMAKE_CURRENT_LIST_DIR}/libs/platforms/toolchain/arm-none-eabi.cmake)

project(efr32_trace_playground)

enable_language(C CXX ASM)

# Generate compile_commands.json for intellisense
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Add some binary output directories 
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)


##### Project Setup #####

# ${EFR32_DEVICE} sets the target specific model name
if (NOT EFR32_DEVICE)
    set(EFR32_DEVICE EFR32MG12P332F1024GL125)  # Sets device / used to locate HAL files
    set(BOARD BRD4166A)				 # Thunderboard Sense 2
    set(BL_MESH ON)
    set(BL_PSSTORE ON)
endif ()
message("EFR32 Device used: ${EFR32_DEVICE}")

# Add additional modules
include(${CMAKE_CURRENT_LIST_DIR}/libs/platforms/toolchain/firmware.cmake)
add_subdirectory(libs/silabs/sdk_support)

# Add the subdirs with the actual code
add_subdirectory(src)


                   
                  