cmake_minimum_required(VERSION 3.9)
project(Fc)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

add_subdirectory(third_party/gtest)
add_subdirectory(test)