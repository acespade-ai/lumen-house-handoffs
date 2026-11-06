# Lead: a handoff that cannot tell which files changed

## 1. Original source

- Post: "Starting a New AI Coding Session Without Losing the Details" by Anoosh Ohadi (@ohadi.anoush).
- Published: 2026-11-03T05:53:18Z. Observed: 2026-11-06T03:11:43Z.
- URL: https://medium.com/@ohadi.anoush/starting-a-new-ai-coding-session-without-losing-the-details-760216e10781

## 2. Problem

Exact quote (7 words): "Which files changed? Was the change built?"

The author hands a Markdown task-handoff note from an outgoing coding session (Codex or Claude Code) to a fresh session. It lists paths, edits, and verification status. Hard to verify: whether a listed file was edited after the note was written — "Another session may have edited the files after the note was written" — or whether an edit was built or tested, not merely claimed. Observed: the new session must manually re-read files for mismatches. Inference: nothing in it automatically detects a changed or missing file.

## 3. Current status

Three days old, it presents the problem as ongoing practice, not solved: its remedy is a template plus an instruction to manually re-check. No verification tooling is mentioned; no follow-up addresses it. I cannot confirm the author is still seeking a solution — the need is live, but they did not ask anyone to build it.

## 4. Product fit and limit

Fit: `verify.mjs` compares each listed file's path, byte size, and SHA-256 against the sender's manifest, exiting 1 with `HASH_MISMATCH` or `FILE_MISSING`. Run before editing, it answers the author's question: the demo's 133-byte file, `"pages": 12` to `"pages": 13`, same size, is caught only by SHA-256.

Limit: the Kit cannot infer a file the sender never listed — the author's API-key incident, a config location known only to the old conversation, would still be invisible to any manifest. Nor can it prove the project runs: a matching file may be unbuilt and untested.

## 5. Useful next step

Reproducible test: generate a manifest for the template's "Relevant files" list, edit one listed file (simulating a later session's change), and confirm `verify.mjs` flags `HASH_MISMATCH` before the new session edits anything. Then ask whether the author would accept an auto-generated manifest as a standard template section.
