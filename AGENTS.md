# Codika Documentation — Agent Navigation Guide

## What is Codika?

Codika is a multi-tenant SaaS platform that turns n8n workflows into deployable, shareable business automations. Users define a **use case** (a folder with `config.ts` + `workflows/*.json`), deploy it via the `codika-helper` CLI, and the platform handles credential isolation, placeholder replacement, version management, execution tracking, and multi-user distribution.

The CLI tool is `codika-helper`, installed via `npm install -g @codika-io/helper-sdk`.

## How to navigate this documentation

This site has **3 tabs** in the sidebar, each serving a different purpose:

### Tab 1: Documentation

For understanding how Codika works. Start here.

| Section | Pages | When to read |
|---------|-------|-------------|
| **Getting Started** | `index.mdx`, `quickstart.mdx` | First visit — what Codika is, how to get started in 7 steps |
| **Core Concepts** | 7 pages in `concepts/` | When you need to understand a specific concept (use cases, processes, workflows, placeholders, triggers, credentials, schemas) |
| **Guides** | 5 pages in `guides/` | When building something specific (first use case, AI workflows, sub-workflows, deployment parameters, file uploads) |
| **Examples** | 5 pages in `examples/` | When you need real-world reference code (minimal search tool, email automation, CRM reporter, RAG proposal generator) |

### Tab 2: CLI Reference

For looking up specific CLI commands, flags, and options.

| Page | CLI command | What it covers |
|------|------------|---------------|
| `cli/overview.mdx` | — | Installation, command hierarchy, resolution chains, configuration |
| `cli/login.mdx` | `login`, `whoami`, `use`, `logout`, `config` | Authentication and profile management |
| `cli/init.mdx` | `init <path>` | Scaffold a new use case folder |
| `cli/verify.mdx` | `verify use-case`, `verify workflow` | Validation rules, --fix, --strict |
| `cli/deploy.mdx` | `deploy use-case`, `deploy process-data-ingestion` | Deployment with version management |
| `cli/trigger.mdx` | `trigger <workflowId>` | Execute workflows, poll for results |
| `cli/get.mdx` | `get use-case`, `get execution` | Download use cases, debug executions |
| `cli/project.mdx` | `project create` | Create platform projects |
| `cli/status.mdx` | `status [path]` | Check identity and deployment readiness |

### Tab 3: Skills

For AI agents using the Codika plugin. Each skill teaches agents how to use a specific CLI command.

| Page | Skill name | Maps to |
|------|-----------|---------|
| `skills/overview.mdx` | — | What skills are, how to install the plugin |
| `skills/setup-codika.mdx` | `setup-codika` | CLI install + auth |
| `skills/create-project.mdx` | `create-project` | `project create` |
| `skills/init-use-case.mdx` | `init-use-case` | `init` |
| `skills/verify-use-case.mdx` | `verify-use-case` | `verify` |
| `skills/deploy-use-case.mdx` | `deploy-use-case` | `deploy use-case` |
| `skills/fetch-use-case.mdx` | `fetch-use-case` | `get use-case` |
| `skills/trigger-workflow.mdx` | `trigger-workflow` | `trigger` |
| `skills/get-execution.mdx` | `get-execution` | `get execution` |

## Quick reference: where to find things

| If you need to... | Go to |
|-------------------|-------|
| Understand what Codika is | `index.mdx` |
| Install the CLI and deploy something fast | `quickstart.mdx` |
| Know how a use case folder is structured | `concepts/use-cases.mdx` |
| Understand the process lifecycle | `concepts/processes.mdx` |
| Learn the mandatory workflow pattern | `concepts/workflows.mdx` |
| Look up a specific placeholder type | `concepts/placeholders.mdx` |
| Know which trigger type to use | `concepts/triggers.mdx` |
| Understand credential scopes (FLEXCRED, USERCRED, etc.) | `concepts/credentials.mdx` |
| Define input/output schemas | `concepts/schemas.mdx` |
| Build a use case from scratch | `guides/first-use-case.mdx` |
| Add AI/LLM nodes to a workflow | `guides/ai-workflows.mdx` |
| Create a sub-workflow | `guides/sub-workflows.mdx` |
| Add user-configurable install parameters | `guides/deployment-parameters.mdx` |
| Upload files from workflows | `guides/file-uploads.mdx` |
| See a minimal example (1 workflow) | `examples/simple-search.mdx` |
| See a complex example (RAG + multi-workflow) | `examples/complex-rag.mdx` |
| Look up CLI command flags | `cli/<command>.mdx` |
| Check all validation rules | `cli/verify.mdx` |

## Critical rules for building use cases

1. Every parent workflow MUST follow: `Trigger → Codika Init → Logic → IF → Submit Result / Report Error`
2. Sub-workflows do NOT have Codika Init, Submit Result, or Report Error nodes
3. Placeholder suffixes are the type name reversed (e.g., `FLEXCRED` → `_DERCXELF`)
4. Credentials go on the model node (`lmChatAnthropic`), NOT on `chainLlm` or `agent`
5. Use `chainLlm` for structured JSON output, `agent` for multi-step reasoning
6. Always validate before deploying: `codika-helper verify use-case <path>`
7. INSTPARM placeholders are context-aware — do NOT add extra quotes in Code nodes

## Terminology

| Term | Meaning |
|------|---------|
| Use case | A folder containing `config.ts` and `workflows/` — the deployment unit |
| Process | The public, discoverable entity created when a use case is deployed |
| Process instance | A user's personal installation with isolated credentials and data |
| Placeholder | Template token like `{{FLEXCRED_ANTHROPIC_ID_DERCXELF}}` replaced at deploy time |
| Codika nodes | Custom n8n nodes: Init, Submit Result, Report Error, Upload File |
| Trigger | How a workflow starts: HTTP, schedule, service event, or sub-workflow |
| Deployment parameter | Value configured at install time, injected via INSTPARM placeholder |

## About this documentation site

- Built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration: `docs.json`
- Preview: `mint dev`
- Check links: `mint broken-links`
- GEO files: `llms.txt` (navigation map for LLMs), `robots.txt` (allows all AI crawlers)
- `llms-full.txt` is auto-generated by Mintlify (full content of all pages)

## Style conventions

- Active voice, second person ("you")
- One idea per sentence
- Tables for reference data
- Code examples with realistic flags
- No real API keys or internal URLs — use placeholder values
