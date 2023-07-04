cmake_minimum_required(VERSION 3.16.0 FATAL_ERROR)

set(CMAKE_SYSTEM_VERSION 10.0 CACHE STRING "" FORCE)

project(IPMsg C CXX)

################################################################################
# Global compiler options
################################################################################
if(MSVC)
    # remove default flags provided with CMake for MSVC
    set(CMAKE_C_FLAGS "")
    set(CMAKE_C_FLAGS_DEBUG "")
    set(CMAKE_C_FLAGS_RELEASE "")
    set(CMAKE_CXX_FLAGS "")
    set(CMAKE_CXX_FLAGS_DEBUG "")
    set(CMAKE_CXX_FLAGS_RELEASE "")
endif()

include(CMake/Utils.cmake)
################################################################################
# Global linker options
################################################################################
if(MSVC)
    # remove default flags provided with CMake for MSVC
    set(CMAKE_EXE_LINKER_FLAGS "")
    set(CMAKE_MODULE_LINKER_FLAGS "")
    set(CMAKE_SHARED_LINKER_FLAGS "")
    set(CMAKE_STATIC_LINKER_FLAGS "")
    set(CMAKE_EXE_LINKER_FLAGS_DEBUG "${CMAKE_EXE_LINKER_FLAGS}")
    set(CMAKE_MODULE_LINKER_FLAGS_DEBUG "${CMAKE_MODULE_LINKER_FLAGS}")
    set(CMAKE_SHARED_LINKER_FLAGS_DEBUG "${CMAKE_SHARED_LINKER_FLAGS}")
    set(CMAKE_STATIC_LINKER_FLAGS_DEBUG "${CMAKE_STATIC_LINKER_FLAGS}")
    set(CMAKE_EXE_LINKER_FLAGS_RELEASE "${CMAKE_EXE_LINKER_FLAGS}")
    set(CMAKE_MODULE_LINKER_FLAGS_RELEASE "${CMAKE_MODULE_LINKER_FLAGS}")
    set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS}")
    set(CMAKE_STATIC_LINKER_FLAGS_RELEASE "${CMAKE_STATIC_LINKER_FLAGS}")
endif()

################################################################################
# Use solution folders feature
################################################################################
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

################################################################################
# Sub-projects
################################################################################
add_subdirectory(external/sqlite3)
add_subdirectory(external/zlib)
add_subdirectory(src/install)
add_subdirectory(src/ipcmd)
add_subdirectory(src/TLib)
add_subdirectory(src/toast)
add_subdirectory(src/uninst)


################################################################################
set(PROJECT_NAME IPMsg)

################################################################################
# Source groups
################################################################################
set(no_group_source_files
    "src/aes.cpp"
    "src/blowfish.cpp"
    "src/cfg.cpp"
    "src/histdlg.cpp"
    "src/image.cpp"
    "src/ipipc.cpp"
    "src/ipmsg.cpp"
    "src/logchildview.cpp"
    "src/logchildviewev.cpp"
    "src/logchildviewpt.cpp"
    "src/logdb.cpp"
    "src/logdbconv.cpp"
    "src/logeditdlg.cpp"
    "src/logimportdlg.cpp"
    "src/logmng.cpp"
    "src/logview.cpp"
    "src/logviewitem.cpp"
    "src/mainwin.cpp"
    "src/mainwincmd.cpp"
    "src/mainwindir.cpp"
    "src/mainwinev.cpp"
    "src/mainwinfile.cpp"
    "src/mainwinmsg.cpp"
    "src/mainwinupd.cpp"
    "src/miscdlg.cpp"
    "src/miscfunc.cpp"
    "src/msgmng.cpp"
    "src/plugin.cpp"
    "src/pubkey.cpp"
    "src/recvdlg.cpp"
    "src/remotedlg.cpp"
    "src/richedit.cpp"
    "src/senddlg.cpp"
    "src/sendmng.cpp"
    "src/sendmsg.cpp"
    "src/setupdlg.cpp"
    "src/share.cpp"
    "src/taskbarui.cpp"
    "src/tipwnd.cpp"
    "src/toolbar.cpp"
    "src/version.cpp"
    "src/ipmsg.rc"
)
source_group("" FILES ${no_group_source_files})

set(Header_Files
    "src/aes.h"
    "src/blowfish.h"
    "src/cfg.h"
    "src/environ.h"
    "src/histdlg.h"
    "src/image.h"
    "src/ipmsg.h"
    "src/ipmsgbase.h"
    "src/ipmsgcmn.h"
    "src/ipmsgdef.h"
    "src/logchildview.h"
    "src/logdb.h"
    "src/logdbconv.h"
    "src/logeditdlg.h"
    "src/logimportdlg.h"
    "src/logmng.h"
    "src/logview.h"
    "src/logviewitem.h"
    "src/mainwin.h"
    "src/miscdlg.h"
    "src/miscfunc.h"
    "src/msgmng.h"
    "src/plugin.h"
    "src/pubkey.h"
    "src/recvdlg.h"
    "src/remotedlg.h"
    "src/resource.h"
    "src/richedit.h"
    "src/senddlg.h"
    "src/setupdlg.h"
    "src/share.h"
    "src/taskbarui.h"
    "src/tipwnd.h"
    "src/toolbar.h"
    "src/version.h"
)
source_group("Header Files" FILES ${Header_Files})

set(Resource_Files
    "src/res/blue.cur"
    "src/res/cross.cur"
    "src/res/genblue.cur"
    "src/res/gengreen.cur"
    "src/res/genred.cur"
    "src/res/genyellow.cur"
    "src/res/green.cur"
    "src/res/LOUPE.cur"
    "src/res/red.cur"
    "src/res/sep.cur"
    "src/res/yellow.cur"
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
add_executable(${PROJECT_NAME} ${ALL_FILES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
set(ROOT_NAMESPACE IPMsg)

################################################################################
# Target name
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    TARGET_NAME_RELEASE "IPMsg"
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
            /Gm-;
            /Od;
            /RTC1;
            /Zi;
            /GS
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
        $<$<CONFIG:Debug>:
            /NOLOGO;
            /NXCOMPAT;
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
        /MACHINE:X64;
        /SUBSYSTEM:WINDOWS;
        /DYNAMICBASE:NO
    )
endif()

################################################################################
# Dependencies
################################################################################
add_dependencies(${PROJECT_NAME}
    sqlite3
    toast
)

# Link with other targets.
target_link_libraries(${PROJECT_NAME} PRIVATE
    TLib
    sqlite3
)
target_link_libraries(${PROJECT_NAME} PRIVATE "${ADDITIONAL_LIBRARY_DEPENDENCIES}")

