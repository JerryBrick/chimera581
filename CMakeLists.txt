project(Chimera581)

cmake_minimum_required(VERSION 3.16)

set(CMAKE_CXX_STANDARD 17)

enable_language(CXX C ASM)

SET(CMAKE_CXX_FLAGS_RELEASE "-O2")

set(CMAKE_ASM_FLAGS "${CMAKE_ASM_FLAGS} -q")

set(CMAKE_RC_FLAGS "${CMAKE_RC_FLAGS} --output-format coff")

include_directories(${CMAKE_SOURCE_DIR}/include)

add_library(chimera SHARED
    src/chimera.cpp
	src/client/client.cpp
	src/client/client_signature.cpp
	src/client/hac2.cpp
	src/client/keystone.cpp
	src/client/open_sauce.cpp
	src/client/path.cpp
	src/client/settings.cpp
	src/client/command/command.cpp
	src/client/command/console.cpp
	src/client/debug/budget.cpp
	src/client/debug/devmode.cpp
	src/client/debug/eep.cpp
	src/client/debug/wireframe.cpp
	src/client/enhancements/auto_center.cpp
	src/client/enhancements/disable_buffering.cpp
	src/client/enhancements/firing_particle.cpp
	src/client/enhancements/gamepad_vertical_scale.cpp
	src/client/enhancements/mouse.cpp
	src/client/enhancements/multitexture_overlay.cpp
	src/client/enhancements/server_messages.cpp
	src/client/enhancements/show_spawn.cpp
	src/client/enhancements/skip_loading.cpp
	src/client/enhancements/throttle_fps.cpp
	src/client/enhancements/uncap_cinematic.cpp
	src/client/enhancements/zoom_blur.cpp
	src/client/fix/camo_fix.cpp
	src/client/fix/descope_fix.cpp
	src/client/fix/fov_fix.cpp
	src/client/fix/magnetism_fix.cpp
	src/client/fix/scope_fix.cpp
	src/client/fix/sniper_hud.cpp
	src/client/fix/widescreen_fix.cpp
	src/client/halo_data/chat.cpp
	src/client/halo_data/global.cpp
	src/client/halo_data/keyboard.cpp
	src/client/halo_data/map.cpp
	src/client/halo_data/resolution.cpp
	src/client/halo_data/script.cpp
	src/client/halo_data/server.cpp
	src/client/halo_data/spawn_object.cpp
	src/client/halo_data/table.cpp
	src/client/halo_data/tag_data.cpp
	src/client/halo_data/tiarace/hce_tag_class_int.cpp
	src/client/hooks/camera.cpp
	src/client/hooks/frame.cpp
	src/client/hooks/map_load.cpp
	src/client/hooks/rcon_message.cpp
	src/client/hooks/tick.cpp
	src/client/hud_mod/offset_hud_elements.cpp
	src/client/interpolation/camera.cpp
	src/client/interpolation/fp.cpp
	src/client/interpolation/interpolation.cpp
	src/client/interpolation/light.cpp
	src/client/interpolation/particle.cpp
	src/client/interpolation/widget.cpp
	src/client/lua/lua.cpp
	src/client/lua/lua_callback.cpp
	src/client/lua/lua_game.cpp
	src/client/lua/lua_io.cpp
	src/client/messaging/messaging.cpp
	src/client/startup/crc32.c
	src/client/startup/fast_startup.cpp
	src/client/visuals/anisotropic_filtering.cpp
	src/client/visuals/gametype_indicator.cpp
	src/client/visuals/letterbox.cpp
	src/client/visuals/server_ip.cpp
	src/client/visuals/vertical_field_of_view.cpp
	src/client/xbox/hud_kill_feed.cpp
	src/client/xbox/safe_zone.cpp
	src/client/xbox/simple_score_screen.cpp
	src/client/xbox/split_screen_hud.cpp
	src/code_injection/signature.cpp
	src/code_injection/hacclient/codefinder.cpp
	src/math/data_types.cpp
	src/version.rc
)

set(INTEL_SYNTAX_FILES
	src/client/halo_data/chat.cpp
	src/client/halo_data/resolution.cpp
	src/client/halo_data/spawn_object.cpp
	src/client/halo_data/script.cpp
	src/client/messaging/messaging.cpp

)

set_source_files_properties(${INTEL_SYNTAX_FILES} PROPERTIES COMPILE_FLAGS "-O0 -masm=intel")

if(CMAKE_BUILD_TYPE STREQUAL "Release")
	set_source_files_properties(src/math/data_types.cpp PROPERTIES COMPILE_FLAGS "-Ofast -msse3 -fno-exceptions")
endif()

set_target_properties(chimera PROPERTIES PREFIX "")
set_target_properties(chimera PROPERTIES OUTPUT_NAME "chimera")
set_target_properties(chimera PROPERTIES LINK_FLAGS "-shared -lws2_32 -static-libgcc -static-libstdc++ -static -luserenv -static -lpthread -static -ladvapi32")

if(CMAKE_BUILD_TYPE STREQUAL "Release")
	get_target_property(CHIMERA_LINK_FLAGS chimera LINK_FLAGS)
	set_target_properties(chimera PROPERTIES LINK_FLAGS "${CHIMERA_LINK_FLAGS} -Wl,--exclude-all-symbols -s")
endif()

target_link_libraries(chimera ${CMAKE_SOURCE_DIR}/lib/liblua.a)