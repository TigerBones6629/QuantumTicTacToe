# Quantum Tic-Tac-Toe

A tic-tac-toe variant where every square is a qubit, not a classical mark. Nothing is real until the board is measured.

[Play it live](#) &middot; single self-contained `index.html`, no install or build step required.

## Concept

Each of the 9 squares is a qubit, initialized to `|0>` (empty). Instead of placing a classical X or O, players apply quantum gates to squares. Squares can sit in superposition — simultaneously marked and unmarked — until the board collapses, at which point every qubit resolves to a definite 0 or 1 and the game outcome is read off the result.

## Rules

Players alternate turns, X first. On each turn a player either applies a gate to a square or forces early collapse:

- **Mark (Pauli-X gate)** — deterministic. This square *will* measure as marked.
- **Superpose (Hadamard gate)** — 50/50 chance of measuring as marked.
- **Entangle (CNOT/CX gate)** — pick an already-touched square as the *control* and an untouched square as the *target*. Their final marked/unmarked outcomes become correlated (both come out the same), though ownership of the target still belongs to whoever made the entangle move, not the control's owner.
- **Collapse** — force measurement immediately, ending the game.

If nobody collapses early, the board auto-collapses once all 9 squares have been touched. Each square remembers its last claimant; on collapse, a square that measures `1` is awarded to that claimant, a `0` stays empty. Standard 3-in-a-row rules decide the winner. Because outcomes are probabilistic, draws happen — and, rarely, both players complete a line simultaneously (a "quantum paradox" tie).

## Implementation

The engine is a hand-rolled 512-dimensional (2^9) complex statevector simulator implementing H, X, and CX gates with proper joint-distribution sampling at measurement time — not a visual approximation of quantum behavior layered on top of classical logic. Verified against expected quantum behavior (exact 50/50 splits from H, perfect correlation from CX, deterministic outcomes from X).

Visuals are built with [Three.js](https://threejs.org/): a 3x3 grid of glowing qubit orbs in a starfield scene, a pulsing wireframe halo for superposed squares, an animated beam between entangled squares, and a collapse animation that resolves the board.

## Running it

Open `index.html` in a browser. That's it — no dependencies, no build step.

## Roadmap

- Bloch-sphere-style visualization mid-game
- Rotation gates (Ry) for tunable probabilities beyond flat 50/50
- A companion Qiskit/Python implementation targeting real IBM Quantum hardware
