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
        A2[Python RNN notebook (GPU) + script (CPU)]
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
    H1["Researcher: goal +<br/>scientific constraints"] --> AG["KIRO: read code,<br/>implement, write tests"]
    AG --> V{"Tests &<br/>diagnostics<br/>pass?"}
    V -- no --> AG
    V -- yes --> H2["Researcher: inspect outputs,<br/>apply domain judgement"]
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
    S --> M[conditional recurrent neural network]
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
    CFG[config: expansion] --> EX["Expand factorial combinations"]
    EX --> C1[Pre-processing of emulator inputs]
    EX --> C2[]
    EX --> C3[Scaled/Original]
    EX --> C4[Scaled/Cumulative]
    C1 & C2 & C3 & C4 --> MET[factorial_metrics.csv]
    MET --> BEST{"Pick best<br/>by metric"}
    BEST --> DOWN["Best performing model:<br/>(plot / sensitivity /<br/>uncertainty / xai)"]
```

---

## 05 — Division of labour (who does what)

```mermaid
flowchart LR
    subgraph Agent["KIRO is strong at"]
        G1[Read/port code fast]
        G2[Write & run tests; numerical parity]
        G3[Catch env traps]
        G4[Keep /specs in sync]
        G5[]
    end
    subgraph Human["Researcher is essential for"]
        N1[Scientific interpretation and ]
        N2[Definition 'faithful']
        N3[Spotting mismatches]
        N4[Scoping and steering]
        N5[Accepting results and taking responsibility]
    end
    Agent <-->|tight loop| Human
```
