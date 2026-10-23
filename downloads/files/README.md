# ESRF hc6528 — sample 400_4 (Qu) — in-situ high-temperature powder diffraction

Analysis date: 2026-10-23. Data acquired 2026-06-10 18:09 to 2026-06-11 04:46 at ESRF beamline ID22 (proposal HC-6528). Irene Urcelay-Olabarria proposed the experiment and measured with Iñigo González de Arrieta and Mireia Sainz-Menchón; Josu M. Igartua treats the data so that the team can interpret them. Site: https://jmigartua.github.io/esrf-hc6528-400-4/ (not indexed), with the data on its Downloads page.

## Folder layout

| Path | Content |
|------|---------|
| `originals/` | The 101 `.xye` files exactly as delivered (formerly `edited-files-hc6528_400_4 2/`). Contents and names untouched. Only change: file mode set from `000` to `644` so they can be read. |
| `renamed/` | One byte-identical copy of each original (SHA-256 verified, modification times preserved) under a sortable, self-describing name. |
| `file_index.csv` | Mapping original name → new name, plus per-file metadata and SHA-256. |
| `drs/` | Project folder for the app diff-ref-struct: `Data` → `renamed/`, six plot sets in `Plots/`, the publication set and the fifteen built figures in `Publish/`. |
| `ecosystem/` | The project site (Quarto, family kit): protocol, data, timeline, figures, findings, ideas, questions, decisions, roadmap, benchmark, log. `cd ecosystem/site && quarto render`. |
| `edited-files-hc6528_400_4.zip` | Source archive, not touched. |

## File format

Plain-text `.xye`, no header, three whitespace-separated columns:

```
2theta_deg   intensity   esd
    0.008000  0.0000000      1567.0943
```

- Step 0.001° (uniform in every file), 2θ from 0.008° or 0.009° to 26.999°.
- The first ~0.02° is beamstop shadow (zero intensity, large esd).
- Integrated intensity is nearly constant across all scans (~4.6 M counts), so the patterns are already normalised and directly comparable.
- Name fields in the originals: `hc6528` proposal, `400_4` sample, `Qu` sample label, `w0003` reduction parameter (not identified), `b0001` = 0.001° bin width, `-NNNNs` = seconds elapsed since the first scan.

## New naming scheme

```
hc6528_400_4_Qu_<seq>_T<temp>C_t<elapsed>s_<class>.xye
hc6528_400_4_Qu_101_background_nosample_t38192s_long.xye
```

| Field | Meaning | Source |
|-------|---------|--------|
| `seq` | 001–101, chronological. Alphabetical order = acquisition order. | elapsed seconds |
| `T<temp>C` | Nominal temperature, zero-padded to 4 digits. | original tag (`900C`, `1270`, …) |
| `t<elapsed>s` | Seconds since scan 001, zero-padded to 5 digits. Absolute start time = 2026-06-10 18:09:48 + elapsed (file mtime matches this exactly for every file). | original name |
| `class` | `long` = 26992 points from 0.008°, median relative error ≈ 0.040. `short` = 26991 points from 0.009°, median relative error ≈ 0.047. | file content |

The `C` suffix of the original tags is not kept in the name, but recorded in `file_index.csv` (column `C_suffix_in_original`). Every C-tagged scan is `long`; three untagged scans (015, 016, 017) are also `long`.

## Experiment timeline

| seq | Temperature | Scans | Spacing | Note |
|-----|-------------|-------|---------|------|
| 001–014 | 900 → 1225 °C, 25 °C steps | 14 long | ~810 s | Heating ramp, one long scan per step |
| 015–017 | 1225, 1240 | 3 long | ~630 s | Transition to time-resolved mode |
| 018–022 | 1240 °C | 5 short | ~262 s | |
| 023–025 | 1245 °C | 3 short | ~262 s | |
| 026–030 | 1250 °C | 5 short | ~262 s | |
| 031 | 1266 °C | 1 short | | |
| 032–070 | 1270 °C | 39 short | ~262 s | Long isothermal hold, ~2.8 h |
| 071 | 1272 °C | 1 short | | |
| 072–079 | 1275 °C | 8 short | ~262 s | |
| 080–083 | 1280 °C | 4 short | ~262 s | |
| 084 | 1290 °C | 1 long | | |
| 085 | 1290 °C | 1 short | | Anomalous: relative error 0.066, well above every other scan. Shorter exposure or partial scan. |
| 086 | 1299 °C | 1 short | | |
| 087–099 | 1300 °C | 13 short | ~262 s | |
| 100 | 1300 °C | 1 long | | |
| 101 | background, no sample | 1 long | | Empty capillary, 0 peaks |

## What the data show

Crude peak detection (points >6σ above a rolling-median background, 2θ > 1°):

| Stage | Peaks above 1° | Max intensity above 1° |
|-------|----------------|------------------------|
| 900–975 °C | 2–13 | ~620 |
| 1000–1250 °C | ~45–60 | ~620–680 |
| 1270 °C hold, start → end | 54 → 138 | 630 → 990 |
| 1275–1280 °C | 150–210 | 1030–1480 |
| 1300 °C series, start → end | 230 → 320 | 1800 → 2630 |
| background | 0 | 675 (capillary/furnace scatter only) |

A crystalline phase appears between 975 and 1000 °C, then stays roughly constant up to 1250 °C. From the 1270 °C hold onward, peak count and peak intensities grow monotonically with time and temperature, i.e. a second phase grows or crystallinity increases. The 1270 °C series (39 scans at ~4.4 min spacing) is the natural dataset for kinetic analysis.

## Caveats

- Temperatures are nominal set points from the scan names; no thermocouple record is in the files.
- `w0003` could not be identified from the data alone.
- Peak counts above are a quick consistency check, not a refined analysis.
