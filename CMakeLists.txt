cmake_minimum_required(VERSION 3.10)

project(bot)


include_directories(${CMAKE_CURRENT_SOURCE_DIR})
add_executable(bot ${CMAKE_CURRENT_SOURCE_DIR}/B/main.cpp)
add_subdirectory(A)
target_link_libraries(bot INTERFACE lib)


find_package(PythonInterp REQUIRED)
add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/C/index.h
	COMMAND ${PYTHON_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/C/preparing.py)
add_custom_target(preparing ALL
	DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/C/index.h)
add_dependencies(bot preparing)


FIND_PACKAGE(Doxygen)
 IF(DOXYGEN_FOUND)
    SET(DOXYGEN_INPUT ${CMAKE_SOURCE_DIR})
    ADD_CUSTOM_TARGET(doc COMMAND ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_SOURCE_DIR}/doc/doxygen.config)
    add_dependencies(bot doc)
 ELSE(DOXYGEN_FOUND)
    MESSAGE(STATUS "WARNING: Doxygen not found - Reference manual will not be created")
ENDIF(DOXYGEN_FOUND)
