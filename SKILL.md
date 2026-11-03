---
name: experiment-temp-cleanup
description: "For computational experiments, simulations, data analysis, and model evaluation: maintain one Markdown attempt log per experiment, clean up disposable scripts, isolated dependencies, and test outputs after validation, and verify integrity while preserving formal execution and reproduction materials. Also use for requested experiment wrap-up, not general disk cleanup or removal of long-term project tests."
---

# Experiment Temporary File Cleanup

Keep one continuously updated Markdown attempt log per experiment. After acceptance checks pass, remove disposable validation files and dependencies while preserving everything needed for formal execution, interpretation, and reproduction.

## Host portability

- Apply this workflow using the host's available file, search, edit, and execution tools. It does not require OpenAI APIs, a specific model, vendor-specific tool names, or `agents/openai.yaml`; that file contains optional UI metadata only.
- Respect the host's filesystem scope, trust settings, and required approval prompts. Existing task authorization does not bypass host restrictions. If tools or permissions prevent a check or cleanup action, report the limitation and retain affected files rather than claiming completion. Do not install another agent or change unrelated host settings to run this workflow.

## At experiment start

- Follow existing project directory conventions. Put disposable scripts, outputs, and isolated test dependencies in a dedicated temporary directory, such as `.experiment-tmp/<run-id>/`, separate from formal data, results, and other people's files.
- Track the temporary paths created during this work, their purposes, and the locations and users of temporary dependencies. Working context is sufficient; do not generate a separate per-file inventory. For inherited experiments, filenames alone do not establish what is disposable.
- Define acceptance checks appropriate to the task, such as input validation, output structure, numerical tolerances, or baseline comparisons. Distinguish successful execution from support for a scientific conclusion. A zero exit code or passing smoke test does not substitute for all applicable checks.
- Follow the user's retention requirements. Do not extend cleanup to unrelated historical files.

## One Markdown record per experiment

- Find and continue the existing experiment record, including an equivalent Markdown record accompanying a notebook or reproduction workflow. Only create a stable file such as `experiments/<experiment-id>.md` when none exists. Do not create separate Markdown files by date, retry, or validation phase, or separate cleanup and failure reports.
- Identify an experiment by its research objective, input scope, and acceptance criteria. Debugging, smoke tests, parameter adjustments, failed retries, and final validation for the same objective belong in the same record. A parameter or script version change alone does not justify another file. Independent research objectives or evaluation protocols may have separate records. Do not delete pre-existing duplicate records without authorization; choose one to continue.
- Briefly document the objective, input locations and versions, acceptance criteria, formal entry point and command, relevant environment and dependency versions, random seeds, and retained result locations. Link existing configuration and documentation rather than duplicating substantial content.
- Maintain one appendable table of meaningful attempts. Continue numbering from the highest existing attempt across dates and conversations. Related checks within one attempt may share a row; rerunning after a code or configuration change is a new attempt.
- Create or update the current row when execution starts, then promptly record passed, failed, interrupted, or incomplete status. Do not wait for eventual success to reconstruct history. Preserve failures rather than overwriting them with success. When resuming interrupted work, clarify the previous attempt's status first.
- Record the actual change from the preceding attempt, relevant code/configuration versions or parameters, command or checks, outcome and key metrics, failure cause, and next planned change. Separate observed failures from confirmed causes. Write "not yet identified" when the cause is unknown, and label hypotheses. In the next row, record the change actually made rather than presenting a plan as completed work.
- After success, use "none" for the next change unless there is a real follow-up. Keep summaries and necessary paths in the table; do not paste full logs, datasets, or tracebacks. Escape literal pipe characters in table cells. Use the user's or project's preferred language for experiment records.

Suggested structure; do not copy fictitious attempts into a real record:

```markdown
# <Experiment name>

- Objective and inputs: <research objective, input locations and versions>
- Acceptance criteria: <checks actually used>
- Formal execution and reproduction: <entry point, command, configuration/environment/seeds or links>
- Formal results: <retained locations, or note that none exist yet>

| Attempt | Time | Actual changes | Command / checks and key configuration | Status and key results | Failure symptoms and cause | Next planned change | Artifacts and cleanup status |
| --- | --- | --- | --- | --- | --- | --- | --- |

## Final validation and cleanup

<Validation status; removed categories and necessary paths; retained materials; actual post-cleanup integrity checks and results; remaining issues. Explicitly mark incomplete work.>
```

This record is retained research and reproduction material. Never classify it as disposable Markdown. Write cleanup information into this same file rather than generating a second report.

## Retention decisions

| Purpose | Default treatment |
| --- | --- |
| One-off exploration, debugging, connectivity, or smoke-test scripts | Delete after validation when no longer needed |
| Toy inputs, duplicate exports, temporary plots, debug logs, intermediate results, coverage outputs, or test reports from those scripts | Delete when eligible, after recording the relevant summary |
| Isolated dependency directories, virtual environments, manifests, or lockfiles created only for disposable tests | Delete only when formal execution, reproduction, and other tasks do not need them; apply the dependency rules below |
| Formal entry points, preprocessing, analysis, and reproduction helpers | Keep; first integrate useful logic from temporary scripts into formal code |
| Formal configuration, dependency manifests and lockfiles, environment information, seeds, versions, and commands | Keep and link from the experiment record |
| Original or irreplaceable data, formal results, metrics, figures, and necessary supporting logs | Keep; negative findings and failed experiments can be formal research evidence |
| Formal weights, checkpoints, and intermediate datasets | Decide based on reproduction, continuation, and task requirements; size or regenerability alone does not justify deletion |
| Existing project tests, CI checks, and long-term regression tests | Keep; integrate temporary checks with lasting regression value into the existing test system |
| Scripts, minimal inputs, and error logs needed for unresolved failures | Keep necessary diagnostic evidence; after resolution and final validation, remove temporary materials no longer needed while preserving the failure summary |

Do not archive every temporary intermediate that can be recreated from original inputs, formal code, and configuration. Preserve materials the user requests or that support formal conclusions. Moving, renaming, compressing, or backing up disposable files does not count as cleanup.

## Disposable dependencies and environments

- Prefer a dedicated environment or directory for test-only dependencies so cleanup can remove it as a unit. Respect existing project environment conventions.
- Determine disposability from purpose, provenance, and actual usage, not directory names such as `.venv`, `node_modules`, or cache. Preserve formal dependency definitions and relevant environment information, as well as runnable environments the user requires.
- Clean only dedicated dependencies, environments, and supporting files from this work that are no longer used. First check for active experiments or processes and references from formal entry points, task configuration, and documentation.
- Preserve formal dependencies, existing project/global/shared environments, shared package caches, and other tasks' dependencies by default. If test packages were mixed into an existing environment, do not guess which packages to uninstall or remove the environment. Record the retained location and reason. Make a targeted restoration only when a clear pre-installation record, understood impact, and task authorization support it.

## Validation and cleanup

1. Confirm that all applicable acceptance checks actually ran and passed and that attempts are recorded in the same experiment log. If checks failed, were interrupted, or remain incomplete, do not perform the success-triggered cleanup or claim validation passed.
2. Confirm that formal execution and reproduction do not depend on files or test environments scheduled for deletion, including configuration, imports, input data, and documentation links. Promote useful logic or files first. Choose the smallest appropriate integrity check before deletion; do not rerun an expensive full experiment solely for cleanup.
3. Before deletion, write necessary validation evidence and failure summaries into the experiment's single Markdown record: actual checks and conclusions, key metrics, commands, relevant configuration and versions, and retained result locations. Integrity checks and cleanup conclusions also belong in this file, not a separate report.
4. Delete only items created during this work or explicitly designated for cleanup, whose temporary purpose is established, that are no longer needed for formal execution, reproduction, or diagnosis, and have no retention requirement. Establish concrete paths and reasons in working context before deletion. Do not repeatedly request confirmation for already authorized cleanup of this work's temporary files. Preserve items with unknown purpose or provenance, or changes involving existing files, and ask only about specific ambiguities that matter.
5. Use resolved absolute paths and verify they fall within the intended experiment temporary directory or individually verified disposable file scope. Recursive deletion is limited to inspected, dedicated temporary or dependency directories from this work. Reject links or junctions pointing outside that scope. Never bulk-delete the project root, shared directories, or files selected solely by `test*`, `tmp*`, or extension. On Windows, use native commands with `-LiteralPath` within the same PowerShell session.
6. After cleanup, verify eligible items are gone and formal entry points, configuration, dependency definitions, inputs, results, and the single experiment record remain present with required links resolving. Inspect version-control changes when available for unintended deletion of formal files. Use the retained formal environment for a proportionate import, configuration-load, or smoke check to detect references to deleted temporary files or environments. Reuse equivalent checks when available; do not install new tools or recreate a removed test environment just to claim cleanup succeeded.
7. Record actual integrity checks and outcomes in the same log's final section. Mark checks that could not run as unverified and explain why. File existence alone does not establish runnable formal execution. If deletion or integrity verification fails, record affected paths and impact and stop expanding deletion. Make necessary repairs from retained formal materials and recheck where feasible, without unrelated expensive reruns. Report unresolved issues and do not claim complete success.

Finish with a concise account of validation status, the single record's path, formal entry points and reproduction materials, removed categories, integrity outcomes, and necessary exceptions. Report reclaimed disk space only if actually measured.
