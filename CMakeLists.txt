# ---[ Create project
cmake_minimum_required(VERSION 2.8.0)
cmake_policy(SET CMP0022 NEW)
project(ReverseTale-LoginServer)

# ---[ Includes
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake-utils)

# -[ Macros and functions
include(Configure)
include(Macros)
include(External)
ResetAllTargets()

# ---[ Create project
CreateTarget(TARGET ReverseTale-LoginServer)

# ---[ Find all source and header files
AddToSources(
    TARGET ReverseTale-LoginServer
    SRC_PATH "src"
    GLOB_SEARCH ".hpp;.cpp"
    FOLDER_NAME "src"
)

# ---[ Load dependencies
add_subdirectory(dep)


ResolveExternal(TARGET ReverseTale-LoginServer)
if (ReverseTale-LoginServer_IS_RESOLVED)
	# ---[ Setup include directories
	AddToIncludes(
		TARGET ReverseTale-LoginServer
		INC_PATH ${CMAKE_CURRENT_SOURCE_DIR}/include
    )

    # ---[ Dependencies
    AddLibrary(
        TARGET ReverseTale-LoginServer
        LIBRARY pthread
        PLATFORM UNIX
    )

    BuildNow(
        TARGET ReverseTale-LoginServer
        EXECUTABLE
        OUTPUT_NAME LoginServer
        DEFINES ${DEFINITIONS}
        C++11
    )

    MakeInstallable(TARGET ReverseTale-LoginServer)

    # Create a Visual Studio Code workspace
    ExternalInstallDirectory(VARIABLE EXTERNAL_DIRECTORY)
    get_filename_component(LIBRARY_PATH ${EXTERNAL_DIRECTORY}/src/ReverseTale_Library_master/ ABSOLUTE)
    get_filename_component(COMMON_SERVER_PATH ${EXTERNAL_DIRECTORY}/src/ReverseTale_Common-Server_master/ ABSOLUTE)
    get_filename_component(LOGIN_SERVER_PATH ${CMAKE_SOURCE_DIR} ABSOLUTE)
    configure_file(${CMAKE_SOURCE_DIR}/project.code-workspace.in ${CMAKE_SOURCE_DIR}/project.code-workspace @ONLY)
endif()

