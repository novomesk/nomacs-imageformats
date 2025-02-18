if(CMAKE_HOST_WIN32) # ubuntu 14.04 has cmake 2.8.12
    CMAKE_MINIMUM_REQUIRED(VERSION 3.0)
else()
    CMAKE_MINIMUM_REQUIRED(VERSION 2.8)
endif()

PROJECT(imageformats)

# # include macros needed
# include("cmake/Utils.cmake")

OPTION (ENABLE_PSD "Enable PSD Plugin" ON)
OPTION (ENABLE_PCX "Enable PCX Plugin" ON)
OPTION (ENABLE_QOI "Enable QOI Plugin" OFF)
OPTION (ENABLE_WEBP "Enable WEBP Plugin (custom build)" OFF)
OPTION (ENABLE_DDS "Enable DDS Plugin (Qt6 only)" OFF)

# not supported yet
#OPTION (ENABLE_FLIF "Enable FLIF Plugin" OFF)

# NMC_PREPARE_PLUGIN()
# NMC_FINDQT()
# NMC_FIND_OPENCV("core" "imgproc")

# set(BUILDING_MULTIPLE_PLUGINS true)

IF (ENABLE_PSD)
    add_subdirectory(libqpsd)
ENDIF(ENABLE_PSD)

IF (ENABLE_PCX)
    add_subdirectory(libqpcx)
ENDIF(ENABLE_PCX)

IF (ENABLE_QOI)
    add_subdirectory(libqqoi)
ENDIF(ENABLE_QOI)

IF (ENABLE_WEBP)
    add_subdirectory(webp)
ENDIF(ENABLE_WEBP)

IF (ENABLE_DDS)
    add_subdirectory(dds)
ENDIF(ENABLE_DDS)

#IF (ENABLE_FLIF)
#    add_subdirectory(qt-flif-plugin)
#ENDIF(ENABLE_FLIF)

# status
MESSAGE(STATUS "")
MESSAGE(STATUS "----------------------------------------------------------------------------------")
MESSAGE(STATUS " ${PROJECT_NAME} for nomacs  <https://nomacs.org>")
MESSAGE(STATUS "")

IF(ENABLE_PSD)
    MESSAGE(STATUS " with PSD .................. YES")
ELSE()
    MESSAGE(STATUS " with PSD .................. NO")
ENDIF()

IF(ENABLE_PCX)
    MESSAGE(STATUS " with PCX .................. YES")
ELSE()
    MESSAGE(STATUS " with PCX .................. NO")
ENDIF()

IF(ENABLE_QOI)
    MESSAGE(STATUS " with QOI .................. YES")
ELSE()
    MESSAGE(STATUS " with QOI .................. NO")
ENDIF()

IF(ENABLE_WEBP)
    MESSAGE(STATUS " with custom WEBP .......... YES")
ELSE()
    MESSAGE(STATUS " with custom WEBP .......... NO")
ENDIF()

IF(ENABLE_DDS)
    MESSAGE(STATUS " with DDS .................. YES")
ELSE()
    MESSAGE(STATUS " with DDS .................. NO")
ENDIF()

# IF(ENABLE_FLIF)
#     MESSAGE(STATUS " with FLIF ................. YES")
# ELSE()
#     MESSAGE(STATUS " with FLIF ................. NO")
# ENDIF()

MESSAGE(STATUS "----------------------------------------------------------------------------------")
