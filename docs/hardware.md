# Hardware

## Board: ESP32-S3-WROOM-1 N16R8 (no screen)
Freenove ESP32-S3 WROOM kit (B0F48DV38M): 16 MB flash, 8 MB octal PSRAM, USB-C, microSD slot. The camera is unused.
microSD on SD_MMC **1-bit**: CLK 39, CMD 38, D0 40 (D1-D3 unused). These pins come from Freenove's published layout
and are **not yet verified on this unit**: the first boot should print `storage=SD card`; if it says flash, check the
pins in `src/config.h`. GPIO 1 and 2 are free for status LEDs (`LED_REGEN_PIN`, `LED_TEMP_WARN_PIN`).
