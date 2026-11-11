# NanoEls H5 — Lathe assembly

Checklist to move from the bench to the machine. References: firmware **H5V65**,
[wiring schematic](cablaggio/schema_cablaggio_en.html) rev. 6
([PNG](cablaggio/schema_cablaggio_en.png),
[A4 PDF](cablaggio/schema_cablaggio_en_A4.pdf)),
[user manual](MANUALE_USO_EN.md); the mandatory machining tests are in
section F below.

General rule: **move up one step only if the previous one passed.** If a check
does not come out right, stop and fix the wiring or the parameter; do not try to
adjust it while running.

---

## A. Before you start

- [ ] Lathe off, plug pulled, spindle free and without a workpiece.
- [ ] Tools: wrenches, screwdriver, multimeter, dial indicator with magnetic
      base, caliper, tape measure for the cables.
- [ ] Stepper driver with datasheet (current per phase, microstep).
- [ ] 5 V supply for the board and motor supply (VMOT) separate or adequate for
      the total current.
- [ ] Emergency stop button that cuts power to drivers and spindle: the panel
      STOP is a software protection, **not** a certified safety device.
- [ ] Photos of the current state of the lathe (to put everything back as it
      was).

## B. Wiring and power supply

Reference pins (details and silkscreen in schematic rev. 6). **Pin 1 = +5V** on
the side of the 5V marking. The signal names in brackets are those printed on
the board.

| Connection | Pins | Signals |
|---|---|---|
| Z driver | 1→4 | +5V · ENABLE (ABIL., GPIO 41) · DIRECTION (SENSO, 42) · STEP (PASSO, 35) |
| X driver | 1→4 | +5V · ENABLE (ABIL., GPIO 16) · DIRECTION (SENSO, 15) · STEP (PASSO, 7) |
| Spindle encoder | 1→4 | +5V · GND (MASSA) · B (14) · A (13) |
| Z handwheel (opt.) | 1→4 | +5V · GND (MASSA) · B (8) · A (18) |
| X handwheel (opt.) | 1→4 | +5V · GND (MASSA) · B (21) · A (47) |
| Display UART | 1→4 | +5V · GND (MASSA) · RX (44) · TX (43) |
| 5V supply | 1→4 | +5V · +5V · GND (MASSA) · GND (MASSA) |

- [ ] Z and X drivers powered at **VMOT** from the driver terminals; the motor
      voltage **does not** enter the board connectors.
- [ ] STEP/DIRECTION/ENABLE inputs of the drivers **optically isolated** (the
      board outputs 5 V).
- [ ] Microstep and driver current set *before* connecting the motors: current
      from the motor datasheet, microstep consistent with the **Motor steps per
      rev** parameter (200 steps × microstep × any reduction).
- [ ] Common grounds: 5 V supply, drivers and board on a single ground point.
- [ ] Spindle encoder mounted **1:1** with the spindle (belt or coupling), belt
      tight, wheel solid and without play; 1200 pulses/rev by default.
- [ ] Handwheels mounted only if used; otherwise leave the connectors free.
- [ ] Controller fixed away from chips and coolant, cables in conduits, shielding
      or twist for encoder and step signals.
- [ ] Emergency stop button installed and tested **with the lathe off**
      (continuity).
- [ ] Check with the multimeter before applying power: no short between +5V and
      GND on the connectors, pin 1 correct on every connector.

## C. Configuration in the web UI

Open `http://<controller-ip>/` (the IP is from DHCP: if it does not answer, look
for the MAC `7c:e8:b1:b0:93:5c` with `arp -a`). Save after each section.

Spindle encoder:

- [ ] **Encoder PPR** = 1200 (or the actual value of the encoder installed).
- [ ] **Encoder backlash** = 0 (increase only if the angle oscillates after a
      reversal).

Z axis (carriage):

- [ ] **Leadscrew pitch**: actual screw pitch (for inch screws, 25.4 / TPI).
- [ ] **Motor steps per rev**: 200 × microstep × reduction.
- [ ] **Max travel**: slightly above the safe physical carriage travel.
- [ ] **Manual speed**: start low and increase after the tests.
- [ ] **Backlash**: measured with the dial indicator, 0 if not measurable.
- [ ] **Invert direction**: enable only after the direction test (§D).

X axis (cross slide):

- [ ] Same fields as Z, with the cross slide screw pitch.
- [ ] **Max travel** referred to the slide, not to the diameter.

Other:

- [ ] **Cone**: Large diameter, Small diameter (the Z travel is the MAX limit
      switch set in the CONE page).
- [ ] **Joystick**: leave it off if not connected.
- [ ] **Configuration backup**: in *Settings*, after checking the parameters,
      press **Download backup** and keep the JSON file. It is used to restore the
      machine after a change or to configure a second one.
- [ ] Note the values chosen below, they are needed for commissioning.

| Parameter | Z | X |
|---|---|---|
| Screw pitch (mm/rev) | | |
| Motor steps per rev | | |
| Max travel (mm) | | |
| Backlash (mm) | | |
| Invert direction | yes / no | yes / no |

## D. First power-up and dry runs

Without a workpiece, with the tool away from the spindle and your hand on the
emergency stop button.

1. [ ] Power-up: display on MENU, web UI reachable, version H5V65.
2. [ ] **ELS OFF**. Jog Z+ (`Z+ →`) and Z− (`← Z−`) with short taps: correct
       direction on the screen, distance equal to STEP measured with the dial
       indicator. If the direction is wrong: switch off, enable **Invert
       direction** in the web UI and try again.
3. [ ] Jog X+ (`↑ X+`) and X− (`↓ X−`) with the same check.
4. [ ] 10 jogs of 1 mm and back: the position returns to 0 (no steps lost).
5. [ ] Spindle by hand: one turn → **REVS** +1 and **ANGLE DEG** returns to ~0;
       **RPM** plausible. If REVS decreases in the working direction, switch off
       and swap A/B on the encoder connector.
6. [ ] Software limit switches: set **TRAVEL LIMIT MAX** and **TRAVEL LIMIT MIN**
       on Z and X (field in the mode card or MAX/MIN buttons in the MACHINING
       page), leaving a margin from the physical stops.
7. [ ] Jog beyond the limit switch: it stops (beep only), the position does not
       continue.
8. [ ] **STOP during a jog**: `ESTOP - Switched off in manual` appears, switch
       off and on again. (Test 2 of the safety commissioning.)
9. [ ] **STOP held 3 s**: general reset, limit switches cleared, pitch 0,
       GEARBOX mode. (Test 3 of the safety commissioning.)
10. [ ] Restart: the NVS parameters are kept and WiFi reconnects.

## E. First tests with the ELS on

Dry, or with a soft test workpiece, low speed, hand on the emergency stop
button.

1. [ ] **GEARBOX**, small pitch (e.g. 0.1 mm/rev): ten turns by hand → the
       carriage advances by 1 mm measured with the dial indicator (pitch ×
       turns).
2. [ ] **X-GEARS** with the same criterion on the cross slide.
3. [ ] **ASYNC**: starts without the spindle; OFF stops it immediately.
4. [ ] Reaching the software limit switch while moving: it stops without going
       past the limit.
5. [ ] Changing mode with the ELS on: the ELS turns off and the procedure is
       cleared.
6. [ ] **Position outside machine**: force a command beyond the maximum travel
       and check the emergency stop (controlled test, dry).

## F. Machining tests (mandatory before commissioning)

Recommended order. Repeat any test that fails after the correction.

- [ ] **Z/X synchronization** on a test workpiece without a tool: live position
      consistent with the spindle turns.
- [ ] **TURNING**: cylinder in several passes, decreasing depth.
- [ ] **FACING**: facing, to the right and to the left.
- [ ] **PARTING**: positive pitch (outside) and negative (inside).
- [ ] **CONE**: Morse 5 with its dimensions; the display must show `1 : 19`.
- [ ] **THREADING**: right-hand and left-hand, multiple starts, cylindrical and
      tapered thread; check with a thread pitch gauge or the die nut.
- [ ] **ELLIPSE**, **GROOVING**, **Y AXIS** if the axis is installed.
- [ ] **G-CODE**: program loaded from the web UI (also from a `.gcode` file),
      execution, pause with the spindle stopped.
- [ ] Live values during machining: RPM, ANGLE DEG, positions, pass i of n.
- [ ] Emergency stops: `ESTOP - Position outside machine` and
      `ESTOP - Switched off in manual` with the cause shown on the TFT and in
      the web UI.
- [ ] **22 TFT pages**, numeric keypad and PS/2 keyboard (if connected).
- [ ] **OTA**: firmware and TFT update from the web UI (ELS OFF; display
      powered from POWER, not from USB). Do not remove power for 2–3 minutes
      after the TFT.
- [ ] Web UI from a phone: positions, limit switches, WiFi, axis parameters.

## G. If something does not add up

| Symptom | Check |
|---|---|
| Axis in the wrong direction | **Invert direction** in the web UI (with ELS off) |
| Pitch doubled or halved | **Motor steps per rev** / **Leadscrew pitch** |
| Position drifting | backlash, motor slip, microstep or driver current |
| Negative or wrong REVS | A/B encoder swapped or wrong PPR |
| `ESTOP - Position outside machine` | **Max travel** too tight or limit switches not set |
| Motor free | axis pill on OFF or **Invert enable** of the driver |
| The ELS does not start | missing limit switches, pitch 0, spindle stopped in G-CODE |

After the correction, repeat the test from the previous step.

## H. Closing

- [ ] Photos of the controller installed and of the encoder (needed for the
      README).
- [ ] Update *Project status* in the [README](README_EN.md) with the tests
      passed.
- [ ] Note the problems found and the solutions here.
- [ ] Realign the publication package if the documentation changes.
