cmake_minimum_required(VERSION 3.16)
project(ProtoExample LANGUAGES CXX)

# --- C++ standard ---
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# --- Default to Release if nothing specified ---
if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
    set(CMAKE_BUILD_TYPE Release CACHE STRING "Build type" FORCE)
    set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS
        Debug Release RelWithDebInfo MinSizeRel)
endif()

# --- Find Wt ---
# Wt 4 splits itself into components. We need the core lib and the HTTP connector.
find_package(Wt REQUIRED COMPONENTS Wt HTTP)

# --- The executable ---
add_executable(testweb
    ProtoExample.cpp
)


target_link_libraries(testweb PRIVATE
    Wt::Wt
    Wt::HTTP
)

# --- Warnings (optional but nice) ---
if(CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    target_compile_options(testweb PRIVATE -Wall -Wextra -Wpedantic)
endif()

# --- Convenience: print how to run it after building ---
add_custom_command(TARGET testweb POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E echo
        "Run with: ./testweb --docroot . --http-address 0.0.0.0 --http-port 8080"
)
