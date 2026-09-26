# SPDX-License-Identifier: Apache-2.0
# esp-hosted P4-host camera app.  The application logic (camera + audio + the
# encrypted nn_netstream/nn_sectun uplink over the C6 Wi-Fi radio) is IDENTICAL
# to nn-app-media — only the esp-hosted TRANSPORT differs: this variant uses
# SDIO 4-bit on the ESP32-P4-Function-EV-Board (on-PCB, pull-ups present),
# whereas nn-app-media uses SPI-HD over the jumper bench.  So we REUSE
# nn-app-media's app_main.c verbatim (no source duplication); the transport is
# selected entirely in sdkconfig.defaults.
# monorepo: sibling app dir; standalone clone: the nn-app-media SUBMODULE.
# Never a vendored copy — copies drift.
set(_media_main "${CMAKE_CURRENT_LIST_DIR}/../../nn-app-media/main")
if(NOT EXISTS "${_media_main}/app_main.c")
    set(_media_main "${CMAKE_CURRENT_LIST_DIR}/../nn-app-media/main")
endif()
if(NOT EXISTS "${_media_main}/app_main.c")
    message(FATAL_ERROR "app_main.c not found — init the nn-app-media submodule")
endif()

idf_component_register(
    SRCS "${_media_main}/app_main.c" "${_media_main}/lp_watchdog.c"
    INCLUDE_DIRS "${_media_main}" "."
    REQUIRES "nn_registry" "console" "nvs_flash" "esp_timer"
             "nn_camera" "nn_audio"
             "nn_netstream" "nn_sectun" "nn_crypto" "nn_ctrl" "nn_timesync" "nn_prov"
             "esp_wifi" "lwip" "esp_http_client" "esp_hosted" "esp_hal_wdt"
)
