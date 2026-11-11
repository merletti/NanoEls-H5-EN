# NanoEls H5 — English edition

English edition of **NanoEls H5**, an electronic leadscrew (ELS) controller for
lathes built on an ESP32-S3 with a Nextion H5 800×480 display. Firmware, display
interface and documentation are in English; the machine geometry and behaviour
match the Italian edition.

**Status:** firmware, TFT and documents are built. The bench test on the lathe
is still pending.

## Contents

| Path | Content |
|---|---|
| [`00_README.md`](00_README.md) | Full index: inventory, build notes, hashes, remaining work |
| [`03_firmware/`](03_firmware/) | English sketch and compiled binary for ESP32-S3 |
| [`02_display/`](02_display/) | Nextion project, compiled TFT, 20 backgrounds, mode cards, previews |
| [`04_documentazione/`](04_documentazione/) | User manual, assembly checklist, wiring schematic (HTML, PNG, A4 PDF) |
| [`01_glossario/`](01_glossario/) | IT→EN glossary and term tables used for the translation |

## Downloads

| File | Notes |
|---|---|
| [`h5-EN-20261111-v65.bin`](03_firmware/h5-EN-20261111-v65.bin) | Firmware for ESP32-S3 / H5V65, for OTA or Arduino IDE |
| [`h5_en.tft`](02_display/h5_en.tft) | Compiled Nextion interface, 800×480 |
| [`h5_en.HMI`](02_display/h5_en.HMI) | Nextion Editor project (pictures 60–79) |

Build and upload instructions are in [`04_documentazione/README_EN.md`](04_documentazione/README_EN.md);
rebuilding the TFT is covered in [`02_display/GUIDA_TFT_EN.md`](02_display/GUIDA_TFT_EN.md).

## Related

- Italian edition: https://github.com/merletti/NanoEls-H5

## License

MIT — see [`LICENSE`](LICENSE).
