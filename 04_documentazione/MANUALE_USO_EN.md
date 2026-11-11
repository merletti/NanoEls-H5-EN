# NanoEls H5 — User manual

Nextion NX8048P050 panel (5", 800×480) with an English interface.
References for this edition: TFT `h5_en.tft` (EN interface 20261111, compiled;
rebuild steps in `02_display/GUIDA_TFT_EN.md`) and firmware
`h5-EN-20261111-v65.bin`. The web UI is in English as well.

Contents:

1. [What it does](#1-what-it-does)
2. [Safety warnings](#2-safety-warnings)
3. [Power on and MENU](#3-power-on-and-menu)
4. [The mode pages](#4-the-mode-pages)
5. [MACHINING: work in progress](#5-machining-work-in-progress)
6. [Entering values](#6-entering-values)
7. [Units of measure](#7-units-of-measure)
8. [PITCH, STARTS, PASSES](#8-pitch-starts-passes)
9. [Axes, travel limits and jog](#9-axes-travel-limits-and-jog)
10. [The modes one by one](#10-the-modes-one-by-one)
11. [Step-by-step procedures](#11-step-by-step-procedures)
12. [STOP, emergencies, restore](#12-stop-emergencies-restore)
13. [Display messages](#13-display-messages)
14. [Common problems](#14-common-problems)
15. [Service pages](#15-service-pages)
16. [Files, updates and backup](#16-files-updates-and-backup)

---

## 1. What it does

It is an ELS (Electronic Lead Screw): it reads the spindle rotation with an encoder and
drives the stepper motors of the carriage (Z axis) and of the cross slide (X axis), plus
the Y axis if it is installed.

In practice:

* in **GEARBOX / X-GEARS** it replaces the gear train: the axis advances by PITCH mm for
  every spindle turn;
* in **TURNING / FACING / THREADING / PARTING / ELLIPSE / GROOVING** it runs a series of
  passes with increasing depth between two travel limits by itself;
* in the other modes it handles the cone, asynchronous feed, G-code programs, joystick
  and Y axis.

The panel is organized in three levels: **MENU** (choice of the mode or of the service),
**mode page** (setting the values) and **MACHINING** (commands and coordinates during
the work).

## 2. Safety warnings

Read before switching the ELS on.

1. **With the ELS on the carriage can move as soon as you turn the spindle, even by
   hand.** With a pitch other than zero the axis position is imposed by the spindle
   rotation: do not put your hands between carriage and tailstock and do not turn the
   spindle to check if the ELS is on.
2. **START accepts all the confirmations with a single touch and starts the machine.**
   On the MACHINING page the green key starts at once with the values already set on the
   mode page: check PITCH, PASSES, SIDE and the travel limits before pressing it. Opening
   the mode page or the MACHINING page alone does not start anything.
3. **Do not change PITCH or STARTS while the machine is threading.** The reference
   coordinate is reset and the thread comes out wrong. Stop, change, start again.
4. **Do not disable an axis while the machine is working.** Steps keep being counted but
   the motor is de-energized: the position shown no longer matches the real one.
5. **`MAX` and `MIN` work only with the ELS off.** With the ELS on the panel answers
   `TURN OFF FIRST`.
6. The travel limit is a **software protection**: it does not replace the mechanical
   limit switches.
7. In an emergency (`ESTOP`) the machine no longer answers the commands: switch the
   controller off and on again after checking the mechanics.

## 3. Power on and MENU

1. Switch the controller and the display on. The panel shows the **MENU**.
2. The firmware is already installed; the TFT has to be reloaded only when the interface
   is updated (see §16).
3. At the top right there is the ELS status:

| Label | Meaning |
|---|---|
| `ELS OFF` | ELS off, the axes do not follow the spindle |
| `ELS ON` | ELS on and in synchronized motion |
| `ELS SIN` | on, waiting for the correct spindle phase before engaging |

Tapping the `ELS` box switches the state; with the ELS on the same tap switches it off.

### The thirteen modes

| Card | Mode | Subtitle |
|---|---|---|
| F1 | GEARBOX | Mode F1 |
| F2 | THREADING | Mode F2 |
| F3 | TURNING | Mode F3 |
| F4 | FACING | Mode F4 |
| F5 | CONE | Mode F5 |
| F6 | PARTING | Mode F6 |
| F7 | ELLIPSE | Mode F7 |
| F8 | ASYNC | Mode F8 |
| F9 | G-CODE | Mode F9 |
| F10 | Y AXIS | Mode F10 |
| F11 | JOYSTICK | Mode F11 |
| F12 | X-GEARS | Mode F12 |
| F13 | GROOVING | Mode F13 |

Tapping a card opens the mode page: the mode is already selected (and the ELS switches
off if it was on), but the machine does not start.

### The service row

At the bottom there are five items:

| Item | Opens |
|---|---|
| `JOG` | the manual axis control and the keypad (§15) |
| `GCODE` | the list of the saved G-code programs (§15) |
| `SETUP` | the machine settings (§15) |
| `WIFI` | network name, password and connection status (§15) |
| `INFO` | firmware, TFT version, IP address, free memory (§15) |

The strip at the bottom shows a hint or the current state, for example `MENU OFF`; on
the right the interface date appears (`EN 20261111`).

## 4. The mode pages

All the F1–F13 pages have the same structure.

**At the top** the mode name and the subtitle `Mode Fn`.
On THREADING, next to the mode name, the thread-hand box appears (`DIR R` or `DIR L`).

**On the left** the mode fields. Each field has:

* the **value box**: tapping it opens the numeric keypad (§6);
* the two keys **`−`** and **`+`**: they correct the value by one step
  (PITCH: 0.01 mm or 0.001"; in TPI 1 thread; STARTS and PASSES: 1).

Some fields are not typed but switched by tapping them or with `‹` `›`:

* `SIDE` — OUTSIDE / INSIDE (turning, cone, parting, ellipse);
* `DIRECTION` — R → L / L → R (facing), FWD / BACK (grooving);
* `DIR` — `+` / `−` (async, Y axis);
* `DIR R` / `DIR L` — thread hand; with the ELS on the tap is refused with
  `TURN OFF FIRST`.

**On the right** the summary card: the drawing of the machining with the measurements,
the `STATUS` line (OFF, ON, SIN, ESTOP), the `MODE` line with `Fn` and at the bottom the
value derived from the measurements (feed, speed, ratio, total reduction). The derived
values are read-only: they change by themselves when the coordinates are modified.

**At the bottom** two keys:

* **`START MODE`** (green): applies the mode with the current values and opens the
  MACHINING page. It does not start the motion.
* **`CANCEL`** (gray): returns to the MENU. The mode stays the selected one.

The travel limits that are not present as a field (for example the X limits in
THREADING) are set in MACHINING with `MAX` / `MIN` (§9), or in
`SETUP → LIMITS AND STOPS`.

## 5. MACHINING: work in progress

After `START MODE` the panel opens the **MACHINING** page. It is the page from which the
work is commanded.

**At the top** — `PITCH` (mm/rev), `STARTS`, `PASSES`, `DIRECTION` (R/L), `TYPE`
(OUTSIDE/INSIDE) and the blue box with the mode name. Tapping `PITCH`, `STARTS` or
`PASSES` opens the keypad; with the ELS on `TURN OFF FIRST` appears.
The `‹ MENU` key at the top right returns to the MENU without stopping the work.

**Axis cards X (red), Y (green), Z (blue)** — the big line is the current position in
mm (or inches), `MAX` and `MIN` are the two travel limits. Below:

* `ZERO` — zeros the axis position (it does not touch the travel limits); confirmation
  `ZERO X`, `ZERO Y` or `ZERO Z`;
* `MAX` — stores the current position as the MAX travel limit;
* `MIN` — stores the current position as the MIN travel limit.

`MAX` and `MIN` work only with the ELS off; with the ELS on they answer
`TURN OFF FIRST`. If the axis is not enabled they answer `AXIS NOT USED`. If the new
MAX ends up below the MIN (or the other way round) the value is refused with
`MAX BELOW MIN` or `MIN ABOVE MAX`.

**Statistics** — `REVS` (spindle turns since power on), `RPM`, `ANGLE DEG` and `PASS`
(current pass / total).

**STATUS and START/STOP** — the `STATUS` line shows `OFF`, `ON` or `SIN`; in an
emergency it shows `ESTOP`. The green box is the key:

* `START` — switches on and starts in a single touch, accepting together all the
  confirmations of the mode with the values already set (passes, side, ratio,
  positioning). It is the normal behavior for all modes except G-CODE.
* `STOP` — switches off at once and stops the movement.
* In **G-CODE** the confirmations stay separate: one touch confirms the program, another
  confirms the spindle and starts.

If the mandatory travel limits are missing the key only beeps and the strip shows
`Set stops` (Z) or `Set X stop`. In THREADING, if the spindle is not turning, after
`START` the state changes to `SIN` and that is normal: as soon as the spindle turns the
machine engages and starts.

**Jog** — `← Z−`, `Z+ →`, `↑ X+`, `↓ X−` move the axis while the key is held; on release
the movement stops. With the ELS on the jog is allowed only in GEARBOX, X-GEARS, ASYNC,
CONE, Y AXIS and JOYSTICK; in the pass modes and in G-CODE a jog switches the ELS off.
The panel releases the jog by itself after 15 seconds of continuous pressure.

## 6. Entering values

Tapping the box of a value opens the **numeric keypad**:

```
1 2 3        OK
4 5 6
7 8 9     CANCEL
. 0 ←
```

* digits `1…9`, `0`, **`.`** (decimal point), **`←`** (deletes the last digit),
  **`OK`** (confirms), **`CANCEL`** (exits without changing);
* up to **12 characters** can be typed;
* there is only one decimal point; if it is typed first, the panel puts a `0` in front;
* the first digit typed **replaces** the previous value;
* a missing or empty value (for example a travel limit not set) can be typed directly.

The value typed is read in the active unit:

| Unit | Example | Meaning |
|---|---|---|
| MM | `1.25` | 1.25 mm |
| IN | `0.125` | 0.125 inch |
| TPI | `8` | 8 threads per inch |
| PASSES / STARTS | `4` | whole number |
| THREAD CONE | `0.05` | taper ratio (0 = cylindrical) |

On the **JOG** page there is also a direct numeric row (`STOP` and `1…9`, `0`, `CANC`,
`OK`) without a decimal point: in millimeters the digits are thousandths
(`1500` = 1.5 mm), in inches thousandths of an inch (`100` = 0.100"), in TPI threads per
inch. The panel previews it as `Use 1.500?`.

## 7. Units of measure

The `MEASURE` key (JOG page) and the `MEASURE` row in `SETUP → UNITS AND FORMAT` cycle
**MM → IN → TPI**.

* MM: coordinates in millimeters, pitch in mm/rev.
* IN: coordinates in inches, pitch in inch/rev.
* TPI: pitch expressed in threads per inch (inch threads).

Changing unit changes the scale in which the values are shown and typed; the mechanical
position does not move. In TPI the PITCH is a number of threads per inch: for a metric
pitch use MM.

## 8. PITCH, STARTS, PASSES

**PITCH** is the most important parameter. Its meaning depends on the mode:

| Mode | PITCH means |
|---|---|
| GEARBOX, X-GEARS, TURNING, FACING, PARTING, THREADING, ELLIPSE | mm of feed per spindle turn |
| CONE | Z feed per turn; X follows from the cone ratio |
| ASYNC, GROOVING, Y AXIS | feed speed independent of the spindle (mm/s, or °/s with a rotary Y axis) |

Sign of the PITCH:

* positive: pass from MIN towards MAX;
* negative: pass from MAX towards MIN;
* in THREADING the sign is the thread hand: positive `R` (right-hand), negative `L`
  (left-hand);
* in PARTING the sign is the side: positive outside, negative inside.

It is set by typing the value and pressing `OK`, or with `−`/`+` (0.01 mm per touch;
0.001" in inches; 1 TPI in TPI).

**STARTS** is the number of starts of a multi-start thread. The actual movement is
**PITCH × STARTS**, but the display shows only the PITCH: with 3 starts and pitch 2 the
axis advances 6 mm per turn while the screen reads `2`. It is used in multi-start threads
and in the gearbox modes; in the other modes leave it at 1.

**PASSES** is the number of passes of the pass modes. In THREADING the total passes
become **PASSES × STARTS**: every start is cut at every depth.

On the side card the derived value (feed, speed, ratio, total reduction) shows the result
of the calculation and is read-only.

## 9. Axes, travel limits and jog

### Setting the travel limits

The travel limits define the working span and, in the pass modes, the depth. The normal
way is from the MACHINING page:

1. Bring the axis to the wanted position with the jog (or by hand, if the motor is free).
2. Tap `MAX` or `MIN`: the travel limit is fixed there.
3. Repeat for the other travel limit.

Alternatively, on the mode page the travel limits present as a field can be typed (for
example `TRAVEL LIMIT MAX` in GEARBOX), and in `SETUP → LIMITS AND STOPS` the
`STOP Z MAX/MIN` and `STOP X MAX/MIN` rows can be read and modified.

### Which travel limits are needed

| Mode | Travel limits required |
|---|---|
| TURNING, FACING, THREADING, ELLIPSE | Z MIN and MAX, X MIN and MAX |
| PARTING | X MIN and MAX |
| GROOVING | Z MIN and MAX, X MIN and MAX |
| GEARBOX, X-GEARS, ASYNC, Y AXIS, CONE, JOYSTICK | the ones used by the mode |

If a mandatory travel limit is missing, `START` only beeps and the strip reports it.

### Meaning during machining

* **MIN** = start of the pass (with positive pitch), **MAX** = end of the pass;
* on the secondary axis (X in turning, Z in facing) the two travel limits are the
  starting coordinate and the final depth.

### Jog

The six arrows of the JOG page (`↑ X+`, `↓ X−`, `↗ Y+`, `↘ Y−`, `← Z−`, `→ Z+`) and the
four of MACHINING (`← Z−`, `Z+ →`, `↑ X+`, `↓ X−`) move while the key is held. The
physical direction depends on the wiring and on the configured inverters: if `X+` goes
the wrong way it is corrected in the configuration, not from the panel.

There is no step-by-step increment: the speed is the one configured in
`SETUP → SPEED AND RAMPS` (`MANUAL SPEED Z/X/Y`). With the ELS off the jog is always
available; with the ELS on the rule of §5 applies.

## 10. The modes one by one

| Card | Page fields | What it commands | Notes |
|---|---|---|---|
| F1 GEARBOX | PITCH, STARTS, TRAVEL LIMIT MAX/MIN | Z synchronized with the spindle | the simplest: it replaces the gear train |
| F2 THREADING | PITCH, STARTS, THREAD CONE, STOP Z MAX; `DIR R/L` box | Z main, X passes | decreasing pass depth; X limits from MACHINING |
| F3 TURNING | PITCH, PASSES, SIDE, STOP Z MAX | Z main, X passes | the classic cylinder in several passes |
| F4 FACING | PITCH, PASSES, DIRECTION, STOP X MAX | X main, Z passes | facing; the direction is R→L or L→R |
| F5 CONE | Ø LARGE, Ø SMALL, Z TRAVEL, SIDE, STOP X MAX | Z + X synchronized | the ratio is calculated from the measurements |
| F6 PARTING | PITCH, PASSES, SIDE, STOP X MAX | X only | the side follows the sign of the PITCH |
| F7 ELLIPSE | PITCH, PASSES, SIDE, STOP Z MAX | Z main, X profile | PITCH cannot be changed while running |
| F8 ASYNC | PITCH, STARTS, DIR, STOP Z MAX | Z at constant speed | starts without the spindle |
| F9 G-CODE | program list | X, Y, Z | requires the spindle running |
| F10 Y AXIS | Y PITCH, STARTS, DIR, STOP Y MAX | Y | requires the Y axis configured |
| F11 JOYSTICK | NORMAL SPEED, RAPID SPEED, DEADBAND, ACTIVE INPUTS | Z and X with the lever, pitch with the potentiometer | requires the joystick |
| F12 X-GEARS | X PITCH, STARTS, STOP X MAX/MIN | X synchronized with the spindle | gearbox on the cross axis |
| F13 GROOVING | PASSES, LEFT REDUCTION, FEED, DIRECTION, STOP Z MAX/MIN | Z feed, X steps | every pass is shorter to let the chip out |

Useful details:

* **Dimensioned card**: on the right of every page there is the drawing of the machining
  with the measurements shown. In the upper part of the card the PITCH appears (`P`,
  `P X`, `P Y`, `FEED`), on the dimension the travel limit used by the mode (`Z MAX`,
  `X MAX`, `Y MAX`). Under the drawing there are `STATUS` and the `MODE` line with
  `F1…F13`.
* **Calculated row**: at the bottom of the card a value derived from the measurements
  appears, read-only: the feed in mm/rev (pitch × starts) on GEARBOX, THREADING,
  TURNING, FACING, PARTING, ELLIPSE, Y AXIS and X-GEARS; the feed speed in mm/min on
  ASYNC; the max speed in mm/min on JOYSTICK; the total reduction in mm on GROOVING; on
  CONE the derived value is the RATIO.
* **CONE**: the ratio is not typed. You enter Ø large, Ø small and Z travel (which is the
  Z MAX travel limit); the controller works out the taper
  `(Ø large − Ø small) / Z travel` and shows it as `1 : x`, where
  `x = Z travel / (Ø large − Ø small)`: it is the same value as in the cone tables (for a
  Morse 5, `1 : 19`). The X, which is radial, moves by half of that value for every mm of
  Z. With measurements missing or equal the mode moves nothing.
* **TURNING / FACING / THREADING**: the depth decreases at every pass (first pass deeper,
  then progressively lighter); between one pass and the next the tool retracts by 0.5 mm
  so as not to rub on the bottom. In the thread the first pass is even deeper and the
  following ones much lighter.
* **PARTING**: only X moves, synchronized with the spindle. The sign of the PITCH decides
  the side (positive outside, negative inside) and the direction of the pass.
* **THREADING — hand**: right-hand or left-hand is the sign of the PITCH, acquired when
  you switch on (the `DIR R/L` box shows it). Changing it with the ELS on is not
  possible: the panel answers `TURN OFF FIRST`. The spindle rotation alone does not
  change the hand.
* **THREADING — outside/inside**: the page has no `SIDE` field. The type used is the one
  stored by the last choice made in another mode (for example `SIDE` in TURNING or
  ELLIPSE). For an inside thread set it first from one of those pages, then switch to
  THREADING.
* **GROOVING**: Z advances at the FEED speed from one travel limit to the other, then X
  moves by the PITCH and it starts again; every pass is shorter by LEFT REDUCTION.
* **JOYSTICK**: the `ACTIVE INPUTS` line lists the axes enabled in the configuration
  (`Z X Y` or only the ones wired). If the hardware is not configured the card does
  nothing and the strip says so (`Joystick off`, `Joystick not connected`).

## 11. Step-by-step procedures

### A. Threading (THREADING)

1. MENU → `THREADING`.
2. Unit: `MEASURE` on `MM` (or `TPI` for inch threads) from the JOG page.
3. Tap `PITCH` and type the pitch (example M8 pitch 1.25: `1.25`), then `OK`.
4. `STARTS`: leave 1 for a normal thread.
5. `THREAD CONE`: `0` for a cylindrical thread; otherwise the taper ratio.
6. `STOP Z MAX`: end of the thread (with positive pitch the pass goes from MIN to MAX).
7. `DIR R/L` box: `R` for a right-hand thread, `L` for a left-hand one.
8. `START MODE`: MACHINING opens.
9. With the ELS off set the missing X travel limits: position the tool, `MAX` / `MIN`.
   For an outside thread `MIN` is the starting coordinate and `MAX` the final depth; for
   an inside thread the other way round.
10. `START`: the machine goes to the starting point, X goes down to the first depth and
    the state changes to `SIN`.
11. Start the spindle at low RPM: the ELS engages and cuts the first pass.
12. Every pass: cutting up to the travel limit, X retract of 0.5 mm, rapid return to the
    starting point, new depth. The strip shows `Pass i of n`.
13. At the end of the passes the carriage returns to the starting point and the ELS
    switches off by itself with an acoustic signal.
14. To anticipate the end: in TURNING/FACING/THREADING, with the ELS on, press `START`
    again to go straight to the next depth. Warning: X goes down immediately.

### B. Turning (TURNING)

1. MENU → `TURNING`.
2. Type the feed per turn (example `0.1`) and `OK`.
3. `PASSES`: number of passes.
4. `SIDE`: `OUTSIDE` or `INSIDE`.
5. `STOP Z MAX`: end of the pass; the MIN is set from MACHINING.
6. `START MODE`, then complete the X and Z travel limits in MACHINING.
7. `START`: the carriage goes to the starting corner, waits for the phase (`SIN`) and
   runs the passes.
8. At the end of the work automatic return and ELS off.

### C. Facing (FACING)

Like turning, but the PITCH is the **radial** feed per turn and the page asks for
`DIRECTION` (`R → L` or `L → R`); the Z travel limit defines the width of the face and
the X travel limit the depth.

### D. Parting (PARTING)

1. MENU → `PARTING`.
2. PITCH with the right sign: positive outside, negative inside. `SIDE` shows the side
   obtained from the sign; tapping it inverts the sign of the PITCH (only with the ELS
   off).
3. `PASSES`, `STOP X MAX`.
4. `START MODE`; in MACHINING set the missing X travel limit.
5. `START` to go.

### E. Cone (CONE)

1. MENU → `CONE`.
2. Type `Ø LARGE`, `Ø SMALL` and `Z TRAVEL`; the `RATIO` in the card updates by itself
   (`1 : 19` for a Morse 5).
3. `SIDE`, `STOP X MAX`.
4. `START MODE`, then `START`: the machine confirms the calculated ratio and starts with
   the chosen side.

### F. Grooving (GROOVING, F13)

1. MENU → `GROOVING`.
2. `PASSES`, `LEFT REDUCTION` (shortening of every pass), `FEED` (mm/rev),
   `DIRECTION`.
3. `Z STOP`: tap the MAX or the MIN value and type it; `−`/`+` correct the value touched
   last.
4. `START MODE`; complete the other travel limits in MACHINING, then `START`.

### G. Gearbox (GEARBOX / X-GEARS)

1. MENU → `GEARBOX` (or `X-GEARS`).
2. Type the PITCH (mm/rev) and `OK`; STARTS at 1.
3. `START MODE` → `START`: the axis follows the spindle as with the mechanical gear
   train.
4. `STOP` to stop and remove the synchronization.

### H. Async (ASYNC)

1. MENU → `ASYNC`.
2. `PITCH` = speed in mm/s (`0.25` = 0.25 mm/s).
3. `START MODE` → `START`: the carriage starts at once, **without** needing to turn the
   spindle.
4. `STOP` to stop.

### I. G-code (G-CODE)

1. Load the programs from the **G-code** section of the web UI: paste the text or use
   **Browse .gcode files**, then **Save**. The allowed names are 2–32 characters among
   letters, digits, dot, dash and underscore.
2. From the MENU open `GCODE` (F9 card or item of the service row): on the left the list
   of the saved files, on the right `DETAILS` with size, lines and modified date.
3. Select the program, then `RUN`: the first touch asks for confirmation
   (`TAP AGAIN TO RUN`), the second within 3 seconds starts.
4. If the spindle is already turning the program starts; otherwise `SPINDLE ON` appears:
   start it and press `START` in MACHINING.
5. During the run only `STOP` and the emergency are active.
6. `DELETE` requires two touches within 3 seconds; `RELOAD LIST` reads the memory again.

### J. Joystick and Y axis

They require the hardware to be configured. In `JOYSTICK` the lever moves Z and X, the
potentiometer adjusts the pitch; `NORMAL SPEED` and `RAPID SPEED` are in rev/s,
`DEADBAND` in ADC units. In `Y AXIS` the axis turns at the speed of the Y PITCH.

## 12. STOP, emergencies, restore

**STOP** (`START/STOP` in MACHINING, red `STOP` in JOG, or the `ELS` box of the MENU)
switches the ELS off immediately: the movement stops at once, without completing the
ramp. It works also during typing and in G-code.

**STOP during a jog** = emergency: the machine locks with `ESTOP - Switched off in
manual`. You have to switch off and on again.

**Changing mode** switches the ELS off and resets the procedure in progress.

**Emergencies** (`ESTOP` in place of OFF/ON, the machine no longer answers the commands):

| Message | Cause |
|---|---|
| `ESTOP - Position outside machine` | steps commanded beyond the configured max travel |
| `ESTOP - Switched off in manual` | STOP pressed during a manual movement |
| `ESTOP - Cannot mark origin` | internal synchronization error |
| `ESTOP - Cannot ON/OFF` | internal error while switching on |

In all cases: switch the panel off, check the mechanics, switch it on again.

**Restore** — `SETUP → RESTORE`:

* `RESTORE MACHINE`: factory values of the machine (steps, travels, backlash, travel
  limits, speeds, units, Y axis). Two touches within 3 seconds; the controller restarts.
* `RESTORE WIFI`: clears network and password. Two touches within 3 seconds; the
  controller restarts.

**Long STOP** — holding `STOP` down for more than 3 seconds performs a general reset:
travel limits cleared, positions zeroed, pitch 0, STARTS 1, GEARBOX mode. The machine
configuration is not reset: that is done from `SETUP → RESTORE`.

## 13. Display messages

The bottom strip shows the message of the last action for a few seconds, then it goes
back to the state of the page (`MENU OFF`, `THREADING OFF`, `JOG OFF`, …).

### Status messages (uppercase)

| Message | Meaning / what to do |
|---|---|
| `TURN OFF FIRST` | operation allowed only with the ELS off |
| `AXIS NOT USED` | axis not enabled: enable it in the configuration |
| `MAX SET` / `MIN SET` | travel limit stored |
| `MAX BELOW MIN` / `MIN ABOVE MAX` | inconsistent travel limit: refused |
| `ZERO X` / `ZERO Y` / `ZERO Z` | position zeroed |
| `TAP AGAIN TO CONFIRM` | destructive action: second touch within 3 s |
| `TAP AGAIN TO RUN` | G-code start: second touch within 3 s |
| `TAP AGAIN TO DELETE` | G-code deletion: second touch within 3 s |
| `TAP AGAIN TO FORGET` | WiFi deletion: second touch within 3 s |
| `NO PROGRAM` | there are no G-code programs to run |
| `PROGRAM INVALID` | the G-code file is empty or unreadable |
| `SPINDLE ON` | G-code ready: start the spindle |
| `G-CODE RUNNING` | program running |
| `LIST UPDATED` | G-code list read again |
| `SAVED, REBOOT...` / `REBOOT TO APPLY` | settings saved, restart |
| `RESTORE, REBOOT...` | factory values restored |
| `WIFI CLEARED, REBOOT...` | WiFi network cleared |
| `SET NETWORK NAME` | the SSID is missing: open it and type it |
| `ESTOP - <reason>` | emergency: switch off and switch on again |

### Strip messages in the modes

| Message | Meaning / what to do |
|---|---|
| `Use 1.500?` | preview of the number typed: `OK` confirms |
| `Set stops` | a Z travel limit is missing: fix it before starting |
| `Set X stop` | an X travel limit is missing (parting or pass modes) |
| `N pass?` / `N passes?` | number of passes |
| `Outside?` / `Inside?` | side of the work (set with SIDE) |
| `Right to left?` / `Left to right?` | facing: direction of the pass |
| `Pitch > 0, outside` / `Pitch < 0, inside` | parting: confirmation of the side |
| `Cone ratio 0.xxxxx?` | thread: taper ratio; 0 = cylindrical |
| `Go Z… X…?` | the carriage is about to move to the starting point |
| `Pass i of n` | pass in progress |
| `No program` / program name | G-code: choice of the program |
| `Spindle ON!` / `Spindle ON. Go?` | G-code: starting the spindle |
| `Set pitch` | grooving or joystick switched on with pitch 0 |
| `Diameter x.xx` | idle: diameter in work (2× X position) |
| `Jog …`, `Sync …`, `Rapid …` | direction commanded by the joystick |
| `Feed …` / `Feed off` / `Feed stopped` | joystick state |
| `Joystick off` / `Joystick not connected` | joystick hardware missing or off |
| `Pitch +` / `Pitch -` | pitch adjusted with the joystick |

Many refusal conditions only give an acoustic signal: start with missing travel limits,
jog beyond the travel limit, key not active.

## 14. Common problems

**START only beeps.**
The mandatory travel limits are missing (`Set stops` / `Set X stop`), the pitch is 0, or
in G-code there is no valid program.

**The carriage does not move with the spindle.**
Check: ELS on (`ON`), PITCH other than 0, axis enabled, travel limit not already reached,
state not `SIN` with the spindle stopped.

**The carriage moves as soon as I touch the spindle.**
It is normal with the ELS on: the position follows the rotation. If it is not wanted,
press `STOP` or `ELS OFF`.

**The feed is double/triple the expected one.**
Check STARTS: it multiplies the pitch and is not shown in the PITCH field.

**The pitch in TPI does not come out right.**
In TPI the PITCH is the number of threads per inch (`8` = 8 TPI). For metric pitches use
MM.

**I touched the axis square and the motor released.**
That touch enables/disables the axis: it must stay enabled during the work. Enable it
again and check the coordinate (the lost steps are not recovered).

**`MAX` / `MIN` answer `TURN OFF FIRST`.**
The travel limits are set only with the ELS off: press `STOP`, position the axis, tap the
key again.

**The thread comes out inside instead of outside (or the other way round).**
The THREADING page has no SIDE: open TURNING (or ELLIPSE), set `SIDE` as needed, then go
back to THREADING.

**Tapping an area of the panel does nothing.**
If a command does not answer, the touch area has to be corrected in the TFT: note which
area and report it.

## 15. Service pages

### JOG

Manual control and quick keypad: ELS state (`OFF`/`ON`), active mode (tapping it returns
to the MENU), `STARTS`, `PITCH`, `MEASURE`, X/Y/Z position (tapping the big number zeros
it), the six jog arrows, `STOP` and the numeric row with `CANC`/`OK`. It does not start
machining cycles: it is for positioning and adjusting.

### GCODE

List of the programs in the controller memory, with the size in kB. On the right
`DETAILS` (name, size, lines, modified), `RUN`, `DELETE`, `RELOAD LIST`. The strip shows
the number of programs. The files are loaded from the web UI (§16).

### SETUP (SETTINGS)

Six sections, each on pages of 7 rows with `PREV`/`NEXT`:

| Section | Main rows |
|---|---|
| AXIS STEPS | encoder pulses, encoder backlash, Z/X/Y motor steps, Z/X screw mm/rev |
| LIMITS AND STOPS | max travel Z/X/Y, backlash Z/X/Y, safe distance, Z/X stops MAX/MIN |
| SPEED AND RAMPS | start speed, acceleration and manual speed per axis, step delay and pause |
| UNITS AND FORMAT | MEASURE, show tacho, show angle, manual step, Y axis enabled/rotary |
| LANGUAGE AND DISPLAY | language (ENGLISH), display (NX8048P050), firmware, service — read only |
| RESTORE | restore machine, restore WiFi |

Tapping a row opens the keypad; `−`/`+` correct by one step; the `YES/NO` and `MEASURE`
rows switch by tapping them. The changes are applied when the controller restarts.

### WIFI

Shows network and password (always masked `********`), `CONNECT`, `FORGET`, state
(`DISABLED`, `CONNECTING`, `CONNECTED`, `AP SETUP`) and IP address. Tapping the network
name or the password opens the keyboard; `CONNECT` saves and restarts; `FORGET` requires
two touches. Set the network with the controller off: the strip reminds you.

### INFO / STATUS

Firmware (`H5V65 20261030`), TFT interface (`EN 20261111`), IP address, uptime, free
memory and errors. It is the page to look at to find the address of the web UI.

## 16. Files, updates and backup

| File | Use |
|---|---|
| `02_display/h5_en.tft` | compiled interface for the panel: copy it to the SD as `h5.tft` (FAT32, root) or upload it from the web UI (*Update Nextion TFT*) |
| `02_display/h5_en.HMI` | editable English Nextion Editor project (pictures 60–79, pages assigned) |
| `02_display/_sorgente/h5_it.HMI` | Italian Nextion Editor project, base for the English HMI |
| `02_display/sfondi/` | English 800×480 backgrounds used to build the TFT (`GUIDA_TFT_EN.md` in `02_display/`) |
| `03_firmware/h5-EN-20261111-v65.bin` | English firmware for ESP32-S3 (H5V65) |
| `03_firmware/h5_en.ino` | source code of the English firmware |

**Upload order**: first the firmware, then the TFT.

* Firmware: from the web UI (*Update ESP32 firmware*) or with the Arduino IDE; with the
  ELS off.
* TFT: from the web UI (*Update Nextion TFT*) or from the SD; the display must be
  powered from the **POWER** port, not from USB; duration about 6 minutes at 115200.
* After the upload the panel restarts from the MENU.
* The G-code programs are loaded from the **G-code** section of the web UI (pasted text
  or `.gcode` file).
* The machine configuration can be downloaded and loaded back from the web UI for
  backup (*Configuration backup*): the file contains the axis, limit, speed and unit
  parameters, not the WiFi network or password.
* The address of the web UI is the one shown in `INFO / STATUS`.
