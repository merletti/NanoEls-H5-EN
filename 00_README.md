# NanoEls H5 — English edition

Workspace for the English edition of the project. It mirrors the layout of
`NanoEls-H5-RU` and is a self-contained delivery folder.

**Status: display, firmware, TFT and documents built.** The vocabulary is
fixed, the 20 English backgrounds are rendered, `h5_en.ino` with its compiled
binary is ready, the Nextion project `h5_en.HMI` and the compiled interface
`h5_en.tft` are in `02_display/`, and the user manual, the lathe assembly
checklist, the README and the wiring schematic are in English. The interface
still has to be checked on the machine; the bench test is the next step.

## What is already here

| Path | Content |
|---|---|
| `01_glossario/GLOSSARIO_IT_EN.md` | IT→EN glossary: machine terms, modes, service screens, TFT short forms, style rules |
| `01_glossario/traduzioni_*.tsv` | Eight term tables: firmware, C++, web HTML/JS, templates, TFT, dual, screens |
| `02_display/sfondi/` | 20 English TFT backgrounds, 800×480, ready for Nextion pictures |
| `02_display/pictures/card_strip_{a,b,c}_en.png` | Quoted mode cards, 288×1790 (five cards each; strip C has one) |
| `02_display/screens_en.html`, `lavorazione_en.html` | English screen previews (same source used for the renders) |
| `02_display/GUIDA_TFT_EN.md` | Step-by-step guide to rebuild `h5_en.tft` in Nextion Editor |
| `02_display/h5_en.HMI` | English Nextion Editor project (pictures 60–79, pages assigned) |
| `02_display/h5_en.tft` | English display interface, compiled (6,501,604 bytes) |
| `02_display/_sorgente/h5_it.HMI` | Italian Nextion Editor project — base for the English HMI |
| `03_firmware/_sorgente/h5.ino` | Italian edition firmware, used as the starting point |
| `03_firmware/h5_en.ino` | English firmware: web interface in English, same geometry and logic |
| `03_firmware/h5-EN-20261111-v65.bin` | Compiled firmware for ESP32-S3 / H5V65 (OTA or Arduino IDE) |
| `03_firmware/_build/` | Generator (`traduci_fw.pl`), term tables, extracted web UI and compile output |
| `04_documentazione/README_EN.md` | Project README in English: files, hardware, build, upload, status |
| `04_documentazione/MANUALE_USO_EN.md` | User manual in English: modes, procedures, messages, emergencies |
| `04_documentazione/MONTAGGIO_TORNIO_EN.md` | Lathe assembly and commissioning checklist |
| `04_documentazione/cablaggio/schema_cablaggio_en.html` | Wiring schematic in English (PNG and A4 PDF alongside) |
| `04_documentazione/_sorgente/schema_cablaggio_it.html` | Italian wiring schematic — source for the English version |
| `LICENSE` | MIT license of the project |

Why the Italian firmware is the base: it is the current source of truth, and
the only structural difference introduced by the Russian edition is the removal
of the card backdrop drawing, because the quoted cards are baked into the
backgrounds. That change is carried over here; everything else that is
Russian-specific (Cyrillic-safe TFT strings, `trTft`, ASCII filtering, CSS
tweaks) is not needed, since English is plain ASCII. The web interface is
translated with the term tables in `01_glossario/`.

## Remaining before release

- Practical test of firmware and TFT on the machine (bench test).
- Publication of the edition, only after explicit approval.

## TFT build

| Item | Value |
|---|---|
| Project | `02_display/h5_en.HMI` (from `_sorgente/h5_it.HMI`, pictures 60–79) |
| Output | `02_display/h5_en.tft` (6,501,604 bytes) |
| SHA-256 | `6c21bc236585ef7bb3a83b2b16fce1990331289a578f423f0d955f658f7bd084` |
| Editor | Nextion Editor 1.68.2.3035, NX8048P050, 800×480 |
| Build output | `File` → *TFT file output* (the toolbar *Compile* alone writes no file) |

The steps to rebuild it from the backgrounds are in `02_display/GUIDA_TFT_EN.md`.

## Firmware build

| Item | Value |
|---|---|
| Source | `03_firmware/h5_en.ino` |
| Binary | `03_firmware/h5-EN-20261111-v65.bin` (1,295,312 bytes) |
| SHA-256 | `75f38eb8c11e5d4f79238bce726534789deac09638b185217fd058acb241ef1b` |
| Toolchain | Arduino CLI 1.5.1, core `esp32` 3.3.12 |
| FQBN | `esp32:esp32:esp32s3:FlashSize=4M,PartitionScheme=min_spiffs,PSRAM=disabled` |
| Size | sketch 1,295,165 bytes (65% of the app partition), RAM 57,456 bytes (17%) |
| UI marker | `EN 20261111` |

To regenerate the source, run `traduci_fw.pl` from `03_firmware/_build/` with
`_sorgente/h5.ino` and the eight tables in `01_glossario/`; then compile
`h5_en.ino` with the settings above.

## Planned build order

1. ~~IT→EN glossary (fixes the vocabulary once for UI, TFT and docs).~~ Done.
2. ~~English backgrounds and card strips, rendered at 800×480 and 288×1790.~~ Done.
3. ~~Web interface translated in `h5_en.ino`, compiled and exported as binary.~~ Done.
4. ~~TFT build guide.~~ Done; `h5_en.tft` compiled and saved in `02_display/`.
5. ~~Documents and wiring schematic in English.~~ Done.
6. ~~Consistency scan (links, strings, sizes) and final inventory.~~ Done
   (11/11: all relative links resolve, no Italian text or wrong edition markers
   in the deliverables, 20 backgrounds, 3 card strips, 16 manual sections).

## Notes

- Reference for layout, geometry and wording: `C:\Users\Admin\Desktop\NanoEls-H5-RU`
  (same page order, same card geometry at x=500, y=74, 288×358).
- Static display labels are inside the background images; only dynamic values,
  statuses and the settings panel are drawn by the firmware, in English.
- The CONE background is rendered from `02_display/_build/cono_en_mockup.html`,
  which reproduces the current five-row page (Ø LARGE, Ø SMALL, Z TRAVEL, SIDE,
  STOP X MAX, ratio in the card). The four-row CONE block in `screens_en.html`
  is old and not used for rendering. The Russian edition still carries the
  four-row CONE background.
- The English manual and assembly checklist use the actual firmware strings
  (`ESTOP - Switched off in manual`, `TRAVEL LIMIT MAX/MIN`, `REVS`, `RPM`,
  `ANGLE DEG`). Two statements inherited from the Italian manual were corrected
  against H5V65: holding STOP for more than 3 s performs the general reset, and
  the configuration backup contains the machine parameters only (not keyboard or
  WiFi).
- INFO / STATUS has two separate cells: firmware `H5V65 20261030` (version date
  embedded in the controller) and interface `EN 20261111`. This is the same
  pattern as the Italian and Russian editions, so the firmware was not rebuilt.
- Do not publish anything (site, releases, repository) without explicit approval.
