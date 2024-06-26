cmake_minimum_required(VERSION 3.22)

project(libe131 C)

set (SOURCE_FILES 
  src/e131.c
  src/e131.h
)

add_library(${PROJECT_NAME} STATIC
	${SOURCE_FILES} 
)

set_target_properties(${PROJECT_NAME} PROPERTIES POSITION_INDEPENDENT_CODE ON)

target_include_directories(${PROJECT_NAME} PUBLIC
	$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
	$<INSTALL_INTERFACE:include>
)

#####################################
# Install

install(DIRECTORY src/ DESTINATION include/ FILES_MATCHING PATTERN *.h)

install(
  TARGETS ${PROJECT_NAME}
  EXPORT ${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
)

set(config_in_builddir ${CMAKE_BINARY_DIR}/share/libe131/cmake/libe131-config.cmake)
configure_file(
  libe131-config.cmake.in
  ${config_in_builddir}
  COPYONLY
)
install(
  FILES ${config_in_builddir}
  DESTINATION share/libe131/cmake
)

set(targets_in_builddir ${CMAKE_BINARY_DIR}/share/libe131/cmake/libe131-targets.cmake)
export(
  EXPORT ${PROJECT_NAME}
  FILE ${targets_in_builddir}
)
install(
  EXPORT ${PROJECT_NAME}
  DESTINATION share/libe131/cmake
  FILE libe131-targets.cmake
)