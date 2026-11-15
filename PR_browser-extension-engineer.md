Title: feat(engineering): add Browser Extension Engineer agent

## What does this PR do?

Adds `engineering/engineering-browser-extension-engineer.md`, a Manifest V3 specialist for Chrome, Edge, and Firefox. The roster already covers frontend, desktop, mobile, mini-program, and CMS work; it did not have an agent for service-worker lifecycle, permission budgets, content-script isolation, `declarativeNetRequest`, or store-safe packaging.

Also appends a README Engineering Division row after Electrical Engineer, and one `agent browser-extension-engineer` line in `scripts/convert-outputs.sha256`.

The file follows the agent template: identity, mission, critical rules, four extractable vanilla-JS examples (manifest, service worker, content/popup/options round-trip, static DNR rules), workflow (permission budget → architecture → messaging contract → storage migrations → tests → store listing), success metrics, and advanced notes (dynamic DNR, side panel, offscreen documents, cross-browser zips).

## Agent Information (if adding/modifying an agent)

- **Agent Name**: Browser Extension Engineer
- **Category**: engineering
- **Specialty**: Manifest V3 extensions — service-worker lifecycle, least-privilege permissions, sender-checked messaging, declarativeNetRequest, and store-safe Chrome/Edge/Firefox packaging

## Motivation

A search of the roster and of open/closed PRs for a browser-extension or Manifest V3 specialist returned nothing. MV3 is a different platform from a web app or a desktop wrapper: the background page is gone, listeners must be registered synchronously, in-memory state dies on suspend, `webRequestBlocking` is gone, content scripts sit next to hostile page JS, and store review rejects broad host permissions. This agent is the specialist for that surface.

## Testing

Example files were extracted from the agent markdown into `/tmp/ext` (no bundler) and exercised as written.

- Headed Chromium (Playwright persistent context, `--load-extension=/tmp/ext`): content script painted a banner and pinged the worker (count 1); popup `GET_STATE` / `PING` reached count 2; CDP `ServiceWorker.stopAllWorkers` left `chrome://serviceworker-internals` at **STOPPED**; reload continued at count 3 with the same `chrome.storage.session` nonce and a new in-memory worker boot id. Options `GET_STATE` still showed stored pingCount 3.
- `npx web-ext lint --source-dir /tmp/ext`: errors 0, warnings 1 (`BACKGROUND_SERVICE_WORKER_IGNORED`; `background.scripts` is the Firefox fallback). Exit 0.
- `./scripts/lint-agents.sh engineering/engineering-browser-extension-engineer.md`: 0 error(s), 0 warning(s). Exit 0.
- `./scripts/lint-agents.sh` (whole roster): 0 error(s), 58 warning(s) in 284 files, all pre-existing. Exit 0.
- `./scripts/check-agent-originality.sh engineering/engineering-browser-extension-engineer.md`: 0.1% overlap, PASSED.
- `./scripts/convert.sh --tool cursor` and `--tool opencode`: 284 agents each; the new agent's description and `WORKER_BOOT_ID` example are present in both outputs.
- `./scripts/test-convert-outputs.sh --update`: added one `agent browser-extension-engineer` line to `scripts/convert-outputs.sha256`.
- `./scripts/test-convert-outputs.sh` (strict): **32 passed, 0 failed**.
- README Engineering table: Browser Extension Engineer row after Electrical Engineer.
- `./scripts/check-divisions.sh`: PASSED (18 divisions).
- `./scripts/check-tools.sh`: PASSED (17 tools).
- `./scripts/check-runbooks.sh`: PASSED (4 runbooks, 64 slug references).
- `./scripts/check-hermes-config-rewrite.sh`: 21/21 cases passed.
- `bash -n scripts/*.sh`: 30/30 passed.
- `./scripts/test-install.sh`: 66 passed, 0 failed, 2 known-broken (xfail).
- Remaining workflow scripts (`test-convert-frontmatter.sh`, `test-convert-slug-collisions.sh`, `test-convert-symlink-output.sh`, `test-convert-fence-model.sh`, `test-frontmatter-closing.sh`, `test-lint-fences.sh`, `test-cli-usage.sh`, `test-agent-selection.sh`, `test-install-openclaw-registration.sh`, `test-install-link-ownership.sh`, `test-install-parallel-logs.sh`, `test-install-hermes-destination.sh`, `test-install-conversion-failure.sh`, `test-install-all-path-collision.sh`, `test-install-selection-completeness.sh`, `test-convert-parallel-failure.sh`, `test-install-continue-after-failure.sh`, `python3 scripts/check-hermes-plugin.py`): all PASS.

Not verified: Chrome Web Store / addons.mozilla.org upload and human review, Firefox runtime (lint only), Edge packaging, and a live `optional_host_permissions` prompt on example.com.

## Checklist

- [x] Follows the agent template structure from CONTRIBUTING.md
- [x] Includes YAML frontmatter with `name`, `description`, `color`
- [x] Has concrete code/template examples (for new agents)
- [ ] Tested in real scenarios
- [ ] Proofread and formatted correctly

## Demo

![demo](https://raw.githubusercontent.com/mvanhorn/pr-demo-assets/main/osc-bulk-1115b/browser-extension-engineer/preview.gif)

Full recording (MP4): https://raw.githubusercontent.com/mvanhorn/pr-demo-assets/main/osc-bulk-1115b/browser-extension-engineer/out.mp4
