# Ensure CodeRabbit configuration

Run this only when no CodeRabbit config file exists in the repository root or
documented locations (`.coderabbit.yaml`, `.coderabbit.yml`, `coderabbit.yaml`,
`coderabbit.yml`).

## 1. Analyze the repository

Inspect enough to propose useful `path_instructions` and defaults:

- README, AGENTS.md, CLAUDE.md, CONTRIBUTING.md, and existing agent docs
- Language and framework manifests
- Test, CI, and infrastructure layout
- Domain docs such as CONTEXT.md or ADRs when present
- Soft-load `$engineering-for-certainty` when available; use it as organizational
  engineering baseline without inventing client-specific rules

Prefer existing project conventions over generic advice. Never copy knowledge
from another client repository.

## 2. Generate through CodeRabbit tooling

Prefer an already-installed CodeRabbit `$coderabbit-config` skill when
available. Otherwise use the official CodeRabbit CLI config workflow already
on the machine (for example `coderabbit config` / agent generate paths
supported by the installed CLI).

Do not install CodeRabbit skills or the CLI from this step. If neither the
config skill nor a usable CLI config command is available, skip ensure-config
with a hint and continue the second eye using CodeRabbit defaults (or skip the
vendor review if the CLI cannot run at all).

Do not hand-author a full YAML file when the vendor skill or CLI can generate
or validate one.

## 3. Review and approve

Present the proposed configuration, including path instructions and defaults
derived from exploration. Save only after the developer approves. If they
decline, continue the second-eye review with CodeRabbit defaults and record
that config was not written.

## Existing configuration

If a config file already exists, leave it unchanged during ordinary second-eye
runs. Recommend improvements only when the user asks to update CodeRabbit
configuration.
