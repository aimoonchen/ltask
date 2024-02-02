cmake_minimum_required(VERSION 3.18)

project(ltask)

set(CMAKE_C_STANDARD 17)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++17 -O3")

set(CMAKE_INSTALL_PREFIX ${PROJECT_SOURCE_DIR}/output)

# set(LTASK_INSTALL_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)
# set(LTASK_INSTALL_BIN_DIR ${PROJECT_SOURCE_DIR}/bin)
# set(LTASK_INSTALL_LIB_DIR ${PROJECT_SOURCE_DIR}/lib)

# https://github.com/walterschell/Lua.git
add_subdirectory(lua)

file(GLOB LTASK_SRCS src/*.c)

add_library(${PROJECT_NAME} SHARED ${LTASK_SRCS})

set(LIBS lua_static)

if (WIN32)
    set_target_properties(${PROJECT_NAME} PROPERTIES COMPILE_FLAGS "/experimental:c11atomics")
    set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS "/export:luaopen_ltask /export:luaopen_ltask_bootstrap /export:luaopen_ltask_exclusive /export:luaopen_ltask_root")
    list(APPEND LIBS ws2_32 winmm)
endif()

target_link_libraries(${PROJECT_NAME} PRIVATE ${LIBS})

install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX}/bin)