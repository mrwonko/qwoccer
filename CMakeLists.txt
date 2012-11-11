cmake_minimum_required(VERSION 2.6)

# add cmake_modules to module search path, for find_package()
set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake_modules" ${CMAKE_MODULE_PATH})

project(qwoccer)

#   add local include directories
include_directories("include")

#   add packages

# find Bullet Physics
if(NOT BULLET_ROOT)
	set(BULLET_ROOT "$ENV{BULLET_ROOT}")
endif(NOT BULLET_ROOT)
find_package(Bullet)
if(NOT BULLET_FOUND)
	message(FATAL_ERROR "Could not find Bullet Physics - install it and/or set BULLET_ROOT accordingly.")
endif(NOT BULLET_FOUND)

include_directories("${BULLET_INCLUDE_DIRS}")

# find OpenGL
find_package(OpenGL REQUIRED)
if(OPENGL_INCLUDE_DIR) #may be empty
	include_directories("${OPENGL_INCLUDE_DIR}")
endif(OPENGL_INCLUDE_DIR)

# find GLEW
if(NOT GLEW_ROOT_DIR)
	set(GLEW_ROOT_DIR "$ENV{GLEW_ROOT}")
endif(NOT GLEW_ROOT_DIR)
find_package(GLEW)
if(NOT GLEW_FOUND)
	message(FATAL_ERROR "Could not find GLEW - install it and/or set GLEW_ROOT accordingly")
endif(NOT GLEW_FOUND)
include_directories("${GLEW_INCLUDE_PATH}")

# find SFML

find_package(SFML 2 REQUIRED system window graphics audio)
include_directories("${SFML_INCLUDE_DIR}")
add_definitions(-DSFML_DYNAMIC)

add_executable("qwoccer"
	"src/main.cpp"
	)

target_link_libraries( "qwoccer"
	# Bullet
	${BULLET_DYNAMICS_LIBRARY} ${BULLET_COLLISION_LIBRARY} ${BULLET_MATH_LIBRARY}
	# SFML
	${SFML_AUDIO} ${SFML_GRAPHICS} ${SFML_WINDOW} ${SFML_SYSTEM}
	# OpenGL / GLEW
	${GLEW_LIBRARY} ${OPENGL_LIBRARIES}
	)