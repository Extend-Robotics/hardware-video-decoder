cmake_minimum_required(VERSION 3.0)

project(
    hvd
)

list(APPEND CMAKE_MODULE_PATH "/home/meglickib/test/vcpkg/installed/x64-linux-er/share/ffmpeg")
set(CMAKE_INSTALL_RPATH $ORIGIN/lib)

find_package(FFMPEG REQUIRED)

add_library(hvd hvd.c)
target_link_libraries(hvd ${FFMPEG_LIBRARIES})

install(TARGETS hvd DESTINATION lib)
install(DIRECTORY ${FFMPEG_LIBRARY_DIRS} DESTINATION lib FILES_MATCHING PATTERN "*.so*")
install(FILES hvd.h DESTINATION include)

add_executable(hvd-decoding-example examples/hvd_decoding_example.c)
target_link_libraries(hvd-decoding-example hvd)

