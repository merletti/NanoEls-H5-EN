# Building the English interface `h5_en.tft`

The compiled interface `h5_en.tft` and the project `h5_en.HMI` are already in
this folder: to use the panel, upload the `.tft` (§7) and skip the build. This
guide explains how to rebuild it in Nextion Editor from the Italian project
`_sorgente/h5_it.HMI`, with the English backgrounds from `sfondi/` inserted.
Working time is about 15 minutes.

The backgrounds already contain every static label (titles, card texts,
drawings), so no component has to be edited: you only import 20 pictures and
assign each page its background.

> **The firmware must be the English one**: `03_firmware/h5-EN-20261111-v65.bin`.
> In the English backgrounds the cards are already baked into the working
> pages; this edition's firmware no longer draws the quoted cards from
> pictures 44–46. The Italian firmware would paint its cards over the English
> ones.

---

## 0. What you need

- **Nextion Editor** (1.65 or newer) installed on Windows.
- The `02_display` folder: it contains `h5_en.HMI` (the finished project),
  `h5_en.tft` (the compiled interface), `_sorgente/h5_it.HMI`, `sfondi/`
  (20 PNGs) and this guide.
- A Nextion NX8048P050 display (H5, 800×480) and a FAT32 microSD card.
- A backup copy of the display's current `h5.tft`, in case you need to go back.

---

## 1. Open the project and save it under a new name

1. Copy `_sorgente/h5_it.HMI` to a separate working folder, for example
   `Documents\NanoEls-EN\`, and open the copy in Nextion Editor:
   **File → Open**.
2. Immediately: **File → Save As** → `h5_en.HMI` in the same folder.
   From now on work only on `h5_en.HMI`; leave the original `h5_it.HMI` alone.

### Three rules you must not break

1. **Do not delete components.** The firmware addresses buttons by ID, and the
   ID is the creation order. After a deletion every following ID shifts and the
   buttons start responding incorrectly. If a component is not needed, set
   `vis` = No.
2. **Do not rename components** (`tX`, `bMode`, `tPitch`…). The firmware writes
   values by those names.
3. **Do not change the page order** in the Page panel: the position in the list
   is the index the firmware uses.

---

## 2. Import the English backgrounds

Bottom-left panel → **Picture** tab → **Add** → pick a file from `sfondi/`.

Import them **one at a time, strictly in the order of the table**. Nextion
appends new pictures at the end, so in `h5_it.HMI` (60 pictures, 0–59) the new
ones become **60–79**. Note the number the editor shows next to each picture: if
it differs from the table, use your own numbers from then on.

| No. | File | Content |
|---|---|---|
| 60 | `home.png` | MENU (13 modes) |
| 61 | `ingranaggi.png` | GEARBOX, F1, with card |
| 62 | `filetto.png` | THREADING, F2, with card |
| 63 | `tornitura.png` | TURNING, F3, with card |
| 64 | `spianatura.png` | FACING, F4, with card |
| 65 | `cono.png` | CONE, F5, with card |
| 66 | `troncatura.png` | PARTING, F6, with card |
| 67 | `ellisse.png` | ELLIPSE, F7, with card |
| 68 | `asincrono.png` | ASYNC, F8, with card |
| 69 | `gcode.png` | G-CODE, F9 |
| 70 | `assey.png` | Y AXIS, F10, with card |
| 71 | `joystick.png` | JOYSTICK, F11, with card |
| 72 | `xgear.png` | X-GEARS, F12, with card |
| 73 | `comando.png` | JOG (manual control) |
| 74 | `impostazioni.png` | SETTINGS (panel headings) |
| 75 | `wifi.png` | WIFI |
| 76 | `info.png` | INFO / STATUS |
| 77 | `scanalatura.png` | GROOVING, F13, with card |
| 78 | `tastiera.png` | numeric keypad |
| 79 | `lavorazione.png` | MACHINING (live page) |

> Do not add the same picture twice and do not delete imported ones: the indexes
> shift. The old Italian pictures (including 44–46) can stay; they are no longer
> used.

---

## 3. Assign the background to each page

**Page** tab → select a page by its **index** (position in the list) → the
**Attribute** panel on the right:

- `sta` = **Image**
- `pic` = the English background number from the table above
- `bco` = `0841`

| Index | Page | `pic` |
|---|---|---|
| 0 | MENU | 60 |
| 1 | GEARBOX (F1) | 61 |
| 2 | THREADING (F2) | 62 |
| 3 | TURNING (F3) | 63 |
| 4 | FACING (F4) | 64 |
| 5 | CONE (F5) | 65 |
| 6 | PARTING (F6) | 66 |
| 7 | ELLIPSE (F7) | 67 |
| 8 | ASYNC (F8) | 68 |
| 9 | G-CODE (F9) | 69 |
| 10 | Y AXIS (F10) | 70 |
| 11 | JOYSTICK (F11) | 71 |
| 12 | X-GEARS (F12) | 72 |
| 13 | JOG (command) | 73 |
| 14 | SETTINGS | 74 |
| 15 | WIFI | 75 |
| 16 | INFO / STATUS | 76 |
| 17 | GROOVING (F13) | 77 |
| 18 | numeric keypad | 78 |
| 21 | MACHINING | 79 |

Pages **19** (settings panel) and **20** (text keyboard) are drawn by the
firmware on a solid background — do not change their `pic`.

> If the import produced different numbers (for example the project holds more
> pictures), use the actual ones: what matters is the page → file match, not the
> number.

---

## 4. What not to touch

- Fonts (`it16`, `it22`, `it30`, `it40`) and page components: they handle
  touches, and the static labels are already in the backgrounds.
- Sprites: picture **38** (values, 800×52), **39** (keyboard), **42** (computed
  line labels), **43** (card values). The firmware draws digits with them;
  shifting their numbers breaks the display.
- Hotspots `m0`…`m17` and the menu buttons.

---

## 5. Check in the simulator

**File → Simulator** (or `Ctrl+F5`):

1. MENU opens with the 13 English mode names and the header `MENU`.
2. Open GEARBOX: title `GEARBOX`, code `F1`, card labels
   `STATUS` / `MODE`, footer `Tap a value to enter it · −/+ to correct`.
3. Touch a value: the numeric keypad opens with `EDIT VALUE`, `OK` and
   `CANCEL`.
4. Check PARTING (F6) and GROOVING (F13): the cards are in place and the value
   fields are empty.
5. Check CONE (F5): five rows, and the computed ratio (RATIO) is in the side
   card.
6. MACHINING page: labels in English, `MAX` / `MIN` buttons in place.

If the background does not appear, the page has no `pic` or `sta` is not
Image.

---

## 6. Compile

**File → TFT file output** (the toolbar *Compile* button opens the same
dialog). Choose the output folder, keep the name `h5_en.tft` and press
**Output**; the editor writes the file when the progress bar ends.

- The result must fit the display's 16 MB flash: the English build is
  6,501,604 bytes.
- On a `picture too large` error, check that the PNGs are 800×480.
- Keep the result named `h5_en.tft` and put it in `02_display/`.

---

## 7. Upload to the display

**Method 1 — microSD (first upload, most reliable):**

1. Format the microSD card as FAT32.
2. Copy `h5_en.tft` to the card root and rename it to **`h5.tft`** (name and
   case matter).
3. Power the display off, insert the card, power it on. A progress bar runs on
   screen; after the success message, power off, remove the card and power on
   again.
4. If the upload does not start: the card is not FAT32, the file is not in the
   root, or the name differs.

**Method 2 — from the controller web interface:**

1. Open the web interface → *Update Nextion TFT* → **Browse** and pick
   `h5_en.tft`.
2. Speed 115200 baud; for a factory display or on the first upload tick
   *First upload / factory display (9600 baud)*.
3. Do not cut power before the completion message (about 6 minutes).

> The interface and the firmware can be updated in any order, but the firmware
> must be from this edition (20261111-v65) — otherwise the working pages will
> show the Italian cards from the old pictures 44–46.

---

## 8. What to check on the assembled display

- [ ] MENU: 13 English names, `MENU` header, `MODES AND SERVICES` subtitle.
- [ ] Any working page: English labels, code F1…F13, card with `STATUS`/`MODE`.
- [ ] Touching a value opens the keypad; `OK` applies, `CANCEL` discards.
- [ ] MACHINING opens when a mode starts and the coordinates update.
- [ ] SETTINGS: panel headings in English, rows and values in Latin script (by
      design: the panel font is Latin-only).
- [ ] WIFI: states `DISABLED`, `CONNECTING`, `CONNECTED`, `AP SETUP`; the
      password is always `********`.
- [ ] On an emergency stop, `ESTOP - <reason>` appears with a red frame.
- [ ] The version line `EN 20261111` is visible bottom-right, and INFO / STATUS
      shows the same code.

If everything matches, save `h5_en.HMI` and `h5_en.tft` in `02_display/` — that
is the finished interface of the English edition.
