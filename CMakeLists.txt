cmake_minimum_required(VERSION 3.16.0)

project(landrop LANGUAGES CXX)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Qt5 COMPONENTS
    Core
    Widgets
    Network
    REQUIRED
)


include(cmake/landrop.cmake)

## https://github.com/facebookincubator/fizz/blob/master/build/fbcode_builder/CMake/FindSodium.cmake
include(cmake/findSodium.cmake)

if(UNIX AND NOT APPLE)
    add_executable(landrop
        ${LANDROP_QRC}
        ${LANDROP_SOURCES}
        ${LANDROP_HEADERS}
        ${LANDROP_UI}
    )

    target_link_libraries(landrop PRIVATE
        Qt5::Core
        Qt5::Widgets
        Qt5::Network
        ${sodium_LIBRARY_RELEASE}
    )

install(TARGETS landrop RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
install(FILES LANDrop/icons/app.svg DESTINATION share/icons/hicolor/scalable/apps RENAME landrop.svg)
install(FILES LICENSE DESTINATION share/licenses/landrop)
install(FILES misc/landrop.desktop DESTINATION share/applications)
endif()

