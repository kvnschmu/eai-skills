# persona

Self-evolving operator persona.

## Overview

State is stored in browser `localStorage`:

- traits — markdown modules (IDENTITY, SOUL, VOICE, BOOTSTRAP)
- data — structured JSON
- hooks — runtime JS actions

Core traits are auto-loaded on init.

## Structure

persona/
├── SKILL.md
├── README.md
├── scripts/index.html
└── workspace/
    ├── IDENTITY.md
    ├── SOUL.md
    ├── VOICE.md
    └── BOOTSTRAP.md

## Build

make

## Notes

- modular persona system
- runtime extensible (traits, data, hooks)
- no rebuild required for updates
- designed for technical/operator workflows
