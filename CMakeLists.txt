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
