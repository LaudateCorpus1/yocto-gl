add_library(openimagedenoise SHARED IMPORTED GLOBAL)

if(APPLE)
  target_include_directories(openimagedenoise INTERFACE /usr/local/include)
  set_target_properties(openimagedenoise PROPERTY
    IMPORTED_LINK_INTERFACE_LANGUAGES "CXX"
    IMPORTED_LOCATION "/usr/local/lib/libOpenImageDenoise.dylib"
    INTERFACE_INCLUDE_DIRECTORIES "/usr/local/include")
endif(APPLE)
if(MSVC)
  target_include_directories(openimagedenoise INTERFACE "/Program\ Files/Intel/OpenImageDenoise/include/")
  set_target_properties(openimagedenoise PROPERTIES
    IMPORTED_LINK_INTERFACE_LANGUAGES "CXX"
    IMPORTED_LOCATION "/Program\ Files/Intel/OpenImageDenoise/bin/OpenImageDenoise.dll"
    IMPORTED_IMPLIB   "/Program\ Files/Intel/OpenImageDenoise/lib/OpenImageDenoise.lib"
    INTERFACE_INCLUDE_DIRECTORIES "/Program\ Files/Intel/OpenImageDenoise/include/")
endif(MSVC)
if(UNIX AND NOT APPLE)
  target_include_directories(openimagedenoise INTERFACE /usr/local/include)
  set_target_properties(openimagedenoise PROPERTY
    IMPORTED_LINK_INTERFACE_LANGUAGES "CXX"
    IMPORTED_LOCATION "/usr/local/lib/libOpenImageDenoise.so"
    INTERFACE_INCLUDE_DIRECTORIES "/usr/local/include")
endif()

add_library(openimagedenoise::openimagedenoise ALIAS openimagedenoise)
