set(PROJECT_NAME uninst)

################################################################################
# Source groups
################################################################################
set(no_group_source_files
    "../install/instcmn.cpp"
    "../version.cpp"
    "uninst.cpp"
    "uninst.rc"
)
source_group("" FILES ${no_group_source_files})

set(Header_Files
    "resource.h"
    "uninst.h"
)
source_group("Header Files" FILES ${Header_Files})

set(ALL_FILES
    ${no_group_source_files}
    ${Header_Files}
)

################################################################################
# Target
################################################################################
add_executable(${PROJECT_NAME} ${ALL_FILES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
set(ROOT_NAMESPACE uninst)

################################################################################
# Target name
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    TARGET_NAME_DEBUG   "uninst"
    TARGET_NAME_RELEASE "uninst$ENV{TargetExt}"
)
################################################################################
# Output directory
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    INTERPROCEDURAL_OPTIMIZATION_RELEASE "TRUE"
)
################################################################################
# MSVC runtime library
################################################################################
get_property(MSVC_RUNTIME_LIBRARY_DEFAULT TARGET ${PROJECT_NAME} PROPERTY MSVC_RUNTIME_LIBRARY)
string(CONCAT "MSVC_RUNTIME_LIBRARY_STR"
    $<$<CONFIG:Debug>:
        MultiThreadedDebug
    >
    $<$<CONFIG:Release>:
        MultiThreaded
    >
    $<$<NOT:$<OR:$<CONFIG:Debug>,$<CONFIG:Release>>>:${MSVC_RUNTIME_LIBRARY_DEFAULT}>
)
set_target_properties(${PROJECT_NAME} PROPERTIES MSVC_RUNTIME_LIBRARY ${MSVC_RUNTIME_LIBRARY_STR})

################################################################################
# Compile definitions
################################################################################
target_compile_definitions(${PROJECT_NAME} PRIVATE
    "$<$<CONFIG:Debug>:"
        "_DEBUG"
    ">"
    "$<$<CONFIG:Release>:"
        "NDEBUG"
    ">"
    "WIN32;"
    "_WINDOWS;"
    "_MBCS"
)

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:
            /Gm;
            /Od;
            /RTC1;
            /Zi
        >
        $<$<CONFIG:Release>:
            /GL;
            /Ox;
            /Ob1;
            /GF;
            /Gy;
            ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT}
        >
        /MP;
        /W3;
        /EHsc;
        ${DEFAULT_CXX_EXCEPTION_HANDLING}
    )
    target_link_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:
            /NOLOGO;
            /DEBUG;
            /INCREMENTAL
        >
        $<$<CONFIG:Release>:
            /DELAYLOAD:user32.dll;
            /DELAYLOAD:winmm.dll;
            /DELAYLOAD:comctl32.dll;
            /DELAYLOAD:crypt32.dll;
            /DELAYLOAD:Netapi32.dll;
            /DELAYLOAD:Wininet.dll;
            /DEBUG:FULL;
            /OPT:REF;
            /LTCG;
            /OPT:ICF;
            /INCREMENTAL:NO
        >
        /MACHINE:X64;
        /SUBSYSTEM:WINDOWS;
        /DYNAMICBASE:NO
    )
endif()

################################################################################
# Dependencies
################################################################################
# Link with other targets.
target_link_libraries(${PROJECT_NAME} PRIVATE
    TLib
)
target_link_libraries(${PROJECT_NAME} PRIVATE "${ADDITIONAL_LIBRARY_DEPENDENCIES}")

