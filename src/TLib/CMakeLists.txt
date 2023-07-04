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

set(ROOT_NAMESPACE TLib)

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
    "_MBCS"
)

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} BEFORE PRIVATE
        $<$<CONFIG:Debug>:
            /Gm-;
            /RTC1;
            /Od;
            /Zi
        >
        $<$<OR:$<CONFIG:Release>,$<CONFIG:MinSizeRel>>:
            /GF;
            /Gy;
            /FAcs
        >
        /MP;
        /W3;
        /EHsc
    )
    target_link_options(${PROJECT_NAME} BEFORE PRIVATE
        /NOLOGO
    )
endif()

