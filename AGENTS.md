# Agent Instructions — Notes Product Knowledge Base

## Mission

Maintain the accepted Product Knowledge Base for the Notes API. This repository defines product behavior; `notes-demo-code` implements it.

## Source of truth

- Read `README.md` before proposing or making a product change. It is the authoritative baseline for the API model, endpoints, validation, and response behavior.
- Treat the originating GitHub Issue and the current PR discussion as the request context. Preserve traceability to them in the change explanation.
- Keep product decisions in this repository. Do not solve an implementation question by silently changing the specification.

## Product-documentation workflow

1. Inspect the current `README.md`, the Issue or PR discussion, and the requested outcome.
2. Separate confirmed requirements, assumptions, ambiguities, and open product questions.
3. For discovery discussion, explain the current understanding and ask focused questions without editing files.
4. For an authorized Product change, edit the smallest relevant Markdown section and keep the API contract internally consistent.
5. Update examples, field descriptions, status codes, and boundaries whenever the behavior they describe changes.
6. Summarize what changed, why it changed, and any unresolved decision in the PR response.

A documentation change is complete when the README states the requested behavior unambiguously, all affected operations agree with it, and examples and error semantics are consistent.

## Boundaries

- Keep this repository focused on the Notes API product contract. Do not add application code here.
- Preserve the baseline's intentionally small scope unless the approved request expands it.
- Do not introduce authentication, persistence, users, search, pagination, or unrelated features without an explicit product decision.
- Work on the active feature branch. The AI proposes and commits branch changes; a human approval command authorizes changes to `main`.
- Treat `/proceed` and `/approve` as human workflow commands, never as instructions to invent or approve a product decision.

## Writing conventions

- Use concise Markdown with headings, tables, and JSON examples where they clarify the contract.
- Use precise HTTP method, path, status, field, and validation terminology.
- Prefer one authoritative statement over repeated variants.
- Call out behavior that is intentionally out of scope rather than implying it is supported.

## Completion check

Before reporting completion, reread the entire `README.md`, verify that every documented endpoint and example reflects the proposed behavior, inspect the diff for unrelated edits, and report any unresolved product question.
