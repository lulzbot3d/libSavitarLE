cmake_policy(SET CMP0091 NEW)
project(libsavitar)
cmake_minimum_required(VERSION 3.20)
find_package(standardprojectsettings REQUIRED)

find_package(pugixml REQUIRED)

set(savitar_SRCS
        src/Namespace.cpp
        src/ThreeMFParser.cpp
        src/SceneNode.cpp
        src/Scene.cpp
        src/MeshData.cpp
        src/Vertex.cpp
        src/Face.cpp
        src/UVCoordinate.cpp
        src/UVCoordinatesIndices.cpp
        src/TextureData.cpp
)

if (BUILD_SHARED_LIBS)
    add_library(Savitar SHARED ${savitar_SRCS})
    if (WIN32)
        set_target_properties(Savitar PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS ON)
    endif ()
else ()
    add_library(Savitar STATIC ${savitar_SRCS})
endif ()

set_project_warnings(Savitar)

target_link_libraries(Savitar PUBLIC pugixml::pugixml)

target_include_directories(Savitar
        PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
        PRIVATE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
)

option(ENABLE_TESTING "Enable unit-testing" OFF)
if (ENABLE_TESTING)
    enable_testing()
    add_subdirectory(tests)
endif ()
