

add_library(gitty.exemplar)
add_library(gitty::exemplar ALIAS gitty.exemplar)

target_sources(gitty.exemplar PRIVATE identity.cpp)

target_sources(gitty.exemplar
        PUBLIC
        FILE_SET HEADERS
        BASE_DIRS ${PROJECT_SOURCE_DIR}/include
        FILES ${PROJECT_SOURCE_DIR}/include/gitty/exemplar/identity.hpp
)

set_target_properties(gitty.exemplar
        PROPERTIES VERIFY_INTERFACE_HEADER_SETS ON EXPORT_NAME exemplar
)

install(
        TARGETS gitty.exemplar
        EXPORT gitty.exemplar
        DESTINATION
        ${CMAKE_INSTALL_LIBDIR}$<$<CONFIG:Debug>:/debug>
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}$<$<CONFIG:Debug>:/debug>
        FILE_SET HEADERS DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

if (GITTY_EXEMPLAR_INSTALL_CONFIG_FILE_PACKAGE)
    include(CMakePackageConfigHelpers)

    configure_package_config_file(
            "${PROJECT_SOURCE_DIR}/cmake/${PROJECT_NAME}-config.cmake.in"
            "${PROJECT_BINARY_DIR}/cmake/${PROJECT_NAME}/${PROJECT_NAME}-config.cmake"
            INSTALL_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}"
            PATH_VARS PROJECT_NAME PROJECT_VERSION
    )

    write_basic_package_version_file(
            "${PROJECT_BINARY_DIR}/cmake/${PROJECT_NAME}/${PROJECT_NAME}-version.cmake"
            VERSION ${PROJECT_VERSION}
            COMPATIBILITY SameMajorVersion
    )

    install(
            FILES
            "${PROJECT_BINARY_DIR}/cmake/${PROJECT_NAME}/${PROJECT_NAME}-config.cmake"
            "${PROJECT_BINARY_DIR}/cmake/${PROJECT_NAME}/${PROJECT_NAME}-version.cmake"
            DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}"
            COMPONENT development
    )

    install(
            EXPORT gitty.exemplar
            DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/${PROJECT_NAME}"
            NAMESPACE gitty::
            FILE ${PROJECT_NAME}-targets.cmake
            COMPONENT development
    )
endif ()
