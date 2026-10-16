# Pipeline ADC

An **8-bit pipeline analog-to-digital converter** with a **1.5-bit-per-stage** architecture and digital error correction, designed at the transistor level in Cadence (600 nm CMOS, 5 V supply).

Final project for **ESE 6680 – Mixed Signal Circuit Design and Modeling**, University of Pennsylvania, Spring 2025 (Dr. Tania Khanna).
Team: **Kai Segimoto**, **Isaac Klein**

**[Read the full report →](REPORT.md)**

## Architecture

Each 1.5-bit stage resolves one bit plus half a bit of redundancy and passes an amplified residue to the next stage.

- **Non-overlapping clock generator**: fanout-of-3 inverter chain producing the switched-capacitor clock phases
- **Flip-around sample-and-hold amplifier**: samples on φ1 and flips the capacitor around the op-amp on φ2
- **1.5-bit flash sub-ADC**: dynamic latch comparators against resistor-divider references (2.85 V / 2.1 V)
- **Sub-DAC and residue amplification (MDAC)**: subtracts the DAC level from the held input and scales it back to full range
- **Time alignment**: SIPO shift-register chains delay each stage's bits so the whole output word lines up
- **Digital error correction**: uses the redundancy to absorb comparator and residue errors

## Results

All results are at a 2 MHz sampling clock.

| Metric | Result |
|---|---|
| Missing codes | None: all 256 codes present, monotonic |
| DNL | +0.48 / −0.51 LSB |
| INL | +0.09 / −1.56 LSB |
| SNDR | **42.81 dB** |
| SFDR | 47.64 dB |
| ENOB | **6.82 bits** |
| Power (worst case, from peak supply currents) | 2.49 W |
| FoM = P / (f<sub>s</sub> · 2<sup>ENOB</sup>) | 1.10 × 10⁻⁸ |

The DNL/INL ramp test and the FFT-based SNDR/SFDR analysis were done in MATLAB. The scripts are in the [report appendix](REPORT.md#appendix-matlab-analysis-code).

## Files

- [`REPORT.md`](REPORT.md): the full design report, readable on GitHub. It covers block schematics, design reasoning, simulations, power, linearity and spectral results, and the MATLAB code. Figures are in `report_images/`.
- `ESE 6680 Pipeline ADC.docx`: the original Word report
- `max_current.csv`, `max_current.xlsx`: peak clock, VDD and bias supply currents from the Cadence transient simulation, used for the power estimate
