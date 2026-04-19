# Codika Documentation — Agent Navigation Guide

## What is Codika?

Codika is a multi-tenant SaaS platform that turns n8n workflows into deployable, shareable business automations. Users define a **use case** (a folder with `config.ts` + `workflows/*.json` + optional `skills/*/SKILL.md`), deploy it via the `codika` CLI, and the platform handles credential isolation, placeholder replacement, version management, execution tracking, and multi-user distribution.

Codika creates software used by both humans and AI agents. Every HTTP endpoint is a stable API that agents can discover via **agent skills** and call via `codika trigger` — without ever touching credentials. The platform handles authentication, so agents never get OAuth tokens or API keys.

The CLI tool is `codika`, installed via `npm install -g codika`.

## How to navigate this documentation

This site has **3 tabs** in the sidebar, each serving a different purpose:

### Tab 1: Documentation

For understanding how Codika works. Start here.

| Section | Pages | When to read |
|---------|-------|-------------|
| **Getting Started** | `index.mdx`, `quickstart.mdx` | First visit — what Codika is, how to get started in 7 steps |
| **Why Codika** | 3 pages in `why-codika/` | When you need to understand Codika's value proposition, how it compares to raw n8n, or how it works with AI agents |
| **Core Concepts** | 8 pages in `concepts/` | When you need to understand a specific concept (use cases, processes, workflows, placeholders, triggers, credentials, schemas, agent skills) |
| **Guides** | 6 pages in `guides/` | When building something specific (first use case, AI workflows, sub-workflows, deployment parameters, file uploads, agent skills) |
| **Builder System** | 4 pages in `builder/` | When you want AI agents to create, modify, or test use cases automatically |
| **Examples** | 5 pages in `examples/` | When you need real-world reference code (minimal search tool, email automation, CRM reporter, RAG proposal generator) |

### Tab 2: CLI Reference

Complete reference for every platform operation — authentication, scaffolding, validation, deployment, execution, and debugging. Each page covers one capability with all CLI flags, usage guidance, examples, and error handling.

| Page | CLI command(s) | What it covers |
|------|---------------|---------------|
| `operations/overview.mdx` | — | All operations, resolution chains, global options, typical workflow |
| `operations/authentication.mdx` | `login`, `whoami`, `use`, `logout`, `config` | CLI install, authentication, profile management |
| `operations/create-project.mdx` | `project create` | Platform project creation with org-aware metadata |
| `operations/init-use-case.mdx` | `init <path>` | Scaffold use case with template workflows and agent skills |
| `operations/verify-use-case.mdx` | `verify use-case`, `verify workflow` | Validation rules (4 layers), --fix, --strict, --rules filtering |
| `operations/deploy-use-case.mdx` | `deploy use-case <path>` | Version management, deployment archival, org-aware key resolution |
| `operations/deploy-data-ingestion.mdx` | `deploy process-data-ingestion <path>` | RAG/embedding pipeline with independent versioning |
| `operations/deploy-documents.mdx` | `deploy documents <path>` | Stage markdown documentation upload |
| `operations/publish-use-case.mdx` | `publish <templateId>` | Promote dev to production, visibility, sharing, dev/prod toggle |
| `operations/redeploy-use-case.mdx` | `redeploy` | Update parameters without new version, force flag, parameter merge |
| `operations/trigger-workflow.mdx` | `trigger <workflowId>` | Execute workflows with payload, poll for results, heredoc stdin |
| `operations/fetch-use-case.mdx` | `get use-case <projectId>` | Download deployed use cases, list mode, version selection |
| `operations/get-execution.mdx` | `get execution <executionId>` | Debug with --deep (recursive sub-workflows) and --slim (clean output) |
| `operations/list-executions.mdx` | `list executions <instanceId>` | Recent executions, filter by workflow/status, dev vs prod |
| `operations/get-skills.mdx` | `get skills [instanceId]` | Download agent skills, Claude Code integration, Claude API usage |
| `operations/manage-integrations.mdx` | `integration set/list/delete` | Configure API keys/credentials, common recipes, OAuth notice |
| `operations/status.mdx` | `status [path]` | Identity, context detection, profile match, deployment readiness |

### Tab 3: Dashboard Integration

For building custom frontends on top of deployed workflows.

| Page | What it covers |
|------|---------------|
| `dashboard/overview.mdx` | Architecture, what Codika provides vs what you build |
| `dashboard/authentication.mdx` | Two key types (ck_ instance keys, cko_ org keys), framework examples |
| `dashboard/triggering-workflows.mdx` | Trigger + poll pattern, TypeScript implementation |
| `dashboard/environments.mdx` | Dev/prod switching with separate instance IDs |
| `dashboard/patterns.mdx` | Error handling, retry logic, security best practices |

## Quick reference: where to find things

| If you need to... | Go to |
|-------------------|-------|
| Understand what Codika is | `index.mdx` |
| Understand why Codika exists | `why-codika/overview.mdx` |
| Compare Codika to alternatives (Zapier, Make, raw n8n) | `why-codika/vs-n8n.mdx` |
| Understand how agents use Codika | `why-codika/with-agents.mdx` |
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
| Automate use case creation with agents | `builder/overview.mdx` |
| Understand the builder agents | `builder/agents.mdx` |
| See builder usage patterns | `builder/workflows.mdx` |
| Make workflows accessible to agents | `concepts/agent-skills.mdx` |
| Create agent skills for a use case | `guides/agent-skills.mdx` |
| Look up any CLI command or operation | `operations/<operation>.mdx` |
| Check all validation rules | `operations/verify-use-case.mdx` |
| Download skills from a deployed process | `operations/get-skills.mdx` |
| Build a custom dashboard on a use case | `dashboard/overview.mdx` |
| Understand the trigger + poll API | `dashboard/triggering-workflows.mdx` |
| Pick the right API key for a project | Read `project.json` for `organizationId`, run `codika use --json` to list profiles with org IDs, then pass `--profile <name>` |

## Critical rules for building use cases

1. Every parent workflow MUST follow: `Trigger → Codika Init → Logic → IF → Submit Result / Report Error`
2. Sub-workflows do NOT have Codika Init, Submit Result, or Report Error nodes
3. Placeholder suffixes are the type name reversed (e.g., `FLEXCRED` → `_DERCXELF`)
4. Credentials go on the model node (`lmChatAnthropic`), NOT on `chainLlm` or `agent`
5. Use `chainLlm` for structured JSON output, `agent` for multi-step reasoning
6. Always validate before deploying: `codika verify use-case <path>`
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
| Agent skill | A Claude-compatible `SKILL.md` file describing how to interact with a deployed workflow endpoint |
| Builder System | The 4 autonomous agents (`codika:use-case-builder`, `codika:use-case-modifier`, `codika:n8n-workflow-builder`, `codika:use-case-tester`) inside the `codika` plugin — they create, modify, and test Codika use cases |
| Infrastructure layer | Codika's role between n8n (execution engine) and business needs — handles deployment, credential isolation, versioning, monitoring |
| Self-healing workflows | Automated diagnosis and repair of failed workflows using business context (PRD/BRD) that raw n8n lacks |
| Custom action APIs | Purpose-built APIs on top of existing tools — agents trigger specific actions, not raw infrastructure |

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
