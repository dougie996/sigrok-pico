# Firmware Release Notes

UF2 files for the sigrok-pico project.

> **Note**: All UF2 files are released here rather than in `/build`.

---

## Tested RP2350 A2 / Gusman Logic Analyzer V6 build

| File | Description |
|------|-------------|
| `pico2_rp2350a2_gusman_logic_analyzer_v6_baseline_v1.uf2` | Tested working Pico 2 / RP2350 A2 baseline build for Gusman Logic Analyzer V6 hardware. Uses Pico SDK/picotool 2.3.1, `pico2`, baseline mode, and leaves GP1 out of the unused UART RX function. See `RP2350A2_GUSMAN_LOGIC_ANALYZER.md` in the repository root. |

SHA-256: `c62048c2b2a5ee76e11a46fb2ac9e0f208f3e75fc932579b77b79ccca8128405`

Working source tag: `rp2350a2-working-v1`

---

## Stable Releases (2023)

| File | Description |
|------|-------------|
| `pico_original.uf2` | Original release (~2023). Supports RP2040/PICO only, 21 digital + 3 analog + D0/D1 as UART |
| `pico_original_serial.uf2` | PR #63 build. Same as above with TUD serial config override |

---

## Major 2025 Refactor

The 2025 refactor includes:
- Redone DMA programming
- RP2350 (PICO 2) support
- 26 and 32 pin digital input modes

> **Note**: Current PulseView releases always start digital pin names at "D2". Even though dig26/dig32 enable D0/D1 as digital inputs, they will show up starting at D2.

### RP2040/PICO Variants

| File | Description |
|------|-------------|
| `pico_baseline.uf2` | Baseline: 21 digital + 3 analog |
| `pico_dig26.uf2` | 26 digital (includes D0/D1) |
| `pico_dig32.uf2` | 32 digital (includes D0/D1) |

### RP2350/PICO2 Variants

| File | Description |
|------|-------------|
| `pico2_baseline.uf2` | Baseline: 21 digital + 3 analog |
| `pico2_dig26.uf2` | 26 digital (includes D0/D1) |
| `pico2_dig32.uf2` | 32 digital (includes D0/D1) |

---

## Recommendation

For new projects, use the 2025 refactor UF2 files for RP2350 support or 26/32 digital pin support. For the specific RP2350 A2 / Gusman Logic Analyzer V6 hardware described above, use the tested tagged build instead.
