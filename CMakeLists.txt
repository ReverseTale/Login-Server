# ---[ Create project
project(ReverseTale-LoginServer)

# ---[ Parse all prefixes
include(cmake-common/Utils.cmake)
String2List(${CMAKE_PREFIX_PATH} CMAKE_PREFIX_PATH)
String2List(${CMAKE_MODULE_PATH} CMAKE_MODULE_PATH)

# ---[ Find all source and header files
file(GLOB_RECURSE sources_Main      src/*.cpp     src/*.h)
set(LoginServer_SOURCES
    ${sources_Main}
)

# ---[ Load dependencies
if (${ReverseTale_DEV})
    include_directories(${CMAKE_SOURCE_DIR}/src/Library/include)
    include_directories(${CMAKE_SOURCE_DIR}/src/Common-Server/src)
else()
    find_package(ReverseTale-Library        REQUIRED)
    find_package(ReverseTale-CommonServer   REQUIRED)
endif()

# ---[ Add definitions and executable
add_definitions(-DMONGOCXX_STATIC -DBSONCXX_STATIC)
add_executable(ReverseTale-LoginServer ${LoginServer_SOURCES})

# ---[ Link dependencies libraries
target_link_libraries(ReverseTale-LoginServer
    ReverseTale-CommonServer
)

# ---[ Set C++11 standard
set_property(TARGET ReverseTale-LoginServer PROPERTY CXX_STANDARD 11)
target_compile_features(ReverseTale-LoginServer PRIVATE cxx_decltype cxx_nullptr cxx_variadic_templates)

# ---[ Setup output directory and include directories
set_target_properties(ReverseTale-LoginServer PROPERTIES
    OUTPUT_NAME LoginServer
    RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/../bin
    RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/../bin
    RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/../bin)


include(CMakePackageConfigHelpers)
set(config_install_dir lib/cmake/ReverseTale-LoginServer)
set(version_config ${PROJECT_BINARY_DIR}/ReverseTale-LoginServer-config-version.cmake)
set(project_config ${PROJECT_BINARY_DIR}/ReverseTale-LoginServer-config.cmake)
set(targets_export_name ReverseTale-LoginServer-targets)

# Generate the version, config and target files into the build directory.
write_basic_package_version_file(
  ${version_config}
  VERSION 0.0.1
  COMPATIBILITY AnyNewerVersion)
configure_package_config_file(
  ${PROJECT_SOURCE_DIR}/config.cmake.in
  ${project_config}
  INSTALL_DESTINATION ${config_install_dir})
export(TARGETS ReverseTale-LoginServer FILE ${PROJECT_BINARY_DIR}/${targets_export_name}.cmake)

install(TARGETS ReverseTale-LoginServer
	EXPORT ${targets_export_name}
	RUNTIME DESTINATION bin/
  	LIBRARY DESTINATION lib/
  	ARCHIVE DESTINATION lib/)

install(FILES ${project_config} ${version_config} DESTINATION ${config_install_dir})
install(EXPORT ${targets_export_name} DESTINATION ${config_install_dir})
