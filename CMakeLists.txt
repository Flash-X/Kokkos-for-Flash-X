# Minimum required for Apple Silicon support (among other things)
cmake_minimum_required(VERSION 3.19)

set(CMAKE_DISABLE_IN_SOURCE_BUILD ON)

project(
    kokkos_flashx
    DESCRIPTION "Minimal Fortran interface to Kokkos for use in Flash-X"
    LANGUAGES Fortran CXX
)

# TODO: condition this on whether unit tests or examples are being built
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# Technically only needs C++17, but there will (eventually) be some GR-solver kernels
# that will make use of C++20 features (e.g., concepts in tensor expression template code)
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Set the installation location to the build directory by defualt
# if an installation directory was not provided
if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}" CACHE PATH "Default installation directory" FORCE)
endif()

# Kokkos is included directly as a submodule so there is no need for find_package()
add_subdirectory(kokkos)

# Add the library source directory (will result in fkokkos-fort and fkokkos-cxx targets)
add_subdirectory(flcl)
