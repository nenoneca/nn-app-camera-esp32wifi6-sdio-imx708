# nn-app-media (ESP32-P4 · media-1)

The **camera host** of the nn media camera system — the ESP32-P4 application
brain. It has no radios of its own (Wi-Fi/BLE/OpenThread are excluded by the
`nn_registry`) and talks to the ESP32-C6 networking co-processor
([`nn-app-media-network`](https://github.com/chalos/nn-app-media-network)) over
SDIO as the **bus master**.

## Build & flash (ESP-IDF v6.0.1)

```bash
. <path-to>/esp-idf-v6.0.1/export.sh           # see chalos/nn-esp-idf
idf.py set-target esp32p4 build
idf.py -p /dev/ttyACM10 flash monitor
```

> This board is an early P4 sample (silicon rev v1.3); `sdkconfig.defaults.esp32p4`
> lowers the minimum chip revision accordingly.

## SDIO wiring (P4 master, GPIO matrix)

| CLK | CMD | D0 | D1 | D2 | D3 |
|-----|-----|----|----|----|----|
| GPIO2 | GPIO3 | GPIO15 | GPIO16 | GPIO17 | GPIO18 |

The shared SDIO transport, feature registry, and `node_mgr` ESP-IDF port live in
`components/` (vendored from the `media/` monorepo). See the monorepo README for
the full architecture, the C6 side, and bring-up notes (external pull-ups are
required on CMD/D0–D3 for reliable data transfer).

## CLI (Milestone 1)

`media>` prompt: `link-connect`, `link-send <text>`, `link-status`.
