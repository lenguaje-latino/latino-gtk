################################################################################
# Para mas información de este archivo visite la pagina de cmake:
# https://cmake.org/documentation/
################################################################################

#Establece la versión mínima requerida de cmake para un proyecto.
cmake_minimum_required(VERSION 3.0.2)

#Establece un nombre para el proyecto
project (latino-gtk)

if(NOT CMAKE_CONFIGURATION_TYPES AND NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Choose the type of build [Debug]" FORCE) # Default to a Debug build
endif()
# set(CMAKE_BUILD_TYPE "Release")
message(STATUS "CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}"   )

#
# Arquitectura del build
# set(TARGET_ARCH_REGEX                       "^.*-march[= ]([^ ]+).*$")
# string(REGEX MATCH                          "${TARGET_ARCH_REGEX}" TARGET_ARCH_MATCH ${CMAKE_C_FLAGS} ${CMAKE_CXX_FLAGS})

if(TARGET_ARCH_MATCH)
    string(REGEX REPLACE                    "${TARGET_ARCH_REGEX}" "\\1" TARGET_ARCH ${CMAKE_C_FLAGS} ${CMAKE_CXX_FLAGS})
else()
    set(TARGET_ARCH                         ${CMAKE_HOST_SYSTEM_PROCESSOR})
endif()

message(STATUS "TARGET_ARCH: ........... ${TARGET_ARCH}")

message("CMAKE_PREFIX_PATH: " ${CMAKE_PREFIX_PATH})
message("PKG_CONFIG_PATH: " ${PKG_CONFIG_PATH})

if(WIN32)
    
else()
    # Use the package PkgConfig to detect GTK+ headers/library files
    # Requerido en Linux / Descomentar para Ubuntu
    FIND_PACKAGE(PkgConfig REQUIRED)
    PKG_CHECK_MODULES(GTK REQUIRED gtkmm-3.0)
endif()

# Setup CMake to use GTK+, tell the compiler where to look for headers
# and to the linker where to look for libraries
INCLUDE_DIRECTORIES(${GTK_INCLUDE_DIRS})
LINK_DIRECTORIES(${GTK_LIBRARY_DIRS})

# Add other flags to the compiler
ADD_DEFINITIONS(${GTK_CFLAGS_OTHER})

LINK_DIRECTORIES(${GTK_LIBRARY_DIRS})

message(STATUS "GTK_INCLUDE_DIRS: ${GTK_INCLUDE_DIRS}")
message(STATUS "GTK_LIBRARY_DIRS: ${GTK_LIBRARY_DIRS}")
message(STATUS "GTK_CFLAGS_OTHER: ${GTK_CFLAGS_OTHER}")

# msys
if(MSYS)
    INCLUDE_DIRECTORIES("/usr/local/include")
    LINK_DIRECTORIES("/usr/local/lib")

    # Si las rutas anteriores no funcionan ejeute
    # make install en la carpeta de latino
    # o establezca la path absoluta ej.
    # INCLUDE_DIRECTORIES("/d/work/lenguaje-latino/latino/latino-core/include")
    # LINK_DIRECTORIES("/d/work/lenguaje-latino/latino/msys2")
endif()

#Agrega una biblioteca al proyecto utilizando los
#archivos de origen especificados.
add_library(latino-gtk SHARED src/latino-gtk.c)

#Vincula un objetivo a bibliotecas determinadas.
target_link_libraries(latino-gtk ${GTK_LIBRARIES} latino)

#Especifica las reglas que se ejecutarán en el momento de la instalación.
if(MSYS)
    install(FILES msys-latino-gtk.dll DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
else()
    install(FILES liblatino-gtk.so DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
endif()
