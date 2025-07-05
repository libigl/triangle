cmake_minimum_required(VERSION 3.5)
project(triangle)

find_package(X11)

if(X11_FOUND)
  option(BUILD_SHOWME    "Build showme executable" OFF)
endif()
option(BUILD_EXECUTABLE "Build triangle executable" OFF)
option(BUILD_LIBRARY    "Build libtriangle library" ON)

if(BUILD_LIBRARY)
  add_library(triangle STATIC triangle.cpp)
  target_compile_definitions(triangle PRIVATE -DTRILIBRARY)
  if(WIN32)
    target_compile_definitions(triangle PRIVATE -DNO_TIMER)
  endif()
  # Generate position independent code
  set_target_properties(triangle PROPERTIES POSITION_INDEPENDENT_CODE ON)
endif()


if(BUILD_EXECUTABLE)
  add_executable(triangle_exec triangle.cpp)
  target_compile_definitions(triangle_exec PRIVATE)
  if(WIN32)
    target_compile_definitions(triangle_exec PRIVATE -DNO_TIMER)
  endif()
  set_target_properties(triangle_exec PROPERTIES OUTPUT_NAME triangle)
endif()

if(X11_FOUND)
  if(BUILD_SHOWME)
    include_directories(${X11_INCLUDE_DIR})
    add_executable(showme_exec showme.cpp)
    target_compile_definitions(showme_exec PRIVATE)
    target_link_libraries(showme_exec ${X11_LIBRARIES})
    set_target_properties(showme_exec PROPERTIES OUTPUT_NAME showme)
  endif()
endif()

