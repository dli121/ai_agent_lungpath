# AGENTS.md

This repository keeps its main project-level agent instructions in
[`CLAUDE.md`](./CLAUDE.md).

Before making non-trivial edits, coding agents should read:

- [`CLAUDE.md`](./CLAUDE.md)
- [`docs/KAGGLE_REQUIREMENTS_REVIEW.md`](./docs/KAGGLE_REQUIREMENTS_REVIEW.md)
- [`docs/CANDIDATE_MOTIF_TABLE_DESIGN.md`](./docs/CANDIDATE_MOTIF_TABLE_DESIGN.md)

This project is a non-diagnostic lung-health AI agent prototype for Kaggle and
IRP engineering work. Agents must not turn it into a clinical diagnosis system,
lung-cancer predictor, missed-diagnosis detector, or medical-liability tool.

Do not add real All of Us participant-level data, restricted Workbench outputs,
API keys, tokens, or private credentials to this repository.

This file is intentionally a normal markdown file, not a symlink, because this
folder may be synced across local file systems.
