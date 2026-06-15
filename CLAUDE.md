# CLAUDE.md - LungPath Agent Engineering Protocol

## 0. Purpose

This file is the project-level instruction set for agents working in this
repository.

The project is a curated engineering deliverable for a Kaggle Healthcare AI
Agent and an IRP-aligned lung-health prototype:

**LungPath Agent: Explainable Pre-Diagnostic Opportunity Graph Assistant**

The system is a non-diagnostic healthcare AI agent. It uses local data, local
rules, timeline construction, candidate motif detection, graph construction, and
structured explanation to organize lung-health events before a first observed
index event.

The project is not:

- a lung-cancer predictor
- a clinical decision support system
- a missed-diagnosis detector
- a medical-liability tool
- a replacement for professional medical care

Before non-trivial edits, read:

- `docs/KAGGLE_REQUIREMENTS_REVIEW.md`
- `docs/CANDIDATE_MOTIF_TABLE_DESIGN.md`

## 1. Role

Agents working in this repository should act as engineering collaborators for a
safe, reproducible, public-demo-ready healthcare AI prototype.

The main responsibilities are:

1. keep the implementation modular and runnable,
2. keep generated explanations tied to supporting events,
3. preserve the non-diagnostic safety boundary,
4. make Kaggle deliverables reproducible without private credentials,
5. maintain clear project documentation.

When discussing project design with the user in Chinese, use plain Chinese. Use
English only for necessary technical terms or deliverable text.

## 2. Safety Boundary

Every patient-facing or demo-facing response must make the scope clear:

- The system does not provide a medical diagnosis.
- The system identifies candidate patterns for follow-up discussion.
- The system does not confirm lung cancer.
- The system does not confirm missed diagnosis or clinical negligence.
- The system is for educational and engineering demonstration only.

Urgent symptoms must trigger a safety-first response before motif explanation.
Examples include:

- severe chest pain
- severe shortness of breath
- coughing blood
- fainting
- sudden confusion
- rapidly worsening symptoms

Do not output disease probabilities, diagnostic labels, or claims that a
clinical opportunity was missed.

Prefer language such as:

- candidate motif
- supporting event
- follow-up discussion
- lung-health review
- timeline review
- screening-eligibility prompt
- care-navigation prompt

Avoid language such as:

- diagnosis
- prediction
- confirmed cancer
- missed diagnosis
- negligence
- risk score, unless it is clearly defined as non-diagnostic engineering output

## 3. Repository Structure

Use this repository structure consistently.

- `src/` contains reusable implementation code for schemas, motif detection,
  timeline construction, graph construction, explanation generation, safety
  checks, and evaluation helpers.
- `tests/` contains verification logic for parsing, motif matching, graph
  validity, and explanation-to-evidence consistency.
- `examples/` contains synthetic or de-identified-style inputs and expected
  outputs for demos, tests, and documentation.
- `docs/` contains stable engineering documentation, including architecture,
  data dictionary, motif specification, evaluation protocol, workflow rules, and
  limitations.
- `scripts/` contains reproducible command-line workflows, such as generating
  synthetic examples, running the demo pipeline, producing figures, or exporting
  allowed non-sensitive outputs.
- `app/` contains visual prototype code for exploring timelines, candidate
  motifs, supporting events, caveats, and follow-up discussion routes.
- `notebooks/` contains Kaggle or local development notebooks.
- `media/` contains public visual assets such as cover images, screenshots, and
  demo-video material.

Keep draft notes, meeting records, and project-management files outside this
repository unless the user promotes them into stable deliverable artifacts.

## 4. Data Rules

This repository must remain safe for public GitHub and Kaggle use.

Allowed:

- synthetic examples
- toy OMOP-style event tables
- public non-sensitive datasets
- de-identified-style examples created for demonstration

Not allowed:

- real All of Us participant-level data
- restricted Workbench outputs
- private clinical records
- API keys, tokens, passwords, or `.env` secrets
- any file that cannot be made public in a Kaggle notebook or GitHub repository

Kaggle notebooks must run without login, paid resources, or required external
LLM API calls.

If optional external APIs are added later, they must be clearly optional and the
local deterministic path must remain the default.

## 5. Engineering Rules

Prefer deterministic, inspectable logic for the core demo.

Keep these concerns separated where practical:

- input parsing
- red-flag checking
- follow-up question generation
- symptom or case retrieval
- candidate motif detection
- timeline building
- graph building
- explanation generation
- evaluation helpers

When adding or changing a motif rule:

1. update the motif documentation,
2. add or update a synthetic example,
3. add or update tests,
4. ensure explanation text cites the supporting fields or events.

Do not let explanation text invent evidence. If evidence is missing, the output
should say what is missing and ask a follow-up question.

## 6. Candidate Motif Policy

The source of truth for motif design is:

- `docs/CANDIDATE_MOTIF_TABLE_DESIGN.md`

Candidate motifs are not diagnostic criteria. They are "rule + evidence +
explanation + safety suggestion" entries.

Motif matching may use:

- single red-flag conditions
- two-condition combinations
- three-condition combinations
- temporal event sequences

Matching strength means explanation strength only. It must not be described as
disease probability.

## 7. Kaggle Deliverable Rules

The final Kaggle deliverable should include:

- public Kaggle writeup, no more than 1500 words
- media gallery with cover image
- public Kaggle notebook with complete runnable code
- demo video, no more than 5 minutes
- public GitHub repository with source, README, dependencies, and run
  instructions

The notebook should demonstrate:

- input parsing
- ReAct-style reasoning and tool selection
- red-flag safety checking
- follow-up question generation
- local retrieval or rule matching
- timeline visualization
- opportunity graph visualization
- structured non-diagnostic response

The notebook must not depend on private data, login, API keys, or paid services.

## 8. Documentation and Writing Style

Use restrained engineering and research language.

Prefer:

- clear claims tied to code, examples, or documentation
- explicit safety limitations
- simple reproducible descriptions
- stable terminology
- concise explanations of what the prototype actually does

Avoid:

- promotional claims
- unsupported clinical value claims
- invented results or citations
- claims of prediction, validation, diagnosis, or clinical effectiveness
- vague language that hides missing evidence

For English deliverables, use plain technical prose. For Chinese discussion with
the user, use clear conversational Chinese unless a technical term is easier to
recognize in English.

## 9. Review Procedure

When reviewing or editing project work:

1. identify the claim or behavior,
2. check whether code, examples, tests, or docs support it,
3. weaken unsupported wording before polishing style,
4. preserve the non-diagnostic boundary,
5. note missing tests or missing evidence before calling the work complete.

For code review, prioritize:

- safety regressions
- broken reproducibility
- incorrect motif matching
- graph or timeline invalidity
- explanation text not tied to evidence
- missing tests for changed behavior

## 10. Git and Repository Hygiene

Do not create commits unless the user explicitly asks.

Before reporting completion after edits, inspect the changed files or diff so
the summary matches the actual work.

Do not delete files unless the task requires it or the user explicitly asks.

Update `.gitignore` when it improves repository hygiene and does not hide files
that should be reviewed.

Never add private data, credentials, generated caches, or local environment
folders to version control.
