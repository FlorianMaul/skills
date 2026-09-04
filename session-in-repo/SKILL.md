---
name: session-in-repo
description: Create (or resume) a docs/ session log in the current git repo and keep saving this conversation into it.
argument-hint: "(optional) short note on what this session is about"
disable-model-invocation: true
---

Goal: give the current git repository a persistent, on-disk log of this conversation, saved as plain Markdown under `docs/`.

Steps to perform immediately when this skill is invoked:

1. Determine the git repository root of the current working directory. If the current directory is not inside a git repository, tell the user and stop.
2. Ensure a `docs/` folder exists at the repo root. Create it if missing.
3. Look for an existing session log in `docs/` for this repo, named `docs/session-log.md`. Also check for any other file in `docs/` that looks like a prior session/handoff log (e.g. contains a "Session" heading or similar conversational structure) and ask the user whether to reuse it instead, if one is found and `docs/session-log.md` does not already exist.
4. If no existing session file is found, create `docs/session-log.md` with a top-level heading (repo name) and a `## Session started <ISO timestamp>` section.
5. If an existing file is found, append a new `## Session resumed <ISO timestamp>` section, and read the file's existing content into context so the conversation continues with full awareness of prior decisions.
6. If the user passed arguments, record them as the stated purpose of this session in the file.
7. From this point forward, for the remainder of the current CLI session, append a dated entry to that file after every subsequent user prompt: a concise summary of the prompt and of the response/decisions made (not a verbatim transcript, unless the user asks for verbatim).
8. Tell the user clearly which file is being used, and remind them that saving only happens while this CLI session stays open — a new session must re-invoke this skill (or be pointed at the file) to resume logging.

Keep entries concise and factual. Do not duplicate large content that already lives in other repo artifacts (specs, ADRs, commits) — reference them by path instead.

Redact secrets, credentials, tokens, or personal data before writing anything to disk.
