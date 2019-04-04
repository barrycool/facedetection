cmake_minimum_required(VERSION 3.4.1)

set(libs "${CMAKE_SOURCE_DIR}/src/main/jnilibs")

add_library(libopencv SHARED IMPORTED)
set_target_properties(libopencv PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libopencv_java3.so")

set(LOCAL_LDLIBS  " -lz ")
add_library(libtengine SHARED IMPORTED)
set_target_properties(libtengine PROPERTIES
        IMPORTED_LOCATION "${libs}/${ANDROID_ABI}/libtengine.so")

include_directories(${CMAKE_SOURCE_DIR}/src/main/cpp)

add_library( # Sets the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp
             src/main/cpp/Tengine_Wrapper.cpp
        )

find_library( # Sets the name of the path variable.
              log-lib

              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

target_link_libraries( # Specifies the target library.
                       native-lib

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} jnigraphics libopencv libtengine)