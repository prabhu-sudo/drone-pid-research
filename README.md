# Drone PID Research

Investigating how PID tuning affects quadcopter hover stability — built and tested on a custom FPV drone, analyzed with Python.

## Status

In progress. Frame built, drone flying, AUX-switch blackbox logging configured. Comparative testing across 3 PID profiles has not yet been flown/logged — this is the current focus. results/ and parts of experiment/ will be populated as test data comes in.

## Objective

Most PID-tuning advice in the FPV community is empirical and anecdotal ("feel"-based tuning). This project aims to take a more rigorous approach: define 3 distinct PID profiles, fly each under matched conditions, log flight data via Betaflight blackbox, and quantitatively compare hover stability, oscillation, and step response across profiles.

## Hardware

| Component | Spec |
|---|---|
| Frame | Mark 4, 5" freestyle, carbon fibre |
| Flight Controller | BotWing F722 (BotLab Dynamics) STM32F722, target BLDY/BOTWINGF722 |
| ESC | BotDrive AM32, 55A |
| Motors | iFlight R5 2207, 2100KV x4 |
| Props | Gemfan SL 5130 UltraLite, 3-blade, M5 mount (2xCW, 2xCCW) |
| Receiver | FlySky FS-i6X transmitter + FS-iA10B 10CH receiver, AFHDS 2A, pre-bound |
| Battery | Zeee 4S 1500mAh 120C Graphene LiPo, XT60, x2 packs |
| Charger | HOTA T6 300W, 1-6S, XT60 |
| Firmware | Betaflight 4.5.4 |

## Repo Structure

build-log/      Weekly build logs (week1.md, week2.md, week3.md) + build photos
design/         design_document.md (methodology/system design) + risk_assessment.md
experiment/     protocol.md (test procedure) + analysis.py (data processing) + raw flight logs
results/        findings.md + generated plots/charts from analysis.py
LICENSE         MIT

## Methodology (summary)

Three PID profiles are configured in Betaflight, each flown as an isolated test (one profile per flight, blackbox logging triggered via AUX switch). Flight conditions, battery charge, location, stick input pattern, are kept as consistent as possible across tests to isolate the effect of PID tuning. Full protocol in experiment/protocol.md. Full design rationale in design/design_document.md.

## Reproducing

1. Clone the repo: git clone https://github.com/prabhu-sudo/drone-pid-research.git
2. Raw blackbox logs (once collected) live in experiment/
3. Run experiment/analysis.py to regenerate plots from raw logs
4. See results/findings.md for the write-up

## Author

prabhu-sudo

## License

MIT, see LICENSE
