##---------------------------------------------------------------------------
## Author:      Sean d'Epagnier
##***************************************************************************
## *   Copyright (C) 2010 by Sean d'Epagnier                              *
## *   License:     GPLv3+
## *   This program is free software; you can redistribute it and/or modify  *
## *   it under the terms of the GNU General Public License as published by  *
## *   the Free Software Foundation; either version 2 of the License, or     *
## *   (at your option) any later version.                                   *
## *                                                                         *
## *   This program is distributed in the hope that it will be useful,       *
## *   but WITHOUT ANY WARRANTY; without even the implied warranty of        *
## *   MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the         *
## *   GNU General Public License for more details.                          *
## *                                                                         *
## *   You should have received a copy of the GNU General Public License     *
## *   along with this program; if not, write to the                         *
## *   xxxxxxxxxxxxxxxxxxxxxxxxxxxxxx,                                       *
## *   xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx                                       *
## ***
#-----------------------------------------------------------------------------
# CI-CMake: v1.0.168.0   08/06/2021   CI-CMake Author: Jon Gough
# ---------------------------------------------------------------------------
## buildCI:yes useODAPI:yes validateJSON:no
##
## ----- When changing this file do NOT change the order in which sections occur        ----- ##
## ----- Changes should only be made between the section blocks that identify where     ----- ##
## ----- these changes should be. The whole configuration process relies on this        ----- ##
## ----- sequence to be successful                                                      ----- ##
## ----- Modify section below to include all the details for your plugin ----- ##
##

set(CMLOC "CMakeLists: ")

# define minimum cmake version
cmake_minimum_required(VERSION 3.1.1)
if(COMMAND cmake_policy)
    if(POLICY CMP0043)
        cmake_policy(SET CMP0043 NEW)
    endif(POLICY CMP0043)
    cmake_policy(SET CMP0048 NEW)
    if(POLICY CMP0077)
        cmake_policy(SET CMP0077 NEW)
    endif(POLICY CMP0077)
endif(COMMAND cmake_policy)

# define plugin name, owner and versions
SET(VERBOSE_NAME "Watchdog")
set(COMMON_NAME "Watchdog")  # Search "getcommonname"in watchdog_pi.cpp
set(TITLE_NAME "Watchdog")
set(PACKAGE_CONTACT "Sean d'Epagnier")
set(PACKAGE "watchdog_pi")
set(SHORT_DESCRIPTION "Implement watchdog ability for opencpn")
set(LONG_DESCRIPTION "Set alarms for changing conditions, boundaries, cog, landfall, ais, wind, nmea, weather, pypilot, wind, deadman...")

set(VERSION_MAJOR "2")
set(VERSION_MINOR "4")
set(VERSION_PATCH "33")
set(VERSION_TWEAK "1")
set(VERSION_DATE "15/06/2021") # DD/MM/YYYY format
set(OCPN_MIN_VERSION "ov50")
set(OCPN_API_VERSION_MAJOR "1")
set(OCPN_API_VERSION_MINOR "16")
set(TP_COMMENT "  * Release for O5 using CI")

set(PARENT "opencpn")

#
#Used to set up a local webserver with git loaded, for testing purposes.
#Now set in plugin configure, needed to build flatpak, also locally.
#set(GIT_REPOSITORY_SERVER "github.com")

#No longer needed, queried.
# set(GIT_USER "rgleason")
# set(GIT_REPOSITORY_NAME "weather_routing_pi")

## Specifies Cloudsmith upload repository suffix for each catalog
set(PROD "prod")   #Standard Repos
set(BETA "beta")   #Standard Repos
set(ALPHA "alpha") #Standard Repos

set(CLOUDSMITH_USER "mike-rossiter")
#set(CLOUDSMITH_USER "rick-gleason")

# Set if Cloudsmith Base Repository name does not match your Git Repository name.
#set(CLOUDSMITH_BASE_REPOSITORY "${GIT_REPOSITORY_NAME}")
set(CLOUDSMITH_BASE_REPOSITORY "watchdog")  #without the pi
# Set the following to use OpenCPN cloudsmith repositories instead of own repositories
# Set Plugin info-url, summary and description
set(XML_INFO_URL "https://opencpn.org/OpenCPN/plugins/watchdog.html")
set(XML_SUMMARY ${SHORT_DESCRIPTION})
set(XML_DESCRIPTION ${LONG_DESCRIPTION})

##
## ----- Modify above to include all the plugin details --------- ##
## ----- Modify below for plugin special requirements------------ ##
##

option(PLUGIN_USE_SVG "Use SVG graphics" ON)

set(CMAKE_CXX_STANDARD 11)

# Use local version of GLU library
set(USE_LOCAL_GLU TRUE)
# Prefer libGL.so to libOpenGL.so, see CMP0072
set(OpenGL_GL_PREFERENCE "LEGACY")
# Don't use local version of GLU library
#set(USE_LOCAL_GLU FALSE)

#Turn ON, only fix found for GL_texture2d, glDisable, GL_BLEND  errors
option(USE_GL "Enable OpenGL support" ON)

message(STATUS "${CMLOC}USE_GL: ${USE_GL}")

if("${CMAKE_BUILD_TYPE}" STREQUAL "")
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "Choose the type of build" FORCE)
endif("${CMAKE_BUILD_TYPE}" STREQUAL "")
set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "" "Debug" "Release" "RelWithDebInfo" "MinSizeRel")
message(STATUS "${CMLOC}Build type: ${CMAKE_BUILD_TYPE}")

##
## ----- The statements below are used to setup standard variables that are required by the CMAKE process - do not remove ----- ##
##

project(${PACKAGE})
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

##=======================================
include("PluginSetup")
##=======================================
## ----- Above is used to setup CMAKE required standard variables - do not remove ----- ##
## ----- Modify section below if there are special requirements for the plugin ----- ##
##

set(CMAKE_VERBOSE_MAKEFILE "Activate verbose mode for make files" ON)

option(Plugin_CXX11 "Use c++11" OFF)


## ----- Modify section above if there are special requirements for the plugin ----- ##
## ----- do not change next section - needed to configure build process ----- ##
#========================================================
include("PluginConfigure")
#========================================================
## ----- do not change section above - needed to configure build process ----- ##
## ----- Change below to match project requirements for source, headers, etc. ----- ##

add_definitions(-DUSE_S57)

# ----Remember to Add your project Set(SRCS and set(HDRS  etc..
# ++++++++++++++Section below is direct from Watchdog +++++++++##


SET(SRCS
            src/watchdog_pi.cpp
            src/WatchdogDialog.cpp
            src/WatchdogPropertiesDialog.cpp
            src/ConfigurationDialog.cpp
            src/NewAlarmDialog.cpp
            src/EditAlarmDialog.cpp
            src/Alarm.cpp
            src/signalk_client.cpp
            src/WatchdogUI.cpp
            src/icons.cpp
            src/wddc.cpp
	)

SET(HDRS
  	        include/AIS_Target_Info.h
			include/Alarm.h
			include/ConfigurationDialog.h
			include/EditAlarmDialog.h
			include/icons.h
			include/msvcdefs.h
			include/NewAlarmDialog.h
            include/signalk_client.h
			include/watchdog_pi.h
			include/WatchdogDialog.h
			include/WatchdogPropertiesDialog.h
			include/WatchdogUI.h
			include/wddc.h
			include/WeatherPanel.h
			include/WindPanel.h
			include/wxWTranslateCatalog.h
	)

# TexFont.cpp has to be here it will not compile
set(OCPNSRC ocpnsrc/TexFont.cpp)
# also src/TexFont.h has to exist AND ocpnhdrs/TexFont.h is not needed.

set(EXTSRC
)

set(EXTINCLUDE
    extinclude/ODAPI.h
    extinclude/ODJSONSchemas.h
)

set(LIBSSRC libs/wxJSON/src/jsonreader.cpp libs/wxJSON/src/jsonval.cpp libs/wxJSON/src/jsonwriter.cpp)

set(LIBHDRS
#    libs/GL/gl.h
#    libs/GL/gl_private.h
#    libs/GL/glext.h
#    libs/GL/glu.h
    libs/wxJSON/include/json_defs.h
    libs/wxJSON/include/jsonreader.h
    libs/wxJSON/include/jsonval.h
    libs/wxJSON/include/jsonwriter.h)

add_definitions(-DPLUGIN_USE_SVG)

set(EXTINCLUDE_DIR ${EXTINCLUDE_DIR} extinclude)


##=============================================================
# Modify/configure 'Include' Directories below for the Plugin
##=============================================================

include_directories(BEFORE ${PROJECT_SOURCE_DIR}/include)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpninclude)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpnsrc/glu/include)
# include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/glu/include)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/ocpn-api)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/wxJSON/include)
#include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpninclude/wxcurl)
#include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpninclude/wxcurl/include)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/jsoncpp)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/libs/nmea0183/src)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/ocpninclude)
include_directories(BEFORE ${PROJECT_SOURCE_DIR}/${EXTINCLUDE_DIR})

## Statement below is required to collect all the set( headers and SRCS  - Adjust as required
## This add_library  can be used in conditionals below.
#==========================================================================
target_sources(${PACKAGE_NAME} PUBLIC ${SRCS} ${HDRS} ${OCPNSRC} ${EXTSRC} ${LIBSSRC} )
#=========================================================================
#ADD_LIBRARY(${PACKAGE_NAME} SHARED ${SRCS} ${HDRS} ${OCPNSRC} ${EXTSRC} ${LIBSRC} ${SRC_JSON} ${SRC_NMEA0183} ${LIBHDRS})


# Needed for android builds
if(QT_ANDROID)
    include_directories(BEFORE ${qt_android_include})
endif(QT_ANDROID)

if(NOT OCPN_FLATPAK_CONFIG)
    # Build environment not available when flatpak is being configured so following statements will not work
    message(STATUS "${CMLOC}Adding target link libraries to ${PACKAGE_NAME}")

    add_subdirectory(libs/jsoncpp)
    add_subdirectory(libs/nmea0183)
    add_subdirectory(libs/tinyxml)

    target_link_libraries(${PACKAGE_NAME} ocpn::jsoncpp)
    target_link_libraries(${PACKAGE_NAME} ocpn::nmea0183)
    target_link_libraries(${PACKAGE_NAME} ocpn::tinyxml)
endif(NOT OCPN_FLATPAK_CONFIG)


## ----- Change above to match project requirements ----- ##


add_definitions(-DTIXML_USE_STL)

#turned off
#add_subdirectory(libs/tinyxml)
#target_link_libraries(${PACKAGE_NAME} ocpn::tinyxml)

#turned off
#add_subdirectory(libs/bzip2)
#target_link_libraries(${PACKAGE_NAME} ocpn::bzip2)

#turned off
#add_subdirectory(libs/nmea0183)
#target_link_libraries(${PACKAGE_NAME} ocpn::nmea0183)

#off anyway
#add_subdirectory("libs/jsoncpp")
#target_link_libraries(${PACKAGE_NAME} ocpn::jsoncpp)
#had errors,  trying PluginNmeaCmake

#off anyway
#add_subdirectory("libs/wxJSON")
#target_link_libraries(${PACKAGE_NAME} ocpn::wxjson)

#needed only if src/glshim project should be included.


## ----- Change above to match project requirements ----- ##
#These are NOT used in Testplugin, used for other plugins
##=======================================
#include("PluginGL")
#include("PluginNMEA") #not for wxfax
#include("PluginJSON")  #not for wxfax  #commented out using libs
##=======================================
## ----- Change above to match project requirements ----- ##
## ----- do not change next section - needed to configure build process ----- ##

include("PluginInstall")
include("PluginLocalization")
include("PluginPackage")

##
## ----- do not change section above - needed to configure build process ----- ##
##
message(STATUS "${CMLOC}include directories: ")
get_property(dirs DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR} PROPERTY INCLUDE_DIRECTORIES)
foreach(dir ${dirs})
    message(STATUS "${CMLOC}dir='${dir}'")
endforeach()
