cmake_minimum_required(VERSION 2.6)
project(triangle)

if(WIN32)
   add_definitions(-DNO_TIMER)
endif()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -DTRILIBRARY -DANSI_DECLARATORS")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DTRILIBRARY -DANSI_DECLARATORS")

add_library(triangle STATIC triangle.c)
