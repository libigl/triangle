cmake_minimum_required(VERSION 3.5)
project(triangle)

option(BUILD_EXECUTABLE "Build triangle executable" OFF)
option(BUILD_LIBRARY    "Build libtriangle library" ON)

if(BUILD_LIBRARY)
  add_library(triangle STATIC triangle.cpp)
  target_compile_definitions(triangle PRIVATE -DTRILIBRARY -DANSI_DECLARATORS)
  if(WIN32)
    target_compile_definitions(triangle PRIVATE -DNO_TIMER)
  endif()
  # Generate position independent code
  set_target_properties(triangle PROPERTIES 
    POSITION_INDEPENDENT_CODE ON
    PUBLIC_HEADER triangle.h
  )
  install(TARGETS triangle
    PUBLIC_HEADER
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
  )
endif()

if(BUILD_EXECUTABLE)
  add_executable(triangle_exec triangle.cpp)
  target_compile_definitions(triangle_exec PRIVATE -DANSI_DECLARATORS)
  if(WIN32)
    target_compile_definitions(triangle_exec PRIVATE -DNO_TIMER)
  endif()
  set_target_properties(triangle_exec PROPERTIES OUTPUT_NAME triangle)
  install(TARGETS triangle_exec
    DESTINATION ${CMAKE_INSTALL_BINDIR}
  )
endif()

