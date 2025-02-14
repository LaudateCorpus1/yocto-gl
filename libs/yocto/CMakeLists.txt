add_library(yocto STATIC
  yocto_math.h yocto_color.h yocto_geometry.h
  yocto_noise.h yocto_sampling.h yocto_shading.h
  yocto_bvh.h yocto_bvh.cpp
  yocto_shape.h yocto_shape.cpp
  yocto_mesh.h yocto_mesh.cpp 
  yocto_image.h yocto_image.cpp
  yocto_scene.h yocto_scene.cpp
  yocto_trace.h yocto_trace.cpp
  yocto_modelio.h yocto_modelio.cpp
  yocto_pbrtio.h yocto_pbrtio.cpp
  yocto_sceneio.h yocto_sceneio.cpp
  yocto_gui.h yocto_gui.cpp
  yocto_parallel.h yocto_cli.h
)

set_target_properties(yocto PROPERTIES CXX_STANDARD 17 CXX_STANDARD_REQUIRED YES)
target_include_directories(yocto PRIVATE ext/)
target_include_directories(yocto PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/..")

target_link_libraries(yocto PRIVATE fast_float)
target_link_libraries(yocto PRIVATE nlohmann_json::nlohmann_json)
target_link_libraries(yocto PRIVATE stb_image::stb_image)
target_link_libraries(yocto PRIVATE tinyexr::tinyexr)
target_link_libraries(yocto PRIVATE cgltf::cgltf)

if(UNIX AND NOT APPLE)
  find_package(Threads REQUIRED)
  target_link_libraries(yocto PUBLIC Threads::Threads)
endif(UNIX AND NOT APPLE)

if(YOCTO_OPENGL)
  target_compile_definitions(yocto PUBLIC -DYOCTO_OPENGL)
  find_package(OpenGL REQUIRED)
  target_link_libraries(yocto PUBLIC glad imgui glfw ${OPENGL_gl_LIBRARY})
endif(YOCTO_OPENGL)

if(YOCTO_EMBREE)
  target_compile_definitions(yocto PUBLIC -DYOCTO_EMBREE)
  target_link_libraries(yocto PUBLIC embree)
endif(YOCTO_EMBREE)

if(YOCTO_DENOISE)
  target_compile_definitions(yocto PUBLIC -DYOCTO_DENOISE)
  target_link_libraries(yocto PUBLIC openimagedenoise)
endif(YOCTO_DENOISE)

# warning flags
if(APPLE)
  target_compile_options(yocto PUBLIC -Wall -Wconversion -Wno-sign-conversion -Wno-implicit-float-conversion -Wno-unused-variable)
endif(APPLE)
if(MSVC)
  target_compile_options(yocto PUBLIC /D_CRT_SECURE_NO_WARNINGS)
  target_compile_options(yocto PUBLIC /EHsc)
endif(MSVC)