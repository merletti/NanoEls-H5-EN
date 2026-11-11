# Italian → English glossary — NanoEls H5

Terms fixed once and used identically on the display, in the firmware, in the manual
and on the website. Entries in **bold** appear on screen, where space is tight: they
are already the shortest form that fits.

English uses American spelling. The web UI uses sentence case; static TFT labels are
uppercase. The TFT character sprite covers bytes 32..126 only, so every string the
firmware draws is plain ASCII (no `°`, `×`, `−`, curly quotes). Baked labels in the
background PNGs are images and can use normal typography, but keep them to the same
forms so the panel never mixes two spellings.

## Machine and machining

| Italian | English | Notes |
|---|---|---|
| tornio | lathe | |
| mandrino | spindle | |
| asse (Z, X, Y) | axis (Z, X, Y) | label form: `Z AXIS` |
| carro / slitta | carriage / cross slide | Z = carriage, X = cross slide |
| passo | **PITCH** | thread pitch and feed per rev |
| passo per giro | **MM/REV** | grey line: `PITCH · MM PER GIRO` → `PITCH · MM/REV` |
| principi (filetto) | **STARTS** | number of starts = multi-start thread |
| filettatura / filetto | **THREAD** / thread | mode name: `THREADING` |
| cono filetto | **THREAD CONE** | |
| passata | **PASS** | |
| avanzamento | **FEED** | |
| fine corsa | **STOP** / travel limit | setup rows: `STOP Z MAX`; mode cards: `TRAVEL LIMIT MAX` |
| finecorsa (componente) | limit switch | |
| corsa | **TRAVEL** | |
| velocità | **SPEED** | |
| giri al secondo | **REV/S** | |
| giri al minuto | **RPM** | only in prose and the work page |
| diametro | **DIAMETER** | |
| misura | **MEASURE** | |
| posizione | **POSITION** | |
| stato | **STATUS** | |
| direzione / senso | **DIR** | direction; in threading = thread hand |
| zona morta | **DEADBAND** | |
| ingresso | **INPUT** | |
| riduzione (taglio sinistro) | **LEFT REDUCTION** | grooving |
| esterno | **OUTSIDE** | |
| interno | **INSIDE** | |
| avanti | **FWD** | |
| attiva modalità | **START MODE** | |
| annulla | **CANCEL** | |
| gioco | backlash | |
| volantino | handwheel | |
| vite madre | leadscrew | |
| arretramento | retract | |
| sovracampionamento | oversampling | ADC |
| riposo motore | motor idle | driver powered only while moving |

## The thirteen modes (display name, main line)

| Italian | English | Notes |
|---|---|---|
| INGRANAGGI | **GEARBOX** | F1 · short form `GEAR` |
| FILETTATURA | **THREADING** | F2 · short form `THREAD` |
| TORNITURA | **TURNING** | F3 · short form `TURN` |
| SPIANATURA | **FACING** | F4 · short form `FACE` |
| CONO | **CONE** | F5 |
| TRONCATURA | **PARTING** | F6 · short form `PART` |
| ELLISSE | **ELLIPSE** | F7 |
| ASINCRONO | **ASYNC** | F8 |
| ASSE Y | **Y AXIS** | F10 |
| JOYSTICK | **JOYSTICK** | F11 · short form `JOY` |
| X-INGRANAGGI | **X-GEARS** | F12 · short form `XGEAR` |
| SCANALATURA | **GROOVING** | F13 · short form `SLOT` |
| COMANDO | **JOG** | manual mode, not an F-mode |

The footer under the mode name carries the English term only once; there is no
second subtitle in this edition.

## Menu and services

| Italian | English |
|---|---|
| MENU | **MENU** |
| MODALITÀ E SERVIZI | **MODES AND SERVICES** |
| COMANDO | **JOG** |
| G-CODE | G-CODE |
| SETUP | **SETUP** |
| WIFI | WIFI |
| INFO | **INFO** |

## Service screens

| Italian | English |
|---|---|
| IMPOSTAZIONI | **SETTINGS** |
| RETE | **NETWORK** |
| NOME RETE (SSID) | **NETWORK NAME (SSID)** |
| PASSWORD | **PASSWORD** |
| CONNETTI | **CONNECT** |
| DIMENTICA | **FORGET** |
| CONNESSO | **CONNECTED** |
| Indirizzo IP | **IP ADDRESS** |
| INFO / STATO | **INFO / STATUS** |
| FIRMWARE | **FIRMWARE** |
| DISPLAY TFT | **TFT DISPLAY** |
| TEMPO ACCESO | **UPTIME** |
| MEMORIA LIBERA | **FREE MEMORY** |
| ERRORI | **ERRORS** |
| nessuno | **none** |
| FILE SALVATI | **SAVED FILES** |
| DETTAGLI | **DETAILS** |
| Dimensione | Size |
| Righe | Lines |
| Modificato | Modified |
| ESEGUI | **RUN** |
| ELIMINA | **DELETE** |
| RICARICA ELENCO | **RELOAD LIST** |
| MODIFICA VALORE | **EDIT VALUE** |
| Tastierino numerico | Numeric keypad |
| DIGITAZIONE | **ENTRY** |
| RIPRISTINA | **RESTORE** |

## TFT short forms

Fixed English strings already used by the firmware; keep them identical in the
backgrounds. Static labels are uppercase; firmware messages keep this exact case.

| Italian | TFT string | Where |
|---|---|---|
| Carro sx / dx | `Carriage L` / `Carriage R` | JOY status |
| Trasversale dentro / fuori | `Cross in` / `Cross out` | JOY status |
| Passo + / − | `Pitch +` / `Pitch -` | message line |
| Rapido / Sincro | `Rapid` / `Sync` | message line |
| Passata | `Pass` | message line |
| Avanzamento | `Feed` | message line |
| Avanti / Dietro | `FWD` / `BACK` | field cards |
| Esterno / Interno | `OUTSIDE` / `INSIDE` | field cards |
| Destra / Sinistra | `R` / `L` | field cards |
| Imposta finecorsa | `Set stops` | message line |
| Accendi il mandrino! | `Spindle ON!` | message line |
| Da destra a sinistra? | `Right to left?` | message line |
| Da sinistra a destra? | `Left to right?` | message line |
| Senso | `DIR` | field cards |
| Unità in pollici | `IN` | unit suffix |

Setup rows and their English labels are in `traduzioni_fw_tft.tsv` and must not be
reworded when the backgrounds are drawn.

## Units and symbols

`mm`, `MB`, `KB` and `in` stay unchanged in English.
`giri/s` → `rev/s`, `giri/min` → `RPM`, `impulsi/giro` → `pulses/rev`,
`passi/giro` → `steps/rev`, `mm/giro` → `mm/rev`, `mm/passata` → `mm/pass`.
Symbols `+ − ← → ↑ ↓` are unchanged in the web UI; on the TFT use `+ - < >` and
write `DEG` instead of `°`.
Version mark in the bottom-right corner: `IT 20261030` → `EN 20261111`.

## Style rules

- No “Please”: use the imperative or a short verb form (“Start mode”, “Tap the value”).
- Never translate the names the firmware uses as identifiers: `F1…F13`, `Z`, `X`, `Y`,
  `X+`, `PULS`, `G-CODE`, `WIFI`.
- Keep one word per concept: pitch (not lead), pass (not cut), feed (not advance),
  travel (not stroke), backlash (not play), handwheel (not flywheel).
- “Sinistra/destra” in threading means the thread hand, not axis inversion; write
  `left-hand` / `right-hand` in prose and `L` / `R` on screen.
- The mode name appears once per page; do not repeat it in a subtitle.
- Web UI sentence case, TFT uppercase: `Saved files` on the web, `SAVED FILES` on the
  panel.
