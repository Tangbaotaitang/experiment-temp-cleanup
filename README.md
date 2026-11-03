# experiment-temp-cleanup

Keep experiments reproducible without leaving a trail of disposable scripts, test outputs, and one-off dependencies.

An instruction-based, provider-neutral skill for AI coding agents. It guides the agent to keep one Markdown attempt log per experiment, remove temporary artifacts after validation passes, and check that the remaining files support formal execution and reproduction.

## What it does

- **Records every meaningful attempt:** Track changes, commands, outcomes, failure causes, and the next planned adjustment.
- **Uses one log per experiment:** Continue the same Markdown file across retries, dates, and conversations.
- **Cleans up after validation:** Remove disposable scripts, intermediate test outputs, and isolated dependencies that are no longer needed.
- **Preserves research materials:** Keep formal scripts, results, original data, dependency definitions, configuration, and reproduction instructions.
- **Checks integrity after cleanup:** Verify retained files and references, and run a proportionate check when needed.

This repository contains instructions and UI metadata. It does not include a standalone cleanup program, background service, or scheduled task. Actions are performed by the agent using the tools and permissions available in its environment.

## When to use it

Use this skill for computational experiments, simulations, data analysis, model evaluations, and experiment wrap-up work that generates disposable files.

It is especially useful when an agent repeatedly creates debugging scripts, smoke-test inputs, temporary plots, or separate environments while trying to get an experiment to pass its acceptance checks.

## Agent compatibility

The reusable workflow lives in the standard `SKILL.md` file, with `name` and `description` frontmatter. It does not call OpenAI APIs or depend on a vendor-specific SDK, tool name, or model. The same core file can be installed in agents that support this format.

`agents/openai.yaml` is optional metadata for OpenAI-compatible interfaces. It is not the execution engine and is not required by the core workflow. For other agents, install `SKILL.md`; there is no need to invent platform-specific YAML files or duplicate the instructions.

The following project-level locations were checked against each vendor's documentation on November 3, 2026. All paths are relative to the target project root:

| Agent | Project installation folder | Documentation |
| --- | --- | --- |
| Codex | `.agents/skills/experiment-temp-cleanup/` | [Skills](https://learn.chatgpt.com/docs/build-skills) |
| Claude Code | `.claude/skills/experiment-temp-cleanup/` | [Skills](https://code.claude.com/docs/en/skills) |
| Cursor | `.cursor/skills/experiment-temp-cleanup/` | [Agent Skills](https://cursor.com/docs/skills) |
| GitHub Copilot | `.github/skills/experiment-temp-cleanup/` | [Agent skills](https://docs.github.com/en/copilot/concepts/customizations/agent-skills) |
| Gemini CLI | `.gemini/skills/experiment-temp-cleanup/` | [Agent Skills](https://geminicli.com/docs/cli/skills/) |
| OpenCode | `.opencode/skills/experiment-temp-cleanup/` | [Agent Skills](https://opencode.ai/docs/skills/) |
| Cline | `.cline/skills/experiment-temp-cleanup/` | [Skills](https://docs.cline.bot/customization/skills) |
| Roo Code | `.roo/skills/experiment-temp-cleanup/` | [Skills](https://roocodeinc.github.io/Roo-Code/features/skills/) |
| Kiro | `.kiro/skills/experiment-temp-cleanup/` | [Agent Skills](https://kiro.dev/docs/skills/) |

This is a documentation-based format and installation compatibility matrix, not an end-to-end test report. The workflow has not been runtime-tested in every listed agent. Skill availability, discovery, invocation, trust settings, cloud environments, and filesystem permissions depend on the installed version and session.

## Installation

1. Download this repository or clone it using an account that has access. While the repository is private, other users need access to download it.
2. Choose the project folder for your agent from the compatibility table.
3. Create that folder and place `SKILL.md` directly inside it. You may also copy `README.md`. Keep `agents/openai.yaml` if your host uses that metadata; other installations need only `SKILL.md`.
4. Open the target project in your agent and verify the skill appears in its skill list or picker. Reload skills or restart the session if needed, following the linked documentation.

For example, a Claude Code project would contain:

```text
your-project/
└── .claude/
    └── skills/
        └── experiment-temp-cleanup/
            └── SKILL.md
```

Codex, Cursor, GitHub Copilot, Gemini CLI, OpenCode, and Roo Code also document support for the shared project path `.agents/skills/`. Use one supported location per agent installation to avoid duplicate entries or conflicting copies. Claude Code and Cline have their own documented locations; do not assume one shared path works in every host.

For user-wide installation, consult your agent's documentation for its personal skill directory. Local user skills are not automatically available in every remote or cloud session.

## Usage

Use your host's skill selector or invocation syntax:

| Host | Example |
| --- | --- |
| Codex CLI / IDE extension | `Use $experiment-temp-cleanup for this experiment.` |
| Claude Code | `/experiment-temp-cleanup` followed by the experiment request |
| Other supported agents | Select the installed skill in the host's skill UI, or request it by name and confirm that the agent loads `SKILL.md`. Invocation syntax varies by host. |

Then describe the experiment, for example:

```text
Run this experiment using experiment-temp-cleanup.
Keep all attempts in one Markdown file. Once the acceptance checks pass,
remove disposable scripts, isolated test dependencies, and temporary outputs.
Preserve the formal scripts and reproduction materials, then check integrity.
```

For an existing experiment:

```text
Use experiment-temp-cleanup to wrap up this experiment.
Continue its existing attempt log. Identify temporary artifacts from their
provenance and purpose, preserve unresolved failure evidence, and clean up
eligible artifacts after confirming validation has passed.
```

Hosts may also choose the skill automatically when its description matches the task. Confirm that it was loaded rather than assuming a name mention guarantees activation. The skill does not bypass host permissions or required confirmation prompts.

### Hosts without native skill discovery

An agent that can read files, edit the workspace, and run the necessary checks can be asked to read `SKILL.md` explicitly:

```text
Read <path-to-this-repository>/SKILL.md and apply its workflow to this experiment.
```

Replace the placeholder with an accessible path, or attach the file through the host's context interface. This is a manual instruction fallback, not a claim of native integration. A chat-only client without filesystem tools can help plan the workflow but cannot perform local cleanup.

## One experiment, one Markdown log

The agent reuses an existing experiment record whenever possible. Otherwise, it creates one stable file such as `experiments/<experiment-id>.md`.

Debugging, smoke tests, parameter adjustments, retries, and final validation for the same research objective belong in that file. A new date or parameter value does not create a new log. Independent objectives or evaluation protocols can have separate records.

The following table is a fictional example, not a record of actual execution:

| Attempt | Time | Changes made | Command / configuration | Status and result | Failure and cause | Next planned change | Artifact status |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Day 1, 10:00 | Initial configuration | `python run.py --config config.yaml` | Failed: input not found | Configuration points to a missing input file | Correct the input path | Minimal error evidence retained |
| 2 | Day 1, 10:20 | Corrected the input path | Same command; updated input path | Failed: output contains NaN | Cause unconfirmed; missing values are a hypothesis | Inspect missing values | Diagnostic output retained |
| 3 | Day 1, 11:00 | Added missing-value handling after confirming the cause | Same command; updated preprocessing | Passed all defined acceptance checks | None | None | Formal results retained; eligible temporary artifacts removed |

The same file also records the objective, input versions, acceptance criteria, formal run command, relevant environment and seed information, result locations, and final cleanup and integrity status.

Attempt numbers continue across sessions. Failed attempts remain visible. Confirmed causes are distinguished from hypotheses, and planned changes are distinguished from changes actually made. Full logs and raw datasets stay outside the summary table.

## Cleanup and retention rules

| Material | Default treatment |
| --- | --- |
| One-off debugging, connectivity, and smoke-test scripts | Remove after validation, once no longer needed |
| Toy inputs, temporary plots, duplicate exports, test reports, and intermediate debugging outputs | Remove when eligible; retain the relevant summary in the experiment log |
| Isolated dependencies or environments created only for disposable tests | Remove after confirming no formal run, reproduction workflow, or active process needs them |
| Formal execution, preprocessing, analysis, and reproduction scripts | Keep |
| Formal configuration, dependency manifests, lockfiles, seeds, and environment information | Keep |
| Original or irreplaceable data, formal results, and evidence supporting conclusions | Keep |
| Formal weights, checkpoints, and intermediate datasets | Decide from reproduction and continuation requirements; size alone does not justify deletion |
| Existing project tests, CI checks, and long-term regression tests | Keep |
| Evidence needed to diagnose an unresolved failure | Keep the necessary evidence until resolved |
| The experiment's single Markdown record | Keep |

Negative scientific findings may be formal results and must not be discarded merely because they are unfavorable.

## Cleanup conditions and integrity checks

Cleanup begins only after the applicable acceptance checks have actually passed. A successful process exit or smoke test is insufficient when other checks remain outstanding.

Before deletion, the agent checks provenance, purpose, dependencies, and retention requirements. Useful logic is moved into formal code first. Shared environments, global packages, shared caches, and other tasks' files are preserved by default. Names such as `test`, `tmp`, `.venv`, or `node_modules` are not sufficient grounds for deletion.

After cleanup, the agent checks that eligible artifacts were removed and that formal entry points, configuration, input data, results, dependency definitions, and the experiment log remain available. Relevant references and version-control changes are inspected. Where appropriate, a small import, configuration-load, or smoke check runs from the retained formal environment.

The actual checks, skipped checks, cleanup failures, and unresolved issues are written back to the same experiment log. Merely confirming that files exist is not reported as proof that the full experiment can run. The skill does not require repeating an expensive full experiment just to verify cleanup.

## Repository contents

```text
experiment-temp-cleanup/
├── README.md             # Overview, installation, and examples
├── SKILL.md              # Agent instructions and decision rules
└── agents/
    └── openai.yaml       # Optional OpenAI UI metadata and default prompt
```

The complete, provider-neutral workflow is defined in [SKILL.md](SKILL.md). The repository does not require additional scripts, an MCP server, or a vendor API key of its own. Any account or model access requirements belong to the agent you use.
