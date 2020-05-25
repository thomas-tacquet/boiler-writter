project(boiler-writter CXX)

################################################################################
# Source groups
################################################################################
set(Fichiers_d27en-tte
    "BoilerException.h"
    "CSGOClient.h"
    "CSGOMatchList.h"
    "GCMsgHandler.h"
    "protobuf/cstrike15_gcmessages.pb.h"
    "protobuf/gcsdk_gcmessages.pb.h"
    "protobuf/gcsystemmsgs.pb.h"
    "protobuf/steammessages.pb.h"
)
source_group("Fichiers d%27en-tête" FILES ${Fichiers_d27en-tte})

set(Fichiers_sources
    "BoilerException.cpp"
    "CSGOClient.cpp"
    "CSGOMatchList.cpp"
    "main.cpp"
    "protobuf/cstrike15_gcmessages.pb.cc"
    "protobuf/gcsdk_gcmessages.pb.cc"
    "protobuf/gcsystemmsgs.pb.cc"
    "protobuf/steammessages.pb.cc"
)
source_group("Fichiers sources" FILES ${Fichiers_sources})

set(ALL_FILES
    ${Fichiers_d27en-tte}
    ${Fichiers_sources}
)

################################################################################
# Target
################################################################################
add_executable(${PROJECT_NAME} ${ALL_FILES})

use_props(${PROJECT_NAME} "${CMAKE_CONFIGURATION_TYPES}" "${DEFAULT_CXX_PROPS}")
set(ROOT_NAMESPACE BoilerWritter)

set_target_properties(${PROJECT_NAME} PROPERTIES
    VS_GLOBAL_KEYWORD "Win32Proj"
)
################################################################################
# Target name
################################################################################
set_target_properties(${PROJECT_NAME} PROPERTIES
    TARGET_NAME_DEBUG   "boiler"
    TARGET_NAME_RELEASE "boiler"
)
set_target_properties(${PROJECT_NAME} PROPERTIES
    INTERPROCEDURAL_OPTIMIZATION_RELEASE "TRUE"
)
################################################################################
# Include directories
################################################################################
target_include_directories(${PROJECT_NAME} PUBLIC
    "${CMAKE_CURRENT_SOURCE_DIR}/home/dev/go/src/github.com/thomas-tacquet/boiler-writterprotobuf-2.5.0/src;"
    "${CMAKE_CURRENT_SOURCE_DIR}/home/dev/go/src/github.com/thomas-tacquet/boiler-writtersteamworks_sdk/sdk/public;"
    "${CMAKE_CURRENT_SOURCE_DIR}/%(AdditionalUsingDirectories)"
)

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
    "_USRDLL;"
    "UNICODE;"
    "_UNICODE"
)

################################################################################
# Compile and link options
################################################################################
if(MSVC)
    target_compile_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:
            /Od;
            ${DEFAULT_CXX_DEBUG_RUNTIME_LIBRARY}
        >
        $<$<CONFIG:Release>:
            /O2;
            /Oi;
            ${DEFAULT_CXX_RUNTIME_LIBRARY};
            /Gy
        >
        /W3;
        ${DEFAULT_CXX_DEBUG_INFORMATION_FORMAT};
        ${DEFAULT_CXX_EXCEPTION_HANDLING};
        /Y-
    )
    target_link_options(${PROJECT_NAME} PRIVATE
        $<$<CONFIG:Debug>:
            /INCREMENTAL
        >
        $<$<CONFIG:Release>:
            /OPT:REF;
            /OPT:ICF;
            /INCREMENTAL:NO
        >
        /DEBUG;
        /SUBSYSTEM:CONSOLE
    )
endif()

################################################################################
# Dependencies
################################################################################
set(ADDITIONAL_LIBRARY_DEPENDENCIES
    "steam_api;"
    "libprotobuf"
)
target_link_libraries(${PROJECT_NAME} PUBLIC "${ADDITIONAL_LIBRARY_DEPENDENCIES}")

target_link_directories(${PROJECT_NAME} PUBLIC
    "$<$<CONFIG:Debug>:"
        "${CMAKE_SOURCE_DIR}/protobuf-2.5.0/vsprojects/Debug"
    ">"
    "$<$<CONFIG:Release>:"
        "${CMAKE_SOURCE_DIR}/protobuf-2.5.0/vsprojects/Release"
    ">"
    "${CMAKE_SOURCE_DIR}/steamworks_sdk/sdk/redistributable_bin"
)

