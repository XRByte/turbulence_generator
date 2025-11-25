cmake_minimum_required(VERSION 3.16)
project(TurbGen LANGUAGES CXX)

find_package(MPI REQUIRED)
find_package(HDF5 COMPONENTS C CXX)

add_library(turbulence INTERFACE)

target_include_directories(turbulence INTERFACE
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/..>
    $<INSTALL_INTERFACE:include>
)

target_link_libraries(turbulence INTERFACE MPI::MPI_CXX)
target_compile_definitions(turbulence INTERFACE HAVE_MPI)

if(HDF5_FOUND)
    target_link_libraries(turbulence INTERFACE HDF5::HDF5)
    target_compile_definitions(turbulence INTERFACE HAVE_HDF5)
endif()

add_library(turbulence::turbulence ALIAS turbulence)

if(CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)

    message(STATUS "TurbGen is running standalone: Building Demo Executables.")

    add_executable(TurbGenDemo
        TurbGenDemo.cpp
        TurbGen.cpp
        TurbGen.h
        HDFIO.h
    )

    target_link_libraries(TurbGenDemo PRIVATE turbulence)

    if(TARGET amrex OR ENABLE_AMREX)
        message(STATUS "AMReX enabled: Adding turbulence plugins")
        # add_subdirectory(plugins/AMReX)
    endif()

else()
    message(STATUS "TurbGen is being included as a module. Skipping .cpp compilation.")
endif()
