# Presentation flowcharts (Mermaid)

These are intentionally **simple**. Render them at <https://mermaid.live> (paste a
block, export PNG/SVG), or in VS Code / GitHub which render Mermaid natively.
Plain-text/ASCII versions of the same diagrams are in `figures_ascii.txt` for quick
drop-in or redrawing in PowerPoint.

---

## 01 — Before / After (why do this at all)

```mermaid
flowchart LR
    subgraph Before["Before: publication scripts"]
        A1[R preprocessing]
        A2[Python RNN notebook + script]
        A3[Sensitivity script]
        A1 -.manual.-> A2 -.manual.-> A3
    end
    subgraph After["After: one tested package"]
        B1[config.yaml]
        B2[JRC-daycent-emulator CLI]
        B3[107 automated tests]
        B1 --> B2
        B3 -. verifies .-> B2
    end
    Before ==>|"Kiro + human"| After
```

---

## 02 — The collaboration loop (the core message)

```mermaid
flowchart LR
    H1["Human: goal +<br/>scientific constraints"] --> AG["Agent: read code,<br/>implement, write tests"]
    AG --> V{"Tests &<br/>diagnostics<br/>pass?"}
    V -- no --> AG
    V -- yes --> H2["Human: inspect outputs,<br/>apply domain judgement"]
    H2 -- "refine intent" --> AG
    H2 -- "accept" --> DONE["Tested, documented<br/>option shipped"]
```

---

## 03 — What the agent built (the emulator pipeline)

```mermaid
flowchart LR
    R[(Raster stacks)] --> H[Harmonize]
    H --> D[Dataset builder]
    D --> S[Scale]
    S --> M[cRNN_bidir<br/>verbatim]
    M --> T[Train + predict]
    T --> SE[Sensitivity]
    T --> U[Uncertainty]
    T --> X[XAI]
    T --> P[Figures]
    CFG[config.yaml] -. drives .-> H & D & S & M & T
```

---

## 04 — The factorial sweep (a human-scoped feature)

```mermaid
flowchart TD
    CFG[config: select cells] --> EX["Expand cells:<br/>Scaling x Cumulative"]
    EX --> C1[Non_scaled/Original]
    EX --> C2[Non_scaled/Cumulative]
    EX --> C3[Scaled/Original]
    EX --> C4[Scaled/Cumulative]
    C1 & C2 & C3 & C4 --> MET[factorial_metrics.csv]
    MET --> BEST{"Pick best<br/>by metric"}
    BEST --> DOWN["Downstream on winner<br/>(plot / sensitivity /<br/>uncertainty / xai)"]
```

---

## 05 — Division of labour (who does what)

```mermaid
flowchart LR
    subgraph Agent["Agent is strong at"]
        G1[Read/port code fast]
        G2[Numerical parity < 1e-8]
        G3[Write & run tests]
        G4[Catch env traps]
        G5[Keep docs in sync]
    end
    subgraph Human["Human is essential for"]
        N1[Scientific intent]
        N2[What 'faithful' means]
        N3[Spotting subtle mismatches]
        N4[Scoping / saying 'less']
        N5[Accepting results]
    end
    Agent <-->|tight loop| Human
```
