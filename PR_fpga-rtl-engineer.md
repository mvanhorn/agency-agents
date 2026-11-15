Title: feat(engineering): add FPGA and RTL Engineer agent

## What does this PR do?

Adds one engineering-division agent, `engineering/engineering-fpga-rtl-engineer.md`, for digital logic design: synthesizable SystemVerilog/VHDL RTL, self-checking testbenches, clock-domain crossing, timing constraints, and FPGA bring-up.

Adds the Engineering Division roster row in `README.md` after Electrical Engineer, and one `agent	fpga-rtl-engineer` line in `scripts/convert-outputs.sha256`.

## Agent Information (if adding/modifying an agent)

- **Agent Name**: FPGA RTL Engineer
- **Category**: engineering
- **Specialty**: RTL, CDC-safe async FIFOs, self-checking simulation, SDC/XDC constraints, timing closure, and FPGA bring-up

## Motivation

The engineering division already has specialists for embedded firmware, electrical power systems, and network operations. It does not have a specialist for digital logic: RTL in SystemVerilog/VHDL, simulation testbenches, clock-domain crossing, timing closure, and FPGA bring-up. This agent fills that gap with runnable examples rather than prose-only guidance.

## Testing

The fenced examples were copied out of the agent file and executed as written (open-source simulators only).

Simulator commands:

```text
verilator --lint-only -Wall async_fifo.sv
# exit 0, zero warnings

verilator --lint-only -Wall --assert axis_handshake_sva.sv
# exit 0, zero warnings

iverilog -g2012 -o tb_async_fifo.vvp async_fifo.sv tb_async_fifo.sv && vvp tb_async_fifo.vvp
# PASS: 1024 words, 0 mismatches
# exit 0

# Broken copy: XOR gray-code bit 1 on wr_gray_next only
iverilog -g2012 -o tb_broken.vvp async_fifo_broken.sv tb_async_fifo.sv && vvp tb_broken.vvp
# FAIL: 16 mismatches after 68 reads (stopping)
# exit 1
```

Repository checks:

```text
./scripts/lint-agents.sh engineering/engineering-fpga-rtl-engineer.md
# Results: 0 error(s), 0 warning(s) in 1 files. PASSED

./scripts/check-agent-originality.sh engineering/engineering-fpga-rtl-engineer.md
# [OK] 0.0%  engineering/engineering-fpga-rtl-engineer.md
# PASSED

./scripts/check-divisions.sh
# PASSED: 18 divisions consistent ...

./scripts/check-tools.sh
# PASSED: 17 tools consistent ...

./scripts/convert.sh --tool cursor --out /tmp/aa-convert
# Converted 284 agents; fpga-rtl-engineer.mdc description intact

./scripts/convert.sh --tool opencode --out /tmp/aa-convert
# Converted 284 agents; name 'FPGA RTL Engineer', color '#008080'

bash scripts/test-convert-outputs.sh
# Results: 32 passed, 0 failed  (284 roster agents x 15 tools)
# PASSED

./scripts/test-install.sh
# Results: 66 passed, 0 failed, 2 known-broken (xfail). PASSED
```

README Engineering Division table includes the FPGA RTL Engineer row after Electrical Engineer. `scripts/convert-outputs.sha256` includes the `agent	fpga-rtl-engineer` line so strict convert-outputs mode passes.

Also passed: `scripts/test-lint-fences.sh`, `test-frontmatter-closing.sh`, `test-convert-frontmatter.sh`, `test-convert-slug-collisions.sh`, `test-cli-usage.sh`, `check-runbooks.sh`, `check-hermes-config-rewrite.sh`, and the install/convert CI helpers wired from `.github/workflows/check-tools.yml`.

Not verified: vendor synthesis (Vivado/Quartus), place-and-route WNS on a physical part, or ILA/SignalTap capture on hardware.

## Checklist

- [x] Follows the agent template structure from CONTRIBUTING.md
- [x] Includes YAML frontmatter with `name`, `description`, `color`
- [x] Has concrete code/template examples (for new agents)
- [ ] Tested in real scenarios
- [ ] Proofread and formatted correctly

## Demo

![demo](https://raw.githubusercontent.com/mvanhorn/pr-demo-assets/main/osc-bulk-1115b/fpga-rtl-engineer/preview.gif)

Full recording (MP4): https://raw.githubusercontent.com/mvanhorn/pr-demo-assets/main/osc-bulk-1115b/fpga-rtl-engineer/out.mp4
