# Parallel Subagents for Codex IDE

Hierarchical multi-agent team composer for Codex IDE.

## Overview

This project generates a working team plan for parallel delivery with:

- 1 top-level orchestrator (`Orchestrator-X`)
- configurable sub-orchestrators
- configurable worker agents

The generated plan enforces supervision and structured feedback loops, while keeping direct code editing limited to workers.

## Key Principles

- `Orchestrator-X` defines direction and approvals only
- `Orchestrator-X` does not perform direct code edits
- sub-orchestrators supervise and aggregate
- workers execute implementation
- every task has at least 2 feedback round trips

## Features

- configurable total team size (`--agents`)
- configurable mode (`--mode low|high`)
- configurable sub-orchestrator count (`--sub-orchestrators auto|n`)
- dynamic reasoning policy with runtime variables
- autonomous reasoning assignment for orchestrator, sub-orchestrators, and workers
- external reasoning policy override via JSON string, JSON file, or environment variable
- global MM persona injection (`Ray` + top-tier Codeforces Grandmaster) for all generated agents
- automatic role allocation by workload complexity
- generated MM-style custom instructions for all roles
- generated feedback matrix and reporting flow

## Requirements

- Node.js 18+
- Windows, macOS, or Linux shell

## Quick Start

```powershell
cd C:\Users\Lemos\Desktop\CODEX\parallel-subagents-for-codex-ide
node scripts/compose-team.js --agents 9 --mode low --sub-orchestrators auto
```

Or with CMD launcher:

```powershell
cd C:\Users\Lemos\Desktop\CODEX\parallel-subagents-for-codex-ide
.\compose-team.cmd --agents 9 --mode low --sub-orchestrators auto
```

## CLI Options

- `--agents <n>`
- `--mode <low|high>`
- `--sub-orchestrators <auto|n>`
- `--output-dir <path>`
- `--reasoning-policy-json <json>`
- `--reasoning-policy-file <path>`

## Modes

- `low` (`low-cost-high-efficiency`)
  - default mode
  - minimizes reasoning cost while preserving required quality gates
- `high` (`high-cost-high-efficiency`)
  - expands reasoning budgets for deeper review margins

## Output Files

- `generated/team-plan.json`
  - hierarchy
  - role assignments
  - persona profile
  - reasoning assignment (autonomous)
  - reasoning policy snapshot
  - feedback matrix
- `generated/agent-instructions.mm.md`
  - MM-style custom instructions for orchestrator, sub-orchestrators, and workers

## Architecture

- Top Layer
  - `Orchestrator-X` (policy-driven reasoning, no direct edits)
- Middle Layer
  - `Sub-Orchestrators` (policy-driven reasoning range, no direct edits)
- Execution Layer
  - `Workers` (policy-driven reasoning range, direct edits enabled)

## Dynamic Reasoning Policy

- The script resolves reasoning from runtime variables, not hardcoded role constants
- Default policy source is internal base policy
- Override precedence:
  - base policy
  - `CODEX_REASONING_POLICY_JSON` environment variable
  - `--reasoning-policy-json`
  - `--reasoning-policy-file`
- On PowerShell, prefer `--reasoning-policy-file` or `CODEX_REASONING_POLICY_JSON` for stable JSON quoting
- Final resolved policy is written to `team-plan.json` under `reasoningPolicy`

## Feedback Flow

1. Round 1: worker implementer -> worker peer reviewer
2. Round 2: worker implementer -> assigned sub-orchestrator
3. Round close: sub-orchestrator aggregate -> `Orchestrator-X`

## Example

```powershell
# 12 total agents, low-cost mode, fixed 3 sub-orchestrators
node scripts/compose-team.js --agents 12 --mode low --sub-orchestrators 3
```

```powershell
# Same setup with a custom reasoning policy file
node scripts/compose-team.js --agents 12 --mode low --sub-orchestrators 3 --reasoning-policy-file .\policy\reasoning.json
```

## Repository Notes

- Main operating rules: [`AGENTS.md`](AGENTS.md)
- Model details: [`docs/ORCHESTRATION_MODEL.md`](docs/ORCHESTRATION_MODEL.md)
