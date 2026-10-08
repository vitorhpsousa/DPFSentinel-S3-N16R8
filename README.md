# DPF Sentinel — ESP32-S3 N16R8 (no screen)

A small, **read-only** OBD-II logger for the diesel particulate filter (DPF) of a 2014 Hyundai ix35 1.7 CRDi.
It reads soot level, differential pressure, exhaust temperature and regeneration state about once a second
over a BLE ELM327 adapter and **saves everything to a microSD card**. Telegram alerts exist but are off by default.

The goal is a legal, non-invasive way to catch a clogging DPF early and keep regenerations completing —
the opposite of DPF/EGR "delete" tuning, which is illegal for road use and harmful. Not a diagnostic tool;
do not base safety or repair decisions on it. Only the ix35 is verified (see [docs/pids.md](docs/pids.md)).

## Supported hardware

| Role | Part |
|---|---|
| OBD adapter | Bluetooth LE ELM327 (Amazon UK B0DQV19QJF) |
| Headless logger | Freenove ESP32-S3 WROOM kit, USB-C, microSD (B0F48DV38M) |
| Optional | DS3231 RTC on I2C ([wiring](docs/wiring.md)) |

Details and pins: [docs/hardware.md](docs/hardware.md).

## Quick start

Windows step-by-step: [docs/flashing-windows.md](docs/flashing-windows.md) (macOS/Linux guides to follow).

```bash
git clone https://github.com/vitorhpsousa/DPFSentinel-S3-N16R8.git && cd DPFSentinel-S3-N16R8
cp include/secrets.example.h include/secrets.h     # optional: your WiFi for NTP time
pio run -t upload                                # build and flash
pio device monitor
```

Insert a FAT32 microSD card. Logs appear in `/obd` as `session_N.csv` (the data) and `raw_N.log`
(every raw adapter reply, for debugging). Plug the adapter into the car and power the board from a
switched USB socket.

## Where is the time from?

No RTC is required. The clock is set from a DS3231 if fitted, else NTP when your WiFi is in range, else from
your phone: join the `DPF-Sentinel` hotspot, open `http://192.168.4.1` and it sets the time. Rows logged
before the clock is known have an empty `unix_time`; `tools/date_session.py` back-fills them from the
`TIME` marker in the raw log. See [docs/log-format.md](docs/log-format.md).

## Layout

```
src/            firmware (obd/ logging/ dpf/ web/ report/ )
include/        secrets.example.h  (copy to secrets.h, gitignored)
test/           native unit tests built from real captured replies:  pio test -e native
tools/          log helpers (date back-fill, Car Scanner log solver)
docs/           hardware, wiring, log format, PIDs, development, known issues
logs/examples/  a real driving session
```

If this saves you a DPF job, you can support the project on [Ko-fi](https://ko-fi.com/vitoi).

Licence: GPL-3.0-or-later. See [NOTICE.md](NOTICE.md).
