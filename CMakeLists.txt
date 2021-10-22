cmake_minimum_required(VERSION 3.0)

project(
    hvd
)

# system vs custom LGPL FFmpeg build

## optionally point to your path or pass as -DFFMPEG_MODULE_PATH="..." from CLI
# set(FFMPEG_MODULE_PATH "/home/meglickib/test/vcpkg/installed/x64-linux/share/ffmpeg")

if(DEFINED FFMPEG_MODULE_PATH) # point to custom FindFFMPEG.cmake with LGPL build
  list(APPEND CMAKE_MODULE_PATH ${FFMPEG_MODULE_PATH})
  set(CMAKE_INSTALL_RPATH $ORIGIN/lib)
  find_package(FFMPEG REQUIRED)
  LIST(GET FFMPEG_LIBRARY_DIRS 0 FFMPEG_LIBRARY_RELEASE_DIR)
else()
  MESSAGE(WARNING "trying to use system FFMmpeg libraries, define FFMPEG_MODULE_PATH for custom distributable FFmpeg LGPL build")
  set(FFMPEG_LIBRARIES avcodec avutil)
endif()

# build

add_library(hvd hvd.c)
target_link_libraries(hvd ${FFMPEG_LIBRARIES})

add_executable(hvd-decoding-example examples/hvd_decoding_example.c)
target_link_libraries(hvd-decoding-example hvd)

# install

install(TARGETS hvd DESTINATION lib)
install(FILES hvd.h DESTINATION include)

if(DEFINED FFMPEG_MODULE_PATH)
  install(DIRECTORY ${FFMPEG_LIBRARY_DIRS} DESTINATION lib FILES_MATCHING PATTERN "*.so*")
endif()
