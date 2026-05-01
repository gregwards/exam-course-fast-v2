# `standards/` — Learning design standards

This folder holds the canonical learning design standards and the architecture for how they integrate into the framework. Two files do the load-bearing work; everything else is source material or derived output.

## Read in this order

1. **`standards.md`** — The canonical source of truth. Every standard the framework enforces. Imported from `source/Pillars_Learning_Design_Standards.docx`. Edit here; regenerate derivatives.
2. **`standards-architecture.md`** — How standards integrate into the framework without compromising the soul, quality, diversity, or creativity of the output. Defines the four enforcement layers (templates, scripts, persona filter, conductor) and maps every standard to a layer.

## Folder structure

```
standards/
├── README.md                    # This file
├── standards.md                 # Canonical source of truth
├── standards-architecture.md    # Enforcement architecture
├── source/                      # Original source materials
│   ├── Pillars_Learning_Design_Standards.docx
│   └── Pillars_Learning_Design_Standards.md
└── derived/                     # Auto-generated from standards.md (TBD)
    ├── template-fragments/
    ├── prompt-fragments/
    └── check-rules.json
```

## Architectural commitment

`standards.md` is the **single source of truth**. Templates, scripts, prompts, and persona filter context are *derived* from it via `scripts/compile_standards.py` (TBD). Do not edit derived artifacts; edit the standards document and regenerate.

When standards change, the workflow is:

1. Edit `standards.md`.
2. Run `scripts/compile_standards.py`.
3. Derived artifacts regenerate.
4. Commit the change.

## Why this design

From `standards-architecture.md`: standards live primarily on the discriminator side of the system, not the generator side. The generator (the conversational AI) gets creative latitude inside structural slots. The discriminator (templates, scripts, persona filter) catches violations and surfaces them back. This is the structured-creative-process commitment to never collapsing generator and discriminator — and it's what keeps standards-compliance from killing creative output.

See `standards-architecture.md` for the full reasoning.

## What's NOT in this folder yet

**The Rize LDS (parent) standards.** `standards.md` references them throughout (`ACC-*`, `LD-ACT-*`, `LD-DSC-*`, `LD-GRD-*`, `LD-QZ-*`, `LD-ARC-*`, `QA-*`) but they are not in the repo. Need Greg to share the source so they can be added to `source/` and integrated into `standards.md`. Tracked in `docs/open-questions.md`.

**The `derived/` folder is empty.** It populates when `compile_standards.py` runs. Nothing to derive into yet because the templates, prompts, and check rules are themselves still TBD.

**`compile_standards.py` does not exist yet.** Its design is implied by `standards-architecture.md` but the implementation is downstream work.

## Status

`standards.md` and `standards-architecture.md` are first drafts as of 2026-04-25. They reflect the imported Pillars standards plus the four-layer enforcement architecture grounded in the structured-creative-process and prompt-architect skills. Both will be revised as the framework is built and as the standards architecture is tested in practice.
