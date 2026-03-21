# Codika Documentation

Documentation for the Codika platform — build, deploy, and manage n8n workflow automations.

Built with [Mintlify](https://mintlify.com).

## Development

Mintlify requires a Node.js LTS version (20 or 22). If you're on a newer non-LTS version (e.g. Node 25), use [nvm](https://github.com/nvm-sh/nvm) to switch:

```bash
nvm use 22
```

Preview locally:

```bash
npx mintlify@latest dev
```

View at `http://localhost:3000`.

## Structure

- `index.mdx` — Platform overview
- `quickstart.mdx` — Getting started guide
- `why-codika/` — Value proposition (why Codika, comparison to n8n, agent integration)
- `concepts/` — Core concepts (use cases, processes, workflows, placeholders, triggers, credentials, schemas)
- `operations/` — CLI reference (all codika commands and platform operations)
- `builder/` — Builder System (AI agents for automated use case creation)
- `guides/` — Step-by-step tutorials
- `examples/` — Real-world use case examples
- `dashboard/` — Dashboard integration guides
- `docs.json` — Mintlify configuration

## Checking links

```bash
mint broken-links
```
