# Opsnook — Sample Data for App Review

Opsnook is a macOS menu-bar (notch) companion that monitors AI coding-agent
sessions (Claude Code, Codex, and others) by reading the session files those
tools write on the user's Mac. On a machine without those tools installed the
panel is naturally empty, so this folder provides **static sample session
files** that exercise the app's main features.

This location is permanent and will stay available for future reviews.

## Install the sample data (about 1 minute)

1. Open **Terminal** and run this single command. It downloads the sample
   archive, unpacks it into your home folder, and marks the sessions as
   recent activity:

   ```sh
   curl -fsSL https://raw.githubusercontent.com/YunFy26/Opsnook/main/review-samples/opsnook-review-samples.zip -o /tmp/opsnook-samples.zip && unzip -o /tmp/opsnook-samples.zip -d ~ && find ~/.claude/projects ~/.codex/sessions -type f -exec touch {} +
   ```

   The archive only adds files under `~/.claude/projects` and
   `~/.codex/sessions` — it does not touch any other configuration.
   (Alternatively, download [`opsnook-review-samples.zip`](./opsnook-review-samples.zip)
   manually and run the `unzip … && find … touch` part against it.)

2. Launch **Opsnook**, click the notch bar to open the panel, and open the
   gear (Settings) page. Under *Agent data access*, grant access for
   **Claude Code** and **Codex**. The folder picker opens pre-positioned at
   the right directory (`~/.claude` / `~/.codex`); just click **Open**.

## What you should see

Back on the **Sessions** tab, seven sample sessions across two projects
(*acme-storefront*, *weather-cli*):

| Session | Demonstrated feature |
| --- | --- |
| “Run the full test suite” | **Running** state (green) |
| “Choose a database migration strategy” | **Waiting for input** with an interactive question card showing the choices |
| “Fix flaky cart total rounding test” | **Error** state with the failure summary |
| “Add checkout API integration tests” | Completed session with a **subagent** badge |
| “Explain the forecast caching layer” | Completed Q&A session |
| Two **Codex** sessions | Multi-agent support (webhook refactor, `--json` flag) |

Also try:

- **Right-click a Claude session → View session timeline** for the
  step-by-step tool timeline.
- The **Activity** tab for message previews across sessions.

## Notes on session states

Opsnook derives states from live file activity, so with static sample files:

- The **Running** sample settles to *Idle* after ~10 minutes (Opsnook treats
  a session with no new output as no longer running).
- Opsnook focuses on recent activity. If you quit and relaunch Opsnook later
  and the panel looks empty, re-run the `find … touch` part of the install
  command to mark the samples as recent again.
- The **Needs approval** state and the **Usage** tab require a live agent
  process on the machine, so they are not covered by static samples. With
  Claude Code installed, an approval prompt in any session appears in
  Opsnook as an actionable approval card.

## Remove the sample data (after testing)

One command removes everything the installer added — and nothing else. It
deletes only the sample files by their exact paths, then clears the
directories they lived in if (and only if) they are now empty:

```sh
rm -rf ~/.claude/projects/-Users-reviewer-Projects-acme-storefront ~/.claude/projects/-Users-reviewer-Projects-weather-cli ~/.codex/sessions/2026/07/15/rollout-2026-07-15T09-12-05-019f9a01-2b3c-7d4e-8f50-a1b2c3d4e5f6.jsonl ~/.codex/sessions/2026/07/15/rollout-2026-07-15T10-31-40-019f9b77-4c5d-7e6f-9a80-b2c3d4e5f6a7.jsonl; rmdir -p ~/.codex/sessions/2026/07/15 ~/.claude/projects 2>/dev/null; echo "Opsnook sample data removed."
```

Any real Claude Code or Codex data that may exist on the machine is left
untouched: the command names only the fictional sample sessions, and the
trailing `rmdir` refuses to remove non-empty directories by design.

All sample content is fictional and was authored solely for App Review.
