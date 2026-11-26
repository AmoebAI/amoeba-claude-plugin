# amoeba-claude-plugin

Marketplace of Claude plugins published by AmoebAI. The catalog is `.claude-plugin/marketplace.json`; each plugin lives under `plugins/`.

| Plugin | What it does |
| --- | --- |
| [`amoeba`](plugins/amoeba) | The Amoeba AI connector plus two skills: how to read an organization through it, and how to hand over a department's Executive Brief. |

## Adding the marketplace

```bash
claude plugin marketplace add AmoebAI/amoeba-claude-plugin
claude plugin install amoeba@amoebai
```

## This repository is public

Anyone can read it: the files, the git history, the pull requests and their descriptions. Nothing private to Amoeba AI goes here, not now and not in the history:

- **No secrets or data.** No tokens, keys, connection strings, customer or employee data, and no internal hosts other than the public connector endpoint.
- **No methodology.** Anyone who installs the plugin can read its skills, so a skill says only when to call a tool and how to present its answer. How Amoeba computes anything stays on the server, behind the connector's sign-in.
- **No internal references.** No private issue or ticket numbers, release versions, staging environments, customer names or Slack threads in commits, pull requests or docs.
- **The history counts too.** A commit that removes a leak does not undo it. If something private lands, treat it as exposed: rotate it and ask security before rewriting history.

When in doubt, leave it out or ask security before pushing.

## Contributing

Each plugin keeps its own `.claude-plugin/plugin.json`; bump its `version` on every change. Validate before pushing:

```bash
claude plugin validate plugins/amoeba --strict
claude plugin validate . --strict
```

## License

Apache License 2.0. See [LICENSE](LICENSE) and [NOTICE](NOTICE).
