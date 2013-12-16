cmake_minimum_required(VERSION 2.6)

option(USE_CLANG "build application with clang" OFF) # OFF is the default
if(USE_CLANG)
  SET (CMAKE_C_COMPILER "/usr/bin/clang")
  SET (CMAKE_CXX_COMPILER "/usr/bin/clang++")
endif(USE_CLANG)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")
include(CheckCXXCompilerFlag)

project(psworlds)

set (psworlds_VERSION_MAJOR 0)
set (psworlds_VERSION_MINOR 0)
set (psworlds_VERSION_PATCH 1)

find_package(SDL REQUIRED)
find_package(SDL_mixer REQUIRED)
find_package(OpenGL REQUIRED)
find_package(LibConfig REQUIRED)

if ( NOT OPENGL_GLU_FOUND)
  message(FATAL_ERROR "Package GLU required, but not found!")
endif( NOT OPENGL_GLU_FOUND)

if ( NOT LIBCONFIGPP_FOUND)
  message(FATAL_ERROR "Package LibConfig++ required, but not found!")
endif( NOT LIBCONFIGPP_FOUND)


include_directories(${SDL_INCLUDE_DIR})
include_directories(${SDL_MIXER_INCLUDE_DIR})
#include_directories(${GLU_INCLUDE_DIR})

configure_file (
  "${psworlds_SOURCE_DIR}/src/version.h.in"
  "${psworlds_SOURCE_DIR}/src/version.h"
)

file(GLOB SOURCES "src/[a-zA-Z]*.cpp")

include_directories(${psworlds_SOURCE_DIR})

add_executable(psworlds ${SOURCES})
target_link_libraries(psworlds ${SDL_LIBRARY} ${SDL_MIXER_LIBRARY} ${PNG_LIBRARY} ${OPENGL_gl_LIBRARY} ${OPENGL_glu_LIBRARY} ${LIBCONFIGPP_LIBRARIES})

