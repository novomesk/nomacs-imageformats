cmake_minimum_required(VERSION 3.5)

project(qqoi)

if (MSVC)
    if(NOT QT_QMAKE_EXECUTABLE)
        find_program(QT_QMAKE_EXECUTABLE NAMES "qmake" "qmake-qt5" "qmake.exe")
    endif()
    if(NOT QT_QMAKE_EXECUTABLE)
        message(FATAL_ERROR "you have to set the path to the Qt5 qmake executable in CMakeUserPaths.cmake")
    endif()
        
    message(STATUS "QMake found: ${QT_QMAKE_EXECUTABLE}")
    get_filename_component(QT_QMAKE_PATH ${QT_QMAKE_EXECUTABLE} PATH)
        
    set(QT_ROOT ${QT_QMAKE_PATH}/)
    set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} ${QT_QMAKE_PATH}/../lib/cmake/Qt5)

endif()

find_package(Qt5 COMPONENTS Core Gui REQUIRED)
set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

file(GLOB LIBQQOI_SOURCES "*.cpp")
file(GLOB LIBQQOI_HEADERS "*.h")

SET(CMAKE_DEBUG_POSTFIX "d")

add_library(${PROJECT_NAME} MODULE ${LIBQQOI_SOURCES} ${LIBQQOI_HEADERS})
target_link_libraries(${PROJECT_NAME} Qt5::Core Qt5::Gui)

set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${CMAKE_CURRENT_BINARY_DIR}/libs)
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${CMAKE_CURRENT_BINARY_DIR}/libs)
set_target_properties(${PROJECT_NAME} PROPERTIES ARCHIVE_OUTPUT_DIRECTORY_REALLYRELEASE ${CMAKE_CURRENT_BINARY_DIR}/libs)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/imageformats")

# install to qt plugins dir
execute_process(
    COMMAND ${QT_QMAKE_EXECUTABLE} -query QT_INSTALL_PLUGINS
    OUTPUT_VARIABLE QT_PLUGINS_DIR
    OUTPUT_STRIP_TRAILING_WHITESPACE)

if (QT_PLUGINS_DIR)

    # status
    message(STATUS "")
    message(STATUS "-----------------------------------------------------------------------------")
    message(STATUS " ${PROJECT_NAME} configured")
    message(STATUS " https://invent.kde.org/frameworks/kimageformats")
    message(STATUS " it will be installed to ${QT_PLUGINS_DIR}/imageformats")
    message(STATUS "-----------------------------------------------------------------------------")
  
else ()
    message(FATAL_ERROR "Qt5 plugin directory cannot be detected.")
endif ()

# Prefix with DESTDIR if available to allow packaging
if (ENV{DESTDIR} AND NOT ENV{DESTDIR} STREQUAL "")
  set(plugins_dir "$ENV{DESTDIR}${QT_PLUGINS_DIR}")
else ()
  set(plugins_dir "${QT_PLUGINS_DIR}")
endif ()

install(
  TARGETS ${PROJECT_NAME}
  LIBRARY DESTINATION "${plugins_dir}/imageformats")
