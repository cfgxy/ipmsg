set(PROJECT_NAME ipcmd)

################################################################################
# Source groups
################################################################################
set(Headers
    "resource.h"
)
source_group("Headers" FILES ${Headers})

set(Sources
    "../ipipc.cpp"
    "../miscfunc.cpp"
    "cmdwin.cpp"
    "ipcmd.cpp"
    "ipcmd.rc"
    "ipcmd.exe.manifest"
)
source_group("Sources" FILES ${Sources})

set(ALL_FILES
    ${Headers}
    ${Sources}
)

################################################################################
# Target
################################################################################
add_executable(${PROJECT_NAME} ${ALL_FILES})

set(ROOT_NAMESPACE ipcmd)

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
    "USE_CMD;"
    "_CONSOLE;"
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
            /MP;
            /Gy;
            /FAcs
        >
        /W3;
        /EHsc;
        /Y-
    )
    target_link_options(${PROJECT_NAME} BEFORE PRIVATE
        $<$<CONFIG:Debug>:
            /DEBUG;
            /INCREMENTAL
        >
        $<$<OR:$<CONFIG:Release>,$<CONFIG:MinSizeRel>>:
            /DELAYLOAD:user32.dll;
            /DELAYLOAD:imm32.dll;
            /DELAYLOAD:winmm.dll;
            /DELAYLOAD:ws2_32.dll;
            /DELAYLOAD:Netapi32.dll;
            /DELAYLOAD:Wininet.dll;
            /OPT:REF;
            /LTCG;
            /OPT:ICF;
            /DYNAMICBASE:NO;
            /INCREMENTAL:NO
        >
        /SUBSYSTEM:CONSOLE
    )
endif()

################################################################################
# Dependencies
################################################################################
add_dependencies(${PROJECT_NAME}
    TLib
)

set(ADDITIONAL_LIBRARY_DEPENDENCIES
    "$<$<CONFIG:Debug>:"
        "imm32;"
        "winmm;"
        "Comctl32;"
        "crypt32;"
        "Shlwapi;"
    ">"
)
target_link_libraries(${PROJECT_NAME} PRIVATE "${ADDITIONAL_LIBRARY_DEPENDENCIES}")

