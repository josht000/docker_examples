cmake_minimum_required(VERSION 3.10)

project(cpp17)

set_target_properties(FooTarget PROPERTIES
    CXX_STANDARD 17
    CXX_EXTENSIONS OFF
    CXX_STANDARD_REQUIRED ON
)

set(sources main.cpp)

add_executable(cpp17_test ${sources})

