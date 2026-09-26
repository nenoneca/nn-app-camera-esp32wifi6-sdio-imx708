# SPDX-License-Identifier: Apache-2.0
cmake_minimum_required(VERSION 3.16)

# Shared nn camera-system components (one level up in the media/ monorepo, or
# vendored into ./components in a standalone clone).
# Monorepo: the shared component set one level up.  Standalone clone: this repo
# carries NO component code at all — the nn_* wrappers come from the
# nn-app-media submodule and the esp_* camera components from nn-app-media's
# OWN nn-modules submodule (hence `git submodule update --init --recursive`).
if(EXISTS "${CMAKE_CURRENT_LIST_DIR}/../components")
    set(EXTRA_COMPONENT_DIRS "${CMAKE_CURRENT_LIST_DIR}/../components")
else()
    set(EXTRA_COMPONENT_DIRS "${CMAKE_CURRENT_LIST_DIR}/nn-app-media/components")
    list(APPEND EXTRA_COMPONENT_DIRS "${CMAKE_CURRENT_LIST_DIR}/nn-app-media/nn-modules/components")
endif()

# Reuse nn-app-media's SHARED base config and select the SDIO transport (pins
# defined in nn-app-media/sdkconfig.transport.sdio).  Monorepo: the files live in
# ../nn-app-media; a standalone clone vendors them next to this app.
if(NOT SDKCONFIG_DEFAULTS)
    set(_shared "${CMAKE_CURRENT_LIST_DIR}/../nn-app-media")
    if(NOT EXISTS "${_shared}/sdkconfig.defaults")
        set(_shared "${CMAKE_CURRENT_LIST_DIR}/nn-app-media")   # standalone: submodule
    endif()
    set(SDKCONFIG_DEFAULTS "${_shared}/sdkconfig.defaults;${_shared}/sdkconfig.transport.sdio")
endif()

include($ENV{IDF_PATH}/tools/cmake/project.cmake)
project(nn-app-camera-esp32wifi6-sdio-imx708)
