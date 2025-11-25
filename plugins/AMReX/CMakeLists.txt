add_library(TurbGenEx TurbGenEx.h)

target_link_libraries(TurbGenEx
    PUBLIC
    turbulence
    amrex
)

target_include_directories(TurbGenEx PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../../..>
)
