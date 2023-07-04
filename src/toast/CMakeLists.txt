set(PROJECT_NAME toast)

################################################################################
# Source groups
################################################################################
set(no_group_source_files
    "toast.cpp"
    "toast.rc"
)
source_group("" FILES ${no_group_source_files})

set(Header_Files
    "resource.h"
    "toast.h"
)
source_group("Header Files" FILES ${Header_Files})

set(Resource_Files
    "toast.def"
)
source_group("Resource Files" FILES ${Resource_Files})

set(ALL_FILES
    ${no_group_source_files}
    ${Header_Files}
    ${Resource_Files}
)

################################################################################
# Target
################################################################################
add_library(${PROJECT_NAME} SHARED ${ALL_FILES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
set(ROOT_NAMESPACE toast)

set_target_properties(${PROJECT_NAME} PROPERTIES
    VS_GLOBAL_KEYWORD "Win32Proj"
)
################################################################################
# Target name
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    TARGET_NAME_DEBUG   "iptoast"
    TARGET_NAME_RELEASE "iptoast"
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
    $<$<CONFIG:Release>:
        MultiThreaded
    >
    $<$<NOT:$<OR:$<CONFIG:Release>>>:${MSVC_RUNTIME_LIBRARY_DEFAULT}>
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
    "_WINDOWS;"
    "_USRDLL;"
    "TOAST_EXPORTS;"
    "UNICODE;"
    "_UNICODE"
)

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:
            /Od
        >
        $<$<CONFIG:Release>:
            /O2;
            /Oi;
            /Gy;
            /FAcs
        >
        /MP;
        /W3;
        /EHsc;
        ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
        ${DEFAULT_CXX_EXCEPTION_HANDLING};
        /Y-
    )
    target_link_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:
            /INCREMENTAL
        >
        $<$<CONFIG:Release>:
            /DELAYLOAD:user32.dll;
            /DELAYLOAD:Activeds.dll;
            /DELAYLOAD:comctl32.dll;
            /DELAYLOAD:crypt32.dll;
            /DELAYLOAD:Dbghelp.dll;
            /DELAYLOAD:gdiplus.dll;
            /DELAYLOAD:imm32.dll;
            /DELAYLOAD:Iphlpapi.dll;
            /DELAYLOAD:msimg32.dll;
            /DELAYLOAD:psapi.dll;
            /DELAYLOAD:wininet.dll;
            /DELAYLOAD:winmm.dll;
            /DELAYLOAD:Ws2_32.dll;
            /DELAYLOAD:Netapi32.dll;
            /DELAYLOAD:Wininet.dll;
            /OPT:REF;
            /LTCG;
            /OPT:ICF;
            /INCREMENTAL:NO
        >
        /DEBUG;
        /SUBSYSTEM:WINDOWS;
        /DYNAMICBASE:NO
    )
endif()

