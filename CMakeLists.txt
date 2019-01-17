# CMake entry point
cmake_minimum_required (VERSION 3.1)
project (nlohmann VERSION 1.0.0)

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
  set(CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/dist CACHE PATH "..." FORCE)
endif()

add_library(nlohmann INTERFACE)
target_include_directories(nlohmann
    INTERFACE
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
)

set(PROJ_CONFIG_DIR lib/cmake/nlohmann)
set(PROJ_EXPORT_NAME Nlohmann)
set(PROJ_EXPORT_NAMESPACE "")

install(TARGETS nlohmann
    EXPORT  ${PROJ_EXPORT_NAME}Targets
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
    INCLUDES DESTINATION include
)

install(DIRECTORY include/ DESTINATION include)

#This is required so that the exported target has the name Lua and not lua
set_target_properties(nlohmann PROPERTIES EXPORT_NAME ${PROJ_EXPORT_NAME})

install(EXPORT ${PROJ_EXPORT_NAME}Targets
    FILE
        ${PROJ_EXPORT_NAME}Targets.cmake 
    NAMESPACE
        ${PROJ_EXPORT_NAMESPACE}
    DESTINATION
        ${PROJ_CONFIG_DIR}
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJ_EXPORT_NAME}ConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY AnyNewerVersion
)

configure_package_config_file(${PROJ_EXPORT_NAME}Config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJ_EXPORT_NAME}Config.cmake
    INSTALL_DESTINATION ${PROJ_CONFIG_DIR}
)

#Install the config, configversion and custom find modules
install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJ_EXPORT_NAME}Config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/${PROJ_EXPORT_NAME}ConfigVersion.cmake
    DESTINATION ${PROJ_CONFIG_DIR}
)

export(EXPORT ${PROJ_EXPORT_NAME}Targets FILE ${CMAKE_CURRENT_BINARY_DIR}/${PROJ_EXPORT_NAME}Targets.cmake  NAMESPACE ${PROJ_EXPORT_NAME}::)


#install_test ( tests/ )
