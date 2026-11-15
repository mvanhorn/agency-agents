# Demo: Browser Extension Engineer

This branch holds captured evidence for `feat/browser-extension-engineer`. The example extension was extracted from `engineering/engineering-browser-extension-engineer.md` into `/tmp/ext` and loaded unpacked in headed Chromium. Proving values are held large for ~3 s each.

## Final frames (`out.mp4` / `preview.gif`)

1. **1 / 9 Test page after content-script ping** — banner `content ping #1`, session `e680c912`, worker boot `013c194d`. Chips: `pingCount = 1`.
2. **2 / 9 Popup GET_STATE · same worker** — `chrome.storage.local pingCount=1`, session `e680c912`, worker RAM boot `013c194d`.
3. **3 / 9 Popup Ping worker · storage incremented** — `pingCount=2`, session `e680c912` unchanged, same boot `013c194d`.
4. **4 / 9 chrome://serviceworker-internals** — **Running Status: RUNNING**.
5. **5 / 9 CDP ServiceWorker.stopAllWorkers** — **Running Status: STOPPED**.
6. **6 / 9 Reload after worker kill** — banner `content ping #3`, session `e680c912` unchanged, worker boot now `e8a92f53`. RAM died; storage survived.
7. **7 / 9 Options GET_STATE after worker kill** — `pingCount=3`, session `e680c912`, worker RAM boot `e8a92f53`.
8. **8 / 9 CHECKS · extracted example** — `web-ext lint` **errors 0**, warnings 1 (`BACKGROUND_SERVICE_WORKER_IGNORED`), EXIT 0.
9. **9 / 9 CHECKS · agency-agents repo** — lint-agents 0/0 PASSED; originality 0.1% PASSED; test-convert-outputs **STRICT 32 / 32** PASSED (`+1 manifest line` `agent browser-extension-engineer`); test-install.sh 66 passed, 0 failed PASSED; check-divisions / tools / runbooks PASSED.

## Media

| File | Spec |
| --- | --- |
| `out.mp4` | 1920×1080, 30 fps, 811 frames, 27.0 s, H.264 yuv420p |
| `preview.gif` | 960×540, 8 fps, 216 frames, 27.0 s, 0.30 MB |
