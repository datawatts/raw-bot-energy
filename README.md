# Raw Bot Energy

A Cursor plugin skill that versions a person's **Grok Bot / Cursor agent roster** into a Cursor **Codebase (Origin) wiki**, with a weekday sync routine and required avatar backups.

Generic by design: no company names, no product repos, no private paths. One wiki per person (or one shared team wiki if you choose that naming).

## Install

**From a public GitHub repo (after you push this):**

1. Open **Customize** in Cursor.
2. Add / import the repository (team marketplace import, or From GitHub if available).
3. Install the **raw-bot-energy** plugin.
4. Invoke the skill when standing up or syncing a bot-energy wiki.

**Local test before publish:**

```bash
mkdir -p ~/.cursor/plugins/local/raw-bot-energy
cp -R . ~/.cursor/plugins/local/raw-bot-energy/
# Reload Window, then check Customize
```

(Team/Enterprise: admins must allow local plugin imports.)

## Publish to the public Cursor Marketplace

1. Push this folder to a **public** GitHub repository (suggested name: `raw-bot-energy`).
2. Submit the repo at [cursor.com/marketplace/publish](https://cursor.com/marketplace/publish).
3. Wait for Cursor's manual review (open source required).

Team-only Publish (Customize → Skills → Publish) only lists on your **team** Default marketplace — it does not make the skill world-public.

## What the skill does

- Creates (or refreshes) a Codebase wiki that stores agent markdown + redacted `_config` JSON + `avatar.png`.
- Sets a weekday sync routine that diffs live agent-data and commits deltas quietly when nothing changed.
- Treats profile pictures as standing config so delete/recreate does not lose faces.

See `skills/raw-bot-energy/SKILL.md`.

## License

MIT
