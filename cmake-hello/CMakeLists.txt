# Required as the first line
cmake_minimum_required(VERSION 3.25)

# Define the crrent project
project(CMakeHello VERSION 1.0.0)

# Tell Abseil which C++ standard to use
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(ABSL_PROPAGATE_CXX_STD ON)
enable_testing()
# Process abseil with CMake
include(FetchContent)
FetchContent_Declare(
    absl
    GIT_REPOSITORY https://github.com/abseil/abseil-cpp.git
    GIT_TAG 20240116.0
)
FetchContent_MakeAvailable(absl)
include(FetchContent)
FetchContent_Declare(
  googletest
  GIT_REPOSITORY https://github.com/google/googletest.git
  GIT_TAG v1.14.0
)
# For Windows: Prevent overriding the parent project's compiler/linker settings
set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)
add_subdirectory(hello)
add_subdirectory(hello_main)
