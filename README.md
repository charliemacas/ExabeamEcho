# Exabeam Echo

**A Windows/Mac desktop tool for replaying saved security event log use cases against an Exabeam Webhook Collector.** Use it to validate correlation rules, test parsers, and demonstrate detections without needing live infrastructure or a real attack.

Echo ships with a built-in library of several hundred MITRE ATT&CK-tagged use cases — realistic, vendor-accurate log samples spanning Windows, cloud platforms (AWS, Azure, GCP, M365, Google Workspace), identity providers (Okta, Duo), and dozens of security products — so you can start replaying detections the moment you install it, with nothing to author yourself.

**👉 [Landing page with download links &amp; checksums](https://charliemacas.github.io/ExabeamEcho/)**

---

## Download

Grab the latest installer from the [Releases page](https://github.com/charliemacas/ExabeamEcho/releases/latest):

- **Windows:** `Exabeam Echo Setup <version>.exe`
- **macOS:** `Exabeam Echo-<version>.dmg` (coming soon)

No admin rights are required to install or run Echo on either platform, and it writes no registry entries — everything it stores lives in your own user profile. Always verify the SHA256 checksum against the [landing page](https://charliemacas.github.io/ExabeamEcho/#checksums) before running an installer.

---

## Getting started

1. **Install and open Echo.**
   - *Windows:* run the `.exe`, follow the installer, then launch from the Start Menu or desktop shortcut.
   - *macOS:* open the `.dmg` and drag **Exabeam Echo** into **Applications**. If Gatekeeper blocks it with *"cannot be opened because the developer cannot be verified,"* open **System Settings → Privacy & Security** and click **Open Anyway**, then open it again — a one-time step per install.

2. **Create a Webhook Cloud Collector in Exabeam, in `RAW` format.** In your Exabeam tenant, go to **Collectors → Cloud Collectors → New Collector → Webhook**, give it a name, and for **Format** choose `RAW` — not `Json`. Click **Install**, then copy the Bearer token and endpoint URL shown on the install screen.

   > **Why `RAW`, not `Json`:** Echo always sends events as plain text, and a `RAW` collector accepts any body — plain text, JSON, XML, whatever the use case contains — while a `Json` collector rejects anything that isn't valid JSON with an HTTP 400. One `RAW` collector covers the entire bundled library, so you only need to do this once.

3. **Add that collector to Echo as a webhook target.** Back in Echo, open **Settings** and add a new Webhook using the endpoint URL and Bearer token you just copied.

4. **(Optional) Configure the Search API** in the same Settings pane if you want Echo to confirm, after each replay, that your logs actually landed and parsed. This needs OAuth client credentials from your Exabeam tenant and the name you gave the collector in **Cloud Collectors**.

   > The OAuth client only needs **read-only Search** permission — Echo never writes to or modifies anything in your Exabeam tenant through this credential, only queries it.

5. **Pick a use case from the sidebar**, organized by MITRE ATT&CK tactic, and click **Replay All**. Watch the Replay tab for per-event HTTP status — if Search API is configured, verification starts automatically once your logs have had time to land.

---

## What you can do with it

- **Replay any use case, or a whole MITRE tactic folder, in one click.**
- **Realistic timing** — a global delay between events, or a per-event delay to simulate a real attack's pacing.
- **Multiple webhook and syslog targets** (Dev / UAT / Production, or separate tenants).
- **Auto-timestamping** — every replayed log's timestamp is rewritten to *now*.
- **Verify in Exabeam** — confirm events landed and parsed, with per-field inspection and saved verify history.
- **Import your own logs** from `.log`/`.txt`, `.csv`, `.json`, or paste them directly.
- **Backup and restore** your whole library as a single file.
- `Ctrl+Shift+R` (`Cmd+Shift+R` on macOS) replays the currently open use case without touching the mouse.

---

## FAQ

**Do I need to build anything to use this?** No — download the installer from Releases and run it.

**Will installing a newer version wipe my use cases, webhooks, or settings?** No. An upgrade only ever refreshes the logs of *built-in* use cases to match the new release, while any name/folder you've customized is kept. Anything you created yourself is never modified, moved, or removed. Webhooks, syslog targets, Search API settings, and saved verify history are untouched.

**Which collector format should I use — `raw` or `json`?** `raw`. See step 2 under *Getting started* above.

**Is my data sent anywhere besides the webhook/collector I configure?** No. Echo has no telemetry and no server component of its own.

---

## License

See [LICENSE.md](LICENSE.md). Echo is free to download and use; it isn't open source, and redistribution/repackaging under a different name is not permitted. "Exabeam" is a trademark of its respective owner — this is an independent, unofficial tool, not published or endorsed by Exabeam.
