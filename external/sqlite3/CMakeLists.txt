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
# Compile definitions
################################################################################
target_compile_definitions(${PROJECT_NAME} PRIVATE
    $<$<CONFIG:Debug>:
        "_DEBUG"
    >
    $<$<OR:$<CONFIG:Release>,$<CONFIG:MinSizeRel>>:
        "NDEBUG"
    >
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
    target_compile_options(${PROJECT_NAME} BEFORE PRIVATE
        $<$<CONFIG:Debug>:
            /Od
        >
        $<$<OR:$<CONFIG:Release>,$<CONFIG:MinSizeRel>>:
            /GL;
            /Oi;
            /Gy
        >
        /MP;
        /W3;
        /Y-
    )

    target_link_options(${PROJECT_NAME} BEFORE PRIVATE
        $<$<CONFIG:Release>:
        /OPT:REF;
        /OPT:ICF
        >
        /SUBSYSTEM:WINDOWS
    )
endif()

