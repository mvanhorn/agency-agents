# Demo: FPGA RTL Engineer agent

Animated replay of real captured command output from the examples in `engineering/engineering-fpga-rtl-engineer.md` and the repository checks.

## What the replay shows

Large-type cards. Most of the 24 s is spent on lint → PASS → broken-CDC FAIL (each held ~5 s). Roster/convert checks are one summary card at the end.

| Time | Frame | On screen |
|------|-------|-----------|
| 0.0–0.3 s | `screenshots/00_start.png` | Title bar only: “Animated replay of real captured command output” |
| 0.3–5.4 s | `screenshots/01_lint.png` | **LINT CLEAN** — `verilator --lint-only -Wall async_fifo.sv`, `exit:0`, zero warnings |
| 5.4–10.5 s | `screenshots/02_pass.png` | **PASS** — Icarus `iverilog`/`vvp`, `PASS: 1024 words, 0 mismatches`, `exit:0` |
| 10.5–15.7 s | `screenshots/03_fail.png` | **FAIL** — gray-code bit 1 XORed on `wr_gray_next`, `FAIL: 16 mismatches after 68 reads (stopping)`, `exit:1` |
| 15.7–24.0 s | `screenshots/04_summary.png` / `05_end.png` | Roster/convert summary: lint-agents 0/0 PASSED, originality 0.0%, convert-outputs 32 passed, `convert-outputs.sha256` +1 `agent fpga-rtl-engineer`, README Engineering Division row, git 3 files +425 `76ec272`. Last ~2 s hold this card. |

## Media

- `out.mp4` — 1920x1080, 30 fps, 720 frames, 24.0 s, H.264 yuv420p
- `preview.gif` — 960x540, 12 fps, 288 frames, 24.0 s, 92 KB
- `commands.log` — exact captured stdout/stderr used in the replay
- `logs/` — full check logs, including `ffprobe.txt`
- `screenshots/` — start, lint, PASS, FAIL, summary, and end frames from `out.mp4`
