set(PROJECT_NAME sqlite3)

################################################################################
# Source groups
################################################################################
set(no_group_source_files
    "ReadMe.txt"
    "sqlite3.c"
)
source_group("" FILES ${no_group_source_files})

set(Header_Files
    "sqlite3.h"
    "sqlite3ext.h"
    "stdafx.h"
    "targetver.h"
)
source_group("Header Files" FILES ${Header_Files})

set(ALL_FILES
    ${no_group_source_files}
    ${Header_Files}
)

################################################################################
# Target
################################################################################
add_library(${PROJECT_NAME} STATIC sqlite3.c)

set(ROOT_NAMESPACE sqlite3)

set_target_properties(${PROJECT_NAME} PROPERTIES
    VS_GLOBAL_KEYWORD "Win32Proj"
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
    "SQLITE_CASE_SENSITIVE_LIKE=1;"
    "SQLITE_ENABLE_FTS3=1;"
    "SQLITE_ENABLE_FTS4=1;"
    "SQLITE_ENABLE_FTS5=1;"
    "SQLITE_ENABLE_FTS3_PARENTHESIS=1;"
    "_MBCS"
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
        /GL;
        /Ox;
        /Oi;
        /Gy
        >
        /MP;
        /W3;
        ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
        ${DEFAULT_CXX_EXCEPTION_HANDLING};
        /Y-
    )

    target_link_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Release>:
        /OPT:REF;
        /OPT:ICF
        >
        /SUBSYSTEM:WINDOWS
    )
endif()

