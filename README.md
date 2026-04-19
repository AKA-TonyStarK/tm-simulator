# General-Purpose Turing Machine Simulator

A fully general-purpose Turing Machine simulator built with vanilla HTML/CSS/JS — no build step, no dependencies, deployable on Vercel.

## Features

- **User-defined machines** — states Q, alphabets Σ/Γ, blank symbol, start/accept/reject states, and transition function δ
- **Dynamic infinite tape** — sparse map representation, extends in both directions, displays a scrollable window around the read/write head
- **Three execution modes** — step-by-step, auto-run with adjustable speed (50–2000 ms/step), and pause
- **L / R / S directions** — Left, Right, and Stay (no-move) all supported
- **Step limit** — configurable step cap to detect/abort infinite loops (set 0 for unlimited)
- **Live transition highlight** — the currently-applied δ row is highlighted in the live table
- **Transition log** — scrollable log of every (state, symbol) → (write, dir, next) applied
- **6 pre-built examples** — 0ⁿ1ⁿ, Palindrome, Binary +1, Copy 1s, Unary Add, Binary Even
- **Import / Export JSON** — save and share machine definitions
- **Error handling** — validates states, transitions, and undefined symbols; clear error messages throughout

## Architecture (modular JS classes)

| Class / Object | Responsibility |
|---|---|
| `Tape`          | Infinite sparse tape (read, write, window view) |
| `TuringMachine` | Definition + transition lookup via Map |
| `Sim`           | Execution engine (step, run, pause, reset) |
| UI helpers      | Render tape, state boxes, delta table, log |
| UI event handlers | Load, add/delete transitions, import/export |

## Run Locally

```bash
# Option 1: just open the file
open index.html

# Option 2: local server
npx serve .
# or
python3 -m http.server 8080
```

## Deploy to Vercel

```bash
npm install -g vercel
vercel deploy
```

Or drag the folder into [vercel.com/new](https://vercel.com/new).

## How to Use

1. Pick an **Example** from the Examples tab, or build your own in Config + Transitions δ.
2. Click **⚙ Load Machine** to initialise the tape.
3. Use **↷ Step** for manual stepping, or **▶ Run** for animation.
4. Adjust the **Speed** slider (50 ms = fast, 2000 ms = slow).
5. **⏸ Pause** mid-run, then **↷ Step** to single-step from there.
6. **↺ Reset** clears machine and tape.
7. **⬇ Export JSON** / **⬆ Import JSON** to share machines.

## Theory Background

A Turing Machine M = (Q, Σ, Γ, δ, q₀, qA, qR, B) where:

- **Q** = finite set of states
- **Σ** = input alphabet (Σ ⊆ Γ \ {B})
- **Γ** = tape alphabet (includes blank B)
- **δ**: Q × Γ → Q × Γ × {L, R, S} = transition function
- **q₀** = start state
- **qA** = accept state
- **qR** = reject state
- **B** = blank symbol (default `_`)

Configurations are (state, tape, head). Computation halts when reaching qA or qR, or when no transition is defined for the current (state, symbol) pair (implicit reject).
