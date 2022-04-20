project(obs-bilibili)

find_package(
  FFmpeg REQUIRED
  COMPONENTS avcodec
             avfilter
             avdevice
             avutil
             swscale
             avformat
             swresample)

find_package(CURL REQUIRED)

add_library(obs-bilibili MODULE)
add_library(OBS::bilibili ALIAS obs-bilibili)

target_sources(
  obs-bilibili
  PRIVATE obs-bilibili-live-source.c)

target_include_directories(obs-bilibili PRIVATE ${CMAKE_BINARY_DIR}/config)

target_link_libraries(
  obs-bilibili
  PRIVATE OBS::libobs
          OBS::media-playback
          OBS::opts-parser
          FFmpeg::avcodec
          FFmpeg::avfilter
          FFmpeg::avformat
          FFmpeg::avdevice
          FFmpeg::avutil
          FFmpeg::swscale
          FFmpeg::swresample
          CURL::libcurl
          Jansson::Jansson)

set_target_properties(obs-bilibili PROPERTIES FOLDER "plugins" PREFIX
                                                                  "")

if(OS_WINDOWS)
  if(MSVC)
    target_link_libraries(obs-bilibili PRIVATE OBS::w32-pthreads)
  endif()

  set(MODULE_DESCRIPTION "OBS Bilibili module")
  configure_file(${CMAKE_SOURCE_DIR}/cmake/bundle/windows/obs-module.rc.in
                 obs-bilibili.rc)

  target_sources(obs-bilibili PRIVATE obs-bilibili.rc)
endif()

setup_plugin_target(obs-bilibili)
