set(PROJECT_NAME TLib)

################################################################################
# Source groups
################################################################################
set(no_group_source_files
    "ipdict.cpp"
    "tads.cpp"
    "tapi32ex.cpp"
    "tapi32u8.cpp"
    "tapp.cpp"
    "tcmndlg.cpp"
    "tdlg.cpp"
    "texcept.cpp"
    "tgdiplus.cpp"
    "tinet.cpp"
    "tini.cpp"
    "tmisc.cpp"
    "tregist.cpp"
    "tstr.cpp"
    "twin.cpp"
)
source_group("" FILES ${no_group_source_files})

set(Header_Files
    "tapi32ex.h"
    "tapi32u8.h"
    "tcmndlg.h"
    "tlib.h"
    "tmisc.h"
)
source_group("Header Files" FILES ${Header_Files})

set(ALL_FILES
    ${no_group_source_files}
    ${Header_Files}
)

################################################################################
# Target
################################################################################
add_library(${PROJECT_NAME} STATIC ${ALL_FILES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
set(ROOT_NAMESPACE TLib)

################################################################################
# Target name
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    TARGET_NAME_DEBUG   "${PROJECT_NAME}_d"
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
    "_LIB;"
    "_MBCS"
)

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:
            /Gm-;
            /RTC1;
            /Zi
        >
        $<$<CONFIG:Release>:
            /Ox;
            /Ob1;
            /GF;
            /Gy;
            ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
            /FAcs
        >
        /MP;
        /W3;
        /EHsc;
        ${DEFAULT_CXX_EXCEPTION_HANDLING}
    )
    target_link_options(${PROJECT_NAME} PRIVATE
        /NOLOGO
    )
endif()

