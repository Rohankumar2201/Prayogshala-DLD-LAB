# Prayogshala · Digital Logic Lab

**Prayogshala** (प्रयोगशाला, "laboratory") is a browser-based Digital Logic Design lab simulator modelled on a physical DLD trainer kit. You place ICs on a breadboard, wire power and signals yourself, flip the input switches, and read the result on the output LEDs. Nothing is pre-connected, and the simulator reads the wires you actually place.

Built for practising lab experiments before the DLD laboratory external exam. No install, no backend, no dependencies: it is a single `index.html`.

**Live demo:** https://rohankumar2201.github.io/prayogshala/

---

## Features

- Trainer-kit layout: +5V/GND supply, 12 output LEDs on top, breadboard in the middle, 12 toggle-switch inputs and 2 pulser switches at the bottom
- Realistic breadboard: split terminal strips, four power rails, DIP ICs straddling the centre gap with notch and pin 1 marker
- Real electrical simulation: an IC does nothing without VCC and GND, unwired inputs show **X**, an unconnected output is reported, and a +5V to GND short or an output clash raises a warning
- ICs available: **7400** NAND, **7402** NOR, **7404** NOT, **7408** AND, **7432** OR, **7486** XOR (datasheet pinouts)
- Truth-table logger, logic analyser, pin-by-pin datasheet view, undo/redo, reset
- Light and dark theme, works on phone and desktop

---

## Run it

**Easiest:** double-click `index.html`.

**Or serve it locally** (from this folder):

```bash
python -m http.server 8000
# open http://localhost:8000
```

There is no `npm install` and no build step.

---

## How to use

1. **Place an IC.** Drag it from the library onto the breadboard. On a phone, tap the IC and then tap the board. Use the **◀ ▶** buttons to nudge a selected IC left or right.
2. **Power the board.** Wire **+5V** to a **+** rail hole and **GND** to a **−** rail hole. Wires turn red and black automatically.
3. **Power each IC.** Pin **14** (VCC, top-left) needs the **+** rail, and pin **7** (GND, bottom-right) needs the **−** rail. Use a hole in the same column strip as the pin: rows **a–d** for the top pins and **g–j** for the bottom pins. Never use rows **e** or **f**, because the IC pins sit there.
4. **Wire the inputs.** Connect an **Input D#** terminal (bottom panel) to the IC input pin.
5. **Wire the outputs.** Connect the IC output pin to an **Output D#** terminal (top panel).
6. **Turn the supply ON** with the power switch.
7. **Test.** Flip the input toggles (up is 1, down is 0). The LED above each toggle shows its level, and the output LEDs show the circuit's result.
8. **Log it.** Press **Log row** for each input combination to build your truth table.

**Wiring:** drag from one terminal to another, or tap one terminal and then another. Click a wire or IC to select it, then press **Delete**. Hover (or tap) any hole to read its level in the probe bar.

**Zoom:** use **−**, **+** and **Fit** above the board. On a phone the board scrolls sideways.

### Pin layout (all supported ICs are 14-pin DIPs, notch on the left)

```
 Top row, pins 14 → 8 (left to right):   14 13 12 11 10  9  8
 Bottom row, pins 1 → 7 (left to right):  1  2  3  4  5  6  7
```

| IC | Function | Gate pins (inputs → output) |
|---|---|---|
| 7400 | NAND | (1,2→3) (4,5→6) (9,10→8) (12,13→11) |
| 7402 | NOR | (2,3→1) (5,6→4) (8,9→10) (11,12→13) |
| 7404 | NOT | (1→2) (3→4) (5→6) (9→8) (11→10) (13→12) |
| 7408 | AND | (1,2→3) (4,5→6) (9,10→8) (12,13→11) |
| 7432 | OR | (1,2→3) (4,5→6) (9,10→8) (12,13→11) |
| 7486 | XOR | (1,2→3) (4,5→6) (9,10→8) (12,13→11) |

VCC is pin 14 and GND is pin 7 on all of them.

---

## Example: Half Adder

Uses a **7486** (XOR) for Sum and a **7408** (AND) for Carry.

| Connection | From | To |
|---|---|---|
| Power | +5V → + rail, GND → − rail | |
| 7486 VCC / GND | + rail → pin 14 | − rail → pin 7 |
| 7408 VCC / GND | + rail → pin 14 | − rail → pin 7 |
| A | Input D1 | 7486 pin 1 and 7408 pin 1 |
| B | Input D2 | 7486 pin 2 and 7408 pin 2 |
| Sum | 7486 pin 3 | Output D2 |
| Carry | 7408 pin 3 | Output D1 |

Expected result:

| A | B | Sum | Carry |
|---|---|---|---|
| 0 | 0 | 0 | 0 |
| 0 | 1 | 1 | 0 |
| 1 | 0 | 1 | 0 |
| 1 | 1 | 0 | 1 |

---

## Troubleshooting

| What you see | Likely cause |
|---|---|
| IC status dot is red / "IC inactive" | VCC or GND is missing or in the wrong column |
| Output shows **X** | An input is floating, or the IC has no power |
| "Hole is occupied" message | You wired into row e or f, where IC pins sit |
| LED never lights | Supply is OFF, or the output is not wired to an Output terminal |
| Short-circuit warning | A wire connects +5V directly to GND |

---

## Roadmap

- [x] **Phase 1:** breadboard, wiring, power, basic gates, switches, LEDs, live simulation
- [ ] **Phase 2:** NAND/NOR universal gates, half/full adder and subtractor experiments
- [ ] **Phase 3:** multiplexer, demultiplexer, encoder, decoder, comparator (74151, 74138, 7483)
- [ ] **Phase 4:** flip-flops, counters, shift registers, pulse generator (7474, 7476)
- [ ] **Phase 5:** examination mode with automatic circuit evaluation and scoring

ICs not yet implemented are labelled as such in the library.

---

## Publish on GitHub Pages

Create an empty repo named `prayogshala` on GitHub, then run these from inside this folder:

```bash
git init
git add .
git commit -m "Prayogshala: Digital Logic Lab"
git branch -M main
git remote add origin https://github.com/Rohankumar2201/prayogshala.git
git push -u origin main
```

Then in the repo go to **Settings → Pages**, choose **Deploy from a branch**, pick `main` and `/ (root)`, and save. The site goes live in about a minute at `https://rohankumar2201.github.io/prayogshala/`.

To publish later changes:

```bash
git add .
git commit -m "Describe the change"
git push
```

If GitHub rejects your password, create a personal access token (Settings → Developer settings → Personal access tokens) and use it instead, or sign in with `gh auth login`.

---

Made by Rohan Kumar · [GitHub](https://github.com/Rohankumar2201) · [LinkedIn](https://www.linkedin.com/in/rohankumar2201/)