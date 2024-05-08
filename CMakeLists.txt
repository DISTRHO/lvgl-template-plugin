# DISTRHO Plugin Framework (DPF)
# Copyright (C) 2021-2024 Filipe Coelho <falktx@falktx.com>
# SPDX-License-Identifier: ISC

cmake_minimum_required(VERSION 3.7)

# setup project
set(CMAKE_CXX_STANDARD 14)
set(NAME lvgl-demo-plugin)
project(${NAME})

# tell LVGL where to find its config file
set(LV_CONF_PATH "${PROJECT_SOURCE_DIR}/src/lv_conf.h")

# include DPF and LVGL
add_subdirectory(deps/dpf)
add_subdirectory(deps/lvgl)

# create plugin build target
dpf_add_plugin(${NAME}
  TARGETS clap lv2 vst2 vst3 jack
  FILES_DSP
      src/PluginDSP.cpp
  FILES_UI
      src/PluginUI.cpp
      deps/dpf-widgets/generic/LVGL.cpp)

# setup build includes and libraries
target_include_directories(${NAME} PUBLIC src)
target_include_directories(${NAME} PUBLIC deps/dpf-widgets/generic)
target_link_libraries(${NAME} PUBLIC lvgl::lvgl $<$<BOOL:${WIN32}>:winmm>)

# FIXME LVGL has no way to configure this, report upstream?
set_target_properties(lvgl PROPERTIES POSITION_INDEPENDENT_CODE ON)

# NOTICE you probably do not want this on your final plugin
target_link_libraries(${NAME} PUBLIC lvgl::demos)
