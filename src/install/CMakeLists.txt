set(PROJECT_NAME installer)

################################################################################
# Source groups
################################################################################
set(no_group_source_files
    "../version.cpp"
    "install.cpp"
    "instcmn.cpp"
    "instcore.cpp"
    "install.rc"
    "install.exe.manifest"
)
source_group("" FILES ${no_group_source_files})

set(Header_Files
    "install.h"
    "resource.h"
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

set(ROOT_NAMESPACE install)

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
    "_WINDOWS;"
    "_MBCS"
)

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} BEFORE PRIVATE
        $<$<CONFIG:Debug>:
            /Gm-;
            /Od;
            /RTC1;
            /Zi
        >
        $<$<OR:$<CONFIG:Release>,$<CONFIG:MinSizeRel>>:
            /Gy
        >
        /MP;
        /W3;
        /EHsc
    )
    target_link_options(${PROJECT_NAME} BEFORE PRIVATE
        $<$<CONFIG:Debug>:
            /NOLOGO;
            /DEBUG;
            /INCREMENTAL
        >
        $<$<OR:$<CONFIG:Release>,$<CONFIG:MinSizeRel>>:
            /DELAYLOAD:user32.dll;
            /DELAYLOAD:winmm.dll;
            /DELAYLOAD:Comctl32.dll;
            /DELAYLOAD:crypt32.dll;
            /DELAYLOAD:Netapi32.dll;
            /DELAYLOAD:Wininet.dll;
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
add_dependencies(${PROJECT_NAME}
    zlib
)

# Link with other targets.
target_link_libraries(${PROJECT_NAME} PRIVATE
    TLib
)
target_link_libraries(${PROJECT_NAME} PRIVATE "${ADDITIONAL_LIBRARY_DEPENDENCIES}")

