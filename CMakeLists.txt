cmake_minimum_required(VERSION 3.14)

project(mylib)

# 追記
enable_testing()

# in-source ビルドガード
if(${CMAKE_SOURCE_DIR} STREQUAL ${CMAKE_BINARY_DIR})
    message(FATAL_ERROR "In-source builds not allowed. Please make a new directory (called a build directory) and run CMake from there. You may need to remove CMakeCache.txt. ")
endif()

# サブディレクトリ内の CMakeLists.txt も読込み
add_subdirectory(lib)
add_subdirectory(test EXCLUDE_FROM_ALL)