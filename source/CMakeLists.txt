add_executable(
    womm
    tracebld.cpp
)

# in a just world it would be possible to have the executable target depend
# on both 32 bit and 64 bit payloads, but in this world it is not
add_library(
    payload SHARED
    trcbld.cpp
)
target_link_libraries(payload PRIVATE lib_detours)
set_target_properties(
    payload PROPERTIES 
    LINK_FLAGS /EXPORT:DetourFinishHelperProcess,@1,NONAME
)
set_property(TARGET payload PROPERTY CXX_STANDARD 17)

target_link_libraries(womm PRIVATE lib_detours)
add_dependencies(
    womm
    payload
)