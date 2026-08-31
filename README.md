# Combolands Assets

A browsable, structured snapshot of Combolands game data and visual references.

- [`assets/`](assets/) contains extracted and procedurally reconstructed visual assets.
- [`data/`](data/) contains normalized entity, mechanic, and enum records.
- [`manifest.json`](manifest.json) is the machine-readable catalog index.
- [`primer/agent-cheatsheet.md`](primer/agent-cheatsheet.md) summarizes the catalog's mechanics and relationships.

Each directory under [`assets/buildings/`](assets/buildings/) has its own README with the building description, basic mechanics, primary card images, related entries, and links to its available terrain, paint, animation, and sprite variants.

This repository contains generated outputs. Rebuild machinery lives in the sibling [`combolands-asset-generators`](../combolands-asset-generators/) repository; evaluation code belongs in the separate [`combolands-eval`](../combolands-eval/) project.
