project(obs-bilibili)

if(MSVC)
	set(obs-ffmpeg_PLATFORM_DEPS
		w32-pthreads)
endif()

find_package(FFmpeg REQUIRED
	COMPONENTS avcodec avfilter avdevice avutil swscale avformat swresample)
include_directories(${FFMPEG_INCLUDE_DIRS})

find_package(Libcurl REQUIRED)
include_directories(${LIBCURL_INCLUDE_DIRS})

include_directories(${OBS_JANSSON_INCLUDE_DIRS})

if(UNIX AND NOT APPLE)
        find_package(Libpci REQUIRED)
        include_directories(${LIBPCI_INCLUDE_DIRS})
endif()

set(obs-bilibili_SOURCES
  obs-bilibili-live-source.c)

if(WIN32)
	set(MODULE_DESCRIPTION "OBS Bilibili module")
	configure_file(${CMAKE_SOURCE_DIR}/cmake/winrc/obs-module.rc.in obs-bilibili.rc)
	list(APPEND obs-bilibili_SOURCES
		obs-bilibili.rc)
endif()

add_library(obs-bilibili MODULE
	${obs-bilibili_SOURCES})
target_link_libraries(obs-bilibili
	libobs
	opts-parser
	media-playback
	${obs-ffmpeg_PLATFORM_DEPS}
	${FFMPEG_LIBRARIES}
  ${OBS_JANSSON_IMPORT}
	${LIBCURL_LIBRARIES})
set_target_properties(obs-bilibili PROPERTIES FOLDER "plugins")

install_obs_plugin_with_data(obs-bilibili data)
