add_executable(
    womm
    tracebld.cpp
)

# in a just world it would be possible to have the executable target depend
# on both 32 bit and 64 bit payloads, but in this world it is not
add_library(
    womm_hook SHARED
    trcbld.cpp
)
target_link_libraries(womm_hook PRIVATE lib_detours)
set_target_properties(
    womm_hook PROPERTIES 
    LINK_FLAGS /EXPORT:DetourFinishHelperProcess,@1,NONAME
)
set_property(TARGET womm_hook PROPERTY CXX_STANDARD 23)
math(EXPR arch_bit "8 * ${CMAKE_SIZEOF_VOID_P}")
set_property(
    TARGET womm_hook PROPERTY OUTPUT_NAME womm_hook${arch_bit}
)

target_compile_options(womm_hook PRIVATE /EHac)

target_link_libraries(womm PRIVATE lib_detours)
add_dependencies(
    womm
    womm_hook
)