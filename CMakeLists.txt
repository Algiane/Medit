cmake_minimum_required(VERSION 3.21)
project(Medit LANGUAGES C)

set(medit_sources 
src/animat.c
src/bbfile.c
src/camera.c
src/cenrad.c
src/chrono.c
src/clip.c
src/clipvol.c
src/critip.c
src/cube.c
src/dlists.c
src/eigenv.c
src/ellipse.c
src/geometry.c
src/gisfil.c
src/hash.c
src/ilists.c
src/image.c
src/inmsh2.c
src/inout.c
src/items.c
src/keyboard.c
src/libmesh5.c
src/listnum.c
src/material.c
src/memory.c
src/menus.c
src/mesh.c
src/mlists.c
src/morphing.c
src/mouse.c
src/normal.c
src/param.c
src/parsar.c
src/parsop.c
src/particle.c
src/path.c
src/persp.c
src/picking.c
src/psfile.c
src/scene.c
src/scissor.c
src/sftcpy.c
src/status.c
src/stream.c
src/tiles.c
src/transform.c
src/util.c
src/vector.c
src/view.c
src/zaldy1.c
src/zaldy2.c
)

set(medit_headers 
src/chrono.h
src/eigenv.h
src/extern.h
src/grafic.h
src/image.h
src/libmesh5.h
src/medit.h
src/memory.h
src/mesh.h
src/sproto.h
)

find_package(GLUT REQUIRED)
find_package(OpenGL REQUIRED GLES3)
if(NOT TARGET OpenGL::GLU)
  find_library(GLU_LIB GLU)
  if(NOT GLU_LIB)
    message(ERROR "GLU NOT FOUND !")
  endif()
  add_library(OpenGL::GLU UNKNOWN IMPORTED)
  set_target_properties(SCOTCH::scotch PROPERTIES
    IMPORTED_LOCATION ${GLU_LIB}
  )
endif()
add_library(Medit3D ${medit_sources} ${medit_headers})
target_link_libraries(Medit3D PUBLIC OpenGL::OpenGL GLUT::GLUT OpenGL::GLU m)

# use GNU standard for file hierarchy installation system
include(GNUInstallDirs)
install(TARGETS Medit3D 
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    )

install(FILES ${medit_headers} DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

add_executable(medit src/medit.c)
target_link_libraries(medit PUBLIC Medit3D)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
install(TARGETS medit DESTINATION ${CMAKE_INSTALL_BINDIR})