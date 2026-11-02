# amoeba-claude-plugin

Marketplace of Claude plugins published by AmoebAI. The catalog is `.claude-plugin/marketplace.json`; each plugin lives under `plugins/`.

| Plugin | What it does |
| --- | --- |
| [`amoeba`](plugins/amoeba) | The Amoeba AI connector plus a skill on how to read an organization through it. |

## Adding the marketplace

```bash
claude plugin marketplace add AmoebAI/amoeba-claude-plugin
claude plugin install amoeba@amoebai
```

## Contributing

Each plugin keeps its own `.claude-plugin/plugin.json`; bump its `version` on every change. Validate before pushing:

```bash
claude plugin validate plugins/amoeba --strict
claude plugin validate . --strict
```
