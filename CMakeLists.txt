
cmake_minimum_required(VERSION 3.24...3.31)

project(gitty.exemplar
        LANGUAGES CXX
        VERSION 0.1.0
)

enable_testing()

option(
        GITTY_EXEMPLAR_INSTALL_CONFIG_FILE_PACKAGE
        "Enable creating and installing a CMake config-file package. Default: ${PROJECT_IS_TOP_LEVEL}. Values: { ON, OFF }."
        ${PROJECT_IS_TOP_LEVEL}
)

include(FetchContent)
include(GNUInstallDirs)

add_subdirectory(src/gitty/exemplar)
