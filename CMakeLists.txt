cmake_minimum_required(VERSION 3.7)

project(luaprompt)

set(headers
        luaprompt.h
        )

set(src
        module.c
        luaprompt.c
        )

###########LUA
find_package(Lua REQUIRED)

link_libraries(${LUA_LIBRARIES})
include_directories(${LUA_INCLUDE_DIR})

MESSAGE("Lua information:")
MESSAGE("  LUA_LIBRARIES: ${LUA_LIBRARIES}")
MESSAGE("  LUA_INCLUDE_DIR: ${LUA_INCLUDE_DIR}")
###########LUA


add_library(${PROJECT_NAME} ${src} ${headers})
set_target_properties(${PROJECT_NAME} PROPERTIES PUBLIC_HEADER luaprompt.h)

include(GNUInstallDirs)


install(TARGETS luaprompt
        ARCHIVE DESTINATION lib
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

if (PACK_IT)

    SET(MAJOR_VERSION "1")
    SET(MINOR_VERSION "0")
    SET(PATCH_VERSION "0")

    IF (EXISTS "${CMAKE_ROOT}/Modules/CPack.cmake")
        INCLUDE(InstallRequiredSystemLibraries)

        SET(CPACK_SET_DESTDIR "on")
        SET(CPACK_PACKAGING_INSTALL_PREFIX "/tmp")
        SET(CPACK_GENERATOR "DEB")

        SET(CPACK_PACKAGE_DESCRIPTION "luaprompt")
        SET(CPACK_PACKAGE_VENDOR "Turox")
        SET(CPACK_PACKAGE_CONTACT "adriel@adriel.eu")
        SET(CPACK_PACKAGE_VERSION_MAJOR "${MAJOR_VERSION}")
        SET(CPACK_PACKAGE_VERSION_MINOR "${MINOR_VERSION}")
        SET(CPACK_PACKAGE_VERSION_PATCH "${PATCH_VERSION}")
        SET(CPACK_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}_${MAJOR_VERSION}.${MINOR_VERSION}.${CPACK_PACKAGE_VERSION_PATCH}")
        SET(CPACK_SOURCE_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}_${MAJOR_VERSION}.${MINOR_VERSION}.${CPACK_PACKAGE_VERSION_PATCH}")


        SET(CPACK_DEBIAN_PACKAGE_PRIORITY "optional")
        SET(CPACK_DEBIAN_ARCHITECTURE ${CMAKE_SYSTEM_PROCESSOR})

        INCLUDE(CPack)

    ENDIF (EXISTS "${CMAKE_ROOT}/Modules/CPack.cmake")
endif ()