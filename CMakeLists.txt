# Specify the minimum CMake version required for this project
cmake_minimum_required(VERSION 3.14)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Define the project name and set the C++ standard
project(BeLightVulkan)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set build output directories to keep build artifacts organized
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

# Find required system packages using pkg-config
find_package(PkgConfig REQUIRED)
pkg_check_modules(GLFW glfw3)

# List all source files that need to be compiled into the executable
set(SOURCES
    src/main.cpp
)

# Create the executable target named "BeLightVulkan" from the specified source files
add_executable(BeLightVulkan ${SOURCES})

# Enable Vulkan-Hpp designated initializers
target_compile_definitions(BeLightVulkan PRIVATE
    VULKAN_HPP_NO_STRUCT_CONSTRUCTORS
    VULKAN_HPP_HANDLE_ERROR_OUT_OF_DATE_AS_SUCCESS
)

# Link the required system libraries to the executable
target_link_libraries(BeLightVulkan
    ${GLFW_LIBRARIES}
    dl
)

# Include the directories that contain headers for the linked libraries and fetched dependencies
target_include_directories(BeLightVulkan PRIVATE
    include
    $ENV{VULKAN_SDK}/include
    ${GLFW_INCLUDE_DIRS}
)
