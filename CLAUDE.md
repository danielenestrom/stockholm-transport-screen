# Working rules for this repository

## Changelog

- Keep `CHANGELOG.md` up to date without being asked. Add an entry for every change to the app (features, fixes, behaviour, layout, data sources, settings).
- Group entries under a `## YYYY-MM-DD` heading, newest date first. Add to today's heading if it exists.
- One bullet per change, written for someone using or hosting the board: what changed and, where useful, why. No file paths or internal detail unless they matter to the reader.
- Skip entries for changes to this file or to the changelog itself.

## Commits

- Commit automatically, without asking, each time a significant change or addition is finished and works: a new feature, a bug fix, a visible layout or behaviour change, or a docs update that stands on its own.
- Do not commit half-finished work or every small tweak while iterating. Group the tweaks into one commit when the change is done.
- Update `CHANGELOG.md` in the same commit as the change it describes.
- Commit on the current branch. Do not push unless asked.
- Never commit `*.local.md` files or `.claude/settings.local.json`.
