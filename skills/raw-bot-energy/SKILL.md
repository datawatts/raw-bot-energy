---
name: raw-bot-energy
description: >-
  use this when creating or syncing a Cursor Codebase wiki that versions a
  person's Grok Bot / Cursor agent roster (config, skills, routines, avatars)
  with a weekday brain-sync routine
---
# raw-bot-energy

Use this when someone needs a Cursor **Codebase (Origin) wiki** that versions their Grok Bot / Cursor agent configuration: roster, channels, routines, skills, standing rules, redacted practical JSON, and **profile pictures**. One wiki per person by default. Do not use this for product application repos.

## Inputs

- Person's preferred name
- Initials (or other short slug) for the repo name (ask if missing)
- Origin / Codebase namespace (the org that already has Codebase)

## Repo name

Prefer lowercase `{initials}-bot-energy`.

- Example: Robin James Walsh → `rjw-bot-energy`
- No middle name: omit that letter (`rw-bot-energy`); do not invent one
- A shared **team** wiki may use a fixed slug such as `raw-bot-energy` or `team-bot-energy` — confirm out loud before creating
- If two people collide, ask before creating

Confirm the full `{namespace}/{slug}` before creating.

## What the wiki is

Document management for **this user's** agent brain. Not a product app. Not runtime state. Not chat transcripts.

### Tree (required)

```
README.md                 # index
HOW-WE-WORK.md            # standing rules for this roster
CONTRIBUTING.md           # who syncs, when, redaction
.gitignore
agents/README.md
agents/<kebab-name>.md
agents/_config/<agent-id>/profile.json
agents/_config/<agent-id>/settings.json
agents/_config/<agent-id>/avatar.png   # REQUIRED when the live agent has a picture
channels/README.md
channels/<kebab-name>.md
channels/_config/<channel-id>/group.json
channels/_config/<channel-id>/profile.json
routines/README.md
routines/<kebab-name>.md
skills/README.md
skills/managed.md         # names + descriptions only
skills/<skill-id>/SKILL.md
memory/README.md
memory/user.md            # standing user facts, no PII dumps
memory/projects/<slug>.md # if projects exist
```

README is the index: roster, channels, routines, skills, memory, redaction. Include this skill under `skills/`.

## Avatars (required)

Profile pictures are standing config. Losing an agent folder must not lose the face.

- For every live agent under the platform's agent-data tree that has `avatar.png` (or `avatar.jpg`), copy it into the wiki as `agents/_config/<id>/avatar.png` (normalize to PNG when practical; otherwise keep the extension and document it).
- On each agent markdown page note `avatar: yes` with a relative link to `_config/<id>/avatar.png`, or `avatar: missing` when there is no picture.
- On delete/recreate: if `_config/<old-id>/avatar.png` exists and the new agent has no face yet, surface that wiki path so the avatar can be restored — do not invent a new face unless asked.
- Diff avatars by size + mtime (or checksum). New or changed avatars count as a standing change and must be committed with the delta.
- When pushing via a cloud coding agent, attach PNG binaries (or a zip of `_config/<id>/avatar.png` trees) — never skip avatars because they are binary.

## Never commit

- Secrets, tokens, API keys, passwords, MCP credentials, `.env`, cookies, gateway/secret stores, connector credential files
- Personal PII on a life-assistant agent (phone, DOB, SSN, government IDs, tax forms, family identifiers). Those pages are name / title / description / routines only.
- Transcripts, chat databases, conversation blobs, `node_modules`

Do **not** put avatars in Never commit — they belong in `_config/<id>/`.

Copy live `profile.json` / `settings.json` / `group.json` only when they contain no secrets. If a field looks like a token, omit it. Validate JSON. Scan for `sk-`, `api_key`, `Bearer`, `password=`, SSN-like numbers.

## How to create the Codebase repo

Cloud-agent tokens for Codebase/Origin are often **per repo**. Empty repos may have **no `main`**, so a launch against a brand-new named slug can fail until `main` exists. A `new_repo` flow may mint a temporary slug instead of `{initials}-bot-energy`. An agent scoped to repo A usually cannot clone repo B.

Use this path (do not skip to a local clone as the source of truth):

1. Ask the person to **create** the Codebase repo `{namespace}/{slug}` if it does not exist, then add **any file** so `main` exists (a one-line README is enough).
2. Launch a Cursor cloud agent **on that repo**. Title it `{slug} wiki`.
3. Build the wiki from **this user's live** agent-data (agents, groups, automations/routines, user-created skills, standing memory, **avatars**). Put text/JSON in the cloud-agent prompt; attach avatar binaries as files. Do **not** tell that agent to clone a different bot-energy repo (the token will often 403).
4. Commit onto `main` (this is a wiki, not a product PR). If the platform forces a branch/PR, report the URL and do not merge unless a human should.
5. If a temporary `tmp-*` (or similar) repo was minted, leave it. Copy content into the named slug via a **new** agent on the named repo, from live agent-data, not by cloning the temp repo.

Give the person **one** human action when blocked (create + seed `main`). Do not wait overnight on a dead handoff.

## After the wiki exists

Create a **weekday late-afternoon** sync routine on the coordinating assistant (the one that owns roster/config), in the user's timezone:

- Name: `{slug} brain sync`
- Schedule: weekday workday (e.g. `0 17 * * 1-5` in their zone) — not midnight, not weekends
- Intent: snapshot live agent-data (same redaction rules **including avatars**), diff the wiki, stay **quiet** if nothing standing changed, otherwise reply to the cloud agent that can write `{slug}` with the delta (attach changed `avatar.png` files) and tell the user only when the wiki actually updated
- Point at the live Codebase URL for `{namespace}/{slug}`. Do not point at a temporary slug.

## Companion sync skill (optional)

If the roster already has a dedicated "sync brain wiki" skill, keep it aligned with this tree and the Avatars rules. This skill is enough to stand up and maintain the wiki on its own.

## Done looks like

- Live Codebase URL for `{namespace}/{slug}`
- README lists this person's roster and the redaction policy
- Every agent with a live picture has `_config/<id>/avatar.png` in the wiki
- Sync routine saved and enabled
- No secrets and no personal PII in git
