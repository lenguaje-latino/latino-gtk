################################################################################
# Para mas información de este archivo visite la pagina de cmake:
# https://cmake.org/documentation/
################################################################################

#Establece la versión mínima requerida de cmake para un proyecto.
cmake_minimum_required(VERSION 3.0.2)

#Establece un nombre para el proyecto
project (latino-gtk)

# Use the package PkgConfig to detect GTK+ headers/library files
FIND_PACKAGE(PkgConfig REQUIRED)
PKG_CHECK_MODULES(GTK REQUIRED gtkmm-3.0)

# Setup CMake to use GTK+, tell the compiler where to look for headers
# and to the linker where to look for libraries
INCLUDE_DIRECTORIES(${GTK_INCLUDE_DIRS})
LINK_DIRECTORIES(${GTK_LIBRARY_DIRS})

# Add other flags to the compiler
ADD_DEFINITIONS(${GTK_CFLAGS_OTHER})

#Agrega un subdirectorio a la compilación.
# add_subdirectory(jansson)

#Añade directorios de inclusión a la compilación
# include_directories(jansson/include)
# include_directories(jansson/src)
# include_directories(include)

#Agrega una biblioteca al proyecto utilizando los
#archivos de origen especificados.
add_library(latino-gtk SHARED src/latino-gtk.c)

#Vincula un objetivo a bibliotecas determinadas.
target_link_libraries(latino-gtk ${GTK_LIBRARIES} latino)

#Especifica las reglas que se ejecutarán en el momento de la instalación.
install(FILES liblatino-gtk.so DESTINATION ${CMAKE_INSTALL_PREFIX}/lib)
