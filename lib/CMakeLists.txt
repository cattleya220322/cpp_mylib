# mylibを共有ライブラリとして生成するためのオプションの定義
option(MYLIB_BUILD_SHARED_LIBS "build mylib as a shared library" OFF)

# オプション定義によって生成するライブラリの種類(SHARED/STATIC)を切り替える部分
if (MYLIB_BUILD_SHARED_LIBS)
    add_library(mylib SHARED)
else()
    add_library(mylib STATIC)
endif()

# mylibを生成するために必要なソースファイル
target_sources(mylib
    PRIVATE # Split.cpp は mylib ライブラリを生成するためだけに必要なので PRIVATE
    Split.cpp
)

# 追記
# C++ の標準規格の指定
target_compile_features(
    mylib
    PRIVATE
    cxx_std_14
)

# mylibを生成するために必要なヘッダファイルが配置されているディレクトリを指定する
target_include_directories(mylib
    # Split.hなどはmylibライブラリを生成するのに加えて、
    # mylibライブラリを利用する他のバイナリなどでも利用するのでPUBLIC
    PUBLIC 
    ${PROJECT_SOURCE_DIR}/include
)

# GNUのディレクトリ構造で定義されたCMAKE_INSTALL_<dir>の変数を読み込む
include(GNUInstallDirs)
# install mylib library
install(TARGETS mylib
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
)
# install headers
install(DIRECTORY ${CMAKE_SOURCE_DIR}/include/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    FILES_MATCHING PATTERN "*.h"
)